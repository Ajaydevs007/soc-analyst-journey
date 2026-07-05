# Alert: SOC140 - Phishing Mail Detected - Suspicious Task Scheduler

**Platform:** LetsDefend  
**Date:** 05 July 2026  
**Severity:** Medium  
**Category:** Phishing / Malicious Attachment  

---

# Alert Summary

A phishing email impersonating a COVID-19 news notification was detected. The email contained a password-protected ZIP attachment carrying a malicious PDF file. Sandbox analysis confirmed the attachment was malicious. Fortunately, the organization's email security gateway blocked the email before it reached the intended recipient, preventing any user interaction.

---

# Alert Details

| Field | Value |
|-------|-------|
| Event ID | 82 |
| Rule | SOC140 - Phishing Mail Detected - Suspicious Task Scheduler |
| Event Time | Mar 21, 2021 – 12:26 PM |
| SMTP IP | 189.162.189.159 |
| Sender | aaronluo@cmail.carleton.ca |
| Recipient | mark@letsdefend.io |
| Subject | COVID19 Vaccine |
| Device Action | Blocked |

---

# Investigation Steps

## Step 1 — Parse the Email

Collected the basic email information from the alert.

- Sender: **aaronluo@cmail.carleton.ca**
- Recipient: **mark@letsdefend.io**
- SMTP IP: **189.162.189.159**
- Subject: **COVID19 Vaccine**
- Sent Time: **Mar 21, 2021 – 12:26 PM**

### Initial Findings

- COVID-19 themed subject used as social engineering.
- Email originated from an external sender.
- Contained a password-protected ZIP attachment.

---

## Step 2 — Inspect Email Content

The email body stated:

> "Hey, did you read breaking news about Covid-19. Open it now!"

It also included the password:

```
infected
```

### Findings

- Urgent language encouraging immediate action.
- Password-protected attachment designed to evade email scanning.
- Classic phishing lure.

---

## Step 3 — Analyze the Attachment

The email contained:

```
72c812cf21909a48eb9cceb9e04b865d.zip
```

The archive contained:

```
Material.pdf
```

Hybrid Analysis results:

- Threat Score: **100/100**
- Classification: **Malicious**

### Findings

- ZIP archive classified as malicious.
- Bundled PDF also classified as malicious.
- Strong evidence of a phishing campaign distributing malware.

---

## Step 4 — Verify Email Delivery

Checked the **Device Action** field.

Result:

```
Blocked
```

### Findings

- Email never reached the recipient.
- No user interaction occurred.
- No endpoint compromise observed.

---

## Step 5 — Verify Logs

Reviewed the Exchange logs.

Verified:

- SMTP IP
- Sender email
- Recipient email
- SMTP communication
- Email metadata

All matched the alert information.

---

# Indicators of Compromise (IOCs)

| Type | Value |
|------|------|
| SMTP IP | 189.162.189.159 |
| Sender | aaronluo@cmail.carleton.ca |
| Recipient | mark@letsdefend.io |
| Subject | COVID19 Vaccine |
| ZIP Attachment | 72c812cf21909a48eb9cceb9e04b865d.zip |
| Bundled File | Material.pdf |
| SHA256 | 60541635bdb3c008a9cfa15d8df82d45ec742131c9b4ad1858b558ef7df2af38 |

---

# Findings

- **Classification:** True Positive
- Email contained a malicious password-protected ZIP archive.
- Hybrid Analysis confirmed the attachment as malicious.
- Bundled PDF file was also malicious.
- Email was successfully blocked by the mail gateway.
- No evidence that the recipient interacted with the attachment.
- No endpoint compromise detected.

---

# Actions Taken

- Verified sender, recipient, and SMTP details.
- Inspected the phishing email content.
- Analyzed the attachment using Hybrid Analysis.
- Confirmed malicious classification.
- Verified the email was blocked before delivery.
- Documented Indicators of Compromise (IOCs).
- Closed the alert as **True Positive**.

---

# Key Learnings

- Password-protected ZIP files are commonly used to bypass email security filters.
- Social engineering themes such as COVID-19 continue to be effective phishing lures.
- Sandbox analysis is essential for validating suspicious attachments.
- A blocked phishing email is still a **True Positive** because the threat itself is real.
- Always verify whether an email was delivered before initiating endpoint containment.

---

# MITRE ATT&CK Mapping

> *(To be added after completing ATT&CK analysis.)*

---

# One Thing to Remember

A phishing email that is blocked before delivery can still represent a genuine threat. Always validate suspicious attachments using sandbox analysis, confirm delivery status, and document the investigation even when no endpoint compromise occurs.