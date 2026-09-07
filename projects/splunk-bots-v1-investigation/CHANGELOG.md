## Investigation 02

Added:

- Web Vulnerability Scanner investigation
- Acunetix detection rule
- Scanner response playbook
- Timeline
- Incident report
- SPL queries
- Evidence documentation

## Investigation 03

Added:
- Joomla CMS identification investigation
- Joomla reconnaissance detection
- CMS investigation playbook
- Timeline
- Incident report
- Artifacts
- SPL queries


## Investigation 04

Added:

- Website defacement investigation
- Malicious download detection
- Defacement response playbook
- Incident report
- Timeline
- Artifacts
- SPL queries

## Investigation 05

Added:

- Dynamic DNS domain investigation
- DNS-based detection rule
- Dynamic DNS response playbook
- Incident report
- Timeline
- Artifacts
- SPL queries


## Investigation 06

Added:

- Pre-staged infrastructure investigation
- DNS detection rule
- Investigation playbook
- Incident report
- Timeline
- Artifacts
- SPL queries


## Investigation 07

Added:

- Historical OSINT investigation
- Threat intelligence enrichment detection
- OSINT playbook
- Incident report
- Timeline
- Artifacts
- Investigation limitations

## Investigation 09

Added:

- Executable upload investigation
- File upload detection rule
- Investigation playbook
- Incident report
- Timeline
- Artifacts
- SPL queries


## Investigation 10

Added:

- Malware hash investigation
- Sysmon execution analysis
- Hash extraction workflow
- Detection rule
- Investigation playbook
- Incident report
- Timeline
- Artifacts
- SPL queries



## Investigation 12

Added:

- Historical OSINT investigation
- Customized malware metadata
- Historical hexadecimal artifact
- ASCII decoding
- Evidence limitation regarding current VirusTotal Community data
- Threat-intelligence artifact documentation

## Investigation 13

Added:

- Staged-domain infrastructure investigation
- Historical WHOIS analysis
- WHOIS artifact extraction
- Hexadecimal fragment concatenation
- ASCII validation
- Historical evidence limitation


## Q14 - Joomla Brute Force Investigation

Added:

- First brute-force password investigation
- HTTP authentication analysis
- Joomla administrator login analysis
- Credential extraction using SPL
- Brute-force detection logic
- Investigation playbook
- Incident report
- Timeline
- Investigation artifacts



## Investigation 15

Added:

- Coldplay password investigation
- Six-character password extraction
- Password/song correlation
- Brute-force password analysis
- Detection rule
- Investigation playbook
- Incident report
- Timeline
- Investigation artifacts


## Q16 — Correct CMS Admin Password

Added investigation and detection documentation for the correct Joomla
administrator password used against imreallynotbatman.com.

Answer: batman

Related source IP:
40.80.148.42

Brute-force source:
23.22.63.114



## Q17 — Average Password Length

Added investigation and detection documentation for the average password
length used during the Joomla brute-force attack.

Raw average:
6.174334140435835

Rounded answer:
6

## Q18 — Password Discovery to Compromised Login

Added investigation and detection documentation for the time interval
between discovery of the correct Joomla password and subsequent
compromised login.

Correct password:
batman

Raw duration:
92.169084 seconds

Rounded answer:
92.17 seconds

## Q19 — Unique Brute-Force Passwords

Added investigation and detection documentation for the number of unique
passwords attempted during the Joomla brute-force attack.

Unique password count:

412

## Q20 — we8105desk IP Address

Added investigation and detection documentation for identifying the most
likely IP address of the workstation `we8105desk` on 24 AUG 2016.

Answer:

192.168.250.100


## Q21 — Cerber Suricata Signature

Added investigation and detection documentation for the Suricata signature
that generated the fewest alerts associated with Cerber ransomware.

Signature ID:

2816763

Signature:

ETPRO TROJAN Ransomware/Cerber Checkin 2

## Q22 — Cerber Redirect FQDN

Added investigation and detection documentation for the Cerber ransomware
DNS redirect identified during the encryption phase.

FQDN:

cerberhhyed5frqa.xmfir0.win

## Q23 — First Suspicious Domain

Added investigation and detection documentation for the first suspicious
domain visited by we8105desk during the Cerber ransomware incident.

Answer:

solidaritedeproximite.org


## Q24 — VBScript Field Length

Added investigation and detection documentation for the malicious
VBScript execution associated with the initial Cerber infection.

CommandLine field length:

4490

## Q25 — USB Key Inserted by Bob Smith

Added investigation and detection documentation for the USB device inserted
by Bob Smith into workstation `we8105desk`.

USB key name:

MALWARE



## Q26 — File Server IP

Added investigation and detection documentation for the remote file server
accessed by Bob Smith's workstation during the Cerber ransomware outbreak.

File server:

we9041srv.waynecorpinc.local

IP:

192.168.250.20



## Q27 — Distinct PDFs Encrypted

Added investigation and detection documentation for PDF files encrypted
by Cerber ransomware on the remote file server.

Distinct PDFs observed:

258

Distinct PDFs encrypted:

257



## Q28 — 121214.tmp ParentProcessId

Added investigation and detection documentation for the initial execution
of 121214.tmp during the Cerber ransomware infection.

ParentProcessId:

2568


## Q29 — Bob Smith TXT File Encryption

Added investigation and detection documentation for Cerber's encryption of
.txt files in Bob Smith's Windows profile.

Distinct encrypted .txt files:

406


## Q30 — Cerber Cryptor File

- Added investigation for the Cerber cryptor download.
- Identified `mhtr.jpg` as the downloaded payload.
- Correlated the download with `solidaritedeproximite.org`.
- Added related infrastructure `92.222.104.182`.
- Added detection, response playbook, timeline, artifact, report, and SPL query.


## Q31 — Ransomware Obfuscation Technique

- Added Q31 investigation.
- Identified steganography as the likely obfuscation technique.
- Linked the finding to `mhtr.jpg`.
- Added detection logic for suspicious image-based payloads.
- Added investigation and response playbook.
- Added artifact and incident report documentation.