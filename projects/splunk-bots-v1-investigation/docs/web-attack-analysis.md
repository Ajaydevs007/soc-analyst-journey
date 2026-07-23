# Investigation: Initial Web Attack Reconnaissance

**Platform:** Splunk BOTS v1  
**Investigation Type:** Web Attack Analysis  
**Status:** Completed

---

# Objective

Investigate suspicious HTTP requests targeting the Joomla web application to determine:

- Source of the attack
- Target web server
- Attack timeline
- Exploitation attempts
- Initial assessment of compromise

---

# Investigation Summary

The investigation identified a single external attacker performing reconnaissance and exploitation attempts against a Joomla web application hosted on `imreallynotbatman.com`.

The attacker primarily targeted Local File Inclusion (LFI), Directory Traversal, and SQL Injection vulnerabilities.

No evidence of successful file disclosure was observed during the initial LFI investigation.

---

# Evidence Collected

## Attacker Information

| Artifact | Value |
|----------|------|
| Source IP | 40.80.148.42 |
| Target IP | 192.168.250.70 |
| Website | imreallynotbatman.com |
| Application | Joomla |

---

## Timeline

Attack Start

2016-08-11 03:07:28

Attack End

2016-08-11 03:47:46

---

# Indicators Observed

## Local File Inclusion

Example payloads:

```
/windows/win.ini
/windows/win.ini%00.jpg
```

Observed:

- 101 direct requests targeting win.ini
- Multiple traversal attempts
- NULL byte injection attempts

---

## Directory Traversal

Observed payload:

```
../../../../../../windows/win.ini
```

Parameter abused:

```
catid=
```

---

## SQL Injection

Observed SQL keywords inside URI parameters including:

- UNION
- SELECT
- INFORMATION_SCHEMA

Multiple HTTP status codes were returned indicating repeated SQL injection attempts.

---

## Joomla Administrator Enumeration

Administrator endpoint accessed:

```
/joomla/administrator/index.php
```

HTTP Methods:

- GET
- POST

Observed POST requests suggest attempts to interact with the Joomla administrator interface.

---

## HTTP Response Analysis

HTTP Status Distribution

| Status | Count |
|--------|------:|
| 200 | 72 |
| 403 | 1 |
| 500 | 28 |

Important Finding:

HTTP 200 responses returned normal Joomla/OpenSearch XML pages rather than the contents of `win.ini`.

Therefore:

- HTTP requests succeeded.
- No evidence that the Local File Inclusion attack successfully exposed `win.ini`.

---

# Analyst Notes

The attacker spent approximately 40 minutes interacting with the Joomla application.

Observed attack techniques include:

- Web Reconnaissance
- Directory Traversal
- Local File Inclusion
- SQL Injection
- Joomla Administrator Enumeration

No evidence of standard browser-based file uploads (`multipart/form-data`) was observed.

---

# MITRE ATT&CK

| Technique | ID |
|-----------|----|
| Active Scanning | T1595 |
| Exploit Public-Facing Application | T1190 |
| Directory Traversal / LFI | T1190 (Observed Exploitation Technique) |
| SQL Injection | T1190 |

---

# Conclusion

The web server was subjected to multiple exploitation attempts targeting Joomla.

Evidence confirms repeated probing for LFI, Directory Traversal, SQL Injection, and administrator access.

At this stage of the investigation, there is insufficient evidence to conclude that the attacker successfully obtained sensitive files through the observed LFI attempts.