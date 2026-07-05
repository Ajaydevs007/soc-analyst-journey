# Additional Phishing Techniques

## Overview

Modern phishing attacks do not always rely on suspicious domains or attacker-controlled websites.

Attackers often abuse legitimate and trusted services to bypass security controls and gain the victim's trust.

Because these services already have a good reputation, users and security tools may be less suspicious of them.

---

# Common Techniques

## 1. Abuse of Cloud Storage Services

Attackers upload malicious files to legitimate cloud storage platforms instead of hosting the files on their own servers.

Examples:

- Google Drive
- Microsoft OneDrive
- Dropbox
- Box

Example:

https://drive.google.com/file/d/xxxxxxxx

Purpose:

- Deliver malware
- Deliver ransomware
- Share malicious documents
- Hide malicious files behind trusted domains

Important:

The domain may be legitimate, but the hosted file may still be malicious.

SOC analysts should analyze the downloaded file before trusting it.

---

## 2. Abuse of Free Subdomain Services

Many legitimate services allow users to create free websites or subdomains.

Examples:

- WordPress
- Blogspot
- Wix
- GitHub Pages
- Microsoft services

Example:

https://example.wordpress.com

Purpose:

Attackers create phishing pages on trusted hosting platforms to avoid suspicion.

Important:

The parent domain may be legitimate, but the subdomain is controlled by the attacker.

SOC analysts should investigate the subdomain instead of trusting the parent domain.

---

## 3. Abuse of Online Form Services

Instead of creating their own phishing website, attackers use legitimate online form services to steal user information.

Examples:

- Google Forms
- Microsoft Forms
- Typeform
- Jotform

Example:

https://docs.google.com/forms/...

Purpose:

- Credential harvesting
- Personal information collection
- Payment information collection

Advantages for attackers:

- Trusted domain
- HTTPS enabled
- Less likely to be blocked
- Difficult for inexperienced users to recognize

---

# Why Do Attackers Use Legitimate Services?

Using trusted services helps attackers:

- Increase user trust
- Bypass email security filters
- Avoid poor domain reputation
- Avoid WHOIS-based detection
- Hide malicious content behind reputable domains

---

# SOC Analyst Investigation Workflow

Suspicious Email

↓

Identify URLs

↓

Is the domain legitimate?

↓

YES

↓

Do NOT assume the content is safe

↓

Analyze:

- Hosted file
- Subdomain
- Form
- Destination URL

↓

Determine whether malicious content is present

---

# Common Examples

## Google Drive

Trusted Domain:

drive.google.com

Possible Abuse:

- Malware hosting
- Phishing documents
- Malicious PDFs

---

## Microsoft OneDrive

Trusted Domain:

onedrive.live.com

Possible Abuse:

- Malware delivery
- Fake invoices
- Credential theft

---

## WordPress

Trusted Domain:

wordpress.com

Possible Abuse:

- Fake login pages
- Credential harvesting

---

## Blogspot

Trusted Domain:

blogspot.com

Possible Abuse:

- Fake banking pages
- Malware download pages

---

## Google Forms

Trusted Domain:

docs.google.com

Possible Abuse:

- Password collection
- Banking information collection
- Credit card theft

---

# Important Points

## Legitimate Domain ≠ Legitimate Content

A trusted domain does NOT guarantee that the hosted content is safe.

Example:

Google Drive is legitimate.

A malware file uploaded to Google Drive is NOT legitimate.

---

## Parent Domain vs Subdomain

Example:

https://company.wordpress.com

Parent Domain:

wordpress.com

Subdomain:

company

The attacker controls the subdomain, not WordPress.

Always analyze the complete URL.

---

## WHOIS Limitation

WHOIS information applies to the parent domain.

Example:

blogspot.com

WHOIS shows:

Google

NOT

The attacker who created:

attacker.blogspot.com

Therefore, WHOIS alone cannot determine whether a hosted subdomain is malicious.

---

# SOC Investigation Checklist

✔ Do not trust a URL simply because it belongs to Google or Microsoft.

✔ Inspect the complete URL.

✔ Check the hosted file.

✔ Analyze attachments before opening.

✔ Verify the destination website.

✔ Inspect forms requesting credentials.

✔ Analyze the email as a whole.

---

# Key Interview Points

- Legitimate services can be abused for phishing.
- Trusted domains do not guarantee trusted content.
- Cloud storage services can host malware.
- Free subdomains may be attacker-controlled.
- Google Forms and Microsoft Forms can be used for credential harvesting.
- WHOIS generally provides information about the parent domain, not the attacker-controlled subdomain.
- Always evaluate the complete email before determining whether it is safe.

---

# Common Interview Questions

### Why do attackers use Google Drive or OneDrive during phishing attacks?

### Is a Google Drive link always safe?

### Why can WordPress or Blogspot subdomains be abused by attackers?

### Can WHOIS identify the owner of a Blogspot or WordPress subdomain?

### Why are Google Forms commonly used in phishing attacks?

### Does a trusted domain guarantee that the content is safe?

### What should a SOC analyst verify before trusting a file hosted on a legitimate cloud service?