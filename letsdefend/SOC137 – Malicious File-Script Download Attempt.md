# SOC137 – Malicious File/Script Download Attempt

**Platform:** LetsDefend  
**Category:** Malware Investigation  
**Status:** ✅ Completed  
**Event ID:** 76  
**Severity:** Medium  
**Classification:** True Positive

---

# Scenario

A security alert was generated after a potentially malicious Microsoft Word document was detected during a file/script download attempt on the endpoint **NicolasPRD**.

The investigation focused on determining whether the document was malicious, analyzing its behavior, checking for network communication, investigating the endpoint, and determining the final incident classification.

---

# Alert Information

| Field | Value |
|---|---|
| Event ID | `76` |
| Event Time | `2021-03-14T19:15:52+03:00` |
| Rule | `SOC137 - Malicious File/Script Download Attempt` |
| Role | Security Analyst |
| Alert Type | Malware |
| Difficulty | Easy |
| MITRE ATT&CK | `T1204` |
| File Name | `INVOICE PACKAGE LINK TO DOWNLOAD.docm` |
| File Hash | `f2d0c66b801244c059f636d08a474079` |
| File Size | `16.66 KB` |
| Device Action | `Blocked` |
| Source Address | `172.16.17.37` |
| Source Hostname | `NicolasPRD` |
| Final Result | `True Positive` |

---

# Investigation Process

## Step 1 – Initial Alert Review

The alert identified a suspicious file/script download attempt on the endpoint.

### Endpoint Information

**Hostname:**

```text
NicolasPRD
```

**Source IP:**

```text
172.16.17.37
```

### File Information

**File Name:**

```text
INVOICE PACKAGE LINK TO DOWNLOAD.docm
```

**MD5 Hash:**

```text
f2d0c66b801244c059f636d08a474079
```

**File Size:**

```text
16.66 KB
```

### Alert Details

The alert was classified as:

```text
Alert Type: Malware
Severity: Medium
```

The security control reported:

```text
Device Action: Blocked
```

The `.docm` extension indicates that the document is a Microsoft Word macro-enabled document.

### Observation

At this stage, the alert indicated a potentially malicious document download attempt, but successful execution or compromise had not yet been established.

---

## Step 2 – Define Threat Indicator

The available threat indicator options were reviewed.

The investigation identified suspicious outbound network activity during endpoint investigation.

The relevant threat indicator selected was:

```text
Unknown or unexpected outgoing internet traffic
```

However, the historical network activity was kept separate from the current SOC137 alert because of the timestamp difference.

---

## Step 3 – Check if Malware Was Quarantined/Cleaned

The alert showed:

```text
Device Action: Blocked
```

The security control prevented the malicious download attempt.

The Endpoint Security information for `NicolasPRD` showed that endpoint containment was not enabled.

The playbook classification for this step was:

```text
Quarantined
```

### Important Distinction

Endpoint containment and file quarantine are different concepts.

```text
File/Download
      ↓
Blocked
      ↓
Malicious file prevented from being obtained
```

The playbook accepted the containment status as:

```text
Quarantined
```

---

## Step 4 – Analyze Malware

The file was analyzed using VirusTotal.

VirusTotal reported:

```text
40/65 security vendors flagged this file as malicious
```

This provided strong evidence that the document was malicious.

### Result

```text
Malicious
```

---

## Step 5 – Analyze Macro Behavior

VirusTotal identified a macro named:

```text
AutoOpen
```

The macro uses the VBA `Shell` function to execute a PowerShell command.

The document's behavior can be represented as:

```text
Word Document
      ↓
AutoOpen Macro
      ↓
VBA Shell Function
      ↓
PowerShell
      ↓
Remote File Download
      ↓
Potential Payload Execution
```

The PowerShell command was heavily obfuscated and contained encoded/compressed content.

### VirusTotal Behavioral Indicators

The analysis showed tags including:

```text
auto-open
macros
powershell
run-file
url-pattern
calls-wmi
```

These indicators supported the classification of the document as malicious.

---

## Step 6 – Investigate Network Indicators

VirusTotal showed multiple contacted domains and IP addresses during its analysis.

The VirusTotal network indicators were compared against the available Log Management data.

The important point was that VirusTotal network activity represents behavior observed inside the VirusTotal analysis environment.

Therefore, the presence of an IP address in VirusTotal does not automatically prove that the affected endpoint contacted the same IP.

### Investigation Result

No matching outbound communication associated with the **March 14 SOC137 alert** was identified in the available Log Management data.

---

## Step 7 – Historical Network Activity

During the endpoint investigation, historical network activity was found from:

```text
172.16.17.37
```

One network event showed:

```text
Source Address: 172.16.17.37
Source Port: 48463
Destination Address: 49.51.12.195
Destination Port: 443
Process: powershell.exe
Request URL: iliuryeqa.info
Request Method: GET
Device Action: Allowed
Parent Process: wmiprvse.exe
Time: 2021-03-07 19:20:57
```

The observed domain was:

```text
iliuryeqa.info
```

VirusTotal also showed malicious/phishing detections for this domain.

### Observation

This activity was suspicious, but it occurred on:

```text
2021-03-07
```

The SOC137 alert occurred on:

```text
2021-03-14 19:15:52
```

Therefore, the historical network activity was not directly attributed to the SOC137 alert.

---

## Step 8 – Investigate Suspicious PowerShell Activity

The endpoint Terminal History contained a heavily obfuscated PowerShell command.

The command contained techniques including:

- Execution Policy Bypass
- Hidden Window
- Non-Interactive Execution
- String Obfuscation
- Base64-Encoded Content
- Compression/Decompression
- Dynamic Command Construction

The command began with behavior similar to:

```text
powershell -ExecutionPolicy Bypass -nop -Window Hidden -NonInteractive
```

### Observation

The PowerShell activity was suspicious.

However, the activity occurred on:

```text
2021-03-07
```

while the SOC137 alert occurred on:

```text
2021-03-14
```

Because of this timestamp difference, the activity was treated as **historical suspicious activity** rather than confirmed activity related to the SOC137 alert.

---

## Step 9 – Investigate DLL Execution

The endpoint Terminal History also showed:

```text
C:\Windows\System32\rundll32.exe /s C:\Users\Nicolas\AppData\Local\Temp\oo2ofzo5.dll DllRegisterServer
```

This shows `rundll32.exe` being used to invoke:

```text
DllRegisterServer
```

from:

```text
C:\Users\Nicolas\AppData\Local\Temp\oo2ofzo5.dll
```

### Why This Is Suspicious

The activity involved:

- `rundll32.exe`
- A DLL located in the user's temporary directory
- `DllRegisterServer`
- Suspicious PowerShell activity
- Historical outbound network communication

However, this activity occurred on March 7 and therefore was not directly correlated with the March 14 SOC137 alert.

---

## Step 10 – Timeline Correlation

Timeline correlation was used to distinguish the current alert from historical endpoint activity.

| Date | Activity |
|---|---|
| `2021-03-07` | Suspicious PowerShell command |
| `2021-03-07` | `rundll32.exe` executed `oo2ofzo5.dll` |
| `2021-03-07` | `powershell.exe` contacted `iliuryeqa.info` |
| `2021-03-07` | Connection to `49.51.12.195:443` |
| `2021-03-14 19:15:52` | SOC137 malicious DOCM download attempt |
| `2021-03-14` | Download was blocked |

### Key Finding

The suspicious PowerShell, DLL execution, and network activity occurred approximately seven days before the SOC137 alert.

Therefore:

```text
Same Host + Suspicious Activity ≠ Automatically Same Incident
```

The March 7 activity was documented separately.

---

## Step 11 – Add Investigation Artifacts

The primary artifact added to the investigation was the MD5 hash of the malicious document.

### MD5 Hash

```text
f2d0c66b801244c059f636d08a474079
```

### Comment

```text
MD5 hash of the malicious DOCM file identified in SOC137.
```

---

# Indicators of Compromise

## Malicious File

```text
INVOICE PACKAGE LINK TO DOWNLOAD.docm
```

## MD5 Hash

```text
f2d0c66b801244c059f636d08a474079
```

## Historical Domain

```text
iliuryeqa.info
```

## Historical IP

```text
49.51.12.195
```

## Historical DLL

```text
oo2ofzo5.dll
```

> The domain, IP, and DLL were identified during historical endpoint investigation and were not directly attributed to the March 14 SOC137 alert.

---

# MITRE ATT&CK Mapping

The alert mapped the activity to:

```text
T1204 – User Execution
```

The malicious document also demonstrated behavior involving:

- Malicious Office macros
- PowerShell execution
- Obfuscated commands
- Remote file download

---

# Threat Intelligence Findings

VirusTotal analysis showed:

```text
40/65 security vendors flagged this file as malicious
```

### Important Behavioral Findings

- `AutoOpen` macro
- VBA `Shell` execution
- PowerShell execution
- Obfuscated PowerShell
- Remote file download
- Potential payload execution

### VirusTotal Tags

```text
auto-open
macros
powershell
run-file
url-pattern
calls-wmi
```

---

# Log Management Findings

The investigation checked Log Management for network indicators associated with the malicious document.

No matching outbound communication associated with the **March 14 SOC137 alert** was identified.

Historical network activity was found from:

```text
172.16.17.37
        ↓
49.51.12.195:443
        ↓
powershell.exe
        ↓
iliuryeqa.info
```

However, this activity occurred on March 7 and was therefore not directly correlated with the March 14 alert.

---

# Endpoint Investigation Summary

| Evidence | Finding |
|---|---|
| Endpoint | `NicolasPRD` |
| IP Address | `172.16.17.37` |
| Malicious DOCM | Detected |
| Download Action | Blocked |
| VirusTotal Detection | 40/65 |
| AutoOpen Macro | Present |
| PowerShell Behavior | Present |
| March 14 C2 Activity | Not Identified |
| Historical PowerShell | Found on March 7 |
| Historical DLL Execution | Found on March 7 |
| Historical Network Activity | Found on March 7 |
| Endpoint Containment | Not Enabled |
| Final Classification | True Positive |

---

# Analyst Notes

SOC137 triggered on NicolasPRD (`172.16.17.37`) for a download attempt involving `INVOICE PACKAGE LINK TO DOWNLOAD.docm`.

The file was blocked by the security control.

VirusTotal analysis identified the document as malicious, with 40/65 security vendors detecting it. The document contains an `AutoOpen` macro that uses the VBA `Shell` function to execute an obfuscated PowerShell command capable of downloading another file.

The malicious file's MD5 is:

```text
f2d0c66b801244c059f636d08a474079
```

No matching outbound connection for the March 14 alert was identified in the available Log Management data.

Historical suspicious activity from March 7 was observed on the endpoint, including PowerShell execution, `rundll32.exe` DLL execution, and outbound communication to `iliuryeqa.info` / `49.51.12.195`.

This historical activity was not directly correlated with the March 14 alert because of the timestamp difference.

---

# Artifacts Collected

### File

```text
INVOICE PACKAGE LINK TO DOWNLOAD.docm
```

### MD5

```text
f2d0c66b801244c059f636d08a474079
```

### Historical Domain

```text
iliuryeqa.info
```

### Historical IP

```text
49.51.12.195
```

### Historical DLL

```text
oo2ofzo5.dll
```

---

# Lessons Learned

## 1. `.docm` Files Require Investigation

Macro-enabled Microsoft Office documents can contain VBA code capable of executing commands when the document is opened.

---

## 2. VirusTotal Is Useful for Malware Investigation

VirusTotal can provide:

- Detection rates
- Malware classifications
- Macro information
- Behavioral indicators
- Network indicators
- File relationships

---

## 3. Obfuscated PowerShell Is an Important Investigation Signal

Encoded and heavily obfuscated PowerShell commands can hide their actual behavior and should be investigated carefully.

---

## 4. Timeline Correlation Is Critical

Suspicious activity on the same endpoint does not automatically belong to the current alert.

An analyst should correlate:

```text
Time
+
Host
+
Process
+
IOC
+
Network Activity
```

before establishing a relationship between events.

---

## 5. Blocked Activity Must Be Interpreted Carefully

The SOC137 alert showed that the malicious download attempt was blocked.

The investigation therefore focused on determining whether there was evidence of successful execution or network communication associated with the March 14 alert.

---

# Skills Practiced

- SOC Alert Triage
- Malware Investigation
- VirusTotal Analysis
- Malicious Office Document Analysis
- Macro Analysis
- PowerShell Analysis
- IOC Extraction
- Network Investigation
- Endpoint Investigation
- Timeline Correlation
- Threat Indicator Identification
- Incident Classification
- Analyst Documentation

---

# SOC Workflow Completed

```text
Alert
  ↓
Initial Triage
  ↓
Threat Indicator Identification
  ↓
Containment Verification
  ↓
Malware Analysis
  ↓
VirusTotal Investigation
  ↓
IOC Extraction
  ↓
Network Investigation
  ↓
Endpoint Investigation
  ↓
Timeline Correlation
  ↓
Analyst Assessment
  ↓
True Positive
  ↓
Case Closed
```

---

# Final Verdict

## ✅ True Positive

The SOC137 alert was a genuine detection of a malicious file/download attempt.

The file:

```text
INVOICE PACKAGE LINK TO DOWNLOAD.docm
```

was confirmed malicious through VirusTotal analysis.

The document contained an `AutoOpen` macro capable of launching an obfuscated PowerShell command and downloading another payload.

The security control **blocked the download attempt**.

No matching outbound communication associated with the March 14 alert was identified.

Historical suspicious activity from March 7 was documented separately and was not directly attributed to the SOC137 alert.

---

# LetsDefend Playbook Result

```text
Result: True Positive
Playbook Score: 15
Success Rate: 100%
```

### Playbook Answers

| Playbook Question | Answer |
|---|---|
| Check if someone requested the C2 | **Not Accessed** |
| Analyze Malware | **Malicious** |
| Check if malware is quarantined/cleaned | **Quarantined** |

---

# Status

✅ Investigation Completed  
✅ True Positive  
✅ Playbook Score: 15/15  
✅ 100% Success Rate