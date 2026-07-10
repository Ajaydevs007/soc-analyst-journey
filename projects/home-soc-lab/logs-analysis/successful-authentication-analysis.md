# Investigation Title


---

# 1. Investigation Overview

## Objective

Investigate a successful Windows network authentication recorded as Event ID 4624 to determine whether the authentication was expected, identify the source of the logon, validate the authentication metadata, and assess whether additional investigation is required following successful access to the Windows endpoint.
---

## Scope

### Investigation Status

| Field            | Value                   |
| ---------------- | ----------------------- |
| Investigation ID | CS-003                  |
| Status           | Closed                  |
| Severity         | Medium                  |
| Disposition      | Benign Security Testing |
| Analyst          | Ajaydev S               |


---

### Investigation Trigger

Splunk identified a successful Windows Security Event ID 4624 generated after a series of failed authentication attempts. Because successful authentication may indicate compromised credentials or unauthorized access, the event required investigation.

---

### Investigation Goals

- Verify the successful authentication.
- Identify the authenticated account.
- Determine the authentication source.
- Validate the authentication protocol.
- Determine whether the authentication was expected.
- Identify recommended post-authentication investigation steps.

---

### Time Window

09 July 2026
---

### Systems Involved

- Kali Linux (Attacker)
- Windows 10 (Victim)
- Splunk Enterprise
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
- Sysmon Operational Log
- Splunk Enterprise

---

# 3. Attack Overview

## Attack Scenario

Following a successful password guessing attack, Hydra authenticated to the Windows SMB service using valid credentials for the local account `socuser`.

Windows generated Event ID 4624 (Successful Logon), providing detailed authentication metadata including the authenticated account, source IP address, workstation name, authentication package, and logon type.

The objective of this investigation is to analyze the successful authentication from a defender's perspective and determine whether additional investigation or response actions would be required.

---

## Attack Details

| Field            | Value                  |
| ---------------- | ---------------------- |
| Attack Phase     | Initial Access         |
| ATT&CK Tactic    | Initial Access         |
| ATT&CK Technique | T1078 – Valid Accounts |
| Attacker         | Kali Linux             |
| Target           | Windows 10             |
| Source IP        | 10.10.10.10            |
| Destination IP   | 10.10.10.20            |
| Tool             | Hydra                  |
| Authentication   | SMB2                   |
| User             | socuser                |


---

## Attack Execution

### Tool

Hydra v9.6
---

### Command

```bash
hydra -L users.txt -P passwords.txt -t 1 -f smb2://10.10.10.20```
---

### Command Output

```text
# Paste terminal output
```

---

### Initial Observation

Windows recorded Event ID 4624 indicating that the account `socuser` successfully authenticated from the Kali Linux workstation (`10.10.10.10`) using NTLM over SMB. Because successful authentication can indicate either legitimate user activity or unauthorized access using valid credentials, additional investigation was required to validate the context of the logon.

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

- Hydra successfully authenticated to the Windows SMB service.
- Valid credentials for the local account `socuser` were identified.
- Successful authentication marked the transition from password guessing to authenticated access.

---

## 4.2 Windows Security Evidence

### Relevant Event IDs

| Event ID | Description |
|----------|-------------|
| 4624 | Successful Logon |

---

### Event Details

| Field | Value |
|--------|-------|
| Target User | socuser |
| Target Domain | DESKTOP-MMAR6A0 |
| Source IP | 10.10.10.10 |
| Source Port | 47792 |
| Workstation | KALI |
| Logon Type | 3 (Network) |
| Authentication Package | NTLM |
| Logon Process | NtLmSsp |
| Key Length | 128 |

---

### Screenshots

- step54-eventviewer-4624.png

---

### Observations

- Windows successfully authenticated the account `socuser`.
- Authentication originated from the Kali Linux attacker.
- The connection was performed over the network using SMB.
- NTLM authentication was successfully completed.

---

## 4.3 Windows Firewall Evidence

### Log Entries

Windows Defender Firewall permitted inbound SMB traffic to TCP port 445.

No dropped packets or blocked authentication attempts were observed during the successful logon.

---

### Observations

Windows Defender Firewall allowed the SMB session to reach the Windows authentication service.

---

## 4.4 Sysmon Evidence

### Relevant Event IDs

No Sysmon Event IDs were directly relevant to the authentication event.

---

### Observations

Authentication events are recorded by Windows Security auditing rather than Sysmon.

---

## 4.5 Splunk Evidence

### Search Queries

```spl
source="WinEventLog:Security"
EventCode=4624
Account_Name="socuser"
```

---

```spl
source="WinEventLog:Security"
EventCode=4624
| table _time Account_Name Logon_Type Authentication_Package_Name Logon_Process Source_Network_Address Workstation_Name
```

---

### Search Results

Splunk identified one successful Windows authentication.

Important metadata included:

- User: `socuser`
- Source IP: `10.10.10.10`
- Workstation: `KALI`
- Logon Type: `3`
- Authentication Package: `NTLM`

---

### Screenshots

- step55-splunk-auth-events.png

---

### Observations

Splunk successfully collected and indexed the successful authentication event.

The event provided sufficient authentication metadata for investigation and correlation.

---

# 5. Evidence Summary

| Evidence Source | Summary |
|-----------------|---------|
| Kali Linux | Confirmed successful SMB authentication using Hydra |
| Windows Security Log | Event ID 4624 recorded successful network logon |
| Windows Firewall | Allowed SMB authentication traffic |
| Sysmon | No authentication telemetry |
| Splunk | Successfully correlated successful authentication metadata |
---

# 6. Investigation

---

## 6.1 Telemetry Analysis

### Kali Linux

Following the successful password guessing attack, Hydra authenticated to the Windows SMB service using valid credentials for the local account `socuser`.

The successful authentication confirmed that the supplied credentials were accepted by Windows and that the attacker obtained network access to the target endpoint.

Hydra terminated the attack immediately after discovering the valid password because the `-f` option was used.

---

### Windows Security Log

Windows Security auditing generated Event ID **4624 (Successful Logon)** after successfully authenticating the account `socuser`.

The event recorded valuable authentication metadata including:

- Target user
- Source IP address
- Source workstation
- Logon type
- Authentication package
- Logon process

Unlike failed authentication events, Event ID 4624 confirms that Windows granted access to the requested resource.

---

### Windows Defender Firewall

Windows Defender Firewall permitted inbound SMB traffic to TCP port 445.

Because the firewall allowed the connection, the authentication request successfully reached the Local Security Authority (LSA), resulting in a successful authentication event.

No firewall blocks or dropped packets were observed.

---

### Sysmon

No Sysmon Event IDs were required for this investigation.

Authentication activity is recorded by Windows Security auditing rather than Sysmon.

This demonstrates that selecting the appropriate telemetry source is critical during investigations.

---

### Splunk Enterprise

Splunk successfully collected and indexed the successful authentication event.

The centralized telemetry allowed rapid identification of:

- Authenticated account
- Source IP
- Source workstation
- Authentication protocol
- Logon type

Splunk enabled analysts to correlate the successful authentication with the earlier brute-force activity.

---

## 6.2 Event Correlation

The investigation correlated evidence collected from Kali Linux, Windows Security logs, and Splunk Enterprise.

### Step 1 — Valid Credentials Obtained

Hydra successfully identified the correct password for the local Windows account `socuser`.

Evidence:

- Hydra terminal output
- Successful authentication message

---

### Step 2 — Windows Authenticated the User

Windows successfully authenticated the supplied credentials.

Windows Security generated Event ID **4624**, confirming that authentication succeeded.

Important event fields included:

- User: `socuser`
- Source IP: `10.10.10.10`
- Workstation: `KALI`
- Logon Type: `3`
- Authentication Package: `NTLM`

---

### Step 3 — Network Logon Established

Logon Type **3** confirmed that the authentication occurred over the network rather than through an interactive desktop session.

This behavior is expected for SMB authentication.

---

### Step 4 — Splunk Correlation

Splunk correlated the successful Event ID 4624 with the preceding failed Event ID 4625 events.

The authentication timeline demonstrated that successful access immediately followed repeated password guessing attempts.

This sequence strongly indicated credential compromise during the controlled security exercise.

---

## 6.3 Root Cause Analysis

The investigation determined that Windows successfully authenticated the local account `socuser` after valid credentials were supplied.

### Root Cause

Hydra successfully guessed the password for the local Windows account using repeated SMB authentication attempts.

After the correct password was identified, Windows accepted the credentials and established a network logon.

---

### Why Event ID 4624 Was Generated

Event ID **4624** was generated because Windows successfully validated the supplied username and password.

Successful authentication indicates that:

- The account exists.
- The credentials are valid.
- Windows granted network access.

---

### Authentication Analysis

Several event fields provided valuable context.

**Target User**

```
socuser
```

Confirmed which account authenticated successfully.

---

**Source IP**

```
10.10.10.10
```

Identified the originating system.

---

**Workstation**

```
KALI
```

Identified the attacker's host.

---

**Authentication Package**

```
NTLM
```

Confirmed the authentication protocol.

---

**Logon Type**

```
3
```

Confirmed a network authentication via SMB.

---

### Analyst Assessment

A successful authentication event alone is not sufficient to determine malicious activity.

An analyst should validate:

- Whether the source IP is expected.
- Whether the workstation normally authenticates.
- Whether the account should access the system.
- Whether the authentication followed suspicious failed logons.
- Whether suspicious activity occurred after login.

Context determines whether Event ID 4624 represents legitimate user activity or unauthorized access.

---

### Defensive Control Assessment

Windows Security auditing successfully recorded the successful authentication.

Splunk centralized the authentication event and provided sufficient metadata for investigation.

The investigation demonstrated that Event ID 4624 should always be analyzed alongside surrounding authentication activity rather than in isolation.

---

### Investigation Conclusion

The investigation confirmed that valid credentials were successfully used to authenticate to the Windows endpoint.

Although the authentication itself was successful, correlation with previous failed authentication attempts demonstrated that the credentials had been obtained through password guessing.

This highlights the importance of correlating successful authentication events with prior security telemetry to accurately identify compromised accounts.
---

# 7. Timeline Reconstruction

| Time | Activity | Telemetry Source | Evidence |
|------|----------|------------------|----------|
| 12:24:51.391 | Multiple failed SMB authentication attempts | Windows Security | Event ID 4625 |
| 12:24:51.552 | Successful SMB authentication | Windows Security | Event ID 4624 |
| 12:24:51 | Valid credentials identified | Hydra | Hydra Output |
| 12:24:52 | Authentication event indexed | Splunk Enterprise | SPL Search |
| 12:25:00 | Successful authentication investigated | Analyst | Investigation |

---

# 8. Findings

## Confirmed Findings

### Finding 1 — Successful Network Authentication Confirmed

Windows successfully authenticated the local account `socuser` over SMB, generating Windows Security Event ID **4624**.

---

### Finding 2 — Authentication Originated from the Kali Attacker

The successful authentication originated from:

- Source IP: `10.10.10.10`
- Workstation: `KALI`

These values matched the attacking system used during the controlled security exercise.

---

### Finding 3 — NTLM Authentication Was Used

Windows authenticated the user using the **NTLM** authentication package through the **NtLmSsp** logon process.

This is expected behavior for SMB authentication within the lab environment.

---

### Finding 4 — Network Logon Confirmed

Logon Type **3** confirmed that the authentication occurred over the network rather than through an interactive desktop session.

This matched the Hydra SMB2 authentication method used during the attack.

---

### Finding 5 — Authentication Correlated with Earlier Brute Force Activity

Correlation with previous Windows Security Event ID **4625** events confirmed that the successful authentication immediately followed repeated failed password attempts.

This demonstrated that the valid credentials had been obtained through password guessing.

---

## Supporting Evidence

| Finding | Supporting Evidence |
|----------|---------------------|
| Successful Authentication | Windows Security Event ID 4624 |
| Source Attribution | Source IP `10.10.10.10` |
| Workstation Identification | Workstation `KALI` |
| Authentication Method | NTLM Authentication Package |
| Attack Correlation | Previous Event ID 4625 sequence |
| Centralized Visibility | Splunk Search Results |

---

# 9. Detection & Telemetry Coverage

| Telemetry Source | Coverage | Notes |
|------------------|----------|------|
| Windows Security Log | ✅ High | Complete authentication metadata through Event ID 4624 |
| Windows Firewall | ⚠️ Low | Allowed SMB traffic but did not record authentication details |
| Sysmon | ❌ None | Authentication events are recorded by Windows Security auditing |
| Splunk | ✅ High | Successfully centralized and correlated authentication telemetry |

---

# 10. Detection Opportunities

## Detection Logic

Generate an alert when a successful Event ID **4624** occurs shortly after multiple Event ID **4625** events for the same user account from the same source IP.

---

## Primary Telemetry

- Windows Security Log
- Splunk Enterprise

---

## Supporting Telemetry

- Windows Firewall
- Active Directory Security Logs (Domain environments)

---

## Detection Strategy

Correlate:

- Username
- Source IP
- Workstation
- Logon Type
- Authentication Package
- Time Window
- Previous failed authentication attempts

Successful authentication following repeated failures should be investigated as potential credential compromise.

---

## Detection Improvements

Future improvements may include:

- Correlating successful authentication with process creation events.
- Monitoring privileged account logons.
- Detecting authentication from unusual workstations.
- Correlating authentication with endpoint activity.
- Risk scoring based on authentication history.

---

# 11. Analyst Reflection

This investigation demonstrated that successful authentication events require careful analysis rather than being automatically considered legitimate.

Although Event ID **4624** simply records a successful logon, correlation with previous failed authentication attempts revealed that the account had been accessed through password guessing.

The investigation reinforced the importance of validating authentication context, including the source IP, workstation, authentication package, and preceding authentication activity.

It also highlighted that successful authentication often represents the beginning of attacker activity rather than the end of an investigation.

---

# 12. Lessons Learned

- Event ID 4624 confirms successful authentication but does not indicate whether the activity is legitimate.
- Authentication events should always be correlated with previous failed logons.
- Logon Type 3 identifies network-based authentication such as SMB.
- NTLM authentication metadata provides valuable investigative context.
- Splunk correlation significantly improves analyst visibility into authentication attacks.
- Successful authentication often marks the transition to post-compromise activity and should trigger additional investigation.

---

# 13. References

## Documentation References

- Microsoft Windows Security Auditing Documentation
- Microsoft Event ID 4624 Documentation
- MITRE ATT&CK
- Splunk Documentation

---

## Investigation Evidence

- Hydra terminal output
- Windows Event Viewer
- Splunk Security searches
- Project screenshots

---

## Related Project Documents

- `mitre-mapping/successful-authentication-mitre.md`
- `splunk-detections/successful-authentication-detection.md`
- `incident-report/successful-authentication-incident-report.md`
- `playbooks/successful-authentication-playbook.md`

---

## Next Investigation

Command Execution Detection & Process Creation Analysis (Sysmon Event ID 1)

---

# 13. References

## Documentation References

- Microsoft Documentation
- Splunk Documentation
- MITRE ATT&CK
- Sysinternals Sysmon Documentation
- Other technical references used

---

## Investigation Evidence

- Kali terminal output
- Windows Event Viewer
- Windows Firewall log
- Splunk search results
- Project screenshots

---

## Related Project Documents

- MITRE Mapping
- Detection Rule
- Incident Report
- Investigation Playbook

---

Template Version: 2.0

Last Updated: 2026-06-26

Author: Ajaydev S