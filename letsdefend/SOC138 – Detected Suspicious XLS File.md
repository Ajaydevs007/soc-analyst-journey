#  SOC138 – Detected Suspicious XLS File

## 🛡️ Lab Information

| Field | Details |
|---|---|
| **Platform** | LetsDefend |
| **Lab ID** | SOC138 |
| **Alert Name** | Detected Suspicious Xls File |
| **Event ID** | 77 |
| **Alert Type** | Malware |
| **Severity** | Medium |
| **Difficulty** | Easy |
| **MITRE ATT&CK** | T1112 – Modify Registry |
| **Source Host** | Sofia |
| **Source IP** | `172.16.17.56` |
| **File Name** | `ORDER SHEET & SPEC.xlsm` |
| **File Size** | 2.66 MB |
| **Device Action** | Allowed |
| **Final Verdict** | True Positive |
| **Playbook Score** | 15/15 |
| **Success Rate** | 100% |
| **Status** | Completed |

---

# 📌 Scenario

A SOC alert was generated after a suspicious Excel macro-enabled file was detected on the endpoint **Sofia**.

The suspicious file was:

```text
ORDER SHEET & SPEC.xlsm
```

The investigation focused on determining whether the file was malicious, analyzing its macro behavior, investigating network activity, correlating indicators, containing the affected endpoint, and determining the final verdict.

---

# 🚨 Alert Information

The original alert contained the following information:

```text
Event ID       : 77
Event Time     : 2021-03-13 20:20:58 +03:00
Rule           : SOC138 - Detected Suspicious Xls File
Role           : Security Analyst
Alert Type     : Malware
Difficulty     : Easy
MITRE ATT&CK  : T1112
File Hash      : 7ccf88c0bbe3b29bf19d877c4596a8d4
File Name      : ORDER SHEET & SPEC.xlsm
File Size      : 2.66 MB
Device Action  : Allowed
Source Address : 172.16.17.56
Source Hostname: Sofia
Severity       : Medium
```

---

# 🔎 Investigation Process

## Step 1 – Initial Alert Triage

The alert identified a suspicious Excel file:

```text
ORDER SHEET & SPEC.xlsm
```

The `.xlsm` extension indicates that the workbook is **macro-enabled**.

Macros are not inherently malicious, but they can be abused to execute commands and perform malicious actions when a document is opened.

The device action was:

```text
Allowed
```

Therefore, the file was not blocked by the security control that generated the alert.

### Initial Hypothesis

> A suspicious macro-enabled Excel document was detected on Sofia's endpoint and was allowed. The file needed to be analyzed to determine whether it was malicious and whether further malicious activity occurred.

---

## Step 2 – VirusTotal Analysis

The file hash from the alert was searched in VirusTotal:

```text
7ccf88c0bbe3b29bf19d877c4596a8d4
```

VirusTotal confirmed the same MD5 hash.

### File Hashes

```text
MD5:
7ccf88c0bbe3b29bf19d877c4596a8d4

SHA-1:
23f0506d857d38c3cd5354b80afc725b5f034744

SHA-256:
7bcd31bd41686c32663c7cabf42b18c50399e3b3b4533fc2ff002d9f2e058813
```

VirusTotal reported:

```text
44/62 security vendors flagged this file as malicious.
```

This provided strong evidence that the Excel file was malicious.

---

## Step 3 – Malware / Macro Analysis

VirusTotal Code Insights identified several suspicious behaviors in the Excel macro.

The document contained an:

```text
Auto_Open
```

subroutine.

`Auto_Open` can execute when the Excel document is opened.

The analysis also identified:

- Obfuscated code
- Obfuscated variable and function names
- Base64-encoded strings
- `ShellExecute`
- `CreateObject`
- File-system manipulation
- Downloading files from remote URLs
- Executing downloaded files
- Self-replication behavior
- Windows API usage

### Observed Behavior Chain

```text
ORDER SHEET & SPEC.xlsm
            ↓
        Auto_Open
            ↓
       VBA Macro
            ↓
       Obfuscation
            ↓
      Base64 Decoding
            ↓
       ShellExecute
            ↓
      Download File
            ↓
     Execute File
```

The combination of these behaviors strongly indicated malicious intent.

### Important Investigation Note

The malware analysis demonstrated what the macro was **capable of doing**.

It did not independently prove that every behavior was successfully executed on Sofia's endpoint.

Endpoint and network evidence were therefore investigated separately.

---

## Step 4 – Endpoint Investigation

The affected endpoint was:

```text
Hostname : Sofia
IP Address: 172.16.17.56
OS       : Windows 10
Domain   : LetsDefend
Primary User: Sofia2020
```

The Endpoint Security page initially showed:

```text
Containment: OFF
```

The endpoint also contained Terminal History records.

One of the visible commands began with:

```text
PowerShell -ENCOD
```

The command contained a large encoded and obfuscated PowerShell payload.

### Suspicious PowerShell Characteristics

The command contained characteristics associated with suspicious PowerShell activity, including:

- Encoded command execution
- Obfuscated strings
- Large encoded payload
- Dynamic command construction

However, the presence of an encoded PowerShell command alone does not establish exactly what actions occurred without further evidence.

---

## Step 5 – Terminal History Investigation

The Endpoint Security interface displayed:

```text
Terminal History (3)
```

Although three terminal-history records were indicated, only one was visible on the current page.

There was no direct evidence showing that an attacker had cleared the terminal history.

Therefore, the investigation did **not** conclude that the attacker had deleted or cleared terminal history.

### Important SOC Lesson

The endpoint's:

```text
Last Login
```

timestamp should not be confused with the execution time of a terminal command.

For correlation, the actual event timestamp associated with the command should be used.

---

## Step 6 – Network Investigation

A Log Management search was performed using the affected endpoint:

```text
Source Address = 172.16.17.56
```

The search returned:

```text
3 events found
```

The observed outbound connections were:

```text
172.16.17.56 → 177.53.143.89:443
172.16.17.56 → 177.53.143.89:443
172.16.17.56 → 35.189.10.17:443
```

---

## Step 7 – IOC Correlation

One of the destination IP addresses was:

```text
177.53.143.89
```

This IP address was also observed during the VirusTotal analysis of the malicious Excel file.

The correlation was:

```text
Malicious XLSM
      ↓
VirusTotal analysis
      ↓
177.53.143.89
      ↓
Sofia endpoint
172.16.17.56
      ↓
177.53.143.89:443
```

This provided a significant IOC correlation between the malware analysis and endpoint network activity.

However, the network activity occurred later than the original alert timestamp.

---

## Step 8 – Timeline Correlation

The original SOC138 alert occurred at:

```text
2021-03-13 20:20:58
```

The observed connection to:

```text
177.53.143.89:443
```

occurred around:

```text
2021-03-13 22:50
```

Therefore, the network connection was **not simultaneous with the alert**.

The investigation therefore avoided claiming that the Excel macro definitively initiated the connection.

### Timeline

```text
20:20:58
    │
    │ SOC138 alert generated
    │
    ↓
Suspicious XLSM detected
    │
    │
    │ Later on the same date
    ↓
22:50 approximately
    │
    ↓
172.16.17.56
      →
177.53.143.89:443
```

### SOC Lesson

> Same IOC + same endpoint does not automatically prove causation.

Timeline and contextual correlation are necessary before attributing activity to a specific alert.

---

## Step 9 – C2 Investigation

The LetsDefend playbook instructed the analyst to determine whether a malicious-file network indicator had been accessed.

The endpoint had contacted:

```text
177.53.143.89:443
```

This IP was also observed during the VirusTotal analysis.

Therefore, the playbook answer was:

```text
C2 Access: Accessed
```

This was the classification expected by the LetsDefend lab based on the available network evidence.

---

## Step 10 – Endpoint Containment

Because the file was confirmed malicious and suspicious network activity was identified, the affected endpoint was contained.

### Endpoint

```text
Hostname : Sofia
IP Address: 172.16.17.56
```

Initial state:

```text
Containment: OFF
```

After containment:

```text
Host Contained
```

The endpoint was successfully isolated.

---

## Step 11 – Artifact Collection

The MD5 hash of the malicious XLSM file was added as an investigation artifact:

```text
7ccf88c0bbe3b29bf19d877c4596a8d4
```

### Artifact Details

```text
Type:
MD5 Hash

Value:
7ccf88c0bbe3b29bf19d877c4596a8d4

Comment:
MD5 hash of the malicious XLSM file identified in SOC138.
```

Hashes are useful for future detection and threat-hunting activities because filenames can be changed easily.

---

# 🧪 Indicators of Compromise

## Malicious File

```text
File Name:
ORDER SHEET & SPEC.xlsm

MD5:
7ccf88c0bbe3b29bf19d877c4596a8d4

SHA-1:
23f0506d857d38c3cd5354b80afc725b5f034744

SHA-256:
7bcd31bd41686c32663c7cabf42b18c50399e3b3b4533fc2ff002d9f2e058813

File Size:
2.66 MB
```

## Network Indicators

```text
177.53.143.89:443
35.189.10.17:443
```

## Affected Endpoint

```text
Hostname:
Sofia

IP:
172.16.17.56
```

---

# 🎯 MITRE ATT&CK

The SOC138 alert was mapped to:

```text
T1112 – Modify Registry
```

### T1112 – Modify Registry

T1112 describes adversary activity involving modification of the Windows Registry.

Registry modifications can be used for purposes such as:

- Configuration changes
- Persistence
- Modifying security settings
- Changing application behavior

The alert's MITRE mapping was recorded as provided by the LetsDefend scenario.

---

# 🌐 Threat Intelligence Findings

VirusTotal analysis produced several important findings.

### Detection

```text
44/62 security vendors
```

flagged the file as malicious.

### Macro Behavior

The document contained:

```text
Auto_Open
```

along with suspicious functionality including:

```text
Obfuscation
Base64 decoding
ShellExecute
CreateObject
File downloading
File execution
File-system manipulation
```

### Network Indicator

VirusTotal analysis included:

```text
177.53.143.89
```

This same IP was subsequently observed in network logs from the affected endpoint.

---

# 📊 Log Management Findings

A search using:

```text
Source Address = 172.16.17.56
```

returned three events.

Observed connections:

```text
172.16.17.56 → 177.53.143.89:443
172.16.17.56 → 177.53.143.89:443
172.16.17.56 → 35.189.10.17:443
```

The two connections to `177.53.143.89` were particularly relevant because the IP was also observed during VirusTotal analysis.

However, the network events occurred later than the initial alert.

---

# 💻 Endpoint Investigation Summary

The affected endpoint was:

```text
Sofia
172.16.17.56
```

Endpoint investigation identified:

- Windows 10 system
- Suspicious encoded PowerShell command
- Terminal History records
- Outbound HTTPS activity
- Connection to a VirusTotal-observed IP
- Endpoint containment initially disabled

The endpoint was subsequently successfully contained.

---

# 📝 Analyst Notes

The SOC138 alert involved the malicious macro-enabled Excel file:

```text
ORDER SHEET & SPEC.xlsm
```

VirusTotal confirmed the file as malicious, with 44/62 security vendors detecting it.

The XLSM contained an `Auto_Open` macro with obfuscation, Base64 decoding, `ShellExecute`, file downloading, and file execution capabilities.

The affected endpoint, Sofia (`172.16.17.56`), generated three outbound network events. Two connections were made to `177.53.143.89:443`, an IP also observed during VirusTotal analysis of the malicious file.

Although this was a significant IOC correlation, the connection occurred later than the original alert timestamp. Therefore, the investigation did not claim that the Excel macro definitively initiated the observed network connection.

The endpoint was successfully contained.

---

# 📦 Artifacts Collected

| Artifact | Value |
|---|---|
| **MD5** | `7ccf88c0bbe3b29bf19d877c4596a8d4` |
| **SHA-1** | `23f0506d857d38c3cd5354b80afc725b5f034744` |
| **SHA-256** | `7bcd31bd41686c32663c7cabf42b18c50399e3b3b4533fc2ff002d9f2e058813` |
| **Destination IP** | `177.53.143.89` |
| **Destination IP** | `35.189.10.17` |
| **Source IP** | `172.16.17.56` |
| **Hostname** | `Sofia` |

---

# 🧠 Lessons Learned

### 1. XLSM files deserve additional scrutiny

The `.xlsm` format supports macros, which can be abused to execute malicious code.

### 2. Macros are not automatically malicious

The presence of a macro alone does not prove maliciousness.

Additional evidence such as obfuscation, command execution, downloading files, or suspicious API calls should be considered.

### 3. VirusTotal is useful for initial malware triage

VirusTotal can provide:

- Detection results
- File hashes
- Static analysis
- Behavioral indicators
- Network indicators
- Malware tags

### 4. IOC correlation needs context

Finding the same IP in malware analysis and endpoint logs is valuable, but the analyst should also investigate:

- Timestamp
- Process
- Direction
- Action
- Host
- Related events

### 5. Don't confuse Last Login with command execution

Endpoint metadata such as Last Login should not be used as the timestamp for another event.

Always use the actual event timestamp when performing timeline correlation.

### 6. Don't assume log clearing without evidence

A missing or incomplete history does not automatically prove that an attacker cleared the logs.

Evidence of log/history manipulation should be identified before making that conclusion.

### 7. Containment is an important response action

Once a malicious file and suspicious endpoint activity are identified, isolating the affected host can help prevent further activity while the investigation continues.

---

# 🛠️ Skills Practiced

- SOC Alert Triage
- Malware Analysis
- Microsoft Office Macro Analysis
- XLSM Investigation
- VirusTotal
- Hash Analysis
- IOC Investigation
- Network Log Analysis
- Endpoint Investigation
- PowerShell Analysis
- Timeline Correlation
- Threat Intelligence
- C2 Investigation
- Endpoint Containment
- Incident Response
- Incident Classification

---

# 🔄 SOC Workflow Completed

```text
Alert Received
      ↓
Initial Triage
      ↓
Identify Suspicious XLSM
      ↓
Hash Investigation
      ↓
VirusTotal Analysis
      ↓
Macro Analysis
      ↓
Endpoint Investigation
      ↓
Network Investigation
      ↓
IOC Correlation
      ↓
Timeline Correlation
      ↓
C2 Investigation
      ↓
Endpoint Containment
      ↓
Artifact Collection
      ↓
Analyst Notes
      ↓
Final Classification
```

---

# ✅ Final Verdict

The suspicious Excel file was confirmed to be malicious.

Key evidence included:

```text
44/62 VirusTotal detections
        +
Malicious Auto_Open macro
        +
Obfuscation
        +
Base64 decoding
        +
ShellExecute
        +
File download/execution capability
        +
Endpoint network activity
        +
IOC correlation
```

The affected endpoint was successfully contained.

The final incident classification was:

> **TRUE POSITIVE**

---

# 🏆 LetsDefend Playbook Result

```text
Result:
True Positive

Playbook Score:
15/15

Success Rate:
100%
```

### Playbook Answers

| Investigation Step | Answer |
|---|---|
| Check if someone requested the C2 | **Accessed** |
| Analyze Malware | **Malicious** |
| Check if malware was quarantined/cleaned | **Not Quarantined** |
| Containment | **Host Contained** |
| Final Result | **True Positive** |

---

# 📌 Status

```text
LAB: SOC138
STATUS: COMPLETED
RESULT: TRUE POSITIVE
SCORE: 15/15
SUCCESS RATE: 100%
```

---

## 🔗 Connect With Me

**GitHub:**  
https://github.com/Ajaydevs007

**LinkedIn:**  
https://www.linkedin.com/in/ajaydev-s