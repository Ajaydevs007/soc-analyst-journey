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
