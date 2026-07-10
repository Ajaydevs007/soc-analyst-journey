# Investigation Title

> Replace with the investigation name (e.g., Reconnaissance Detection & Telemetry Analysis)

---

# 1. Investigation Overview

## Objective

Investigate repeated failed SMB authentication attempts followed by a successful authentication to determine whether a brute-force attack was performed against the Windows 10 endpoint.

The investigation aims to identify the attack technique, correlate evidence from Windows Security logs and Splunk Enterprise, determine how the attack progressed, evaluate the effectiveness of audit logging, and document the telemetry generated throughout the authentication attack.

---

## Scope

### Investigation Status

| Field            | Value                   |
| ---------------- | ----------------------- |
| Investigation ID | CS-002                  |
| Status           | Closed                  |
| Severity         | Medium                  |
| Disposition      | Benign Security Testing |
| Analyst          | Ajaydev S               |


---

### Investigation Trigger

Multiple Windows Security Event ID 4625 (Failed Logon) events originating from the same source IP address were observed in Splunk. The failed authentication attempts were immediately followed by a successful Event ID 4624 (Successful Logon), indicating a possible brute-force authentication attack.

---

### Investigation Goals

- Determine whether a brute-force attack occurred.
- Identify the targeted user account.
- Correlate failed and successful authentication events.
- Identify the source workstation and IP address.
- Determine the authentication protocol used.
- Develop detection opportunities for future authentication attacks.

---

### Time Window

09 July 2026
---

### Systems Involved

- Kali Linux (Attacker)
- Windows 10 (Victim)
- Splunk Enterprise
- Splunk Universal Forwarder
- Windows Security Log
---

# 2. Lab Environment

| Component | Details |
|-----------|---------|
| Hypervisor | VirtualBox |
| Attacker | Kali Linux (10.10.10.10) |
| Victim | Windows 10 (10.10.10.20) |
| SIEM | Splunk Enterprise |
| Log Forwarder | Splunk Universal Forwarder |
| Endpoint Telemetry | Sysmon (SwiftOnSecurity Configuration) |
| Network | Internal Network (`soc-lab-net`) |



## Telemetry Sources

- Windows Security Log
- Splunk Enterprise

---

# 3. Attack Overview

## Attack Scenario

A brute-force authentication attack was simulated from the Kali Linux attacker against the Windows 10 endpoint using Hydra over SMB2.

The objective was to generate multiple failed authentication attempts followed by a successful authentication in order to analyze Windows Security Event IDs 4625 and 4624, correlate the events in Splunk, and evaluate authentication telemetry generated during a password attack.
---

## Attack Details

| Field            | Value                         |
| ---------------- | ----------------------------- |
| Attack Phase     | Credential Access             |
| ATT&CK Tactic    | Credential Access             |
| ATT&CK Technique | T1110.001 – Password Guessing |
| Attacker         | Kali Linux                    |
| Target           | Windows 10                    |
| Source IP        | 10.10.10.10                   |
| Destination IP   | 10.10.10.20                   |
| Tool             | Hydra                         |
| Attack Type      | SMB2 Password Guessing        |
| Target User      | socuser                       |


---

## Attack Execution

### Tool

Hydra v9.6
---

### Command

```bash
hydra -L users.txt -P passwords.txt -t 1 -f smb2://10.10.10.20
```

---

### Command Output

```text
Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2026-07-09 02:54:49
[WARNING] Workgroup was not specified, using "WORKGROUP"
[DATA] max 1 task per 1 server, overall 1 task, 6 login tries (l:1/p:6), ~6 tries per task
[DATA] attacking smb2://10.10.10.20:445/
[WARNING] 10.10.10.20 might accept any credential
[445][smb2] host: 10.10.10.20   login: socuser   password: Lab@12345
[STATUS] attack finished for 10.10.10.20 (valid pair found)
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2026-07-09 02:54:49```

---

### Initial Observation

Hydra successfully authenticated to the Windows SMB service after several failed password attempts. Windows Security logs generated multiple Event ID 4625 entries followed immediately by a successful Event ID 4624 for the same user account, indicating that password guessing activity had succeeded.

---

# 4. Evidence Collection

Document all evidence collected during the investigation.

---

## 4.1 Attacker Evidence (Kali Linux)

### Tool

Hydra v9.6

---

### Commands Executed

```bash
hydra -L users.txt -P passwords.txt -t 1 -f smb2://10.10.10.20
```

---

### Terminal Output

```text
[445][smb2] host: 10.10.10.20
login: socuser
password: Lab@12345

1 of 1 target successfully completed, 1 valid password found
```

---

### Screenshots

- step52-hydra-smb2-attack.png

---

### Initial Observations

- Hydra successfully authenticated to the SMB service.
- Five incorrect passwords were attempted before the correct password was identified.
- The attack stopped immediately after discovering the valid credentials due to the `-f` option.
- The attack targeted the local Windows account `socuser`.

---

## 4.2 Windows Security Evidence

### Relevant Event IDs

| Event ID | Description |
|----------|-------------|
| 4625 | Failed Logon |
| 4624 | Successful Logon |

---

### Event Details

#### Event ID 4625

| Field | Value |
|--------|-------|
| Target User | socuser |
| Source IP | 10.10.10.10 |
| Workstation | KALI |
| Logon Type | 3 (Network) |
| Authentication Package | NTLM |
| Logon Process | NtLmSsp |
| Status | 0xC000006D |
| SubStatus | 0xC000006A |

---

#### Event ID 4624

| Field | Value |
|--------|-------|
| Target User | socuser |
| Source IP | 10.10.10.10 |
| Workstation | KALI |
| Logon Type | 3 (Network) |
| Authentication Package | NTLM |
| Logon Process | NtLmSsp |

---

### Screenshots

- step53-eventviewer-4625.png
- step54-eventviewer-4624.png

---

### Observations

- Five failed authentication events were generated.
- A successful authentication immediately followed the failed attempts.
- The same source IP and workstation appeared in all authentication events.
- Logon Type 3 confirmed a network authentication over SMB.
- NTLM was used for authentication.

---

## 4.3 Windows Firewall Evidence

### Log Entries

No Windows Defender Firewall events were required for this investigation.

The SMB connection was permitted by the configured firewall rule.

---

### Observations

Windows Defender Firewall allowed inbound SMB traffic, enabling authentication events to be generated.

---

## 4.4 Sysmon Evidence

### Relevant Event IDs

No Sysmon Event IDs were required for authentication analysis.

---

### Observations

Authentication activity is primarily recorded within the Windows Security Log rather than Sysmon.

---

## 4.5 Splunk Evidence

### Search Queries

```spl
source="WinEventLog:Security"
(EventCode=4625 OR EventCode=4624)
Account_Name="socuser"
```

---

```spl
source="WinEventLog:Security"
(EventCode=4625 OR EventCode=4624)
Account_Name="socuser"
| table _time EventCode Account_Name Logon_Type Authentication_Package_Name Logon_Process Source_Network_Address Workstation_Name Failure_Reason Status Sub_Status
```

---

### Search Results

Splunk identified:

- Five Event ID 4625 (Failed Logon)
- One Event ID 4624 (Successful Logon)

All events originated from:

- Source IP: 10.10.10.10
- Workstation: KALI

---

### Screenshots

- step55-splunk-auth-events.png

---

### Observations

Splunk successfully correlated the failed and successful authentication events.

The authentication sequence clearly demonstrated repeated password guessing followed by successful access using valid credentials.

---

# 5. Evidence Summary

| Evidence Source | Summary |
|-----------------|---------|
| Kali Linux | Confirmed Hydra SMB2 password attack |
| Windows Security Log | Recorded Event IDs 4625 and 4624 |
| Windows Firewall | Allowed SMB authentication traffic |
| Sysmon | No relevant authentication telemetry |
| Splunk | Correlated authentication events and attacker information |

---

# 6. Investigation

---

## 6.1 Telemetry Analysis

### Kali Linux

The attacker executed a password guessing attack against the Windows SMB service using Hydra over SMB2.

Hydra attempted authentication using one username (`socuser`) and multiple passwords contained within `passwords.txt`. Five incorrect passwords were attempted before the correct password (`Lab@12345`) was successfully identified.

Hydra immediately terminated the attack after discovering valid credentials due to the use of the `-f` option.

This confirmed successful credential compromise during the controlled security test.

---

### Windows Security Log

Windows Security auditing successfully recorded every authentication attempt.

Each incorrect password generated Event ID **4625 (Failed Logon)**.

The final successful authentication generated Event ID **4624 (Successful Logon)**.

The Security log also recorded valuable authentication metadata including:

- Target user
- Source IP address
- Workstation name
- Authentication package
- Logon process
- Logon type
- Failure status codes

This provided complete visibility into the authentication attack.

---

### Windows Defender Firewall

Windows Defender Firewall permitted inbound SMB traffic to TCP port 445.

Because the firewall allowed the connection, authentication requests reached the Local Security Authority (LSA), allowing Windows Security logs to record both failed and successful logon events.

No firewall blocks or dropped packets were observed during this investigation.

---

### Sysmon

No Sysmon Event IDs were used during this investigation.

Authentication events are generated by Windows Security auditing rather than Sysmon.

This demonstrates that analysts should select telemetry sources appropriate to the activity being investigated.

---

### Splunk Enterprise

Splunk successfully centralized Windows Security events generated during the password attack.

By correlating Event IDs 4625 and 4624, Splunk reconstructed the complete authentication sequence from the first failed password through the successful authentication.

Additional fields such as Source Network Address, Workstation Name, Logon Type, Authentication Package, Status, and SubStatus provided valuable context for the investigation.

---

## 6.2 Event Correlation

The investigation correlated evidence collected from Kali Linux, Windows Security logs, and Splunk Enterprise to reconstruct the authentication attack.

### Step 1 — Brute Force Initiated

The attacker executed Hydra using the SMB2 module against the Windows 10 endpoint.

Hydra attempted authentication against the local account `socuser` using a password list containing six candidate passwords.

Evidence:

- Hydra terminal output
- Attack command

---

### Step 2 — Failed Authentication Attempts

For each incorrect password, Windows generated Event ID **4625 (Failed Logon)**.

Each failed event contained consistent metadata:

- Target User: `socuser`
- Source IP: `10.10.10.10`
- Workstation: `KALI`
- Logon Type: `3`
- Authentication Package: `NTLM`

The repeated failures demonstrated password guessing activity against the same user account.

---

### Step 3 — Successful Authentication

After several failed attempts, Hydra submitted the correct password.

Windows successfully authenticated the user and generated Event ID **4624 (Successful Logon)**.

The successful authentication contained the same:

- Source IP
- Workstation
- Logon Type
- Authentication Package

This confirmed that the successful logon originated from the same attacker responsible for the earlier failed attempts.

---

### Step 4 — Centralized Visibility Through Splunk

Splunk ingested all Windows Security events generated during the attack.

Correlation of Event IDs 4625 and 4624 enabled the entire authentication sequence to be reconstructed.

The investigation confirmed that the attack consisted of repeated failed authentication attempts followed immediately by successful access using valid credentials.

---

## 6.3 Root Cause Analysis

The investigation determined that the observed authentication events accurately reflected a password guessing attack performed against the Windows SMB service.

### Root Cause

The attacker executed Hydra using the SMB2 protocol against the local Windows account `socuser`.

Hydra attempted multiple passwords until valid credentials were discovered.

Once the correct password was submitted, Windows authenticated the user and established a successful network logon.

---

### Why Event ID 4625 Was Generated

Each incorrect password caused Windows authentication to fail.

Windows Security auditing generated Event ID **4625** for every failed authentication attempt.

The event Status value:

```text
0xC000006D
```

indicated a generic logon failure.

The SubStatus value:

```text
0xC000006A
```

identified the specific cause as an incorrect password.

These fields confirmed that the user account existed and that authentication failed because invalid credentials were supplied.

---

### Why Event ID 4624 Was Generated

After multiple failed attempts, Hydra submitted the correct password.

Windows successfully authenticated the account and generated Event ID **4624 (Successful Logon)**.

The event recorded:

- Target User: `socuser`
- Source IP: `10.10.10.10`
- Workstation: `KALI`
- Authentication Package: `NTLM`
- Logon Type: `3`

These values confirmed successful SMB network authentication from the attacking system.

---

### Authentication Analysis

The attack exhibited a common brute-force pattern:

1. Multiple failed authentication attempts against the same account.
2. All attempts originated from the same external IP address.
3. The same workstation generated every authentication request.
4. Successful authentication immediately followed the failed attempts.

This sequence strongly indicates password guessing rather than normal user activity.

---

### Defensive Control Assessment

Windows Security auditing functioned correctly by recording both failed and successful authentication events.

Splunk successfully centralized and correlated these events, allowing the complete authentication attack to be reconstructed.

The investigation demonstrated that Windows Security logs provide comprehensive visibility into password guessing attacks when logon auditing is enabled.

---

### Investigation Conclusion

The investigation confirmed that a controlled brute-force attack was successfully performed against the Windows SMB service.

Repeated Event ID 4625 entries followed immediately by Event ID 4624 confirmed that valid credentials were obtained through password guessing.

Correlation of Windows Security logs and Splunk telemetry enabled complete reconstruction of the authentication attack and verified the effectiveness of Windows Security auditing for detecting brute-force activity.

---

# 7. Timeline Reconstruction

| Time | Activity | Telemetry Source | Evidence |
|------|----------|------------------|----------|
| 12:24:51.391 | First failed SMB authentication | Windows Security | Event ID 4625 |
| 12:24:51.436 | Second failed SMB authentication | Windows Security | Event ID 4625 |
| 12:24:51.446 | Third failed SMB authentication | Windows Security | Event ID 4625 |
| 12:24:51.470 | Fourth failed SMB authentication | Windows Security | Event ID 4625 |
| 12:24:51.488 | Fifth failed SMB authentication | Windows Security | Event ID 4625 |
| 12:24:51.552 | Successful SMB authentication | Windows Security | Event ID 4624 |
| 12:24:51 | Credentials identified | Hydra | Hydra Output |
| 12:24:52 | Events indexed | Splunk Enterprise | SPL Search |

---

# 8. Findings

## Confirmed Findings

### Finding 1 — Brute Force Attack Confirmed

The investigation confirmed that a password guessing attack was performed against the local Windows account `socuser` using Hydra over SMB2.

---

### Finding 2 — Multiple Failed Authentication Attempts

Five failed authentication attempts generated Windows Security Event ID **4625**.

Each failed event originated from the same:

- Source IP
- Workstation
- User account

indicating repeated password guessing activity.

---

### Finding 3 — Successful Authentication Achieved

Following the failed attempts, Hydra successfully authenticated using the correct password.

Windows generated Event ID **4624**, confirming successful network authentication.

---

### Finding 4 — Authentication Used NTLM

Windows authenticated the SMB session using the NTLM authentication package through the NtLmSsp logon process.

---

### Finding 5 — Splunk Successfully Correlated the Attack

Splunk centralized all authentication events and enabled complete reconstruction of the attack timeline.

Repeated failed logons followed immediately by a successful logon clearly demonstrated a successful brute-force attack.

---

## Supporting Evidence

| Finding | Supporting Evidence |
|----------|---------------------|
| Brute Force Confirmed | Hydra terminal output |
| Failed Authentication | Windows Security Event ID 4625 |
| Successful Authentication | Windows Security Event ID 4624 |
| Source Attribution | Source IP 10.10.10.10 |
| Event Correlation | Splunk Security Log Search |

---

# 9. Detection & Telemetry Coverage

| Telemetry Source | Coverage | Notes |
|------------------|----------|------|
| Windows Security Log | ✅ High | Complete authentication visibility through Event IDs 4625 and 4624 |
| Windows Firewall | ⚠️ Low | Allowed SMB traffic but did not provide authentication details |
| Sysmon | ❌ None | Authentication events are not recorded by Sysmon |
| Splunk | ✅ High | Successfully correlated failed and successful logon events |

---

# 10. Detection Opportunities

The investigation identified several opportunities to improve brute-force detection.

## Detection Logic

Generate an alert when multiple Event ID **4625** events targeting the same account originate from the same source IP within a short time window and are followed by Event ID **4624**.

---

## Primary Telemetry

- Windows Security Log
- Splunk Enterprise

---

## Supporting Telemetry

- Windows Firewall
- Active Directory (Domain environments)

---

## Detection Strategy

Correlate:

- Username
- Source IP
- Workstation
- Logon Type
- Authentication Package
- Time Window

Alert when repeated failed logons transition into a successful authentication.

---

## Detection Improvements

Future improvements may include:

- Threshold-based detection (e.g., five failed logons within one minute).
- Account lockout monitoring.
- Correlation with reconnaissance activity.
- Risk scoring for repeated authentication failures.
- Threat intelligence enrichment for external IP addresses.

---

# 11. Analyst Reflection

At the beginning of this investigation, the authentication attack could not be executed successfully because the existing Windows account was linked to a Microsoft account rather than a standalone local account.

Through systematic troubleshooting, a dedicated local account (`socuser`) was created and validated using native Windows authentication and SMB testing before Hydra was used.

This approach ensured that the investigation focused on understanding the authentication process rather than assuming the attack tool was at fault.

The investigation demonstrated the value of validating credentials, network connectivity, and authentication services before conducting password attacks.

It also reinforced the importance of correlating Windows Security logs with SIEM data to accurately reconstruct authentication activity.

---

# 12. Lessons Learned

- Windows Security Event IDs 4625 and 4624 provide comprehensive visibility into authentication activity.
- Logon Type 3 indicates a network authentication such as SMB.
- Status and SubStatus fields provide valuable insight into authentication failures.
- NTLM authentication metadata can assist in attack attribution.
- Splunk correlation enables analysts to reconstruct authentication attacks.
- Verifying credentials independently before using password attack tools simplifies troubleshooting.
- Dedicated local accounts provide predictable authentication behavior during security testing.

---

# 13. References

## Documentation References

- Microsoft Windows Security Auditing Documentation
- Microsoft Event ID 4624 Documentation
- Microsoft Event ID 4625 Documentation
- MITRE ATT&CK
- Splunk Documentation
- Hydra Documentation

---

## Investigation Evidence

- Hydra terminal output
- Windows Event Viewer
- Splunk Security Log searches
- Project screenshots

---

## Related Project Documents

- `mitre-mapping/brute-force-mitre.md`
- `splunk-detections/brute-force-detection.md`
- `incident-report/brute-force-incident-report.md`
- `playbooks/brute-force-playbook.md`

---

## Next Investigation

Successful Authentication Analysis (Event ID 4624)

---

Template Version: 2.0

Last Updated: 2026-06-26

Author: Ajaydev S