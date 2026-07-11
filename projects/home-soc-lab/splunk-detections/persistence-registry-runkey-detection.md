# Registry Run Key Persistence Detection Rule

---

# Detection Information

| Field | Value |
|--------|-------|
| Detection Name | Windows Registry Run Key Persistence |
| Detection ID | DET-005 |
| Severity | High |
| Status | Active |
| Author | Ajaydev S |

---

# Objective

Detect registry modifications that establish persistence by creating or modifying Windows Run or RunOnce registry keys.

The detection focuses on identifying unauthorized autorun entries commonly abused by attackers to maintain persistence after gaining access to a Windows system.

---

# MITRE ATT&CK Mapping

| Field | Value |
|--------|-------|
| Tactic | Persistence |
| Technique | T1547.001 – Registry Run Keys / Startup Folder |

---

# Data Sources

## Primary Telemetry

- Sysmon Event ID 13 (Registry Value Set)
- Sysmon Event ID 1 (Process Creation)

---

## Supporting Telemetry

- Windows Security Log
- PowerShell Operational Log
- Windows Defender
- Registry Auditing (if enabled)

---

# Detection Logic

Generate an alert when:

- A new value is created under:
  - `HKCU\Software\Microsoft\Windows\CurrentVersion\Run`
  - `HKLM\Software\Microsoft\Windows\CurrentVersion\Run`
  - `RunOnce`
- `reg.exe` modifies autorun locations.
- PowerShell modifies registry persistence locations.
- Unknown executables are configured to launch automatically.

---

# Splunk Searches

## Registry Value Set

```spl
source="WinEventLog:Microsoft-Windows-Sysmon/Operational"
EventCode=13
TargetObject="*CurrentVersion\\Run*"
| table _time User Image TargetObject Details
```

---

## Registry Process Creation

```spl
source="WinEventLog:Microsoft-Windows-Sysmon/Operational"
EventCode=1
Image="*reg.exe"
| table _time User CommandLine ParentImage
```

---

## Combined Persistence Hunt

```spl
source="WinEventLog:Microsoft-Windows-Sysmon/Operational"
(EventCode=1 OR EventCode=13)
| table _time EventCode Image TargetObject CommandLine User
```

---

# Expected Alert

## Alert Title

```
Windows Registry Run Key Persistence Detected
```

---

## Alert Description

```
A registry Run or RunOnce key has been modified.

Review the process responsible for the change, the configured executable, the executing user, and determine whether the persistence mechanism is authorized.
```

---

# Alert Metadata

| Field | Value |
|--------|-------|
| Severity | High |
| Confidence | High |
| ATT&CK Technique | T1547.001 |
| Trigger | Sysmon Event ID 13 |
| Response | Investigate Registry Persistence |

---

# Investigation Steps

1. Review Sysmon Event ID 13.
2. Identify the modified registry path.
3. Review the configured executable.
4. Correlate with Sysmon Event ID 1.
5. Verify the executing process (`reg.exe`, PowerShell, etc.).
6. Identify the executing user.
7. Determine whether the persistence entry is expected.
8. Investigate follow-on activity.

---

# False Positives

Registry Run Keys may be modified during:

- Software installation
- Software updates
- OneDrive configuration
- Microsoft Edge autorun configuration
- Enterprise endpoint management
- Authorized administrative activity

Investigate the executable path and user context before escalating.

---

# Tuning Recommendations

- Baseline common autorun entries.
- Alert only on new or modified values.
- Suppress known Microsoft and enterprise software entries.
- Alert on executables outside trusted directories.
- Correlate with recent authentication and process creation events.

---

# Detection Validation

## Lab Validation

- Created a registry value:
  - `HomeSOCLab`
- Registry path:
  - `HKCU\Software\Microsoft\Windows\CurrentVersion\Run`
- Sysmon generated Event ID 13.
- Sysmon generated Event ID 1 (`reg.exe`).
- Splunk successfully indexed both events.

---

# Related Documents

- `logs-analysis/persistence-detection-analysis.md`
- `mitre-mapping/persistence-registry-runkey-mitre.md`
- `incident-report/persistence-registry-runkey-incident-report.md`
- `playbooks/persistence-registry-runkey-playbook.md`

---

Template Version: 2.0

Last Updated: 2026-07-11

Author: Ajaydev S