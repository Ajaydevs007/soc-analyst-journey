# Email Header Analysis Workflow

## Overview

Email Header Analysis is the process of examining an email's header to determine whether an email is legitimate or part of a phishing attack.

Unlike learning individual header fields, header analysis combines multiple headers to investigate a suspicious email.

The objective is to answer questions such as:

- Who claims to have sent the email?
- Did the email actually come from that organization?
- Was the email spoofed?
- Where would replies be sent?
- Did the email follow a legitimate route?
- Does the overall evidence indicate phishing?

---

# SOC Investigation Workflow

Suspicious Email

↓

Open Original Message (.eml)

↓

Analyze Email Headers

↓

Verify Email Authentication

↓

Analyze Email Body

↓

Final Verdict

---

# Email Header Analysis Checklist

## Step 1 — Verify the Claimed Sender

Check:

- From

Questions:

- Who claims to have sent the email?
- Does the display name match the email address?
- Does the domain appear legitimate?

Example:

From:
Microsoft Security <security@microsoft.com>

Remember:

The From field only represents the claimed identity.

---

## Step 2 — Verify the Sending Mail Server

Check:

- Received
- SMTP Server
- SMTP Public IP

Questions:

- Which mail server sent the email?
- What is the sender's public IP?
- Does the sending server belong to the claimed organization?

Example:

Received:
from smtp.office365.com
by mx.google.com

Extract:

- Sending Mail Server
- Sending Server IP
- Receiving Mail Server

---

## Step 3 — Verify the Mail Infrastructure

Use:

- MX Lookup
- SPF Lookup
- DKIM Lookup
- DMARC Lookup

Questions:

- Which mail servers is the domain authorized to use?
- Does the sender's mail server match the organization's email infrastructure?
- Is the SMTP server authorized?

Example Workflow:

Sender Domain

↓

MX Lookup

↓

Identify legitimate mail servers

↓

Compare with Received Header

↓

Legitimate or Spoofed?

---

## Step 4 — Verify Email Authentication

Check:

Authentication-Results

Review:

- SPF
- DKIM
- DMARC

Questions:

- Did SPF pass?
- Did DKIM pass?
- Did DMARC pass?

Important:

Authentication PASS does NOT guarantee that the email is safe.

Compromised legitimate accounts can still pass all authentication checks.

---

## Step 5 — Compare From and Reply-To

Questions:

- Does Reply-To belong to the same organization?
- Is the reply redirected to another domain?

Example:

From:
security@paypal.com

Reply-To:
paypalhelp@gmail.com

Possible Red Flag:

Reply is redirected outside the organization.

---

## Step 6 — Verify Return-Path

Questions:

- Does Return-Path belong to the organization?
- Does the email infrastructure make sense?

Remember:

Return-Path determines where delivery failure (bounce) messages are sent.

It is NOT the same as Reply-To.

---

## Step 7 — Analyze the Email Body

Inspect:

- URLs
- Attachments
- Social Engineering
- Urgency
- Requests for credentials
- Requests for payment

Questions:

- Does the email contain suspicious links?
- Does it contain malicious attachments?
- Is the user being manipulated?

---

# Two Key Questions During Header Analysis

## 1. Was the email sent from the correct SMTP server?

Verification Process:

Received Header

↓

Extract SMTP Server

↓

MX Lookup

↓

SPF Verification

↓

Determine whether the server is authorized

---

## 2. Do the From and Reply-To fields match?

Compare:

From

↓

Reply-To

Questions:

- Are both domains related?
- Is Reply-To redirecting responses elsewhere?

Important:

A mismatch is suspicious but does not automatically mean the email is phishing.

Always evaluate the email as a whole.

---

# Final Investigation Checklist

✔ Verify From

✔ Verify Received

✔ Identify Sending SMTP Server

✔ Verify SMTP Public IP

✔ Compare SMTP Server with MX Records

✔ Verify SPF

✔ Verify DKIM

✔ Verify DMARC

✔ Compare From and Reply-To

✔ Verify Return-Path

✔ Analyze URLs

✔ Analyze Attachments

✔ Review Email Content

✔ Determine Final Verdict

---

# SOC Investigation Flow

Suspicious Email

↓

Check From

↓

Check Received

↓

Identify SMTP Server

↓

Verify MX / SPF / DKIM / DMARC

↓

Compare From and Reply-To

↓

Verify Return-Path

↓

Analyze URLs

↓

Analyze Attachments

↓

Analyze Email Content

↓

Determine:

- Legitimate Email

OR

- Phishing Email

---

# Key Interview Points

- Email header analysis combines multiple header fields to investigate phishing.
- The From field shows the claimed sender only.
- Received headers identify the actual mail route.
- MX, SPF, DKIM, and DMARC help verify the sender's infrastructure.
- Reply-To should be compared with the From field.
- Return-Path should be validated as part of the email infrastructure.
- Authentication PASS does not guarantee the email is safe.
- Always evaluate the email as a whole before making a final decision.