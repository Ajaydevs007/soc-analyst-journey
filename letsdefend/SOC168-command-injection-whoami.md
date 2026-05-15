# Alert: SOC168 — Whoami Command Detected in Request Body
**Platform:** LetsDefend  
**Date:** 10 May 2026  
**Severity:** Critical  
**Category:** Command Injection / Remote Code Execution  
**Classification:** True Positive — Attack SUCCESSFUL  
**Actions Taken:** Device Contained + Tier 2 Escalated  

## Medium Walkthrough
Full detailed step-by-step walkthrough published on Medium:  
https://medium.com/@ajaydevsv/investigating-a-command-injection-attack-soc168-whoami-command-detected-in-request-body-02dd39281c1d

---

## Alert Details
| Field | Value |
|-------|-------|
| EventID | 118 |
| Event Time | Feb 28, 2022, 04:12 AM |
| Rule | SOC168 - Whoami Command Detected in Request Body |
| Source IP | 61.177.172.87 |
| Destination IP | 172.16.17.16 |
| Destination Host | WebServer1004 |
| HTTP Method | POST |
| Requested URL | https://172.16.17.16/video/ |
| User Agent | Mozilla/4.0 MSIE 6.0 Windows NT 5.1 SV1 |
| Alert Trigger | Request Body Contains whoami string |
| Device Action | Allowed |

---

## Step 1 — Understanding the Alert

Rule name: SOC168 - Whoami Command Detected in Request Body

The keyword whoami is a Linux reconnaissance command.
Attackers execute whoami immediately after gaining
command execution to identify the privilege level
of the compromised process.

Presence of whoami in an HTTP request body strongly
suggests one of:
- Command Injection via web parameter
- Remote Code Execution (RCE)
- Active web shell usage

This immediately flagged as high priority investigation.

---

## Step 2 — Traffic Direction

Source: 61.177.172.87 (Public IP — Internet)
Destination: 172.16.17.16 (Internal — WebServer1004)

Direction: Internet → Company Network

The attack originated externally from the internet
targeting a public-facing internal web server.

---

## Step 3 — Attacker IP Investigation

Checked 61.177.172.87 across three threat
intelligence platforms:

### VirusTotal
- 4/92 security vendors flagged as malicious
- Associated with known malicious activity

### AbuseIPDB
- Reported 86,681 times from multiple sources
- Confirmed history of malicious activity
- IP belongs to ChinaNet infrastructure

### Cisco Talos
- Present in active blocklists
- Confirmed malicious reputation

### IP Ownership Details
| Field | Value |
|-------|-------|
| Network | 61.177.128.0/17 |
| ASN | AS4134 |
| ASN Label | ChinaNet |
| Registry | APNIC |
| Country | China (CN) |

Verdict: Confirmed malicious external IP

---

## Step 4 — Target Server Investigation

| Field | Value |
|-------|-------|
| Hostname | WebServer1004 |
| OS | Ubuntu 20.04.02 |
| Primary User | webadmin3 |
| Last Login | Feb 08, 2022, 11:11 AM |
| Domain | letsdefend.local |

Key observations:
- Ubuntu Linux server — targeted with Linux commands
- Last legitimate login was Feb 08 — 20 days before attack
- Public-facing web server — exposed attack surface
- No admin logged in at time of attack — 04:12 AM

---

## Step 5 — HTTP Traffic Analysis

Searched Log Management for traffic between
61.177.172.87 and 172.16.17.16.

Found 5 malicious POST requests within 5 minutes:

| Time | URL | POST Parameter | Purpose |
|------|-----|---------------|---------|
| 04:11 AM | /video/ | ?c=ls | Directory listing — map server files |
| 04:12 AM | /video/ | ?c=whoami | Check executing user identity |
| 04:13 AM | /video/ | ?c=uname | Get OS version and kernel info |
| 04:14 AM | /video/ | ?c=cat /etc/passwd | Read all user accounts |
| 04:15 AM | /video/ | ?c=cat /etc/shadow | Steal password hashes |

### Critical Findings from Traffic Analysis

Finding 1 — Systematic attack chain:
The commands follow a deliberate progression:
Reconnaissance → Identity check → OS fingerprint
→ User enumeration → Credential theft

This is not random — it is an experienced attacker
following a methodical post-exploitation checklist.

Finding 2 — All responses HTTP 200:
Every malicious request received a successful
200 OK response. No blocking occurred at any layer.

Finding 3 — Device Action Allowed:
No security control blocked any request.
All traffic reached the server unimpeded.

Finding 4 — Early morning timing:
04:11 AM to 04:15 AM — deliberate off-hours attack.
Attackers choose early morning to minimize chance
of real-time detection by SOC analysts on shift.

Finding 5 — /etc/shadow accessed:
This is the most critical finding.
/etc/shadow contains encrypted password hashes
for every user account on the system.
If retrieved, attacker can crack hashes offline
and gain access to every account on WebServer1004.
This immediately escalates severity to Critical.

---

## Step 6 — Planned Test Verification

Checked Email Security for:
- IP address: 61.177.172.87
- Hostname: WebServer1004
- Username: webadmin3

No planned penetration test emails found.
No scheduled maintenance communications found.
Hostname does not match any attack simulation
product (Verodin, AttackIQ, Picus etc.)

Verdict: NOT a planned test — real attack

---

## Step 7 — Attack Success Determination

Method: Endpoint Security Terminal History

Opened WebServer1004 in Endpoint Security.
Checked Terminal History around 04:11-04:15 AM
February 28, 2022.

Result: Commands found in terminal history.
The injected commands were actually executed
on the server by the web application process.

This confirms:
- The /video/ endpoint accepts and executes
  OS commands via the POST parameter ?c=
- Input validation is completely absent
- The web application is running with sufficient
  privileges to read /etc/passwd and /etc/shadow

Verdict: Attack was SUCCESSFUL — server COMPROMISED

---

## Step 8 — Containment Actions

Since confirmed compromise — immediate action taken.

Action: Clicked Request Containment in Endpoint Security
Result: WebServer1004 isolated from network

Containment prevents:
- Further attacker commands executing
- Lateral movement to other internal systems
- Persistence mechanisms being installed
- Additional data exfiltration

---

## Step 9 — Tier 2 Escalation

Escalation required because:
- Attack succeeded — confirmed execution
- Server is compromised
- /etc/shadow was accessed — credential data at risk
- Public-facing server compromise may indicate
  wider attack campaign

Tier 2 escalation: YES — performed immediately

---

## MITRE ATT&CK Mapping

| Tactic | Technique | ID | What happened |
|--------|-----------|-----|--------------|
| Initial Access | Exploit Public-Facing Application | T1190 | Attacker exploited unvalidated POST parameter |
| Execution | OS Command Injection | T1059.004 | Commands executed via ?c= parameter |
| Discovery | System Information Discovery | T1082 | whoami and uname gathered OS info |
| Discovery | File and Directory Discovery | T1083 | ls mapped server directory structure |
| Credential Access | OS Credential Dumping | T1003 | cat /etc/shadow stole password hashes |
| Credential Access | Unsecured Credentials | T1552 | /etc/passwd read for user enumeration |

Full attack chain:
T1190 → T1059.004 → T1082 → T1083 → T1003

---

## Indicators of Compromise (IOCs)

| IOC | Type | Description |
|-----|------|-------------|
| 61.177.172.87 | IP Address | Attacker source IP — ChinaNet |
| 172.16.17.16 | IP Address | Compromised server |
| https://172.16.17.16/video/ | URL | Vulnerable endpoint |
| ?c=whoami | POST Parameter | Command injection parameter |
| ?c=cat /etc/shadow | POST Parameter | Credential theft command |
| Mozilla/4.0 MSIE 6.0 Windows NT 5.1 | User Agent | Attacker browser fingerprint |

---

## Recommendations

### Immediate Actions (same day)
1. Reset ALL passwords on WebServer1004 —
   /etc/shadow may have been exfiltrated
2. Force password reset for webadmin3 account
3. Check outbound traffic from WebServer1004
   after 04:15 AM — look for data exfiltration
4. Audit all other accounts that use same
   passwords as WebServer1004 accounts

### Short Term (within 1 week)
5. Fix the /video/ endpoint — implement input
   validation and sanitization for the ?c= parameter
6. The application should NEVER pass user input
   directly to OS commands — use allowlists
7. Run web application with minimum required
   privileges — not as root
8. Deploy WAF rules to block OS command keywords
   in POST body parameters

### Long Term
9. Conduct full web application security audit
10. Implement file integrity monitoring on /etc/
11. Enable auditd for syscall level logging
12. Review all web endpoints for similar
    command injection vulnerabilities
13. Implement security code review process
    for all future deployments

---

## Key Learnings

1. POST body is as dangerous as URL parameters
   Always examine the full HTTP request — not
   just the URL — when investigating web attacks

2. HTTP 200 does not mean safe
   All 5 malicious requests got 200 responses.
   Success code does not confirm legitimate traffic.

3. /etc/shadow access = Critical severity immediately
   Do not wait for additional evidence. Treat it as
   full credential compromise and reset everything.

4. Off-hours timing is deliberate attacker strategy
   04:12 AM attacks exploit reduced SOC coverage.
   Consider enhanced alerting during off-hours.

5. Systematic command progression = experienced attacker
   The ls → whoami → uname → passwd → shadow chain
   shows methodical post-exploitation methodology.

6. Contain before full investigation is complete
   When execution is confirmed, containment cannot
   wait. Isolate first, investigate further second.

7. Input validation prevents this entire attack
   A single line of code sanitizing the ?c= parameter
   would have stopped all 5 commands completely.

---

## Final Verdict

| Question | Answer |
|----------|--------|
| Is traffic malicious? | Yes |
| Attack type | Command Injection / RCE |
| Planned test? | No |
| Traffic direction | Internet → Company Network |
| Attack successful? | Yes — confirmed via terminal history |
| Containment performed? | Yes — server isolated |
| Tier 2 escalation? | Yes — attack succeeded |
| Severity | Critical |

---

## Tools Used
- LetsDefend Log Management — traffic analysis
- LetsDefend Endpoint Security — terminal history
- VirusTotal — IP reputation
- AbuseIPDB — IP abuse history
- Cisco Talos — IP blocklist check
- LetsDefend Email Security — planned test verification
