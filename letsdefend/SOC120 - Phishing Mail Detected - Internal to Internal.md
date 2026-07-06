# Alert: SOC120 - Phishing Mail Detected - Internal to Internal

**Platform:** LetsDefend  
**Date:** 06 July 2026  
**Severity:** Medium  
**Category:** Email Security / False Positive  

---

# Alert Summary

An alert was generated for a potential phishing email sent between two internal users. The investigation confirmed that the email was legitimate internal business communication with no malicious content, attachments, or URLs. The alert was therefore classified as a **False Positive**.

---

# Alert Details

| Field | Value |
|-------|-------|
| Event ID | 52 |
| Rule | SOC120 - Phishing Mail Detected - Internal to Internal |
| Event Time | Feb 07, 2021 – 04:24 AM |
| SMTP Address | 172.16.20.3 |
| Sender | john@letsdefend.io |
| Recipient | susie@letsdefend.io |
| Subject | Meeting |
| Device Action | Allowed |

---

# Investigation Steps

## Step 1 — Parse the Email

Collected the email metadata from the alert.

- SMTP Address: **172.16.20.3**
- Sender: **john@letsdefend.io**
- Recipient: **susie@letsdefend.io**
- Subject: **Meeting**
- Sent Time: **Feb 07, 2021 – 04:24 AM**

### Findings

- Email originated from an internal user.
- Recipient was also an internal user.
- Subject appeared normal and business related.

---

## Step 2 — Review Email Content

Email Body:

> Hi Susie, Can we arrange a meeting today if you are available?

### Findings

- Normal business communication.
- No urgency or fear tactics.
- No requests for credentials.
- No suspicious wording.
- No impersonation attempts.
- No social engineering indicators.

---

## Step 3 — Check for Attachments and URLs

Verified whether the email contained any malicious payload.

### Findings

- **Attachments:** None
- **URLs:** None

Since there were no attachments or hyperlinks, there was no malicious content to analyze.

---

## Step 4 — Verify Email Delivery

Reviewed the Device Action field.

Result:

```
Allowed
```

### Findings

- Email was successfully delivered.
- Delivery was expected because the email was legitimate.

---

## Step 5 — Determine Alert Classification

After reviewing all available evidence:

- Internal sender
- Internal recipient
- Legitimate meeting request
- No attachments
- No URLs
- No malicious content
- No phishing indicators

The alert was classified as a **False Positive**.

---

# Indicators Reviewed

| Type | Value |
|------|------|
| SMTP Address | 172.16.20.3 |
| Sender | john@letsdefend.io |
| Recipient | susie@letsdefend.io |
| Subject | Meeting |
| Device Action | Allowed |
| Attachments | None |
| URLs | None |

---

# Findings

- **Classification:** False Positive
- Internal business communication.
- No phishing indicators identified.
- No attachments or URLs.
- No Indicators of Compromise (IOCs).
- Email was correctly delivered.
- No malicious activity detected.

---

# Actions Taken

- Parsed email metadata.
- Reviewed sender and recipient information.
- Inspected email content.
- Verified absence of attachments.
- Verified absence of URLs.
- Confirmed legitimate internal communication.
- Closed the alert as **False Positive**.

---

# Key Learnings

- Not every phishing alert is malicious.
- Always validate alerts using the available evidence.
- Internal emails should still be investigated to rule out account compromise or impersonation.
- Legitimate business emails can occasionally trigger detection rules.
- Properly identifying False Positives helps reduce alert fatigue and allows analysts to focus on genuine threats.

---

# MITRE ATT&CK Mapping

Not Applicable (False Positive)

No malicious techniques or adversary behavior were observed during the investigation.

---

# One Thing to Remember

A good SOC analyst doesn't only detect attacks—they also recognize when an alert is benign. Always investigate the evidence before deciding whether an alert is a True Positive or a False Positive.