# LetsDefend Phishing Investigation Playbook

## Objective

This playbook provides a structured workflow for investigating phishing email alerts in LetsDefend and follows the incident response process used by SOC analysts.

---

# Phase 1 - Parse Email

## Objective

Collect all basic information about the suspicious email before beginning the investigation.

### Collect Basic Information

| Question | Evidence | Record |
|----------|----------|--------|
| When was the email sent? | Date / Event Time | Date & Time |
| What is the SMTP Address? | SMTP Address / Received Header | SMTP IP |
| What is the sender address? | From / Source Address | Sender Email |
| What is the recipient address? | To / Destination Address | Recipient Email |
| What is the subject? | Subject | Email Subject |
| Is the mail content suspicious? | Email Body | Yes / No |
| Are there any attachments? | MIME / Attachment Section | Yes / No |
| Are there any URLs? | Email Body | Yes / No |

---

# Phase 2 - URL / Attachment Analysis

## Objective

Determine whether the URLs or attachments are malicious.

### If URLs Exist

Perform:

- VirusTotal
- URLScan
- URLHouse
- WHOIS
- Browser Reputation

Decision:

- Malicious
- Non-Malicious

---

### If Attachments Exist

Collect:

- File Name
- Extension
- SHA256 Hash

Analyze Using:

- VirusTotal
- Hybrid Analysis
- Any.Run

Decision:

- Malicious
- Non-Malicious

---

# Phase 3 - Dynamic Analysis (If Required)

If reputation checks are inconclusive:

Execute inside a sandbox.

Recommended:

- Any.Run
- Hybrid Analysis
- Joe Sandbox
- VMRay

Observe:

- Process Creation
- File Creation
- Registry Changes
- Network Connections
- DNS Requests
- C2 Communication
- Persistence

Collect IOCs.

---

# Phase 4 - Delivery Verification

## Objective

Determine whether the email reached the user's mailbox.

Check:

**Device Action**

Possible Values:

| Device Action | Meaning |
|---------------|---------|
| Delivered | Email reached the user |
| Blocked | Email blocked before delivery |
| Quarantined | Email moved to quarantine |
| Deleted | Email automatically removed |

Decision:

- Delivered
- Not Delivered

---

# Phase 5 - Delete Malicious Email

## Objective

Prevent additional users from interacting with the phishing email.

Action:

Delete the malicious email from the recipient's mailbox.

Verification:

- Email successfully deleted
- No longer accessible to the user

---

# Phase 6 - Check User Activity

## Objective

Determine whether any user interacted with the phishing email.

Investigate:

Log Management

Search For:

- C2 Domains
- C2 IP Addresses
- Malicious URLs
- File Hashes
- Process Execution

Determine:

- Opened
- Not Opened

If Opened:

Record:

- User
- Hostname
- Timestamp
- Process Name
- Network Connection

---

# Phase 7 - Containment

## Objective

Prevent further compromise.

If malicious activity is confirmed:

Perform containment through the EDR platform.

Actions:

- Isolate Endpoint
- Stop Malicious Process
- Block IOC
- Prevent Lateral Movement

Verification:

- Endpoint Contained

---

# Investigation Checklist

## Parse Email

☐ Record Date

☐ Record SMTP Address

☐ Record Sender

☐ Record Recipient

☐ Record Subject

☐ Review Email Body

☐ Check Attachments

☐ Check URLs

---

## URL / Attachment Analysis

☐ VirusTotal

☐ URLScan

☐ URLHouse

☐ Any.Run

☐ Hybrid Analysis

☐ Determine Reputation

---

## Dynamic Analysis

☐ Execute Sandbox (If Required)

☐ Observe Processes

☐ Observe Network

☐ Observe Registry

☐ Observe Persistence

---

## User Impact

☐ Email Delivered?

☐ Email Deleted?

☐ User Opened URL?

☐ User Executed File?

---

## Containment

☐ Endpoint Isolated

☐ IOC Blocked

☐ Incident Contained

---

# IOC Checklist

Collect:

- Sender Email
- Recipient Email
- SMTP IP
- Subject
- URLs
- Domains
- Attachments
- SHA256 Hash
- MD5 Hash
- IP Addresses
- C2 Domains
- C2 IPs
- Registry Keys
- Process Names
- Parent Process
- Child Process

---

# Investigation Workflow

Receive Phishing Alert

↓

Parse Email

↓

Header Analysis

↓

Static Analysis

↓

URL / Attachment Analysis

↓

Dynamic Analysis (If Required)

↓

Check Delivery Status

↓

Delete Malicious Email

↓

Check User Activity

↓

Contain Endpoint (If Required)

↓

Collect IOCs

↓

Determine Final Verdict

↓

Close Incident

---

# Artifacts to Collect

## Email Information

- Sender Email
- Recipient Email
- Subject
- Date & Time
- SMTP IP
- Message-ID

---

## Header Artifacts

- From
- Reply-To
- Return-Path
- Received Headers
- SPF Result
- DKIM Result
- DMARC Result

---

## URL Artifacts

- Full URL
- Domain
- Subdomain
- Redirect URL
- URL Reputation

---

## Attachment Artifacts

- File Name
- File Extension
- File Size
- SHA256 Hash
- MD5 Hash
- File Reputation

---

## Network Artifacts

- Source IP
- Destination IP
- C2 Domain
- DNS Queries
- HTTP Requests

---

## Endpoint Artifacts

- Hostname
- Username
- Process Name
- Parent Process
- Child Process
- Registry Changes
- Scheduled Tasks
- Persistence Mechanisms

---

# Analyst Notes Template

## Alert Summary

- Alert Name:
- Alert Time:
- Severity:

---

## Email Details

- Sender:
- Recipient:
- Subject:
- SMTP IP:

---

## Analysis Performed

- Header Analysis:
- URL Analysis:
- Attachment Analysis:
- Dynamic Analysis:

---

## Findings

- Email spoofing detected? (Yes/No)
- Malicious URL? (Yes/No)
- Malicious Attachment? (Yes/No)
- User Clicked Link? (Yes/No)
- User Executed File? (Yes/No)

---

## Indicators of Compromise (IOCs)

- Domains:
- URLs:
- IP Addresses:
- File Hashes:

---

## Containment Actions

- Email Deleted
- Endpoint Isolated
- IOC Blocked
- User Notified

---

## Final Verdict

- Legitimate
- Spam
- Phishing
- Malware Delivery
- Business Email Compromise (BEC)

---

## Recommendation

Document the lessons learned, update detection rules if necessary, and monitor for similar phishing attempts targeting other users.