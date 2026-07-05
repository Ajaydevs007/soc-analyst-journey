# Email Header Analysis

## What is an Email Header?

An email consists of two main parts:

- Header
- Body

The **Header** contains metadata about the email, while the **Body** contains the actual message.

Think of it like a physical letter:

Envelope (Header)

- Sender
- Recipient
- Date
- Route

Letter (Body)

- Actual message

---

## Why is Email Header Analysis Important?

SOC analysts analyze email headers to:

- Identify the claimed sender
- Verify sender authenticity
- Trace the email's route
- Detect spoofing attempts
- Investigate phishing attacks
- Build an attack timeline

---

# Email Header Workflow

Suspicious Email

↓

Read Header

↓

Check:

- From
- To
- Date
- Subject
- Reply-To
- Return-Path
- Received
- Message-ID
- Authentication-Results
- X-Spam-Status

↓

Determine if the email is legitimate or phishing

---

# Header Fields

## 1. From

Purpose:

Shows the claimed sender of the email.

Example:

From:
Indeed <donotreply@match.indeed.com>

SOC Analyst Questions:

- Who is claiming to have sent the email?
- Does the display name match the email address?
- Does the domain look legitimate?
- Can the sender be verified using SPF, DKIM, DMARC, and Received headers?

Example of a Red Flag:

From:
Microsoft Security <microsoft-alerts@gmail.com>

Display name claims Microsoft but the email address belongs to Gmail.

---

## 2. To

Purpose:

Shows the intended recipient(s).

Example:

To:
ajaydev7002kv1@gmail.com

SOC Analyst Questions:

- Who was targeted?
- Was one user targeted?
- Was the email sent to many users?
- Is it a spear-phishing attack or a mass phishing campaign?

---

## 3. Date

Purpose:

Shows when the sender's mail server sent the email.

SOC Analyst Questions:

- When was the email sent?
- Does the timestamp match the incident?
- Can the timestamp be correlated with SIEM, EDR, Firewall, or Proxy logs?
- Can it help build an attack timeline?

---

## 4. Subject

Purpose:

Summarizes the email.

SOC Analyst Questions:

- What is the email about?
- Are multiple users receiving emails with the same subject?
- Is this part of a phishing campaign?
- Does the subject contain urgency or social engineering?

---

## 5. Reply-To

Purpose:

Specifies where replies will be sent.

Example:

From:
security@paypal.com

Reply-To:
paypalhelp@gmail.com

SOC Analyst Questions:

- Does Reply-To match the From domain?
- Is the reply redirected to another domain?
- Could the attacker receive replies?

Red Flag:

Different domains between From and Reply-To.

---

## 6. Return-Path

Purpose:

Specifies where delivery failure (bounce) messages are sent.

SOC Analyst Questions:

- Does Return-Path match the sender's organization?
- Does the email infrastructure appear legitimate?
- Is the Return-Path domain suspicious?

Important:

Return-Path is NOT the same as Reply-To.

---

## 7. Received

Purpose:

Shows the path taken by the email.

Every mail server that processes an email adds one Received header.

Golden Rules:

- One mail server adds one Received header.
- Read Received headers from bottom to top.

FROM = Sending Mail Server

BY = Receiving Mail Server

Information Extracted:

- Sending mail server
- Sending server IP
- Receiving mail server
- Recipient
- Timestamp
- Transfer protocol

SOC Analyst Questions:

- What route did the email take?
- Where did the email originate?
- What was the sender's mail server?
- What was the sender's public IP?

---

## 8. Message-ID

Purpose:

Unique identifier assigned to every email.

Example:

Message-ID:
<abc123@mail.company.com>

SOC Analyst Questions:

- Is the Message-ID present?
- Can it be used to correlate logs?
- Does it appear properly formatted?

Common Uses:

- SIEM searches
- Email gateway searches
- Mail server log correlation

---

## 9. MIME-Version

Purpose:

Indicates that the email uses the MIME standard.

Example:

MIME-Version: 1.0

MIME enables:

- HTML emails
- Images
- PDFs
- Word documents
- ZIP files
- Attachments

Important:

MIME-Version does NOT indicate that the email is malicious or contains an attachment.

---

## 10. Authentication-Results

Purpose:

Stores the results of SPF, DKIM, and DMARC verification.

Created by:

The recipient's mail server (e.g., Gmail, Microsoft 365, Exchange).

Example:

Authentication-Results:

SPF = PASS

DKIM = PASS

DMARC = PASS

SOC Analyst Questions:

- Did SPF pass?
- Did DKIM pass?
- Did DMARC pass?
- Which receiving mail server performed the checks?

Important:

Authentication PASS does NOT guarantee that the email is safe.

Compromised legitimate accounts can still pass SPF, DKIM, and DMARC.

---

## 11. X-Spam-Status

Purpose:

Indicates whether the spam filter classified the email as spam.

Example:

X-Spam-Status:
No, score=2.3 required=5.0

Meaning:

score

Spam score assigned by the spam filter.

required

Spam threshold.

If:

score < required

↓

Inbox

If:

score ≥ required

↓

Spam Folder

SOC Analyst Questions:

- Was the email classified as spam?
- Why did it reach the inbox?
- Did it bypass spam filtering?

Important:

X-Spam-Status: No does NOT mean the email is safe.

---

# Header Analysis Order (SOC Workflow)

1. From
2. To
3. Date
4. Subject
5. Reply-To
6. Return-Path
7. Received
8. Authentication-Results
9. Message-ID
10. MIME-Version
11. X-Spam-Status

---

# SOC Investigation Workflow

Suspicious Email

↓

Open Original Message

↓

Analyze Headers

↓

Verify:

- SPF
- DKIM
- DMARC

↓

Trace Received Headers

↓

Inspect:

- Sender
- Reply-To
- Return-Path
- Message-ID

↓

Determine:

Legitimate Email

or

Phishing Email

---

# Key Interview Points

- The From field only shows the claimed sender.
- Authentication-Results is created by the recipient's mail server.
- SPF, DKIM, and DMARC PASS do not guarantee that an email is safe.
- One mail server adds one Received header.
- Read Received headers from bottom to top.
- Message-ID uniquely identifies an email.
- Reply-To determines where replies are sent.
- Return-Path determines where bounce messages are sent.
- MIME allows emails to contain attachments and rich content.
- X-Spam-Status indicates the spam filter's decision, not whether the email is safe.