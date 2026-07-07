# SOC114 – Malicious Attachment Detected – Phishing Alert

**Platform:** LetsDefend  
**Category:** Phishing Investigation  
**Status:** ✅ Completed  
**Event ID:** 45

---

# Scenario

A high-severity phishing alert was generated after an email containing a malicious ZIP attachment was received by an employee. The email successfully bypassed the mail gateway and was delivered to the recipient's mailbox.

The objective was to determine whether the attachment was malicious, verify if the email had been delivered, investigate whether the malware had been executed, and perform containment if compromise was confirmed.

---

# Alert Information

| Field | Value |
|--------|-------|
| Rule | SOC114 - Malicious Attachment Detected - Phishing Alert |
| Event ID | 45 |
| Severity | High |
| Event Time | Jan 31, 2021, 03:48 PM |
| SMTP Address | 49.234.43.39 |
| Sender | accounting@cmail.carleton.ca |
| Recipient | richard@letsdefend.io |
| Subject | Invoice |
| Device Action | Allowed |

---

# Investigation Process

## Step 1 – Parse Email

Collected the email metadata.

### Findings

| Item | Value |
|------|-------|
| Time Sent | Jan 31, 2021, 03:48 PM |
| SMTP Address | 49.234.43.39 |
| Sender | accounting@cmail.carleton.ca |
| Recipient | richard@letsdefend.io |
| Subject | Invoice |
| Suspicious | Yes |
| Attachment | Password-Protected ZIP |
| Password | infected |

### Observations

- External sender
- Invoice-themed phishing lure
- Password-protected ZIP attachment
- Password included in the email
- Common phishing delivery technique

---

## Step 2 – Check Attachments

The email contained:

- Password-protected ZIP archive
- Password: `infected`

Password-protected archives are frequently used by attackers to bypass email security solutions that cannot inspect encrypted attachments.

---

## Step 3 – Analyze the Attachment

The attachment was uploaded to **Hybrid Analysis**.

### Result

- Threat Score: **100/100**
- Verdict: **Malicious**

The SHA256 hash was also checked in VirusTotal.

### VirusTotal

- Detection: **7/63**
- Classification: **Trojan**

The attachment was confirmed to be malicious.

---

## Step 4 – Check Mail Delivery

The incident details showed:

```
Device Action: Allowed
```

Therefore,

**Result**

- Email successfully reached the recipient's mailbox.

Playbook Answer:

> Delivered

---

## Step 5 – Delete the Email

Since the malicious email reached the user's mailbox, it was removed immediately.

Action Taken:

- Deleted malicious email from recipient mailbox.

Purpose:

- Prevent future execution
- Prevent reopening
- Prevent accidental forwarding

---

## Step 6 – Collect Indicators of Compromise (IOCs)

VirusTotal provided several Indicators of Compromise.

### SHA256

```
5c34c14865f4a98f6cc623710e445f479175aeafafcb55614b139fb61cff9de7
```

### Contacted Domains

```
andaluciabeach.net
```

### Contacted URL

```
http://andaluciabeach.net/image/network.exe
```

### Contacted IPs

```
142.250.196.78

45.137.22.189

70.38.21.229
```

---

## Step 7 – Hunt for Malware Execution

The contacted IP addresses were searched first.

### Result

No connections were found for

- 142.250.196.78
- 45.137.22.189
- 70.38.21.229

Next, the contacted domain was searched.

Search Query

```
andaluciabeach.net
```

A matching proxy log was discovered.

---

## Step 8 – Confirm Malware Execution

The proxy log revealed the following:

| Field | Value |
|--------|-------|
| Log Type | Proxy |
| Source IP | 172.16.17.45 |
| Destination IP | 5.135.143.133 |
| Destination Port | 443 |
| Request Method | GET |
| Request URL | http://andaluciabeach.net/image/network.exe |
| Device Action | Allowed |
| Process | EQNEDT32.EXE |
| Parent Process | excel.exe |
| Parent Process MD5 | 8b88ebbb05a0e56b7dcc708498c02b3e |

### Evidence

The following process chain was observed:

```
excel.exe
      │
      ▼
EQNEDT32.EXE
      │
      ▼
HTTP GET Request
      │
      ▼
network.exe
```

This confirmed:

- Attachment was opened.
- Malware executed successfully.
- Outbound communication to malicious infrastructure occurred.

Playbook Answer:

> Opened

---

## Step 9 – Containment

Since malware execution was confirmed, the endpoint was isolated using the LetsDefend EDR.

### Endpoint Information

| Field | Value |
|--------|-------|
| Hostname | RichardPRD |
| User | richard |
| IP Address | 172.16.17.45 |
| Operating System | Windows 10 |
| Domain | letsdefend.local |
| Status | Host Contained |

Containment prevents:

- Further malware downloads
- C2 communication
- Lateral movement
- Data exfiltration

---

# Indicators of Compromise (IOCs)

## Email

| IOC | Value |
|------|-------|
| Sender | accounting@cmail.carleton.ca |
| SMTP IP | 49.234.43.39 |
| Subject | Invoice |

---

## File

| IOC | Value |
|------|-------|
| File | 3.zip |
| Password | infected |
| SHA256 | 5c34c14865f4a98f6cc623710e445f479175aeafafcb55614b139fb61cff9de7 |

---

## Domain

```
andaluciabeach.net
```

---

## URL

```
http://andaluciabeach.net/image/network.exe
```

---

## Contacted IPs

```
142.250.196.78

45.137.22.189

70.38.21.229
```

---

## Internal Host

```
172.16.17.45
```

---

# MITRE ATT&CK Mapping

| Tactic | Technique |
|----------|-----------|
| Initial Access | T1566.001 – Phishing Attachment |
| Execution | T1204.002 – User Execution: Malicious File |
| Defense Evasion | T1027 – Obfuscated/Encrypted Files |
| Command and Control | T1071.001 – Application Layer Protocol |
| Incident Response | Endpoint Isolation |

---

# Artifacts Collected

- Event ID: 45
- Alert Rule: SOC114 – Malicious Attachment Detected – Phishing Alert
- SMTP IP: 49.234.43.39
- Sender Email: accounting@cmail.carleton.ca
- Recipient Email: richard@letsdefend.io
- Subject: Invoice
- Password-Protected ZIP Attachment
- ZIP Password: infected
- SHA256 Hash
- Hybrid Analysis Report
- VirusTotal Report
- Contacted Domain
- Contacted URL
- Contacted IP Addresses
- Proxy Log
- Parent Process
- Process Name
- Parent Process MD5
- EDR Containment Evidence

---

# Analyst Notes

The investigation began after a high-severity phishing alert identified an email containing a password-protected ZIP attachment. The email, themed as an invoice, originated from an external sender and successfully bypassed the email gateway, reaching the recipient because the device action was **Allowed**.

The ZIP archive was analyzed using Hybrid Analysis and VirusTotal, both confirming the attachment as malicious. Hybrid Analysis assigned a threat score of **100/100**, while VirusTotal classified the file as a Trojan.

The malicious email was deleted from the user's mailbox to prevent additional interaction. To determine whether the malware had already executed, the IOCs extracted from VirusTotal were investigated. Searches for the contacted IP addresses returned no results, but searching for the contacted domain **andaluciabeach.net** revealed a proxy log showing that the endpoint **172.16.17.45** requested `http://andaluciabeach.net/image/network.exe`.

The proxy log showed the request originated from **EQNEDT32.EXE**, launched by **excel.exe**, confirming that the malicious attachment had been executed successfully.

Since malware execution was verified, the affected endpoint was immediately isolated using the EDR platform to prevent additional command-and-control communication and lateral movement.

The incident was classified as a **True Positive**.

---

# Lessons Learned

- Password-protected ZIP files are commonly used to bypass email filtering.
- Always validate attachments using multiple sandbox services.
- IOC hunting is essential after confirming a malicious attachment.
- Searching only IP addresses may miss evidence; domains and URLs should also be investigated.
- Process correlation (`excel.exe → EQNEDT32.EXE`) provides strong evidence of execution.
- Endpoint isolation should be performed immediately after confirming compromise.

---

# Skills Practiced

- Email Triage
- Phishing Analysis
- Malware Attachment Analysis
- Hybrid Analysis
- VirusTotal
- IOC Extraction
- Threat Hunting
- Proxy Log Analysis
- Process Correlation
- Incident Response
- Endpoint Containment
- EDR Investigation
- SOC Playbook Execution

---

# Final Verdict

**True Positive**

The phishing email successfully reached the recipient and contained a malicious password-protected ZIP attachment. Malware execution was confirmed through proxy logs showing outbound communication initiated by **EQNEDT32.EXE** after being launched from **excel.exe**. The malicious email was removed from the mailbox, and the compromised endpoint was successfully contained using the EDR platform.