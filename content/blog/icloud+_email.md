---
title: How to Use iCloud+ to Create a Custom Email Address with Your Own Domain Name
date: 2025-11-06 20:08:47 +07:00
tags: [Apple]
description: Using iCloud+ to Create a Custom Email Address
---

If you want an email address that looks more professional than `@icloud.com`—for example, `you@yourdomain.com`—Apple’s iCloud+ makes that possible.  
With iCloud+, you can use your own custom domain name for iCloud Mail, keeping your email within Apple’s secure ecosystem.

This guide explains how to set up a custom email domain with iCloud+ step by step.

---

## What You Need Before You Start

Before you begin, make sure you have:

1. An active iCloud+ subscription  
   (Included with any paid iCloud storage plan.)
2. A domain name you own  
   You can purchase one from registrars such as Namecheap, GoDaddy, or Google Domains.
3. Access to your domain’s DNS settings  
   You’ll need these to connect your domain to Apple’s mail servers.

---

## Step 1: Go to iCloud Settings for Custom Email Domains

You can set up your custom email domain on a Mac, iPhone, or directly from the web.

1. Visit [icloud.com](https://www.icloud.com) and sign in.  
2. Click your name or account icon in the top-right corner.  
3. Select **iCloud Settings**.  
4. Under **Custom Email Domain**, click **Manage** and then **Add a Domain**.

---

## Step 2: Add Your Custom Domain

When prompted:

1. Enter your domain name (for example, `yourdomain.com`).  
2. Choose whether you want to:
   - Use the domain only for yourself, or  
   - Share it with family members through Family Sharing.  
3. Click **Continue**.

---

## Step 3: Add the Email Addresses You Want

You can now create one or more custom email addresses, such as:
- `hello@yourdomain.com`
- `me@yourdomain.com`
- `support@yourdomain.com`

You can add up to five custom domains with iCloud+, and each domain can have three email addresses per user.

---

## Step 4: Update Your DNS Records

Apple will show you a set of DNS records that you need to add at your domain registrar. These usually include:

- **MX records** – Direct email to Apple’s mail servers.  
- **TXT records** – Verify ownership of your domain.  
- **CNAME records** – Provide security and spam protection.  

Add the following records to your DNS settings, then return to iCloud and click **Verify**. Verification may take a few minutes or up to an hour.

#### MX Records:

```
Record Type: MX Host: @ Value: mx01.mail.icloud.com. Priority: 10
```

```
Record Type: MX Host: @ Value: mx02.mail.icloud.com. Priority: 10
```

#### DKIM Record:

```
Record Type: CNAME Host: sig1._domainkey Value: sig1.dkim.example.com.at.icloudmailadmin.com.
```

For more details, check Apple’s official support guide:  
[Set up an existing domain with iCloud Mail](https://support.apple.com/en-us/102374)

---

## Step 5: Start Using Your Custom Email

Once verified, your new email address (for example, `you@yourdomain.com`) will appear in:
- The Mail app on your iPhone, iPad, and Mac  
- iCloud Mail on the web  

For more details, check Apple’s official support guide:  
[Set up a custom email domain with iCloud Mail](https://support.apple.com/en-us/HT212514)

