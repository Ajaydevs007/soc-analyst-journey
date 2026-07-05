# LetsDefend Phishing Investigation Playbook

## Objective

This playbook provides a structured workflow for investigating phishing email alerts in LetsDefend.

Follow each step sequentially during every phishing investigation.

---

# Phase 1 - Parse Email

## Objective

Collect basic information about the suspicious email before beginning analysis.

---

### Step 1 - Identify the Email Timestamp

Question:

When was the email sent?

Evidence:

- Date Header
- Event Time

Record:

- Date
- Time
- Timezone

---

### Step 2 - Identify the SMTP Address

Question:

What is the email's SMTP Address?

Evidence:

- SMTP Address
- Received Header

Record:

- SMTP IP
- Mail Server IP

Purpose:

- Reputation Check
- WHOIS Lookup
- Cisco Talos
- AbuseIPDB
- VirusTotal

---

### Step 3 - Identify the Sender

Question:

What is the sender address?

Evidence:

- From
- Source Address

Record:

- Email Address
- Domain

---

### Step 4 - Identify the Recipient

Question:

What is the recipient address?

Evidence:

- To
- Destination Address

Record:

- Email Address

---

### Step 5 - Review the Email Content

Question:

Is the mail content suspicious?

Look for:

- Urgency
- Credential Requests
- Fake Login Pages
- Invoice Themes
- COVID Themes
- Gift Cards
- Payment Requests
- Grammar Mistakes
- Social Engineering

Decision:

- Suspicious
- Not Suspicious

---

### Step 6 - Check for URLs or Attachments

Question:

Are there any attachments or URLs?

Inspect:

- Hyperlinks
- Buttons
- QR Codes
- Attached Files

Decision:

- Yes
- No

---

# Phase 2 - Analyze URLs / Attachments

## Objective

Determine whether the URL or attachment is malicious.

Perform ONLY if URLs or attachments exist.

---

### URL Analysis

Collect:

- Full URL
- Domain
- Subdomain

Check Using:

- VirusTotal
- URLScan
- URLHouse
- WHOIS

Determine:

- Malicious
- Non-malicious

---

### Attachment Analysis

Collect:

- File Name
- Extension
- Hash

Check Using:

- VirusTotal
- Hybrid Analysis
- Any.Run

Determine:

- Malicious
- Non-malicious

---

# Phase 3 - Dynamic Analysis (If Required)

Execute suspicious content inside a sandbox.

Recommended Platforms:

- Any.Run
- Hybrid Analysis
- Joe Sandbox
- VMRay

Observe:

- Process Creation
- Network Connections
- Registry Changes
- File Creation
- Persistence
- C2 Communication

Collect IOCs.

---

# Phase 4 - Delivery Verification

## Objective

Determine whether the email reached the user's mailbox.

Question:

Was the email delivered?

Check:

Device Action

Possible Values:

| Device Action | Meaning |
|--------------|---------|
| Blocked | Email was blocked before reaching the user |
| Delivered | Email reached the mailbox |
| Quarantined | Email moved to quarantine |
| Deleted | Email removed automatically |

Decision:

- Delivered
- Not Delivered

---

# Investigation Checklist

## Parse Email

☐ Record Date

☐ Record SMTP Address

☐ Record Sender

☐ Record Recipient

☐ Review Subject

☐ Review Email Body

☐ Determine if Content is Suspicious

---

## URL / Attachment Analysis

☐ URLs Present

☐ Attachments Present

☐ VirusTotal

☐ URLScan

☐ URLHouse

☐ Hybrid Analysis

☐ Any.Run

---

## Dynamic Analysis

☐ Execute in Sandbox (If Required)

☐ Observe Processes

☐ Observe Network

☐ Observe Registry

☐ Collect IOCs

---

## Delivery Status

☐ Check Device Action

☐ Delivered?

☐ Blocked?

☐ Quarantined?

---

# IOC Checklist

Collect:

- Sender Email

- Recipient Email

- SMTP IP

- Subject

- Domains

- URLs

- Attachments

- SHA256 Hash

- File Names

- IP Addresses

- C2 Servers

- Registry Keys

- Process Names

---

# Investigation Workflow

Receive Phishing Alert

↓

Parse Email

↓

Collect Email Metadata

↓

Analyze Header

↓

Analyze URLs

↓

Analyze Attachments

↓

Dynamic Analysis (If Required)

↓

Collect IOCs

↓

Check Delivery Status

↓

Determine Verdict

↓

Recommend Containment

---

# Final Investigation Report

## Basic Information

- Sender
- Recipient
- SMTP IP
- Subject
- Time Sent

## Analysis

- Header Analysis
- URL Analysis
- Attachment Analysis
- Dynamic Analysis

## Indicators of Compromise

- Domains
- URLs
- IP Addresses
- Hashes

## Delivery Status

- Delivered
- Blocked
- Quarantined

## Final Verdict

- Legitimate
- Spam
- Phishing
- Malware Delivery
- Business Email Compromise (BEC)