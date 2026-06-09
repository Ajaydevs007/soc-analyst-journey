# Sysmon Fundamentals

## What is Sysmon?

Sysmon (System Monitor) is a Microsoft Sysinternals tool that provides detailed visibility into system activity.

Unlike native Windows logs, Sysmon generates rich telemetry that helps security analysts detect malicious behavior and investigate incidents.

---

## Why Do We Need Sysmon?

Windows Security Logs provide limited information.

Sysmon enhances visibility by recording:

- Process Creation
- Network Connections
- File Creation
- Registry Changes
- Driver Loading
- Process Injection Activity
- DNS Queries
- File Hashes

Because of this, Sysmon is one of the most important data sources in a SOC environment.

---

## What Does Sysmon Record?

### Process Activity
- Process creation
- Parent-child relationships
- Command-line arguments

### Network Activity
- Outbound network connections
- Source and destination IP addresses
- Ports used

### File Activity
- File creation events
- File hashes

### Registry Activity
- Registry key modifications
- Persistence mechanisms

### DNS Activity
- DNS queries made by processes

### Process Injection
- Remote thread creation
- Process access events

---

## Important Sysmon Event IDs

| Event ID | Description |
|-----------|-------------|
| 1 | Process Creation |
| 3 | Network Connection |
| 7 | Image Loaded |
| 8 | Create Remote Thread |
| 10 | Process Access |
| 11 | File Create |
| 13 | Registry Value Set |
| 22 | DNS Query |

---

## Common SOC Use Cases

### Event ID 1 – Process Creation

Detect:

- PowerShell execution
- Command Prompt activity
- Malware execution
- Suspicious parent-child relationships

---

### Event ID 3 – Network Connection

Detect:

- Connections to malicious IPs
- Reverse shells
- Beaconing activity

---

### Event ID 7 – Image Loaded

Detect:

- Suspicious DLL loading
- DLL hijacking attempts

---

### Event ID 8 – Create Remote Thread

Detect:

- Process injection
- Malware techniques
- Credential dumping activity

---

### Event ID 10 – Process Access

Detect:

- LSASS access attempts
- Credential theft tools such as Mimikatz

---

### Event ID 11 – File Create

Detect:

- Malware dropping files
- Ransomware file activity

---

### Event ID 13 – Registry Value Set

Detect:

- Persistence mechanisms
- Registry modifications by malware

---

### Event ID 22 – DNS Query

Detect:

- Connections to malicious domains
- Command-and-Control (C2) communication

---

## Why SOC Analysts Love Sysmon

Sysmon provides:

- More visibility than default Windows logs
- Better incident investigation
- Improved threat hunting
- Detection of attacker techniques
- Rich telemetry for SIEM tools

---

## Sysmon Log Location

```text
Event Viewer
└── Applications and Services Logs
    └── Microsoft
        └── Windows
            └── Sysmon
                └── Operational
```

---

## Key Takeaway

Windows Logs tell you **that something happened**.

Sysmon tells you **exactly how it happened**.

Because of this, Sysmon is one of the most valuable log sources for SOC analysts.
