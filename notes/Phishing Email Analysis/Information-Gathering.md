# Information Gathering

## Email Spoofing

Attackers can forge the sender address and make an email appear to come from a trusted source.

Example:

From: ceo@company.com

The email may actually originate from an attacker-controlled server.

---

## Email Authentication Protocols

### SPF (Sender Policy Framework)

Purpose:

Determines which servers are allowed to send emails for a domain.

Question answered:

"Did the email come from an authorized mail server?"

---

### DKIM (DomainKeys Identified Mail)

Purpose:

Ensures that the email content has not been modified.

Question answered:

"Was the email altered after being sent?"

DKIM uses:

- Private Key (sender)
- Public Key (published in DNS)

---

### DMARC (Domain-based Message Authentication, Reporting and Conformance)

Purpose:

Defines what action should be taken when SPF and/or DKIM checks fail.

Policies:

- p=none → Monitor only
- p=quarantine → Send to Spam
- p=reject → Reject the email

---

## MX Records

MX (Mail Exchange) records indicate which mail servers receive emails for a domain.

Example:

indeed.com → Google Workspace

Question answered:

"Which email infrastructure does the organization use?"

---

## SPF Records

SPF records specify which servers are authorized to send emails.

Question answered:

"Which servers may send emails for this domain?"

---

## DKIM Records

DKIM records contain public keys used to verify digital signatures.

Question answered:

"Can the email signature be validated?"

---

## DMARC Records

DMARC records define how receiving mail servers should handle failed authentication attempts.

Question answered:

"What should happen if authentication fails?"

---

## SMTP IP Address

The sender's mail server IP address can be obtained from email headers.

Example:

Received from:
156.70.22.250

WHOIS lookup can be used to determine the owner of the IP address.

Possible owners:

- Google LLC
- Microsoft
- SparkPost
- SendGrid
- Organization itself

---

## Authentication Does Not Guarantee Safety

Even when:

SPF = PASS
DKIM = PASS
DMARC = PASS

the email may still be malicious.

Possible reason:

- Compromised legitimate accounts
- Business Email Compromise (BEC)

Therefore, authentication only proves legitimacy of infrastructure, not safety of content.

---

## Email Traffic Analysis

When investigating phishing campaigns, SOC analysts may search using:

### Sender Address

Example:

info@letsdefend.io

### SMTP IP Address

Example:

127.0.0.1

### Domain

Example:

@letsdefend.io

### Organization Name

Example:

letsdefend

Attackers may use multiple email providers while keeping the same organization name.

### Subject Line

The sender address and IP may change, but the subject often remains similar.

---

## Additional Information During Investigation

SOC analysts should also examine:

- Recipient addresses
- Delivery timestamps
- Frequency of emails
- Targeted users

Repeated attacks against the same users may indicate that their email addresses have been exposed.

---

## Email Address Harvesting

Attackers may collect email addresses using tools such as:

- theHarvester
- Public websites
- Data breaches
- Pastebin leaks

Publishing personal email addresses publicly increases exposure to phishing attacks.

---

## Key Takeaway

SPF, DKIM, and DMARC help verify email authenticity, but analysts must still investigate email content, infrastructure, and campaign characteristics.