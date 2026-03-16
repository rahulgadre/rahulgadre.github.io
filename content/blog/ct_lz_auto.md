---
title: AWS Control Tower Landing Zone 4.0 — Auto-Enrollment
date: 2026-03-16T12:00:00.000Z
tags: ["AWS", "Control Tower", "Landing Zone 4.0", "Auto-Enrollment", "Governance", "Multi-Account"]
description: Simplifying account governance at scale with AWS Control Tower Auto-Enrollment feature
---

# Simplifying Account Governance at Scale with AWS Control Tower Auto-Enrollment

Managing hundreds of AWS accounts under a unified governance framework has always been a challenge for organizations. With the release of **Landing Zone 4.0**, AWS Control Tower introduces a powerful new capability — **auto-enrollment** — that simplifies how accounts are brought under Control Tower governance, eliminating manual overhead and reducing operational complexity.

---

## What Is Auto-Enrollment?

Auto-enrollment is a new feature added to AWS Control Tower's Landing Zone settings. It automatically enrolls an AWS account into Control Tower governance when the account is moved into a registered Organizational Unit (OU). This means the account automatically inherits all the controls and baselines associated with the target OU — without any manual intervention.

If you're managing Control Tower through the CLI, you'll notice a new parameter called **"remediation type"** with the value set to **"inheritance drift"**, which governs how Control Tower handles account movements between OUs.

---

## What Does "Enrolled" Mean in Control Tower?

An enrolled account in AWS Control Tower is one that has all the proper controls and baselines inherited from its parent OU. In other words, it is in an **ideal, compliant state** — fully governed and aligned with the organizational policies defined for that OU.

---

## The Challenge Before Auto-Enrollment

Before this feature, enrolling accounts into Control Tower was a time-consuming process, especially for organizations with large existing AWS environments.

- **New accounts** created through Account Factory were automatically enrolled.
- **Existing accounts**, however, posed a significant challenge. If an organization had hundreds of accounts and deployed Control Tower on top of an existing AWS Organization, each account had to be enrolled individually.
- While options like **registering an entire OU** or **manually enrolling accounts** existed, these approaches were cumbersome and operationally heavy.
- Additionally, moving an account between OUs outside the Control Tower console would create what is known as **"move account drift"** or **"inheritance drift"** — requiring manual remediation.

---

## How Auto-Enrollment Solves This

With auto-enrollment enabled, the process is seamless:

1. **Move an account** into a registered OU using AWS Organizations APIs such as `MoveAccount` or `CreateAccount`.
2. **Control Tower automatically detects** the movement, compares the controls between the previous OU and the target OU, and brings the account into a compliant state consistent with the target OU.
3. **No manual remediation** is needed — the inheritance drift that previously required manual intervention is now handled automatically in the back end.

---

## Overcoming the Concurrent Operations Limit

One of the most impactful benefits of auto-enrollment is that it helps address the **concurrent account operations limit** in AWS Control Tower.

- When launching accounts through **Service Catalog Account Factory**, there is a limit of **5 concurrent account operations**, which can be increased to a maximum of **10**.
- This meant that only a limited number of account-related actions could be performed at any given time.
- With auto-enrollment, organizations can now bypass this limitation by leveraging AWS Organizations APIs to move or create accounts directly, with Control Tower automatically handling the enrollment in the background.

---

## Key Considerations

- **Eventual Consistency:** Auto-enrollment is an eventually consistent action. The effects may not be instant and can take time depending on the number of controls and accounts in the target OU.
- **Faster for Smaller OUs:** In cases where the baselines between the source and target OUs are similar and the number of accounts is small, the enrollment effect can be near-immediate.
- **No Drift Remediation Needed:** The inheritance or move account drift that previously required manual intervention is now automatically resolved.

---

## Comparison: Before vs. After Auto-Enrollment

| Feature | Before Auto-Enrollment | With Auto-Enrollment |
|---------|----------------------|---------------------|
| Account enrollment | Manual and time-consuming | Automatic upon account movement |
| Move account drift | Required manual remediation | Automatically resolved |
| Concurrent operations limit | Limited to 5-10 operations | Bypassed via Organizations APIs |
| Large-scale enrollment | Cumbersome OU registration | Seamless and scalable |
| Control inheritance | Inconsistent without remediation | Automatic and consistent |

---

## Getting Started

Auto-enrollment is available as part of the **Landing Zone 4.0** settings in AWS Control Tower. To enable it:

1. Navigate to your **Control Tower Landing Zone settings**.
2. Enable the **auto-enrollment** feature.
3. If using the CLI, set the **"remediation type"** parameter to **"inheritance drift"**.

---

## Conclusion

The auto-enrollment feature in AWS Control Tower Landing Zone 4.0 is a game-changer for organizations managing large multi-account environments. It eliminates the operational burden of manually enrolling accounts, automatically resolves drift, and bypasses concurrent operation limits — making governance at scale simpler, faster, and more reliable.
