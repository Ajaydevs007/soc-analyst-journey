# Detection Lab — Active Directory + Microsoft Sentinel

> **Project 1 of 7 — SOC Analyst Home Lab Series**

A hands-on SOC detection lab built to simulate an Active Directory environment, generate a controlled password-spray attack, collect Windows security telemetry, detect the attack using KQL, and generate a Microsoft Sentinel security incident.

The project focuses on the core SOC workflow:

**Build → Attack → Collect → Detect → Alert → Investigate → Document**

---

## 🎯 Project Objective

The goal of this project was to build a small enterprise-style security environment and demonstrate how a SOC analyst can detect authentication-based attacks using Microsoft Sentinel.

The project covers:

1. Build an Active Directory environment.
2. Configure a Windows Server Domain Controller.
3. Join a Windows 10 workstation to the domain.
4. Create lab users and organizational units.
5. Configure Windows security auditing using Group Policy.
6. Generate controlled authentication failures.
7. Simulate a Kerberos password-spray attack from Kali Linux.
8. Collect Windows Security events using Azure Monitor Agent.
9. Ingest security events into Microsoft Sentinel.
10. Develop a KQL detection for password spraying.
11. Create a Microsoft Sentinel Analytics Rule.
12. Generate and investigate the resulting Sentinel incident.

---

## ✅ Project Status

**Status: COMPLETE**

The main detection objective was successfully achieved.

The password-spray attack was simulated from Kali Linux, the resulting Kerberos authentication failures were collected in Microsoft Sentinel, a behavioral KQL detection was developed, and the detection successfully generated a Sentinel incident.

---

# 🏗️ Lab Architecture

```text
                         ┌──────────────────────┐
                         │      Kali Linux      │
                         │       Attacker       │
                         │                      │
                         │   Kerberos Password  │
                         │        Spray         │
                         └──────────┬───────────┘
                                    │
                                    │
                              soc-lab-net
                                    │
                    ┌───────────────┴───────────────┐
                    │                               │
                    ▼                               ▼
        ┌─────────────────────┐         ┌─────────────────────┐
        │  Windows Server VM  │         │    Windows 10 VM    │
        │                     │         │                     │
        │   Domain Controller │◄───────►│  Domain Workstation │
        │                     │         │                     │
        │   Active Directory  │         │    Domain Joined    │
        │   AD DS + DNS       │         │                     │
        └──────────┬──────────┘         └──────────┬──────────┘
                   │                               │
                   └───────────────┬───────────────┘
                                   │
                         Azure Monitor Agent
                                   │
                                   ▼
                    ┌──────────────────────────┐
                    │    Log Analytics         │
                    │       Workspace          │
                    └────────────┬─────────────┘
                                 │
                                 ▼
                    ┌──────────────────────────┐
                    │   Microsoft Sentinel     │
                    │                          │
                    │       KQL Detection      │
                    │             │             │
                    │      Analytics Rule      │
                    │             │             │
                    │         Incident         │
                    └──────────────────────────┘

🖥️ Lab Environment
Component	Purpose	Status
Kali Linux	Attack simulation	✅ Complete
Windows Server 2022	Domain Controller	✅ Complete
Windows 10	Domain workstation	✅ Complete
Active Directory	Identity and authentication	✅ Complete
DNS	Domain name resolution	✅ Complete
VirtualBox	Virtualization	✅ Complete
Microsoft Azure	Cloud infrastructure	✅ Complete
Log Analytics	Central log storage	✅ Complete
Azure Monitor Agent	Windows telemetry collection	✅ Complete
Microsoft Sentinel	SIEM and detection	✅ Complete
KQL	Detection engineering	✅ Complete
Sentinel Analytics Rule	Automated detection	✅ Complete
Sentinel Incident	Alert investigation	✅ Complete


🌐 Network Design
The lab uses an isolated internal VirtualBox network named:
soc-lab-net

The VMs use two network interfaces.
NAT
Used for:
- Internet connectivity
- Windows updates
- Azure connectivity
- Software installation
Internal Network — soc-lab-net
Used for:
- Kali → Domain Controller communication
- Kali → Windows 10 communication
- Windows Server → Windows 10 communication
- Isolated attack traffic
The internal lab traffic remains separated from the normal home network.
🔐 Active Directory Environment
Domain
SOC-LAB.LOCAL

Domain Controller
SOC-LAB-DC
Windows Server 2022

The Windows Server machine was promoted to a Domain Controller and configured with:
- Active Directory Domain Services
- DNS
- Global Catalog
- Domain authentication
- Group Policy
Windows Endpoint
SOC-LAB-WIN10
Windows 10 Pro

The workstation was successfully joined to:
SOC-LAB.LOCAL

👥 Active Directory Structure
The lab contains dedicated organizational units for:
SOC-LAB.LOCAL
│
├── SOC-Users
│   ├── analyst01
│   ├── user01
│   └── user02
│
├── SOC-Computers
│   └── SOC-LAB-WIN10
│
└── SOC-Admins

The accounts were created specifically for controlled lab authentication testing.
No production accounts were used.
🛡️ Security Auditing
Two separate Group Policy Objects were configured.
Domain Controller Security Auditing
SOC-LAB - DC Security Auditing

Configured auditing included:
- Audit Logon — Failure
- Audit Kerberos Authentication Service — Failure
- Audit Credential Validation — Failure
Windows Endpoint Security Auditing
SOC-LAB - Windows Endpoint Security Auditing

Configured auditing included:
- Audit Credential Validation — Failure
- Audit Logon — Failure
These policies were verified using Windows auditpol.
🔎 Authentication Investigation
Before performing the password-spray simulation, a controlled failed authentication was generated.
The Domain Controller recorded:
Event ID: 4771
Account: user01
Client IP: 10.10.10.20
Failure Code: 0x18
Service: krbtgt/SOC-LAB

Event ID 4771 provided useful Kerberos authentication telemetry and demonstrated how an authentication failure appears inside Windows Security logs.
This established the baseline before moving into attack simulation and detection engineering.
🔥 Attack Simulation — Kerberos Password Spray
The attack was performed from:
Kali Linux

The tool used was:
Kerbrute

The lab contained multiple low-privilege domain accounts:
analyst01
user01
user02

A controlled incorrect password was tested against multiple accounts.
Example command:
~/tools/kerbrute_linux_amd64 passwordspray \
  -d SOC-LAB.LOCAL \
  --dc 10.10.10.10 \
  ~/tools/soc-lab-users.txt \
  '<REPLACE_WITH_DISPOSABLE_LAB_TEST_PASSWORD>'

The attack generated multiple Kerberos pre-authentication failures on the Domain Controller.
📊 Attack Telemetry
The password spray generated:
Event ID: 4771

The important pattern was:
Same Source IP
        ↓
Multiple Failed Kerberos Authentication Attempts
        ↓
Multiple User Accounts
        ↓
Short Time Window

This behavior is more meaningful for password-spray detection than simply counting failed logins against one account.
☁️ Microsoft Sentinel
Windows Security events were collected using:
Azure Monitor Agent
        ↓
Data Collection Rule
        ↓
Log Analytics Workspace
        ↓
Microsoft Sentinel

The relevant table used for detection was:
SecurityEvent

The collected telemetry included authentication-related Windows Security events such as:
4624
4625
4771

The primary password-spray detection was based on:
Event ID 4771

because the Domain Controller provided the useful source IP associated with the Kerberos pre-authentication failure.

🔍 KQL Detection
The detection identifies a source IP generating failed Kerberos authentication attempts against multiple accounts within a short time period.
Final detection logic:

SecurityEvent
| where TimeGenerated > ago(15m)
| where EventID == 4771
| where isnotempty(TargetUserName)
| summarize
    FailedAttempts = count(),
    UniqueAccounts = dcount(TargetUserName),
    Accounts = make_set(TargetUserName),
    FirstSeen = min(TimeGenerated),
    LastSeen = max(TimeGenerated)
    by IpAddress
| where UniqueAccounts >= 3
| order by UniqueAccounts desc

Detection logic
The query:
1. Looks at the last 15 minutes.
2. Filters for Kerberos pre-authentication failures.
3. Groups events by source IP.
4. Counts failed authentication attempts.
5. Counts unique targeted accounts.
6. Collects the affected account names.
7. Requires at least 3 unique accounts.
8. Returns the source IP showing password-spray behavior.

🚨 Microsoft Sentinel Analytics Rule
The KQL detection was converted into a Microsoft Sentinel scheduled Analytics Rule.
Rule
SOC-LAB - Kerberos Password Spray Detection

Configuration
Setting	Value
Rule Type	Scheduled Query Rule
Severity	Medium
Query Frequency	Every 5 minutes
Query Period	Last 15 minutes
Trigger Threshold	Greater than 0
Event Grouping	Single alert
Incident Creation	Enabled
MITRE Tactic	Credential Access
MITRE Technique	T1110.003 — Password Spraying
Status	Enabled


🚨 Incident Generation
The password-spray simulation successfully triggered the detection rule.
The resulting Sentinel incident was:
SOC-LAB - Kerberos Password Spray Detection

The incident was generated from the Sentinel Analytics Rule after the KQL query identified the password-spray behavior.
This validated the complete detection pipeline:
Kali Linux
     ↓
Password Spray
     ↓
Kerberos Authentication Failures
     ↓
Windows Security Events
     ↓
Azure Monitor Agent
     ↓
Log Analytics
     ↓
Microsoft Sentinel
     ↓
KQL Detection
     ↓
Analytics Rule
     ↓
Security Incident

🧪 Investigation Workflow
The project follows a simplified SOC investigation workflow:
Attack
  ↓
Telemetry Generation
  ↓
Log Collection
  ↓
Detection
  ↓
Alert
  ↓
Incident
  ↓
Triage
  ↓
Investigation
  ↓
MITRE ATT&CK Mapping
  ↓
Documentation

The focus of this project was detection engineering and initial incident investigation.

📸 Screenshots & Evidence

Evidence is organized by project phase.
Lab Setup
[Lab Setup Screenshots](screenshots/01-lab-setup/)

Active Directory
[Active Directory Screenshots](screenshots/02-active-directory/)

Domain Join
[Domain-join configuration and context](note/02-active-directory.md)

Microsoft Sentinel
[Sentinel incident evidence](screenshots/04-detection/)

Log Collection
[Authentication telemetry evidence](screenshots/02-active-directory/)

Password Spray
[Password-spray evidence](screenshots/03-password-spray/)

Detection
[Detection and incident evidence](screenshots/04-detection/)

## 📚 Documentation

| Phase | Document |
|---|---|
| Lab setup | [01-lab-setup.md](note/01-lab-setup.md) |
| Active Directory and domain join | [02-active-directory.md](note/02-active-directory.md) |
| Sentinel setup and log ingestion | [03-sentinel-setup.md](note/03-sentinel-setup.md) |
| Password-spray simulation | [04-password-spray.md](note/04-password-spray.md) |
| KQL detection | [05-kql-detection.md](note/05-kql-detection.md) |
| Detection query | [password-spray-detection.kql](kql-queries/password-spray-detection.kql) |

Additional foundational notes:

- [Active Directory](Notes/AD.md)
- [AD DS](Notes/ADDS.md)
- [Domain](Notes/Domain.md)
- [Internal Network](Notes/internal-network.md)
- [NAT](Notes/nat.md)
- [Workgroup](Notes/workgroup.md)

🛠️ Technologies Used
- Windows Server 2022
- Windows 10
- Active Directory Domain Services
- DNS
- Group Policy
- Kali Linux
- Kerbrute
- VirtualBox
- Microsoft Azure
- Azure Arc
- Azure Monitor Agent
- Data Collection Rules
- Log Analytics
- Microsoft Sentinel
- Kusto Query Language (KQL)
- Microsoft Sentinel Analytics Rules
- MITRE ATT&CK
- Git
- GitHub

💡 Key Learnings
Through this project, I gained hands-on experience with:
- Active Directory fundamentals
- AD DS and Domain Controllers
- DNS and domain authentication
- Windows domain joining
- Organizational Units
- Group Policy
- Windows Security Event Logs
- Kerberos authentication
- Event ID 4771
- Authentication failure investigation
- Password-spray attack behavior
- Kerbrute
- Azure Arc
- Azure Monitor Agent
- Data Collection Rules
- Log Analytics
- Microsoft Sentinel
- Kusto Query Language
- Detection engineering
- Sentinel Analytics Rules
- Security incident generation
- MITRE ATT&CK mapping
- SOC investigation workflow

⚠️ Security & Cost Considerations
This is a controlled cybersecurity lab environment.
- No production systems were targeted.
- No real user accounts were used.
- Password spraying was performed only against intentionally created lab accounts.
- The lab network was isolated using the VirtualBox internal network.
- Azure resources were used only for this project.
- Azure usage was monitored to control costs.
- No passwords, API keys, tokens, or Azure credentials are stored in this repository.

Never commit:
.env
API keys
Azure credentials
Passwords
Tokens
Client secrets
Private keys

📈 Project Progress
- [x] GitHub project structure
- [x] Windows Server VM
- [x] Windows 10 VM
- [x] Internal lab network
- [x] Active Directory Domain Services
- [x] Domain Controller
- [x] DNS
- [x] Test accounts
- [x] Windows 10 domain join
- [x] Security auditing GPOs
- [x] Authentication failure validation
- [x] Azure account
- [x] Log Analytics Workspace
- [x] Microsoft Sentinel
- [x] Azure Arc
- [x] Azure Monitor Agent
- [x] Data Collection Rule
- [x] Windows Security event ingestion
- [x] Password-spray simulation
- [x] KQL detection
- [x] Sentinel Analytics Rule
- [x] Sentinel incident generation
- [x] Incident validation
- [x] Project documentation
- [x] Evidence screenshots
- [x] Project 1 completion


🚧 Future Extension


The initial project scope was focused on building and validating the core detection pipeline.
Possible future extensions include:
- Sentinel automation rules
- Logic Apps / SOAR
- Automated incident enrichment
- AI-assisted incident triage
- Additional MITRE ATT&CK detections
- Entity mapping
- Additional attack simulations
- Automated response actions


These are future extensions and are not claimed as completed components of Project 1.
🎓 Key Takeaway

A detection is more valuable when it is tested against an actual attack.

This project demonstrates the complete core SOC detection workflow:

Build the Environment
        ↓
Simulate the Attack
        ↓
Generate Security Telemetry
        ↓
Collect the Logs
        ↓
Develop KQL Detection
        ↓
Create Sentinel Analytics Rule
        ↓
Generate Security Incident
        ↓
Investigate and Document

The project demonstrates hands-on experience with both attack simulation and defensive detection engineering, rather than only theoretical knowledge.
