# 🛡️ Home SOC Lab

<div align="center">

![Platform](https://img.shields.io/badge/Platform-Windows%2010-blue)
![SIEM](https://img.shields.io/badge/SIEM-Splunk%20Enterprise-orange)
![Telemetry](https://img.shields.io/badge/Telemetry-Sysmon-green)
![Attacker](https://img.shields.io/badge/Attacker-Kali%20Linux-red)
![Framework](https://img.shields.io/badge/MITRE-ATT%26CK-purple)
![Status](https://img.shields.io/badge/Project-Completed-success)
![License](https://img.shields.io/badge/License-MIT-lightgrey)

*A self-built Security Operations Center (SOC) lab demonstrating attack simulation, endpoint telemetry collection, SIEM monitoring, threat detection, and incident response using Windows, Sysmon, Splunk Enterprise, and the MITRE ATT&CK framework.*

</div>

---

# 📖 Project Overview

The **Home SOC Lab** is a practical Blue Team project built to simulate real-world cyber attacks and investigate them using enterprise SOC methodologies.

The lab was designed from the ground up using Windows 10, Kali Linux, Splunk Enterprise, Splunk Universal Forwarder, and Sysmon to generate, collect, and analyze endpoint telemetry. Each simulated attack was investigated using a structured workflow that included evidence collection, log analysis, MITRE ATT&CK mapping, detection engineering, incident reporting, and SOC playbook development.

Rather than focusing solely on attack execution, this project emphasizes **how defenders detect, investigate, document, and respond to security events** using real Windows logs and SIEM data.

---

# 🎯 Project Objectives

The primary goals of this project were to:

- Build a fully functional Home SOC Lab from scratch.
- Configure Windows endpoint auditing and Sysmon telemetry.
- Forward Windows Event Logs to Splunk Enterprise using the Splunk Universal Forwarder.
- Simulate realistic attack scenarios within a controlled lab environment.
- Investigate endpoint and authentication events using Splunk and Event Viewer.
- Correlate telemetry across multiple Windows log sources.
- Develop practical Splunk detection rules.
- Map attacker behavior to the MITRE ATT&CK framework.
- Produce professional investigation reports, incident reports, and SOC playbooks.
- Build a cybersecurity portfolio demonstrating practical SOC analyst skills.

---

# ⭐ Key Features

- 🛡️ Windows Endpoint Monitoring with Sysmon
- 📊 Splunk Enterprise SIEM Integration
- 🔍 Windows Security Event Analysis
- 📡 Centralized Log Collection
- 🧩 MITRE ATT&CK Mapping
- 🚨 Detection Engineering with Splunk SPL
- 📄 Incident Reporting
- 📘 SOC Investigation Playbooks
- 📂 Structured Documentation
- 🧠 Troubleshooting Knowledge Base

---

# 🏗️ Lab Environment

| Component | Technology |
|-----------|------------|
| Attacker Machine | Kali Linux |
| Target Machine | Windows 10 |
| SIEM | Splunk Enterprise |
| Log Forwarder | Splunk Universal Forwarder |
| Endpoint Telemetry | Sysmon (SwiftOnSecurity Configuration) |
| Virtualization | Oracle VirtualBox |
| Network | Internal Network + NAT |
| Investigation Tools | Event Viewer, Splunk Search, Command Prompt, PowerShell |

---

# 📌 Project Scope

This project focuses on the complete lifecycle of detecting and investigating Windows-based attacks, including:

- Reconnaissance Detection
- Password Guessing (Brute Force)
- Successful Authentication Analysis
- Command Execution Detection
- Registry-Based Persistence Detection

Each scenario is documented using a standardized SOC investigation methodology supported by real telemetry collected from the lab environment.

---

# 📈 Skills Demonstrated

### Security Monitoring

- Windows Event Analysis
- Sysmon Telemetry Analysis
- SIEM Monitoring
- Log Correlation
- Event Investigation

### Detection Engineering

- Splunk SPL Queries
- Detection Rule Development
- Alert Validation
- Telemetry Correlation

### Incident Response

- Investigation Workflow
- Evidence Collection
- Timeline Reconstruction
- Root Cause Analysis
- Incident Reporting

### Threat Intelligence

- MITRE ATT&CK Mapping
- Attack Lifecycle Analysis
- Threat Detection
- TTP Identification

### Documentation

- Investigation Reports
- Detection Rules
- Incident Reports
- SOC Playbooks
- Technical Documentation
- Troubleshooting Guides


---

# 🏛️ Home SOC Lab Architecture

```text
                              Internet
                                  │
                        ┌─────────┴─────────┐
                        │    NAT Network    │
                        └─────────┬─────────┘
                                  │
          ┌───────────────────────┴───────────────────────┐
          │                                               │
 ┌──────────────────────┐                     ┌────────────────────────┐
 │     Kali Linux       │                     │      Windows 10        │
 │----------------------│                     │------------------------│
 │ Role: Attacker       │◄──────────────────►│ Role: Victim Endpoint   │
 │ IP: 10.10.10.10      │  Internal Network  │ IP: 10.10.10.20         │
 │ Tools:               │    10.10.10.0/24   │ Sysmon                  │
 │ • Nmap               │                     │ Windows Security Logs   │
 │ • Hydra              │                     │ Event Viewer            │
 │ • SMBClient          │                     │ Splunk UF               │
 │ • Impacket           │                     │                         │
 └──────────────────────┘                     └──────────────┬─────────┘
                                                            │
                                                            │
                                                            ▼
                                              ┌────────────────────────┐
                                              │ Splunk Universal       │
                                              │ Forwarder              │
                                              └──────────────┬─────────┘
                                                             │
                                                             │ TCP 9997
                                                             ▼
                                              ┌────────────────────────┐
                                              │ Splunk Enterprise SIEM │
                                              │                        │
                                              │ Search & Investigation │
                                              │ Detection Engineering  │
                                              │ Incident Response      │
                                              └────────────────────────┘
```

---

# 🌐 Network Topology

| Device | Interface | IP Address | Purpose |
|----------|-----------|------------|---------|
| Kali Linux | eth0 | DHCP (NAT) | Internet Access |
| Kali Linux | eth1 | 10.10.10.10 | Attack Network |
| Windows 10 | Ethernet | DHCP (NAT) | Internet Access |
| Windows 10 | Ethernet 2 | 10.10.10.20 | Victim Endpoint |
| Splunk Enterprise | Host Machine | Local Network | SIEM |

---

# 🔄 Telemetry Flow

```text
Attacker Activity
        │
        ▼
Windows Endpoint
        │
        ▼
Windows Event Logs
        │
        ├──────────────┐
        │              │
        ▼              ▼
 Security Logs     Sysmon Logs
        │              │
        └──────┬───────┘
               ▼
Splunk Universal Forwarder
               │
               ▼
      Splunk Enterprise
               │
               ▼
 Investigation & Detection
```

---

# ⚔️ Attack Lifecycle

The following attack chain was simulated and investigated during this project.

```text
Reconnaissance
       │
       ▼
Password Guessing
       │
       ▼
Successful Authentication
       │
       ▼
Command Execution
       │
       ▼
Registry Persistence
```

---

# 🔍 Investigation Workflow

Every attack followed the same investigation methodology.

```text
Attack Simulation
        │
        ▼
Telemetry Generation
        │
        ▼
Event Collection
        │
        ▼
Splunk Investigation
        │
        ▼
Evidence Collection
        │
        ▼
MITRE ATT&CK Mapping
        │
        ▼
Detection Rule Creation
        │
        ▼
Incident Report
        │
        ▼
SOC Playbook
```

---

# 📡 Telemetry Sources

The Home SOC Lab combines multiple telemetry sources to provide visibility into endpoint activity.

| Source | Purpose |
|---------|----------|
| Windows Security Logs | Authentication, Account Management, Process Creation |
| Sysmon | Process, Network, Registry, DNS, File Activity |
| Event Viewer | Local Event Validation |
| Splunk Enterprise | Centralized Log Analysis |
| Splunk Universal Forwarder | Log Collection & Forwarding |

---

# 🧩 Attack Simulation Workflow

Each attack simulation followed a structured workflow to ensure repeatability and accurate telemetry collection.

```text
Generate Attack
       │
       ▼
Validate Windows Logs
       │
       ▼
Validate Sysmon Events
       │
       ▼
Validate Splunk Ingestion
       │
       ▼
Investigate Evidence
       │
       ▼
Document Findings
       │
       ▼
Create Detection Rule
       │
       ▼
Write Incident Report
       │
       ▼
Develop SOC Playbook
```

---

# 🛠️ Technologies Used

### Operating Systems

- Windows 10
- Kali Linux

### SIEM & Monitoring

- Splunk Enterprise
- Splunk Universal Forwarder
- Sysmon

### Offensive Tools

- Nmap
- Hydra
- Impacket
- SMBClient

### Windows Tools

- Event Viewer
- Command Prompt
- PowerShell
- Registry Editor
- Local Group Policy

### Virtualization

- Oracle VirtualBox

### Documentation

- GitHub
- Markdown

---

# 📂 Repository Structure

```text
home-soc-lab/
│
├── README.md
│
├── notes/
│   └── lab-build-journal.md
│
├── screenshots/
│   ├── case-study-1/
│   ├── case-study-2/
│   ├── case-study-3/
│   ├── case-study-4/
│   └── case-study-5/
│
├── templates/
│   ├── investigation-template.md
│   ├── mitre-template.md
│   ├── detection-template.md
│   ├── incident-report-template.md
│   └── playbook-template.md
│
├── knowledge-base/
│   └── troubleshooting/
│
├── logs-analysis/
├── mitre-mapping/
├── splunk-detections/
├── incident-report/
└── playbooks/
```

---

# 📚 Case Studies

The Home SOC Lab consists of five practical SOC investigations covering different phases of the attack lifecycle.

| Case Study | Scenario | Primary Logs | MITRE ATT&CK | Status |
|------------|----------|--------------|--------------|--------|
| 01 | Network Reconnaissance Detection | Sysmon Event ID 3 | T1046 | ✅ Complete |
| 02 | Password Guessing (Hydra SMB) | Windows Event ID 4625 | T1110.001 | ✅ Complete |
| 03 | Successful SMB Authentication | Windows Event ID 4624 | T1078 | ✅ Complete |
| 04 | Command Execution Analysis | Sysmon Event ID 1 | T1059 | ✅ Complete |
| 05 | Registry Run Key Persistence | Sysmon Event ID 13 | T1547.001 | ✅ Complete |

---

# 🎯 MITRE ATT&CK Coverage

The simulated attacks were mapped to the MITRE ATT&CK Enterprise Framework.

| Tactic | Technique ID | Technique |
|---------|--------------|-----------|
| Reconnaissance | T1046 | Network Service Discovery |
| Credential Access | T1110.001 | Password Guessing |
| Initial Access | T1078 | Valid Accounts |
| Execution | T1059 | Command and Scripting Interpreter |
| Persistence | T1547.001 | Registry Run Keys / Startup Folder |

---

# 📊 Detection Coverage

During this project, multiple Windows telemetry sources were analyzed and correlated.

| Log Source | Event IDs | Purpose |
|------------|-----------|---------|
| Windows Security | 4624 | Successful Authentication |
| Windows Security | 4625 | Failed Authentication |
| Windows Security | 4688 | Native Process Creation |
| Sysmon | 1 | Process Creation |
| Sysmon | 3 | Network Connections |
| Sysmon | 13 | Registry Value Set |
| Sysmon | 22 | DNS Query |

---

# 📄 Documentation Produced

Each case study includes five professional SOC documents.

| Document | Purpose |
|----------|---------|
| Investigation Report | Detailed technical investigation |
| MITRE ATT&CK Mapping | Technique and tactic mapping |
| Splunk Detection Rule | Detection engineering and SPL queries |
| Incident Report | SOC incident documentation |
| Investigation Playbook | Repeatable analyst workflow |

### Project Totals

| Documentation | Count |
|---------------|------:|
| Investigation Reports | 5 |
| MITRE ATT&CK Mappings | 5 |
| Detection Rules | 5 |
| Incident Reports | 5 |
| SOC Playbooks | 5 |
| **Total Documents** | **25** |

---

# 🛠 Skills Demonstrated

## Security Operations

- Security Event Monitoring
- Windows Log Analysis
- Endpoint Telemetry Analysis
- Threat Detection
- Incident Investigation
- Event Correlation

---

## SIEM

- Splunk Enterprise
- Splunk Search Processing Language (SPL)
- Log Correlation
- Search Optimization
- Detection Validation

---

## Endpoint Security

- Sysmon Configuration
- Windows Security Auditing
- Registry Monitoring
- Process Monitoring
- Authentication Monitoring

---

## Detection Engineering

- Detection Rule Development
- MITRE ATT&CK Mapping
- Telemetry Correlation
- False Positive Analysis
- Detection Validation

---

## Incident Response

- Evidence Collection
- Timeline Reconstruction
- Root Cause Analysis
- Incident Reporting
- Playbook Development

---

## Windows Internals

- Windows Authentication
- NTLM Authentication
- Windows Registry
- Process Creation
- Event Viewer
- Local Security Policy

---

# 📚 Documentation Standards

Every investigation follows a standardized SOC documentation workflow.

```text
Attack Simulation
        │
        ▼
Evidence Collection
        │
        ▼
Log Analysis
        │
        ▼
MITRE ATT&CK Mapping
        │
        ▼
Detection Engineering
        │
        ▼
Incident Report
        │
        ▼
SOC Playbook
```

Each investigation contains:

- 📄 Investigation Report
- 🎯 MITRE ATT&CK Mapping
- 🚨 Detection Rule
- 📋 Incident Report
- 📘 SOC Investigation Playbook

This structure ensures that every investigation is consistent, repeatable, evidence-driven, and aligned with common SOC documentation practices.


---

# 📸 Project Screenshots

The following screenshots demonstrate the Home SOC Lab environment, telemetry collection, attack simulations, and investigation workflow.

## Lab Environment

- Windows 10 Endpoint
- Kali Linux Attacker
- Splunk Enterprise
- Sysmon Configuration
- Splunk Universal Forwarder

---

## Attack Simulations

### Case Study 1 — Reconnaissance

- Nmap TCP Connect Scan
- Splunk Detection
- Sysmon Network Events

---

### Case Study 2 — Password Guessing

- Hydra SMB Brute Force
- Windows Event ID 4625
- Splunk Authentication Analysis

---

### Case Study 3 — Successful Authentication

- Windows Event ID 4624
- NTLM Authentication
- Logon Type Analysis
- Splunk Correlation

---

### Case Study 4 — Command Execution

- Sysmon Event ID 1
- Process Creation
- Parent-Child Process Analysis
- Splunk Investigation

---

### Case Study 5 — Registry Persistence

- Registry Run Key Creation
- Sysmon Event ID 13
- Registry Analysis
- Splunk Detection

---

# 🎓 Key Learning Outcomes

This project provided practical experience across multiple Blue Team disciplines.

### SOC Operations

- Security Monitoring
- Alert Investigation
- Incident Documentation
- Threat Detection
- Event Correlation

---

### Windows Security

- Windows Security Event Logs
- Windows Authentication
- NTLM Authentication
- Process Creation Analysis
- Registry Monitoring

---

### Endpoint Monitoring

- Sysmon Configuration
- Process Monitoring
- Registry Monitoring
- Network Connection Monitoring
- DNS Query Monitoring

---

### SIEM

- Splunk Enterprise Administration
- Splunk SPL Queries
- Log Correlation
- Detection Validation
- Telemetry Investigation

---

### Detection Engineering

- MITRE ATT&CK Mapping
- Detection Rule Development
- False Positive Considerations
- Investigation Workflow
- Playbook Development

---

# 🚀 Future Roadmap

The Home SOC Lab will continue to expand with additional attack simulations and defensive use cases.

## Phase 2

- Active Directory Lab
- Domain Controller Deployment
- Kerberos Authentication
- Group Policy Analysis
- Sysmon Advanced Configuration

---

## Detection Engineering

- Sigma Rules
- Custom Splunk Correlation Searches
- Splunk Dashboards
- Risk-Based Alerting
- Detection Tuning

---

## Threat Hunting

- PowerShell Hunting
- LOLBin Detection
- Scheduled Task Persistence
- WMI Persistence
- Service Persistence
- Lateral Movement Detection

---

## Incident Response

- Malware Investigation
- Memory Forensics
- Timeline Analysis
- IOC Development
- Threat Hunting Scenarios

---

# 📚 References

The following resources were used while building and documenting this project.

### Microsoft

- Microsoft Learn
- Windows Event Documentation
- Windows Security Auditing

---

### Splunk

- Splunk Enterprise Documentation
- Splunk Search Reference
- Splunk Universal Forwarder Documentation

---

### Sysmon

- Microsoft Sysinternals
- Sysmon Documentation
- SwiftOnSecurity Sysmon Configuration

---

### MITRE

- MITRE ATT&CK Framework

---

### Community Resources

- TryHackMe
- LetsDefend
- PortSwigger Web Security Academy

---

# 👨‍💻 About Me

Hi, I'm **Ajaydev S**.

I'm an aspiring SOC Analyst with a strong interest in:

- Blue Team Operations
- Threat Detection
- Detection Engineering
- Incident Response
- Windows Security
- SIEM Technologies
- Threat Hunting

This repository documents my hands-on learning journey while building practical cybersecurity skills through self-hosted lab environments and structured investigations.

---

# 🤝 Feedback

Suggestions, improvements, and constructive feedback are always welcome.

If you have recommendations for improving the lab or documentation, feel free to open an issue or submit a pull request.

---

# ⭐ Support

If you found this project useful or interesting:

- ⭐ Star the repository
- 🍴 Fork the project
- 📢 Share it with others interested in Blue Teaming and SOC Operations

Your support helps motivate future improvements and additional case studies.

---

# 📄 License

This project is licensed under the **MIT License**.

You are welcome to use this repository for learning and educational purposes.

---

# 🏁 Project Status

## Phase 1

**Status:** ✅ Completed

### Completed Case Studies

- ✅ Reconnaissance Detection
- ✅ Password Guessing Detection
- ✅ Successful Authentication Analysis
- ✅ Command Execution Investigation
- ✅ Registry Persistence Detection

### Documentation Produced

- 📄 5 Investigation Reports
- 🎯 5 MITRE ATT&CK Mappings
- 🚨 5 Detection Rules
- 📋 5 Incident Reports
- 📘 5 SOC Investigation Playbooks

**Total:** **25 Professional SOC Documents**

---

<div align="center">

## ⭐ Thank you for visiting this repository!

**Built to learn. Documented to share. Improved through practice.**

</div>