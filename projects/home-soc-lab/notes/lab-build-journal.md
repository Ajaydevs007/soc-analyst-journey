# Home SOC Lab Build Journal

This document records the step-by-step construction, configuration, validation, and troubleshooting performed while building the Home SOC Lab.

It serves as engineering documentation for the lab infrastructure and telemetry pipeline before attack simulations and SOC investigations were performed.

---

# Lab Architecture

## Components

- Kali Linux (Attacker)
- Windows 10 (Victim Endpoint)
- Splunk Enterprise (SIEM)
- Splunk Universal Forwarder
- Sysmon (SwiftOnSecurity Configuration)
- VirtualBox

---

## Network Design

### Adapter 1

- NAT
- Internet access

### Adapter 2

- Internal Network
- Network Name:

```
soc-lab-net
```

---

## Static IP Addresses

| Device | Interface | IP Address |
|----------|-----------|------------|
| Kali Linux | eth1 | 10.10.10.10 |
| Windows 10 | Ethernet 2 | 10.10.10.20 |

---

# Day 1 — Lab Setup

---

# Step 1 — Network Connectivity Validation

## Configuration

Configured VirtualBox networking:

- Adapter 1 → NAT
- Adapter 2 → Internal Network (`soc-lab-net`)

Assigned static IP addresses:

- Kali Linux: `10.10.10.10`
- Windows 10: `10.10.10.20`

---

## Validation

Connectivity tests performed:

- Kali → Windows
- Windows → Kali

Result:

- Successful communication
- 0% packet loss

---

## Observation

Both virtual machines received the address `10.0.2.15` on their NAT adapters.

This is expected because VirtualBox creates isolated NAT networks for each virtual machine.

---

## Conclusion

The dedicated attack network (`10.10.10.0/24`) was functioning correctly and ready for attack simulation.

---

# Step 2 — Windows Audit Policy Configuration

Configured Advanced Audit Policy using Local Group Policy.

Enabled:

- Audit Logon (Success + Failure)
- Audit Process Creation (Success)
- Audit User Account Management (Success + Failure)
- Include command line in process creation events

Applied changes:

```cmd
gpupdate /force
```

---

## Purpose

Prepare Windows to generate authentication and process execution telemetry required for SOC investigations.

---

## Validation

Verified audit policy was successfully applied.

---

# Step 3 — Native Windows Process Creation Verification

Executed:

- whoami
- hostname
- ipconfig

Verified:

Windows Logs

```
Security
```

Observed:

- Event ID 4688
- Process Name
- Command Line
- User Information

---

## Conclusion

Native Windows Security auditing was functioning correctly.

---

# Step 4 — Sysmon Installation

Installed Sysmon using the SwiftOnSecurity configuration.

Verified:

- Sysmon service running
- Sysmon Operational log created
- Events generated successfully

---

## Validation

Confirmed Sysmon Event ID 1.

Observed fields:

- Image
- CommandLine
- ProcessId
- User

Example:

Image

```
C:\Windows\System32\mmc.exe
```

Command Line

```
"C:\Windows\system32\mmc.exe"
"C:\Windows\system32\eventvwr.msc"
```

---

## Purpose

Enhance endpoint telemetry beyond native Windows logging.

---

# Step 5 — Sysmon Configuration Validation

Validated the SwiftOnSecurity Sysmon configuration.

Observed telemetry:

- Event ID 1 → Process Creation
- Event ID 3 → Network Connection
- Event ID 13 → Registry Modification
- Event ID 22 → DNS Query

---

## Observation

Not every activity generated Sysmon events.

This is expected because the SwiftOnSecurity configuration intentionally uses include/exclude rules to reduce telemetry noise.

---

## Lesson Learned

Missing telemetry does not necessarily indicate a logging failure.

Always review the Sysmon configuration before modifying logging rules.

---

# Step 6A — Splunk Receiving Configuration

Configured Splunk Enterprise as the receiving indexer.

Enabled:

TCP Receiving

```
Port 9997
```

---

## Validation

Verified using:

```cmd
netstat -ano | findstr 9997
```

Observed:

Splunk listening successfully on TCP port 9997.

---

# Step 6B — Splunk Universal Forwarder Installation

Installed Splunk Universal Forwarder on the Windows endpoint.

Configured forwarding destination:

```
192.168.29.237:9997
```

Verified:

- SplunkForwarder service running
- Successful network connectivity

---

## Purpose

Forward Windows telemetry to Splunk Enterprise.

---

# Step 6C — Universal Forwarder Inputs Configuration

Configured Windows Event Log collection:

- Application
- Security
- System
- Microsoft-Windows-Sysmon/Operational

Restarted:

```
SplunkForwarder
```

---

## Observation

Windows Event Log inputs do not appear in:

```
splunk list monitor
```

Validation instead performed using:

```
splunk list inputstatus
```

Observed:

```
splunk-winevtlog.exe
```

actively reading Windows Event Logs.

---

## Lesson Learned

Windows Event Log inputs are handled differently from traditional file monitors.

---

# Step 6D — Telemetry Ingestion Verification

Validated end-to-end telemetry collection.

Successfully observed:

- WinEventLog:Application
- WinEventLog:Security
- WinEventLog:System
- WinEventLog:Microsoft-Windows-Sysmon/Operational

Verified Windows Event Viewer events appeared inside Splunk Enterprise.

---

## Conclusion

The telemetry pipeline was fully operational.

---

# Troubleshooting Highlights

Several issues were encountered while building the lab.

Rather than documenting each issue here, detailed write-ups are available within the project knowledge base.

Examples include:

- Sysmon log forwarding issues
- Windows Firewall blocking SMB
- SMB authentication validation
- Remote UAC token filtering
- EventCode ambiguity across Windows logs
- PsExec troubleshooting
- Splunk Universal Forwarder troubleshooting

See:

```
knowledge-base/troubleshooting/
```

---

# Lessons Learned

Building a SOC lab requires more than installing security tools.

Reliable telemetry depends on:

- Correct network configuration
- Proper Windows Audit Policy
- Endpoint telemetry collection
- SIEM ingestion validation
- Understanding Sysmon filtering
- Structured troubleshooting
- Continuous validation after configuration changes

---

# Lab Build Complete

The Home SOC Lab was successfully configured with:

- Windows 10 Endpoint
- Kali Linux Attacker
- Splunk Enterprise
- Splunk Universal Forwarder
- Sysmon
- Windows Security Auditing
- VirtualBox Internal Network

The environment was validated before beginning attack simulation and SOC investigations.

---

# Related Documentation

## Investigation Documents

```
projects/home-soc-lab/logs-analysis/
```

## MITRE ATT&CK Mapping

```
projects/home-soc-lab/mitre-mapping/
```

## Detection Rules

```
projects/home-soc-lab/splunk-detections/
```

## Incident Reports

```
projects/home-soc-lab/incident-report/
```

## SOC Playbooks

```
projects/home-soc-lab/playbooks/
```

## Troubleshooting Knowledge Base

```
knowledge-base/troubleshooting/
```

---

**Lab Status:** ✅ Operational

**Project:** Home SOC Lab

**Author:** Ajaydev S