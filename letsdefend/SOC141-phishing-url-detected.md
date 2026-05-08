# Alert: SOC141 - Phishing URL Detected
**Platform:** LetsDefend  
**Date:** 07 May 2026  
**Severity:** High  
**Category:** Phishing  

## Alert Summary
A user accessed a suspicious URL that is known or 
suspected to be malicious. The request was NOT blocked — 
meaning the user successfully reached the phishing page.
This makes it a confirmed True Positive requiring 
immediate investigation.

## Alert Details
| Field | Value |
|-------|-------|
| Source IP | 172.16.17.49 |
| Destination IP | 91.189.114.8 |
| Affected User | EmilyComp |
| Time of Access | Mar 22, 2021, 09:23 PM |
| URL Accessed | http://mogagrocol.ru/wp-content/plugins/akismet/fv/index.php?email=ellie@letsdefend.io |
| Request Blocked | NO — user reached the page |
| User Agent | Mozilla/5.0 Windows NT 6.1 Chrome/79.0 |

## Investigation Steps

### Step 1 — Analyze the URL
- Domain: mogagrocol.ru — Russian TLD (.ru) — suspicious
- Path mimics legitimate WordPress plugin path 
  (wp-content/plugins/akismet) — disguise technique
- URL contains email parameter — likely credential 
  harvesting page targeting ellie@letsdefend.io
- HTTP not HTTPS — no encryption — attacker can 
  intercept everything entered

### Step 2 — VirusTotal Check
- Checked URL on VirusTotal
- Domain mogagrocol.ru flagged as malicious
- Multiple engines detected phishing activity
- Confirmed malicious destination

### Step 3 — Check if Request was Blocked
- Request was NOT blocked
- User 172.16.17.49 (EmilyComp) successfully 
  reached the phishing page
- Cannot confirm if credentials were entered
- Must treat as potential credential compromise

### Step 4 — Analyze User Agent
- Windows NT 6.1 = Windows 7 — outdated OS
- Chrome 79 — outdated browser version
- Old OS and browser = higher vulnerability to 
  exploitation from the phishing page

### Step 5 — Assess Scope
- Source IP 172.16.17.49 is internal — 
  172.16.x.x is private IP range
- Only one internal host affected
- No evidence of lateral movement yet

## Findings
- **Classification: True Positive**
- User EmilyComp accessed confirmed phishing URL
- Request was not blocked — user reached the page
- Credential harvesting page targeting email addresses
- Outdated OS and browser increases risk of compromise
- Immediate action required

## Actions Taken / Recommended
1. Block destination IP 91.189.114.8 at firewall
2. Block domain mogagrocol.ru at DNS/proxy level
3. Reset EmilyComp's password immediately
4. Check EmilyComp's account for suspicious 
   activity after 09:23 PM — unauthorized logins
5. Check if any other internal hosts accessed 
   same URL or IP
6. Notify EmilyComp's manager and IT team
7. Advise EmilyComp to report any unusual 
   account activity

## Key Learnings
- .ru domains with WordPress plugin paths are a 
  common phishing disguise technique
- NOT blocked = user reached page = treat as 
  credential compromise until proven otherwise
- Always check if other users accessed same IOC
- Old OS/browser = higher risk endpoint
- Email in URL parameter = credential harvesting

## MITRE ATT&CK Mapping
- T1566 — Phishing (Initial Access)
- T1598 — Phishing for Information (credential 
  harvesting)
- T1078 — Valid Accounts (if credentials compromised)

## One Thing to Remember
When a phishing URL is NOT blocked, always assume 
credentials were entered until proven otherwise. 
Treat it as a confirmed compromise and act 
accordingly — reset passwords first, investigate second.
