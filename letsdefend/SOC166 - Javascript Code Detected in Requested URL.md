# Alert: SOC166 - Javascript Code Detected in Requested URL

**Platform:** LetsDefend
**Date:** 09 May 2026
**Severity:** Medium
**Category:** Web Attack — Cross-Site Scripting (XSS)
**Classification:** True Positive — Attack NOT Successful

## Alert Details

| Field            | Value                  |
| ---------------- | ---------------------- |
| Source IP        | 112.85.42.13           |
| Destination IP   | 172.16.17.17           |
| Destination Host | WebServer1002          |
| Attack Time      | Feb 26, 2022, 06:56 PM |
| HTTP Method      | GET                    |
| Response Code    | 302                    |
| Response Size    | 0 bytes                |
| Device Action    | Allowed                |

---

## Attacker Profile

* IP: 112.85.42.13
* Country: China
* ASN: AS4837 — CHINA UNICOM China169 Backbone
* Reputation:

  * VirusTotal: 1/92 engines flagged malicious
  * AbuseIPDB: Reported 45,325 times from 474 sources
* Attack Type: Manual XSS probing activity
* Traffic Origin: External/Public IP

---

## Attack Timeline — Reconnaissance to Exploitation Attempts

### Step 1 — Reconnaissance

06:34 PM
GET / → HTTP 200

Attacker verified target website availability.

06:35 PM
GET /about-us/ → HTTP 200

Attacker mapped accessible pages and performed
basic reconnaissance.

---

### Step 2 — Parameter Discovery

06:45 PM
GET /search/?q=test → HTTP 200

Attacker identified search parameter as potential
input injection point.

---

### Step 3 — XSS Exploitation Attempts

06:46 PM
/search/?q=prompt(8)

Basic JavaScript execution test.

Response:
HTTP 302 — 0 bytes

---

06:46 PM
/search/?q=<img src=q onerror=prompt(8)>

Attempted event-handler based XSS bypass using
HTML image onerror execution.

Response:
HTTP 302 — 0 bytes

---

06:50 PM
/search/?q=<script>for((i)in(self))eval(i)(1)</script>

Obfuscated JavaScript payload attempting dynamic
function execution.

Response:
HTTP 302 — 0 bytes

---

06:53 PM
/search/?q=<svg><script>alert(1)

SVG/script injection attempt commonly used to bypass
basic filtering mechanisms.

Response:
HTTP 302 — 0 bytes

---

06:56 PM
/search/?q=<script>javascript:alert(1)</script>

Classic reflected XSS payload attempting direct
JavaScript execution.

Response:
HTTP 302 — 0 bytes

---

## Attack Pattern Analysis

The attacker followed a clear manual testing sequence:

1. Verify website accessibility
2. Identify injectable parameters
3. Attempt simple payloads
4. Gradually escalate to more advanced bypass payloads

This behavior is consistent with a human attacker
manually testing reflected XSS vulnerabilities rather
than automated scanning tools.

---

## Attack Success Determination

### Method 1 — HTTP Response Code and Size Analysis

Legitimate requests:

| Request         | Status | Response       |
| --------------- | ------ | -------------- |
| /               | 200    | Normal content |
| /about-us/      | 200    | Normal content |
| /search/?q=test | 200    | Normal content |

Malicious XSS payloads:

| Payload Requests | Status | Response |
| ---------------- | ------ | -------- |
| All XSS attempts | 302    | 0 bytes  |

Analysis:

* HTTP 302 indicates redirection occurred
* Response size 0 indicates no content was returned
* No page containing injected payload was served

Conclusion:

The application likely redirected or rejected
malicious requests before rendering content,
preventing payload execution.

---

### Method 2 — Endpoint and Server Verification

Checked endpoint activity on WebServer1002:

* No JavaScript execution events detected
* No suspicious browser activity observed
* No alert() or prompt() execution evidence
* No session hijacking indicators found
* No follow-on compromise activity identified

Conclusion:

No evidence confirms successful XSS execution.

---

## MITRE ATT&CK Mapping

* T1190 — Exploit Public-Facing Application
* T1059.007 — JavaScript
* T1595 — Active Scanning
* T1592 — Gather Victim Host Information

---

## Final Assessment

This alert is classified as:

TRUE POSITIVE — UNSUCCESSFUL XSS ATTACK ATTEMPT

Evidence strongly indicates the attacker attempted
multiple reflected XSS payloads against the search
functionality; however:

* all malicious requests returned HTTP 302,
* response size remained 0 bytes,
* no payload reflection observed,
* no JavaScript execution evidence found,
* no endpoint compromise indicators detected.

The attack was likely mitigated by application
security controls, filtering, or input validation.

---

## Actions Recommended

1. Block source IP 112.85.42.13 at perimeter firewall
2. Add IOC to internal threat intelligence feeds
3. Review WAF and input validation rules
4. Verify search functionality properly sanitizes user input
5. Continue monitoring for repeated attacks from same ASN
6. Check whether same IP targeted additional web assets

---

## Key Learnings

* Sequential payload escalation often indicates
  manual attacker behavior
* HTTP 302 + 0 bytes is a strong indicator that
  malicious requests were redirected or rejected
* Successful reflected XSS commonly returns
  HTTP 200 with rendered payload content
* Endpoint validation is essential before declaring
  successful exploitation
* Response size comparison is one of the fastest
  methods for assessing web attack success

---

## One Thing to Remember

When investigating XSS alerts, do not assume
“Device Action: Allowed” means compromise occurred.

A request may reach the server but still fail due to:

* application sanitization,
* redirect logic,
* WAF filtering,
* CSP enforcement,
* secure output encoding.

Always verify:

1. Was the payload reflected?
2. Did JavaScript execute?
3. Was there follow-on activity?
