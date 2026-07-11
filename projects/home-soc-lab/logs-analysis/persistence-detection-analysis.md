# Persistence Detection & Registry Autorun Analysis

---

# Case Study Information

| Field | Value |
|--------|-------|
| Case Study | 5 |
| Investigation | Registry Run Key Persistence |
| Platform | Windows 10 |
| SIEM | Splunk Enterprise |
| Endpoint Telemetry | Sysmon |
| ATT&CK Tactic | Persistence |
| ATT&CK Technique | T1547.001 – Registry Run Keys / Startup Folder |
| Analyst | Ajaydev S |
| Status | Completed |

---

# 1. Investigation Overview

## Objective

The objective of this investigation was to simulate a common Windows persistence technique by creating a Registry Run Key and verifying that Sysmon and Splunk successfully detected and recorded the registry modification.

The investigation focused on understanding how registry-based persistence appears within endpoint telemetry and how analysts can identify autorun mechanisms commonly abused by attackers.

---

## Scenario

Following successful command execution on the Windows endpoint, a Registry Run Key was created under the current user's autorun location.

The registry entry was configured to launch Notepad automatically when the user logs on. Although Notepad is a legitimate Windows application, the same persistence mechanism is frequently abused by attackers to automatically execute malicious payloads after user logon.

Sysmon recorded the registry modification using Event ID 13 (Registry Value Set), and Splunk successfully centralized the telemetry for investigation.

---

# 2. Lab Environment

| Component | Value |
|----------|-------|
| Attacker Machine | Kali Linux |
| Target Machine | Windows 10 |
| SIEM | Splunk Enterprise |
| Endpoint Monitoring | Sysmon |
| Network | VirtualBox NAT Network |

---

# 3. Attack Simulation

## Registry Persistence Command

```cmd
reg add "HKCU\Software\Microsoft\Windows\CurrentVersion\Run" /v HomeSOCLab /t REG_SZ /d "C:\Windows\System32\notepad.exe" /f
```

---

## Verification Command

```cmd
reg query "HKCU\Software\Microsoft\Windows\CurrentVersion\Run"
```

---

## Expected Behavior

Windows stores the new registry value within the current user's Run key.

During user logon, Windows automatically executes applications configured within this registry location.

Sysmon records the registry modification using Event ID 13.

Splunk ingests the event and makes it available for investigation.

---

# 4. Evidence Collection

---

## 4.1 Registry Modification Command

### Command Executed

```cmd
reg add "HKCU\Software\Microsoft\Windows\CurrentVersion\Run" /v HomeSOCLab /t REG_SZ /d "C:\Windows\System32\notepad.exe" /f
```

---

### Verification Command

```cmd
reg query "HKCU\Software\Microsoft\Windows\CurrentVersion\Run"
```

---

### Result

The registry value was successfully created within the current user's **Run** registry key.

The verification confirmed the presence of the following persistence entry:

```text
HomeSOCLab    REG_SZ    C:\Windows\System32\notepad.exe
```

---

### Screenshots

- step63-create-registry-persistence.png
- step64-registry-query.png

---

### Initial Observations

- Registry value successfully created.
- Persistence configured within the current user's Run key.
- Windows will automatically launch the configured application during user logon.
- The persistence mechanism uses a legitimate Windows feature commonly abused by attackers.

---

## 4.2 Sysmon Evidence

### Relevant Event IDs

- Event ID 13 — Registry Value Set
- Event ID 1 — Process Creation (`reg.exe`)

---

### Event ID 13 Analysis

Sysmon recorded the registry modification immediately after the `reg add` command executed.

Key evidence extracted from the event:

| Field | Observed Value |
|--------|----------------|
| Event ID | 13 |
| Operation | SetValue |
| Rule Name | T1060, RunKey |
| Image | `C:\Windows\System32\reg.exe` |
| Target Object | `HKU\S-1-5-21-1881654287-3370865730-3194882591-1001\Software\Microsoft\Windows\CurrentVersion\Run\HomeSOCLab` |
| Details | `C:\Windows\System32\notepad.exe` |
| User | `DESKTOP-MMAR6A0\ajayd` |
| Process ID | 7952 |

---

### Registry Path Interpretation

The registry modification was performed using the `HKCU` alias:

```cmd
reg add "HKCU\Software\Microsoft\Windows\CurrentVersion\Run" ...
```

Sysmon recorded the corresponding registry path under the user's registry hive (`HKU\<SID>`):

```text
HKU\S-1-5-21-1881654287-3370865730-3194882591-1001\
Software\Microsoft\Windows\CurrentVersion\Run\HomeSOCLab
```

This behavior is expected because `HKCU` (HKEY_CURRENT_USER) is an alias that maps to the currently logged-on user's registry hive under `HKEY_USERS (HKU)`.

---

### Event ID 1 Analysis

Sysmon also generated Event ID 1 because the registry modification was performed using `reg.exe`.

The process creation event confirmed:

- Executable: `C:\Windows\System32\reg.exe`
- Parent Process: `cmd.exe`
- Executing User: `DESKTOP-MMAR6A0\ajayd`
- Command Line: `reg add ...`

This event identifies **which process performed the registry modification**, while Event ID 13 identifies **what registry value was modified**.

---

### Screenshots

- step65-eventviewer-registry-event13.png
- step66-eventviewer-registry-details.png

---

### Initial Observations

- Sysmon successfully recorded the registry value modification.
- The registry value was written by `reg.exe`.
- The persistence entry points to `notepad.exe`.
- The registry modification was performed by the user `DESKTOP-MMAR6A0\ajayd`.

---

## 4.3 Splunk Evidence

### Search Queries

```spl
source="WinEventLog:Microsoft-Windows-Sysmon/Operational"
EventCode=13
TargetObject="*CurrentVersion\\Run\\HomeSOCLab"
```

---

```spl
source="WinEventLog:Microsoft-Windows-Sysmon/Operational"
(EventCode=12 OR EventCode=13)
| table _time EventCode TargetObject Details Image User
```

---

### Search Results

Splunk successfully indexed the Sysmon registry modification event.

The event contained the registry path, process responsible for the modification, configured autorun value, and executing user.

The search confirmed that the registry persistence mechanism was successfully recorded and could be investigated centrally through Splunk.

---

### Screenshots

- step67-splunk-registry-events.png
- step68-splunk-registry-details.png

---

### Initial Observations

- Splunk successfully ingested Sysmon Event ID 13.
- Registry persistence was visible through centralized telemetry.
- The modified registry value and associated process were available for investigation.
- The event provided sufficient context to identify the persistence mechanism.

---

# 5. Evidence Summary

| Source | Evidence | Value |
|--------|----------|-------|
| Command Prompt | `reg add` | Created registry persistence |
| Registry | Run Key | Autorun configuration |
| Sysmon Event ID 13 | Registry Value Set | Captured persistence creation |
| Sysmon Event ID 1 | Process Creation | Recorded `reg.exe` execution |
| Splunk | Indexed Sysmon Events | Centralized investigation |


# 5. Investigation

---

## 5.1 Telemetry Analysis

### Registry Persistence Creation

The investigation confirmed that a new registry value was created within the current user's **Run** registry key using the Windows Registry Console Tool (`reg.exe`).

The registry modification was performed with the following command:

```cmd
reg add "HKCU\Software\Microsoft\Windows\CurrentVersion\Run" /v HomeSOCLab /t REG_SZ /d "C:\Windows\System32\notepad.exe" /f
```

The command successfully created a persistence entry named **HomeSOCLab** that points to **Notepad**.

---

### Sysmon Event ID 13

Sysmon generated **Event ID 13 (Registry Value Set)** immediately after the registry modification.

Key telemetry observed during the investigation:

| Field | Value |
|--------|-------|
| Event ID | 13 |
| Operation | Registry Value Set |
| Image | `C:\Windows\System32\reg.exe` |
| Target Object | `HKU\S-1-5-21-1881654287-3370865730-3194882591-1001\Software\Microsoft\Windows\CurrentVersion\Run\HomeSOCLab` |
| Registry Value | `C:\Windows\System32\notepad.exe` |
| User | `DESKTOP-MMAR6A0\ajayd` |

The event confirmed that `reg.exe` modified the user's Run key by creating a new autorun value.

---

### Sysmon Event ID 1

Sysmon also generated **Event ID 1 (Process Creation)** because `reg.exe` executed to perform the registry modification.

The process creation event provided additional context including:

- Executable image
- Parent process (`cmd.exe`)
- Command line
- Executing user
- Integrity level

Together, Event ID 1 and Event ID 13 provide a complete picture of the persistence activity.

---

### Splunk Enterprise

Splunk successfully indexed the registry modification event.

Analysts were able to identify:

- The modified registry key
- The executable configured for autorun
- The process responsible for the change
- The executing user
- The event timestamp

This demonstrates how centralized logging simplifies persistence investigations.

---

## 5.2 Event Correlation

The persistence activity was reconstructed by correlating endpoint telemetry collected from Sysmon and Splunk.

### Step 1 — Registry Modification

The user executed `reg.exe` to add a new value within the Windows Run registry key.

---

### Step 2 — Process Creation

Sysmon Event ID 1 recorded the execution of `reg.exe`.

The process metadata identified:

- Parent Process: `cmd.exe`
- Executable: `reg.exe`
- User: `DESKTOP-MMAR6A0\ajayd`

---

### Step 3 — Registry Value Set

Immediately after process execution, Sysmon generated Event ID 13.

The event confirmed:

- Registry value created
- Registry path modified
- Autorun executable configured

---

### Step 4 — Centralized Logging

Splunk ingested both Sysmon events, allowing analysts to correlate:

- Process execution
- Registry modification
- User context
- Persistence mechanism

without requiring direct access to the endpoint.

---

## 5.3 Root Cause Analysis

### Root Cause

The investigation determined that the registry modification was intentionally performed as part of an authorized Home SOC Lab exercise.

The persistence mechanism used the Windows Run registry key to configure Notepad to launch automatically when the user logs on.

---

### Why This Is Considered Persistence

Windows automatically processes registry values located under:

```text
HKCU\Software\Microsoft\Windows\CurrentVersion\Run
```

during user logon.

Any executable configured within this location will automatically start whenever the associated user signs in.

Although this investigation used `notepad.exe`, attackers frequently abuse the same mechanism to execute malicious payloads while maintaining access to compromised systems.

---

### Why Sysmon Detected It

Sysmon monitors registry modifications based on its configuration.

When `reg.exe` created the new registry value, Sysmon generated Event ID 13 and recorded:

- Registry path
- Registry value
- Executing process
- Executing user
- Timestamp

This telemetry enables defenders to detect unauthorized persistence mechanisms.

---

### Defensive Control Assessment

The deployed Sysmon configuration successfully detected registry persistence and forwarded the telemetry to Splunk Enterprise.

The investigation demonstrated that registry monitoring provides valuable visibility into persistence techniques that may not be captured by standard Windows Security auditing.

---

### Investigation Conclusion

The registry Run Key persistence was successfully created, detected by Sysmon Event ID 13, and centralized within Splunk Enterprise.

The investigation confirmed that endpoint telemetry provided sufficient visibility to identify the persistence mechanism, attribute the registry modification to the responsible process, and reconstruct the sequence of events.

The activity was classified as **Authorized Security Testing** performed within the Home SOC Lab.

# 6. Timeline Reconstruction

| Time | Activity | Telemetry Source | Evidence |
|------|----------|------------------|----------|
| Replace with actual time | `reg.exe` executed | Sysmon Event ID 1 | Process Creation |
| Replace with actual time | Registry value created | Sysmon Event ID 13 | Registry Value Set |
| Replace with actual time | Splunk indexed Event ID 13 | Splunk Enterprise | Registry Telemetry |
| Replace with actual time | Registry value verified | Command Prompt | `reg query` Output |

> Replace the timestamps with the actual event times observed in Sysmon or Splunk.

---

# 7. Findings

## Confirmed Findings

### Finding 1 — Registry Persistence Successfully Created

A new registry value named **HomeSOCLab** was successfully created within the current user's Run registry key.

---

### Finding 2 — Persistence Mechanism Detected

Sysmon Event ID 13 successfully detected the registry value modification and recorded the persistence mechanism.

---

### Finding 3 — Responsible Process Identified

Sysmon Event ID 1 confirmed that `reg.exe` performed the registry modification.

---

### Finding 4 — Executing User Confirmed

The persistence mechanism was created by:

`DESKTOP-MMAR6A0\ajayd`

---

### Finding 5 — Centralized Visibility Confirmed

Splunk successfully ingested the registry modification event and made it available for investigation.

---

## Supporting Evidence

| Finding | Supporting Evidence |
|----------|---------------------|
| Registry Persistence | Registry Query Output |
| Registry Modification | Sysmon Event ID 13 |
| Responsible Process | Sysmon Event ID 1 |
| User Attribution | User Field |
| Centralized Logging | Splunk Search Results |

---

# 8. Detection & Telemetry Coverage

| Telemetry Source | Coverage | Notes |
|------------------|----------|------|
| Windows Security Log | ⚠️ Limited | Does not provide detailed registry modification telemetry. |
| Sysmon Event ID 1 | ✅ High | Identified the process (`reg.exe`) responsible for the change. |
| Sysmon Event ID 13 | ✅ High | Recorded the modified registry value and target registry path. |
| Splunk Enterprise | ✅ High | Successfully centralized registry persistence telemetry for investigation. |

---

# 9. Detection Opportunities

## Detection Logic

Generate alerts when:

- Registry Run Keys are modified.
- New autorun registry values are created.
- `reg.exe` modifies persistence locations.
- PowerShell modifies autorun registry keys.
- Registry modifications occur from unusual parent processes.

---

## Primary Telemetry

- Sysmon Event ID 13
- Sysmon Event ID 1

---

## Supporting Telemetry

- Windows Security Log
- PowerShell Operational Log
- Windows Defender
- Registry Auditing (if enabled)

---

## Detection Strategy

Correlate:

- Process creation
- Registry modification
- Parent process
- Executing user
- Registry path
- Registry value

to identify unauthorized persistence mechanisms.

---

## Detection Improvements

Future improvements include:

- Monitor Startup Folder persistence.
- Detect Scheduled Task persistence.
- Detect WMI Event Subscription persistence.
- Detect Registry RunOnce modifications.
- Correlate persistence with prior authentication and command execution events.

---

# 10. Analyst Reflection

This investigation demonstrated how registry-based persistence can be identified using endpoint telemetry.

By correlating Sysmon Event ID 1 with Event ID 13, it was possible to determine both the process responsible for the modification and the registry value that established persistence.

The investigation also highlighted the importance of understanding common Windows autorun mechanisms, as attackers frequently abuse legitimate operating system features to maintain access after compromising an endpoint.

Centralized visibility through Splunk simplified event correlation and provided sufficient context to accurately classify the activity as authorized security testing.

---

# 11. Lessons Learned

- Registry Run Keys are a common Windows persistence mechanism.
- Sysmon Event ID 13 provides detailed registry modification telemetry.
- Event ID 1 identifies the process responsible for the change.
- Correlating process execution with registry modifications improves investigation accuracy.
- Registry monitoring significantly enhances endpoint visibility.
- Persistence should always be investigated alongside authentication and execution telemetry.

---

# 12. References

## Documentation References

- Microsoft Registry Documentation
- Sysinternals Sysmon Documentation
- Splunk Documentation
- MITRE ATT&CK Framework

---

## Investigation Evidence

- Command Prompt
- Registry Query Output
- Sysmon Event ID 1
- Sysmon Event ID 13
- Event Viewer
- Splunk Search Results
- Project Screenshots

---

## Related Project Documents

- `mitre-mapping/persistence-registry-runkey-mitre.md`
- `splunk-detections/persistence-registry-runkey-detection.md`
- `incident-report/persistence-registry-runkey-incident-report.md`
- `playbooks/persistence-registry-runkey-playbook.md`

---

## Investigation Conclusion

The Registry Run Key persistence technique was successfully simulated, detected, and investigated using Sysmon and Splunk.

The investigation confirmed that Sysmon Event ID 13 provides detailed visibility into registry modifications while Event ID 1 identifies the process responsible for creating persistence.

By correlating these events, analysts can accurately identify, attribute, and investigate registry-based persistence techniques commonly used by attackers.

The activity was confirmed as **Authorized Security Testing** within the Home SOC Lab.

---

Template Version: 2.0

Last Updated: 2026-07-11

Author: Ajaydev S