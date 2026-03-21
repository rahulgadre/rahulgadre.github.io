---
title: Fixing Claude Code Authentication Loops with Amazon Bedrock and AWS SSO
date: 2026-03-20 18:10:00 -06:00
tags: ["AWS", "Amazon Bedrock", "Claude Code", "IAM Identity Center", "SSO"]
description: How to resolve Claude Code authentication loops and OIDC connection failures when using Amazon Bedrock with AWS IAM Identity Center (SSO)
---

If you're using Claude Code with Amazon Bedrock and authenticating via AWS IAM Identity Center (SSO), you may have encountered a frustrating issue: Claude Code repeatedly opening browser tabs demanding re-authentication — sometimes dozens of times — despite having a long-lived permission set session. This post breaks down the root cause and walks through the fix.

## The Problem

Teams deploying Claude Code with Bedrock via SSO often configure `~/.claude/settings.json` like this:

```json
{
  "awsAuthRefresh": "aws sso login",
  "env": {
    "AWS_PROFILE": "my-bedrock-profile",
    "AWS_REGION": "us-east-1",
    "CLAUDE_CODE_USE_BEDROCK": "1"
  }
}
```

With this configuration, developers report two distinct issues:

1. **Authentication loop** — Claude Code repeatedly opens browser tabs for re-authentication (up to 8+ times per day), sometimes spawning dozens of tabs in seconds. Affects developers running multiple concurrent sessions.
2. **OIDC connection failures** — `ERR_CONNECTION_CLOSED` errors when connecting to `oidc.us-east-1.amazonaws.com`, requiring a full system restart to resolve.

## Root Cause

### Issue 1: The Auth Loop

The `awsAuthRefresh` setting tells Claude Code to run `aws sso login` whenever credentials appear invalid. Here's what goes wrong:

1. The OIDC access token in `~/.aws/sso/cache/` expires after **1 hour** — this is a service-side default in IAM Identity Center and is **not configurable**, regardless of your permission set session duration.
2. When the token expires, Claude Code detects invalid credentials and fires `awsAuthRefresh`.
3. `aws sso login` opens a browser tab for authentication.
4. If anything interferes with credential validation after login completes (VPN TLS interception, credential propagation delays, multiple concurrent sessions), Claude Code sees the credentials as still invalid and fires `awsAuthRefresh` again.
5. Each retry spawns a new OAuth callback server on a different port, producing repeated browser popups.

With multiple concurrent Claude Code sessions, this can produce 50+ browser tabs in under two minutes, with all sessions stuck indefinitely.

**The key insight**: the `awsAuthRefresh` setting is **not necessary** when using the SSO token provider configuration (the `sso-session` block in `~/.aws/config`). Claude Code uses the AWS SDK for JavaScript v3 internally, which has built-in support for automatic SSO token refresh. When the 1-hour access token expires, the SDK silently calls the `CreateToken` API using the refresh token (~3 month lifetime) to obtain a new access token — no browser interaction required.

### Issue 2: OIDC Connection Failures

The `ERR_CONNECTION_CLOSED` error when connecting to `oidc.us-east-1.amazonaws.com` is caused by VPN TLS interception interfering with the connection to the OIDC endpoint. Symptoms include:

- Errors persist after VPN disconnect/reconnect
- Only a full system restart resolves the issue
- Blank or garbled authentication pages in the browser

This indicates corrupted TLS state at the OS level, caused by the VPN proxy caching or corrupting responses.

## Understanding SSO Session Types

A common source of confusion is the relationship between different session durations in IAM Identity Center:

| Session Type | Duration | What It Controls |
|---|---|---|
| **OIDC access token** | 1 hour (fixed) | Token in `~/.aws/sso/cache/` used by the SDK to call `GetRoleCredentials` |
| **OIDC refresh token** | ~3 months | Used by the SDK to silently obtain new access tokens |
| **Permission set session** | Configurable (e.g., 12 hours) | Lifetime of the IAM role credentials obtained via `GetRoleCredentials` |
| **User interactive session** | Configurable in IdP | Controls when the user must re-enter username/password in the browser |

The 1-hour access token is **designed** to be silently refreshed by the SDK using the refresh token. This is why the `sso-session` configuration in `~/.aws/config` is critical — it enables this automatic refresh mechanism.

## The Fix

### Step 1: Remove `awsAuthRefresh` from Claude Code Settings

Edit `~/.claude/settings.json` and remove the `awsAuthRefresh` entry:

```json
{
  "env": {
    "AWS_PROFILE": "my-bedrock-profile",
    "AWS_REGION": "us-east-1",
    "CLAUDE_CODE_USE_BEDROCK": "1"
  }
}
```

### Step 2: Ensure `~/.aws/config` Uses the `sso-session` Format

Your AWS config must use the `sso-session` block to enable automatic token refresh:

```ini
[profile my-bedrock-profile]
sso_session = my-sso-session
sso_account_id = <account-id>
sso_role_name = <role-name>
region = us-east-1
output = json

[sso-session my-sso-session]
sso_start_url = https://<your-directory-id>.awsapps.com/start/#
sso_region = us-east-1
sso_registration_scopes = sso:account:access
```

If your config uses the legacy format (with `sso_start_url` directly in the profile block instead of referencing a `sso_session`), the SDK cannot perform silent token refresh and you will be prompted every hour.

### Step 3: Remove `~/.aws/credentials` If It Exists

If a `~/.aws/credentials` file exists, remove or rename it. This file can interfere with the SDK credential resolution chain, causing it to use static credentials instead of the SSO token provider.

```bash
mv ~/.aws/credentials ~/.aws/credentials.bak
```

### Step 4: Authenticate Once Per Day

After applying the configuration changes, authenticate once at the start of the day:

```bash
aws sso login --profile my-bedrock-profile
```

From this point forward, Claude Code handles all credential refreshes silently without opening a browser.

### Step 5: Address VPN TLS Interception (for OIDC Connection Failures)

If you're using a VPN with TLS interception (e.g., GlobalProtect, Zscaler), work with your security team to add TLS interception exceptions for:

- `oidc.us-east-1.amazonaws.com`
- `portal.sso.us-east-1.amazonaws.com`
- `<your-directory-id>.awsapps.com`

Replace the region in the endpoints above if you're using a different AWS region.

As a temporary workaround, clearing browser history/cache can resolve garbled authentication pages when they occur.

## Verification

After applying the fix, you can verify that silent token refresh is working:

```bash
# Check current token expiry
cat ~/.aws/sso/cache/*.json | jq '.expiresAt'
```

You should see a token with a 1-hour expiry. After that hour passes, Claude Code should continue working without opening a browser — the SDK will silently refresh the token and you'll see an updated `expiresAt` value in the cache.

If you are still prompted to re-authenticate after ~1 hour, this indicates VPN TLS interception is also blocking the SDK's silent refresh call. In that case, the TLS exceptions from Step 5 become critical.

## Scaling Considerations

For production deployments of Claude Code with Amazon Bedrock at scale, AWS recommends the **Direct IdP Integration** pattern using OIDC federation directly to IAM, as described in the blog post [Claude Code deployment patterns and best practices with Amazon Bedrock](https://aws.amazon.com/blogs/machine-learning/claude-code-deployment-patterns-and-best-practices-with-amazon-bedrock/). This approach:

- Avoids the SSO token refresh mechanism entirely
- Provides user-level cost attribution and monitoring
- Supports OpenTelemetry integration for observability

A deployable reference implementation is available in the [Guidance for Claude Code with Amazon Bedrock](https://aws.amazon.com/solutions/guidance/claude-code-with-amazon-bedrock/).

## References

- [IAM Identity Center credential provider — SSO token provider configuration](https://docs.aws.amazon.com/sdkref/latest/guide/feature-sso-credentials.html)
- [Understanding authentication sessions in IAM Identity Center](https://docs.aws.amazon.com/singlesignon/latest/userguide/authconcept.html)
- [Session duration considerations for identity sources, AWS CLI, and SDKs](https://docs.aws.amazon.com/singlesignon/latest/userguide/user-session-duration-prereqs-considerations.html)
- [Guidance for Claude Code with Amazon Bedrock](https://aws.amazon.com/solutions/guidance/claude-code-with-amazon-bedrock/)
- [Claude Code deployment patterns and best practices with Amazon Bedrock](https://aws.amazon.com/blogs/machine-learning/claude-code-deployment-patterns-and-best-practices-with-amazon-bedrock/)
