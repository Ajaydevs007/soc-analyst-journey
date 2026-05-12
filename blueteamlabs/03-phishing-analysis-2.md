# Challenge: Phishing Analysis 2
**Platform:** Blue Team Labs Online  
**Date:** 10 May 2026  
**Difficulty:** Easy  
**Category:** Phishing Analysis  

## Tools Used
| Tool | Purpose |
|------|---------|
| Mozilla Thunderbird | Safe email file opening |
| Sublime Text | View raw email content safely |
| URL2PNG | View suspicious URLs without browser |
| CyberChef | Decode Base64 encoded content |

## Why Safe Tools Only
Never open phishing files in normal browser
or text editor — accidental execution risk.
Thunderbird and Sublime Text isolate content.
URL2PNG renders screenshots — browser never
touches the malicious URL directly.

## Email Analysis Findings

### Basic Email Details
| Field | Value |
|-------|-------|
| Sending Address | amazon@zyevantoby.cn |
| Recipient | saintington73@outlook.com |
| Subject | Your Account has been locked |
| Date Sent | Wed, 14 Jul 2021 01:40:32 +0900 |
| Company Impersonated | Amazon |

## Indicators of Phishing

### Sender Domain Analysis
Sending address: amazon@zyevantoby.cn

Red flags:
- Domain is zyevantoby.cn — Chinese TLD (.cn)
- Has nothing to do with amazon.com
- Legitimate Amazon emails come from
  @amazon.com or @amazon.co.uk etc
- Attacker used "amazon" as the local part
  to make it look legitimate at first glance

### Subject Line Analysis
"Your Account has been locked"

Classic urgency technique — creates panic
and pressure to click without thinking.
Urgency is one of the most common phishing
social engineering tactics.

### Main Call-to-Action URL
Raw URL found by right-clicking button
and copying link location:

https://emea01.safelinks.protection.outlook.com/
?url=https%3A%2F%2Famaozn.zzyuchengzhika.cn%2F
%3Fmailtoken%3Dsaintington73%40outlook.com

Decoded destination:
https://amaozn.zzyuchengzhika.cn/
?mailtoken=saintington73@outlook.com

Red flags:
- Domain is amaozn — note the typo
  Amazon misspelled as AMAOZN
- Hosted on zzyuchengzhika.cn — Chinese domain
- mailtoken parameter contains victim email
  pre-filled — credential harvesting ready
- URL2PNG result: page could not be loaded
  — site already taken down

### Typosquatting
amaozn.zzyuchengzhika.cn
vs legitimate amazon.com

Letter swap: amazon → amaozn
Classic typosquatting technique to fool
victims who do not read URLs carefully.

### Base64 Encoding
Email body content was Base64 encoded.

Why attackers use Base64:
- Bypasses email content filters
- Scanners cannot easily read the HTML
- Encoded content looks like random characters
- Only decoded when email client renders it

Investigation method:
1. Opened raw email in Sublime Text
2. Identified Base64 encoded section
3. Copied encoded data
4. Pasted into CyberChef
5. Used From Base64 operation
6. Decoded HTML revealed full email content

### Logo URL Extraction
After Base64 decoding in CyberChef:
Decoded HTML contained img src tags.
Found Amazon logo being pulled from:

https://images.squarespace-cdn.com/content/
52e2b6d3e4b06446e8bf13ed/1500584238342-
OX2L298XVSKF8AO6I3SV/amazon-logo

Red flag: Legitimate Amazon emails use
Amazon's own CDN for images — not
Squarespace. This confirms the email
is not from real Amazon.

### Facebook URL Found
One URL in the email body contained
a Facebook profile link.

Facebook username: Famir.boyka.7

This may be the attacker's social media
profile — accidentally left in the template
or used for tracking clicks.

## Attack Chain

Attacker creates fake Amazon page
on Chinese domain (amaozn.zzyuchengzhika.cn)
↓
Sends phishing email from amazon@zyevantoby.cn
Subject: Your Account has been locked (urgency)
↓
Victim clicks "Unlock Account" button
↓
Redirected through safelinks.protection.outlook.com
(legitimate Microsoft URL wrapping — adds trust)
↓
Arrives at fake Amazon credential harvesting page
mailtoken parameter pre-fills victim email
↓
Victim enters password → credentials stolen


## SafeLinks Wrapping — Important Finding
The malicious URL was wrapped inside:
emea01.safelinks.protection.outlook.com

This is Microsoft's URL protection service.
Attackers abuse it because:
- The outer URL looks legitimate (microsoft.com)
- Some email filters trust safelinks URLs
- Victims see a Microsoft URL and trust it
- The actual malicious URL is URL-encoded inside

Always decode the inner URL to find
the real destination.

## MITRE ATT&CK Mapping
| Technique | ID | Description |
|-----------|-----|-------------|
| Phishing | T1566.001 | Spearphishing attachment |
| Obfuscated Files | T1027 | Base64 encoding to evade filters |
| Acquire Infrastructure | T1583.001 | Attacker registered fake domain |
| Impersonation | T1656 | Impersonating Amazon brand |

## Key Learnings
1. Always right-click buttons to copy URL —
   never click directly in phishing emails
2. Base64 in email body = deliberate obfuscation
   to bypass content filters — always decode
3. SafeLinks wrapping makes malicious URLs
   look legitimate — always decode inner URL
4. Typosquatting: amaozn vs amazon —
   always check spelling of domains carefully
5. Pre-filled email in URL parameter =
   credential harvesting page ready to capture
6. Logo hosted on wrong CDN = fake email
   Legitimate companies use their own CDN

## Comparison with Phishing Analysis 1
| Feature | Phishing 1 | Phishing 2 |
|---------|-----------|-----------|
| Encoding | None | Base64 |
| URL wrapping | Direct | SafeLinks wrapper |
| Brand impersonated | None specific | Amazon |
| Typosquatting | No | Yes — amaozn |
| Urgency tactic | Undeliverable | Account locked |
| Complexity | Basic | Intermediate |

## One Thing to Remember
Base64 encoding in email body is a red flag.
Legitimate emails do not encode their content.
When you see random character strings in
raw email source — always try Base64 decode
in CyberChef. What looks like noise often
reveals the full attack infrastructure.
