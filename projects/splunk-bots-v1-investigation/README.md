# Splunk BOTS v1 Investigation

A structured investigation of the BOTS v1 training dataset using Splunk. This project records the query approach, observations, and defensive significance of selected investigation questions.

> **Credential handling:** Historical password values from the training data are intentionally redacted in this public repository. SPL examples show extraction and analysis methods, not passwords to reuse.

## Start Here

| Document | Purpose |
|---|---|
| [Dataset overview](docs/dataset-overview.md) | What the dataset contains and how it is organized |
| [Investigation methodology](docs/investigation-methodology.md) | Repeatable investigation approach |
| [Lab setup](docs/lab-setup.md) | Environment and data setup |
| [Web attack analysis](docs/web-attack-analysis.md) | Context for web activity in the dataset |
| [Attack timeline](timeline/attack-timeline.md) | Timeline built from investigation findings |

## Investigation Index

### Web reconnaissance and application discovery

| Investigation | Notes |
|---|---|
| [01 — Web application vulnerability scanning](investigations/01-web-application-vulnerability-scanning.md) | Identify vulnerability-scanning activity |
| [Q02 — Web vulnerability scanner](investigations/Q02-Web-Vulnerability-Scanner.md) | Identify scanner from HTTP evidence |
| [Q03 — CMS identification](investigations/Q03-Content-Management-System-Identification.md) | Identify the Joomla application |
| Q04 — Website defacement file identification | **Needs completion:** the current investigation file is empty. Do not treat it as completed evidence. |

### Infrastructure, malware, and enrichment

| Investigation | Notes |
|---|---|
| [Q05 — Dynamic DNS domain](investigations/INV-005-Dynamic-DNS-Domain-Identification.md) | DNS-related investigation |
| [Q06 — Pre-staged infrastructure](investigations/INV-006-Po1s0n1vy-Pre-Staged-Infrastructure.md) | Identify related attacker infrastructure |
| [Q07 — OSINT attribution](investigations/INV-007-OSINT-Attribution.md) | Historical OSINT enrichment |
| [Q08 — Brute-force activity](investigations/INV-008-Brute-Force-Attack.md) | Investigate authentication attempts |
| [Q09 — Malicious executable upload](investigations/INV-009-Malicious-Executable-Upload.md) | Analyze an executable upload |
| [Q10 — Uploaded executable hash](investigations/INV-010-MD5-Hash-of-Uploaded-Executable.md) | Identify an artifact hash |
| [Q11 — Spear-phishing malware](investigations/INV-011-Spear-Phishing-Malware-SHA256.md) | Investigate a malware artifact |
| [Q12 — Special hexadecimal code](investigations/INV-012-Special-Hex-Code.md) | Analyze a suspicious encoded value |
| [Q13 — Staged-domain WHOIS](investigations/INV-013-Staged-Domain-WHOIS.md) | Enrich a staged domain |

### Credential-related investigation sequence

The following reports preserve investigation reasoning while redacting recovered password values.

| Investigation | Notes |
|---|---|
| [Q14 — First brute-force password](investigations/INV-014-First-Brute-Force-Password.md) | Chronological extraction from HTTP form data |
| [Q15 — Six-character password pattern](investigations/INV-015-Coldplay-Password.md) | Candidate filtering and contextual enrichment |
| [Q16 — CMS administrator credential](investigations/INV-016-Correct-CMS-Admin-Password.md) | Correlate authentication activity |
| [Q17 — Average password length](investigations/INV-017-Average-Password-Length.md) | Summarize credential characteristics |
| [Q18 — Password discovery to login](investigations/INV-018-Password-Discovery-to-Compromised-Login.md) | Correlate event timestamps |
| [Q19 — Unique password count](investigations/INV-019-Unique-Brute-Force-Passwords.md) | Count distinct attempted values |

### Endpoint, network, and ransomware activity

| Investigation | Notes |
|---|---|
| [Q20 — Workstation IP](investigations/INV-020-we8105desk-IP.md) | Identify a workstation |
| [Q21 — Cerber Suricata signature](investigations/INV-021-Cerber-Suricata-Signature.md) | IDS signature investigation |
| [Q22 — Cerber redirect FQDN](investigations/INV-022-Cerber-Redirect-FQDN.md) | DNS / redirect evidence |
| [Q23 — First suspicious domain](investigations/INV-023-First-Suspicious-Domain.md) | Identify suspicious-domain activity |
| [Q24 — VBScript field length](investigations/INV-024-VBScript-Field-Length.md) | Investigate VBScript-related telemetry |
| [Q25 — USB key insertion](investigations/INV-025-USB-Key-Bob-Smith.md) | Investigate removable-media activity |
| [Q26 — File server IP](investigations/INV-026-File-Server-IP.md) | Identify a file-server connection |
| [Q27 — Encrypted PDFs](investigations/INV-027-Distinct-PDFs-Encrypted.md) | Summarize file-encryption evidence |
| [Q28 — Parent process ID](investigations/INV-028-121214-tmp-ParentProcessId.md) | Examine process lineage |
| [Q29 — TXT file encryption](investigations/INV-029-Bob-Smith-TXT-Encryption.md) | Investigate ransomware impact |
| [Q30 — Cerber cryptor file](investigations/INV-030-cerber-cryptor-download.md) | Investigate a cryptor artifact |
| [Q31 — Ransomware obfuscation](investigations/INV-031-ransomware-steganography.md) | Document an obfuscation finding |

## Supporting Material

- [SPL query library](spl/) — queries and query notes
- [Detection notes](detections/) — detection concepts and query logic
- [Incident reports](reports/) — concise summaries and recommendations
- [Playbooks](playbooks/) — repeatable investigation steps
- [Artifacts](artifacts/) — supporting artifact records, with secrets redacted
- [Timeline records](timeline/) — event-sequence notes
- [Screenshots](screenshots/) — available evidence images
- [Changelog](CHANGELOG.md) — repository documentation history

## Investigation Standards

- Separate observations, interpretation, and conclusions.
- Record the relevant dataset index, sourcetype, fields, and SPL.
- Treat extracted credentials and other sensitive values as sensitive—even when they come from a public training dataset.
- Keep screenshots and query output focused on the evidence needed to support a finding.
- Use this material for learning and authorized analysis only.
