# SOC132 – Same Malicious File Found on Multiple Sources

**Platform:** LetsDefend  
**Category:** Malware Investigation  
**Status:** ✅ Completed  
**Event ID:** 68  
**Severity:** Medium  
**Classification:** True Positive

---

# Scenario

A malware alert was generated after the same malicious file was identified across multiple sources.

The file was named `msi.bat` and was associated with multiple endpoints:

- MikeComputer
- JohnComputer
- Sofia

The objective was to determine whether the file was malicious, identify its behavior, investigate potential C2 communication, check the affected endpoints, verify whether the malware had been cleaned, and classify the incident.

---

# Alert Information

| Field | Value |
|--------|-------|
| Rule | SOC132 - Same Malicious File Found on Multiple Sources |
| Event ID | 68 |
| Severity | Medium |
| Event Time | 2021-03-01 15:16:48 +03:00 |
| Alert Type | Malware |
| Role | Security Analyst |
| MITRE ATT&CK | T1204 |
| File Name | `msi.bat` |
| File Size | 2.12 KB |
| MD5 | `3dc649bc1be6f4881d386e679b7b60c8` |
| Source Address | `172.16.17.14` |
| Source Hostnames | MikeComputer, JohnComputer, Sofia |
| Device Action | Cleaned |

---

# Investigation Process

## Step 1 – Initial Alert Review

The investigation started by reviewing Event ID 68.

The alert indicated that the same malicious file had been detected across multiple sources.

### Findings

| Item | Value |
|------|-------|
| File Name | `msi.bat` |
| File Size | 2.12 KB |
| MD5 | `3dc649bc1be6f4881d386e679b7b60c8` |
| Source Address | `172.16.17.14` |
| Affected Sources | MikeComputer, JohnComputer, Sofia |
| Device Action | Cleaned |

The presence of the same file across multiple sources required further investigation to determine the nature of the file and whether additional malicious activity had occurred.

---

## Step 2 – Analyze the Malware

The file hash was investigated using VirusTotal.

### SHA256

```text
7538b8a61dd42c874e7e153dad02c528f06c397344e70de01fdc98a5c28030bf
```

### VirusTotal Detection

```text
16/60 security vendors flagged the file as malicious
```

Multiple security vendors associated the sample with malicious classifications including:

- Trojan
- Backdoor
- Reverse Shell
- Agent

The VirusTotal results confirmed that the file was malicious.

---

## Step 3 – Analyze Malware Behavior

VirusTotal behavioral analysis provided additional information about the sample.

The code was observed creating a socket connection to:

```text
81.68.99.93:443
```

The analysis indicated communication between the socket and:

```text
cmd.exe
```

The sample created two threads:

```text
p2s
s2p
```

The `p2s` thread reads output from the `cmd.exe` process and sends it through the socket connection.

The `s2p` thread reads data from the socket connection and writes it to the standard input of `cmd.exe`.

This behavior is consistent with reverse-shell functionality.

### Observed Communication Flow

```text
Remote Host
     |
     | TCP/443
     v
81.68.99.93
     |
     v
Network Socket
     |
     v
cmd.exe
     |
     +---- stdin
     |
     +---- stdout
```

The identified remote address was therefore treated as a potential C2 indicator for further investigation.

---

## Step 4 – Identify Potential C2 Address

The IP address identified during malware analysis was:

```text
81.68.99.93
```

The associated port was:

```text
443
```

Therefore:

```text
81.68.99.93:443
```

was investigated as a potential C2 indicator.

It is important to distinguish between malware behavior observed in VirusTotal and confirmed activity within the organization's environment.

VirusTotal demonstrated that the sample communicated with this address during sandbox analysis, but this alone does not prove that the affected endpoints communicated with the same address.

---

## Step 5 – Hunt for C2 Communication

The potential C2 address was searched in the LetsDefend Log Management system.

### Search

```text
Destination Address equals "81.68.99.93"
```

### Result

```text
0 events found
```

No matching events were identified for the destination address.

Additional source-to-destination searches were also performed.

### Source Address

```text
172.16.17.14
```

### Destination Address

```text
81.68.99.93
```

### Result

```text
0 events found
```

Additional internal source addresses reviewed during the investigation were also searched against the potential C2 address.

### Results

```text
172.16.17.14 → 81.68.99.93
0 events found

172.16.17.82 → 81.68.99.93
0 events found

172.16.17.56 → 81.68.99.93
0 events found
```

No matching internal network events were identified.

Therefore, C2 communication with `81.68.99.93` could not be confirmed using the available Log Management data.

---

## Step 6 – Investigate Affected Endpoints

The affected sources identified in the alert were:

- MikeComputer
- JohnComputer
- Sofia

Each endpoint was investigated separately.

The investigation focused on:

- Suspicious PowerShell activity
- Process execution
- Malware-related activity
- Network communication
- Activity around the alert timeframe

---

## Step 7 – Investigate MikeComputer

A suspicious encoded PowerShell command was identified on MikeComputer.

The command used encoded PowerShell execution:

```text
powershell -e ...
```

The command contained heavily obfuscated PowerShell content and appeared suspicious.

However, the event did not temporally correlate with Event ID 68.

Because the activity did not align with the alert timeframe, it was not attributed to the current SOC132 incident.

### Investigation Decision

```text
Suspicious PowerShell Activity
            |
            v
     Check Timestamp
            |
            v
 No Temporal Correlation
            |
            v
 Not Attributed to SOC132
```

This demonstrates the importance of temporal correlation during SOC investigations.

Suspicious activity should not automatically be considered related to an incident without sufficient supporting evidence.

---

## Step 8 – Investigate JohnComputer

JohnComputer was reviewed for activity associated with the malware alert.

No relevant suspicious activity was identified during the investigation.

No additional evidence was found linking JohnComputer to:

- Malware execution
- C2 communication
- Suspicious PowerShell activity
- Related malicious processes

Therefore, no additional malicious activity was established on JohnComputer during the investigation.

---

## Step 9 – Investigate Sofia

A suspicious encoded PowerShell command was also identified on Sofia.

The command used encoded PowerShell execution:

```text
powershell -EncodedCommand ...
```

The command contained heavily obfuscated PowerShell content and suspicious network-related strings.

However, the event did not temporally correlate with Event ID 68.

Therefore, the PowerShell activity was not attributed to the current SOC132 incident.

### Investigation Decision

```text
Suspicious PowerShell Activity
            |
            v
     Check Timestamp
            |
            v
 No Temporal Correlation
            |
            v
 Excluded from SOC132
```

---

## Step 10 – Verify Malware Containment

The original alert showed:

```text
Device Action: Cleaned
```

The LetsDefend playbook required verification of whether the malware had been quarantined or cleaned.

The playbook confirmed:

```text
Quarantined
```

This indicated that the endpoint security control had already performed remediation against the detected malware.

---

## Step 11 – Add Investigation Artifacts

The following artifacts were identified during the investigation.

### File Name

```text
msi.bat
```

### MD5

```text
3dc649bc1be6f4881d386e679b7b60c8
```

### SHA256

```text
7538b8a61dd42c874e7e153dad02c528f06c397344e70de01fdc98a5c28030bf
```

### Potential C2

```text
81.68.99.93:443
```

These indicators were documented as part of the investigation.

---

# Indicators of Compromise (IOCs)

## File

| IOC | Value |
|------|-------|
| File Name | `msi.bat` |
| File Size | 2.12 KB |
| MD5 | `3dc649bc1be6f4881d386e679b7b60c8` |
| SHA256 | `7538b8a61dd42c874e7e153dad02c528f06c397344e70de01fdc98a5c28030bf` |

---

## Potential C2

```text
81.68.99.93
```

### Port

```text
443
```

### Protocol

```text
TCP
```

---

## Internal Source Address

```text
172.16.17.14
```

---

## Affected Hosts

```text
MikeComputer
JohnComputer
Sofia
```

---

# MITRE ATT&CK Mapping

| Tactic | Technique |
|----------|-----------|
| Execution | T1204 – User Execution |

The `T1204` technique was provided by the LetsDefend alert.

The reverse-shell behavior observed during VirusTotal analysis provides additional behavioral context, but additional ATT&CK techniques were not explicitly assigned by the LetsDefend alert.

---

# Threat Intelligence Findings

VirusTotal analysis produced the following findings:

| Finding | Result |
|---------|--------|
| SHA256 | `7538b8a61dd42c874e7e153dad02c528f06c397344e70de01fdc98a5c28030bf` |
| Detection | 16/60 |
| File | `msi.bat` |
| Classification | Malicious |
| Behavioral Finding | Reverse-shell-like behavior |
| Potential C2 | `81.68.99.93:443` |

The behavioral analysis showed the sample creating a socket connection and interacting with `cmd.exe`.

---

# Log Management Findings

The potential C2 address was searched in the LetsDefend Log Management system.

### Query

```text
Destination Address equals "81.68.99.93"
```

### Result

```text
0 events found
```

Additional source-to-destination searches were performed.

### Results

```text
172.16.17.14 → 81.68.99.93
0 events found

172.16.17.82 → 81.68.99.93
0 events found

172.16.17.56 → 81.68.99.93
0 events found
```

No matching C2 communication was identified in the available logs.

---

# Endpoint Investigation Summary

| Host | Finding | Decision |
|------|---------|----------|
| MikeComputer | Suspicious encoded PowerShell | Not correlated with SOC132 |
| JohnComputer | No relevant suspicious activity | No additional evidence |
| Sofia | Suspicious encoded PowerShell | Not correlated with SOC132 |

The suspicious PowerShell activity found on MikeComputer and Sofia was investigated but excluded from the current incident due to the lack of temporal correlation.

---

# Analyst Notes

The investigation began after Event ID 68 triggered the **SOC132 - Same Malicious File Found on Multiple Sources** detection. The alert identified the file `msi.bat`, with an MD5 hash of `3dc649bc1be6f4881d386e679b7b60c8`, across multiple sources including MikeComputer, JohnComputer, and Sofia. The endpoint action was recorded as **Cleaned**.

The file hash was investigated using VirusTotal. The corresponding SHA256 was identified as `7538b8a61dd42c874e7e153dad02c528f06c397344e70de01fdc98a5c28030bf`. VirusTotal reported that **16/60 security vendors** flagged the sample as malicious. Multiple vendors associated the sample with classifications including Trojan, Backdoor, Reverse Shell, and Agent.

Behavioral analysis showed that the sample created a socket connection to `81.68.99.93` over port `443`. The sample also interacted with `cmd.exe` through the network socket. This behavior was consistent with reverse-shell functionality and identified `81.68.99.93:443` as a potential C2 indicator.

The potential C2 address was searched in LetsDefend Log Management. The destination search returned **0 events**. Additional searches correlating the C2 address with the investigated internal source addresses also returned no events. Therefore, C2 communication with the organization's endpoints could not be confirmed using the available logs.

The affected endpoints were then investigated individually. MikeComputer and Sofia contained suspicious encoded PowerShell commands. However, these events did not align with the timestamp of Event ID 68 and were therefore not attributed to the current incident. JohnComputer did not show relevant suspicious activity.

The endpoint remediation status was then verified. The original alert showed **Device Action: Cleaned**, and the playbook confirmed the malware as **Quarantined**.

Based on the available evidence, the file was confirmed to be malicious and the alert was classified as a **True Positive**.

---

# Artifacts Collected

- Event ID: 68
- Alert Rule: SOC132 - Same Malicious File Found on Multiple Sources
- File Name: `msi.bat`
- File Size: 2.12 KB
- MD5 Hash
- SHA256 Hash
- VirusTotal Detection Results
- VirusTotal Behavioral Analysis
- Potential C2 IP Address
- C2 Port
- Log Management Search Results
- Source IP Address
- Affected Hostnames
- Endpoint Remediation Status
- Analyst Notes
- LetsDefend Playbook Results

---

# Lessons Learned

- The same malicious file appearing across multiple endpoints should be investigated for potential malware distribution.
- File hashes are useful for identifying and correlating malware samples across security tools.
- VirusTotal can provide useful malware detection and behavioral information.
- Behavioral analysis can reveal potential C2 infrastructure.
- C2 indicators identified in sandbox environments should be validated against internal network telemetry.
- A `0 events found` result is still valuable evidence during an investigation.
- Suspicious activity should be correlated with the alert timeline before being attributed to an incident.
- Encoded PowerShell commands require additional investigation because encoding can hide the actual command content.
- Endpoint remediation should be verified rather than assumed.
- A malicious file being detected does not automatically prove successful C2 communication.
- Analysts should clearly distinguish confirmed evidence from activity that could not be correlated.

---

# Skills Practiced

- SOC Alert Triage
- Malware Analysis
- File Hash Analysis
- VirusTotal
- Malware Behavioral Analysis
- C2 Identification
- Log Management
- IOC Hunting
- Endpoint Investigation
- PowerShell Investigation
- Timeline Correlation
- Threat Intelligence
- Malware Containment
- Incident Documentation
- Analyst Notes
- SOC Playbook Execution
- Incident Classification

---

# SOC Workflow Completed

- [x] Alert Triage
- [x] Initial Alert Review
- [x] File Hash Investigation
- [x] Malware Analysis
- [x] VirusTotal Investigation
- [x] Behavioral Analysis
- [x] C2 Identification
- [x] C2 Log Search
- [x] Endpoint Investigation
- [x] PowerShell Investigation
- [x] Timeline Correlation
- [x] IOC Collection
- [x] Malware Containment Verification
- [x] Artifacts Added
- [x] Analyst Notes Added
- [x] Playbook Executed
- [x] Incident Classification
- [x] Alert Closed

---

# Final Verdict

**True Positive**

The `msi.bat` file was confirmed to be malicious through VirusTotal analysis, where **16/60 security vendors** detected the sample. Behavioral analysis showed reverse-shell-like functionality involving a network socket and `cmd.exe`, with `81.68.99.93:443` identified as a potential C2 address.

The potential C2 address was investigated through LetsDefend Log Management, but no corresponding internal network events were found. The affected endpoints were also investigated, and suspicious PowerShell activity found on MikeComputer and Sofia was excluded from the incident because it did not correlate with the alert timeframe.

The endpoint security action was recorded as **Cleaned**, and the playbook confirmed the malware as **Quarantined**.

The incident was therefore classified as a **True Positive**.

---

# LetsDefend Playbook Result

```text
Result: True Positive
Playbook Score: 15/15
Success Rate: 100%
Status: Closed
```