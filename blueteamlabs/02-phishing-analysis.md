# Challenge: Phishing Analysis
**Platform:** Blue Team Labs Online  
**Date:** 11 May 2026  
**Difficulty:** Easy  
**Category:** Phishing Analysis  

## Tools Used
| Tool | Purpose | Why Safe |
|------|---------|----------|
| Mozilla Thunderbird | Open .eml file safely | Email client isolates execution |
| Sublime Text | View raw email content | Text editor cannot execute scripts |
| whois.domaintools.com | Reverse DNS lookup | Web based — no local execution |
| URL2PNG | View suspicious URL safely | Renders screenshot — no browser execution |

## Why Avoid Normal Browser and Text Editor?
Opening phishing files in normal browser or
text editor risks executing malicious content.
- Browser executes JavaScript automatically
- Text editor may trigger associated applications
- Safe tools isolate content without execution

## Email Analysis Findings

### Basic Email Details
| Field | Value |
|-------|-------|
| Primary Recipient | kinnar1975@yahoo.co.uk |
| Subject | Undeliverable: Website contact form submission |
| Date Sent | 18 March 2021 04:14 |
| Attached File | Website contact form submission.eml |

### IP Analysis
| IP | Role |
|----|------|
| 91.90.123.43 | Attacker IP |
| 103.9.171.10 | X-Originating-IP — Website server |

### Reverse DNS Lookup
IP: 103.9.171.10
Resolved Host: c5s2-1e-syd.hosting-services.net.au
Tool used: whois.domaintools.com

### Malicious URL Found in Attachment
https://35000usdperwwekpodf.blogspot.sg?p=3D9swg
https://35000usdperwwekpodf.blogspot.co.il?o=3D0hnd

Hosted on: Blogspot (Google's blogging platform)
URL2PNG result: "Blog has been removed"
Page has been taken down — likely reported

## Attack Chain Reconstruction













## Key Technical Concepts Learned

### SMTP
Simple Mail Transfer Protocol — the standard
protocol for sending emails between mail servers.
Understanding SMTP headers helps trace email origin.

### X-Originating-IP
Email header that reveals the IP address of the
server that originally sent the email.
Critical for tracing attack origin beyond
mail server hops.

### Email Bounce Attack Technique
Attacker submitted malicious content via website
contact form. The website's automated notification
system forwarded the malicious URL to the target.
The bounce message made it appear legitimate —
coming from a real website notification system.

### Safe Analysis Tools
Always use isolated tools for phishing analysis:
- Thunderbird — email client with sandboxing
- Sublime Text — pure text, no execution
- URL2PNG — screenshot service, no direct access
- whois tools — web based, no local risk

## MITRE ATT&CK Mapping
| Technique | ID | Description |
|-----------|-----|-------------|
| Phishing | T1566 | Email-based attack delivery |
| Phishing for Information | T1598 | Harvesting via malicious URL |
| Acquire Infrastructure — Web Services | T1583.006 | Using Blogspot to host phishing page |

## One Thing to Remember
Never open phishing email attachments or URLs
in your normal browser or text editor.
Always use isolated safe tools — Thunderbird,
Sublime Text, URL2PNG, any.run sandbox.
The goal is to analyze without executing.
Even a text editor can trigger file associations
that execute malicious content.

## Difficulty I Faced
Understanding the email bounce chain — why
the originating IP was different from the
attacker IP. Resolved by tracing the full
SMTP header chain step by step.
