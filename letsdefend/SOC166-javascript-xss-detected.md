# Alert: SOC166 - Javascript Code Detected in Requested URL
**Platform:** LetsDefend  
**Date:** 09 May 2026  
**Severity:** High  
**Category:** Web Attack — Cross Site Scripting (XSS)  
**Classification:** True Positive — Attack NOT Successful  

## Alert Details
| Field | Value |
|-------|-------|
| EventID | 116 |
| Source IP | 112.85.42.13 |
| Destination IP | 172.16.17.17 |
| Destination Host | WebServer1002 |
| Attack Time | Feb 26, 2022, 06:56 PM |
| HTTP Method | GET |
| Requested URL | https://172.16.17.17/search/?q=<script>javascript:alert(1)</script> |
| Device Action | Allowed |
| Tier 2 Escalation | Not Required |

## Attacker Profile
| Field | Finding |
|-------|---------|
| Source IP | 112.85.42.13 |
| Location | China — China Unicom Jiangsu Province |
| ASN | AS4837 — CHINA UNICOM China169 Backbone |
| VirusTotal | 1/92 engines flagged |
| AbuseIPDB | 45,325 reports from 474 distinct sources |
| First reported | November 20, 2020 |
| Verdict | Confirmed malicious |

## Attack Timeline — Manual XSS Testing Pattern
| Time | URL | Status | Size | Notes |
|------|-----|--------|------|-------|
| 06:34 PM | https://172.16.17.17/ | 200 | Normal | Reconnaissance |
| 06:35 PM | https://172.16.17.17/about-us/ | 200 | Normal | Site mapping |
| 06:45 PM | /search/?q=test | 200 | Normal | Finding injection point |
| 06:46 PM | /search/?q=prompt(8) | 302 | 0 bytes | XSS attempt 1 — blocked |
| 06:46 PM | /search/?q=img onerror=prompt(8) | 302 | 0 bytes | XSS attempt 2 — blocked |
| 06:50 PM | /search/?q=script for eval | 302 | 0 bytes | XSS attempt 3 — blocked |
| 06:53 PM | /search/?q=svg script alert | 302 | 0 bytes | XSS attempt 4 — blocked |
| 06:56 PM | /search/?q=script javascript:alert(1) | 302 | 0 bytes | XSS attempt 5 — blocked |

## Attack Pattern Analysis
This is a MANUAL XSS testing pattern — not automated.
The attacker:
1. First mapped the website (homepage, about-us)
2. Found the search parameter accepts input
3. Systematically tried 5 different XSS bypass techniques
4. Each attempt used a different payload to evade filters

XSS Bypass Techniques Used:
- Basic JavaScript alert — prompt(8)
- HTML img tag with onerror event handler
- SVG-based script injection
- Obfuscated JavaScript using for/eval
- Classic script tag with javascript: protocol

## Was It Planned?
Checked mailbox for related artifacts.
No planned penetration test found.
Verdict: NOT a planned test.

## Attack Success Determination

### Method 1 — HTTP Response Analysis
All XSS payloads returned:
- Status code: 302 (Redirect)
- Response size: 0 bytes

Normal legitimate requests returned:
- Status code: 200
- Response size: content present

302 + 0 bytes = server redirected request away
Nothing was returned to attacker
Payload was never reflected back to execute

### Method 2 — Endpoint Security Check
Checked WebServer1002 in Endpoint Security.

Terminal History:
- Only docker-compose commands from 02.02.2022
- Nothing around Feb 26, 2022 attack time
- No suspicious command execution found

Browser History:
- Only Docker and Namecheap URLs from 02.02.2022
- Nothing around attack time

Network Action:
- Only connections from 02.02.2022
- Nothing around attack time

### Conclusion
No JavaScript execution occurred.
No payload was reflected back to attacker.
Server successfully blocked all 5 XSS attempts.
Attack was NOT successful.

## MITRE ATT&CK Mapping

### Primary Technique
T1059.007 — Command and Scripting Interpreter: JavaScript

Tactic: Execution
The attacker attempted to inject JavaScript code
into the web application to execute malicious
scripts in victims browsers.

### Supporting Techniques
T1190 — Exploit Public-Facing Application
Tactic: Initial Access
Attacker targeted a public-facing web server
attempting to exploit insufficient input validation
in the search parameter.

T1083 — File and Directory Discovery
Tactic: Discovery
Attacker first browsed homepage and about-us page
to map the application structure before attacking.

T1027 — Obfuscated Files or Information
Tactic: Defense Evasion
Multiple obfuscation techniques used to bypass
XSS filters:
- img onerror handler — bypasses script tag filters
- svg script tag — bypasses HTML sanitization
- for/eval obfuscation — bypasses keyword detection
- javascript: protocol — alternative execution method

## What is XSS — Simple Explanation
XSS (Cross-Site Scripting) injects malicious
JavaScript into web pages viewed by other users.

Types:
- Reflected XSS — payload in URL, executes when
  victim clicks malicious link (this attack type)
- Stored XSS — payload saved in database, executes
  for every user who visits the page
- DOM-based XSS — payload manipulates client-side
  JavaScript directly

For reflected XSS to succeed:
1. Server must accept payload without sanitizing
2. Server must reflect it back in HTTP response
3. Victim must load that response in browser

This attack failed at step 2 — server returned
302 redirect with 0 bytes instead of reflecting
the payload back.

## Actions Recommended
1. Block 112.85.42.13 at perimeter firewall
2. Add to threat intelligence blocklist
3. Review WAF rules — device action showed
   Allowed meaning traffic reached server
   before being redirected
4. Implement Content Security Policy (CSP)
   headers to prevent future XSS execution
5. Enable input validation and output encoding
   on all search parameters

## Tier 2 Escalation Decision
NOT required because:
- Attack came from internet — not internal
- Attack did not succeed
- No compromise of internal systems
- No lateral movement detected

Escalation required only when:
- Attack succeeds
- Internal device is compromised
- Traffic direction is internal to internal

## Key Learnings
1. 302 + 0 bytes = attack blocked and redirected
   = XSS did not execute
2. Manual XSS testing has a clear pattern —
   reconnaissance first, then systematic bypass
   attempts
3. Five different bypass techniques in one session
   shows an experienced attacker — not a script kiddie
4. Device Action = Allowed does NOT mean attack
   succeeded — it means traffic reached the server
   The server itself then blocked via redirect
5. Always check ALL endpoint security tabs —
   Processes, Network Action, Terminal History,
   Browser History

## One Thing to Remember
Device Action Allowed ≠ Attack Successful
The firewall allowed the traffic to reach the server.
The server application then blocked it via 302 redirect.
These are two different layers of defense.
Always check response code AND size together.
