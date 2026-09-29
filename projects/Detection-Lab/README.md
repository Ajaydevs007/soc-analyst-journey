# Detection Lab — Active Directory + Microsoft Sentinel + AI-Assisted Triage

> **Project 1 of 7 — SOC Analyst Home Lab Series**

A hands-on cybersecurity detection lab built to simulate a small enterprise environment and investigate a password-spray attack using **Active Directory, Microsoft Sentinel, KQL, Logic Apps, and OpenAI-assisted incident triage**.

---

## 🚧 Project Status

**Status:** 🟡 In Progress

This project is being built step by step. Documentation and evidence will be updated as each component is completed and verified.

---

## 🎯 Project Objective

Build and investigate a realistic SOC detection scenario:

1. Deploy a Windows Server domain controller.
2. Configure Active Directory Domain Services (AD DS).
3. Create a test Active Directory domain and user accounts.
4. Join a Windows 10 workstation to the domain.
5. Configure Microsoft Sentinel and Log Analytics.
6. Forward Windows Security events into Sentinel.
7. Simulate a password-spray attack from Kali Linux.
8. Detect the attack using KQL.
9. Create a Sentinel Analytics Rule.
10. Generate a Sentinel incident.
11. Trigger a Logic App from the incident.
12. Send relevant incident details to the OpenAI API.
13. Generate an AI-assisted incident summary and MITRE ATT&CK mapping.
14. Write the AI-generated triage information back into the Sentinel incident.

---

## 🏗️ Lab Architecture

```text
                         ┌──────────────────────┐
                         │      Kali Linux      │
                         │      Attacker        │
                         │                      │
                         │  Password Spray      │
                         └──────────┬───────────┘
                                    │
                              soc-lab-net
                                    │
              ┌─────────────────────┴─────────────────────┐
              │                                           │
              ▼                                           ▼
┌──────────────────────────┐               ┌──────────────────────────┐
│    Windows Server VM     │               │     Windows 10 VM        │
│                          │               │                          │
│   Domain Controller      │◄─────────────►│   Domain Workstation     │
│   Active Directory       │               │                          │
│   AD DS + DNS            │               │   Domain Joined          │
└────────────┬─────────────┘               └────────────┬─────────────┘
             │                                          │
             └──────────────────┬───────────────────────┘
                                │
                       Azure Monitor Agent
                                │
                                ▼
                    ┌────────────────────────┐
                    │   Log Analytics        │
                    │      Workspace          │
                    └───────────┬────────────┘
                                │
                                ▼
                    ┌────────────────────────┐
                    │  Microsoft Sentinel    │
                    │                        │
                    │  KQL Detection         │
                    │  Analytics Rule        │
                    │  Incident              │
                    └───────────┬────────────┘
                                │
                                ▼
                    ┌────────────────────────┐
                    │     Logic App / SOAR   │
                    └───────────┬────────────┘
                                │
                                ▼
                    ┌────────────────────────┐
                    │       OpenAI API       │
                    │      GPT-4o-mini       │
                    │                        │
                    │ AI-assisted triage     │
                    │ + MITRE ATT&CK mapping │
                    └───────────┬────────────┘
                                │
                                ▼
                    Sentinel Incident Comment
```

---

## 🖥️ Lab Environment

| Component           | Purpose                     | Status     |
| ------------------- | --------------------------- | ---------- |
| Kali Linux          | Attack simulation           | ⏳ Existing |
| Windows Server      | Domain Controller           | ⏳ Planned  |
| Windows 10          | Domain workstation          | ⏳ Planned  |
| Active Directory    | Identity and authentication | ⏳ Planned  |
| Microsoft Sentinel  | SIEM                        | ⏳ Planned  |
| Log Analytics       | Central log storage         | ⏳ Planned  |
| Azure Monitor Agent | Windows log collection      | ⏳ Planned  |
| Logic App           | SOAR automation             | ⏳ Planned  |
| OpenAI API          | AI-assisted triage          | ⏳ Planned  |

---

## 🌐 Network Design

The lab will use two network interfaces on each VM:

### NAT

Used for:

* Internet access
* Windows updates
* Azure connectivity
* Required software installation

### Internal Network — `soc-lab-net`

Used for:

* Kali ↔ Windows Server communication
* Kali ↔ Windows 10 communication
* Windows Server ↔ Windows 10 communication
* Isolated lab traffic

---

## 🔐 Active Directory

### Domain

> To be documented after AD DS deployment.

### Domain Controller

> To be documented after Windows Server configuration.

### Test Accounts

> To be documented after account creation.

---

## 🛡️ Attack Scenario

The simulated attack will be a **password-spray attack** against low-privilege Active Directory accounts.

The goal is to generate realistic failed authentication events and investigate them as a SOC analyst.

### Attack Flow

```text
Kali Linux
    │
    │ Password Spray
    ▼
Active Directory Accounts
    │
    │ Failed Authentication
    ▼
Windows Security Events
    │
    ▼
Azure Monitor Agent
    │
    ▼
Microsoft Sentinel
    │
    ▼
KQL Detection
    │
    ▼
Sentinel Incident
```

---

## 🔎 Detection

The primary detection will identify a pattern where:

> Multiple accounts experience failed authentication attempts originating from the same source within a short time window.

### KQL

```kusto
// Final detection query will be added here
```

Full query:

[`kql-queries/password-spray-detection.kql`](kql-queries/password-spray-detection.kql)

### Why This Detection Matters

A password spray differs from traditional brute-force activity because an attacker can attempt a small number of common passwords across many accounts instead of repeatedly attacking a single account.

The detection therefore focuses on **authentication failures across multiple accounts from a common source** rather than simply counting failures against one account.

---

## 🚨 Sentinel Analytics Rule

The KQL detection will eventually be converted into a Microsoft Sentinel Analytics Rule.

Documentation:

[`notes/07-kql-detection.md`](notes/07-kql-detection.md)

Status:

> ⏳ Not configured yet

---

## 🤖 AI-Assisted Incident Triage

A Sentinel incident will trigger a Logic App.

The automation will:

```text
Sentinel Incident
       │
       ▼
Logic App
       │
       ├── Account names
       ├── Source IP
       ├── Timestamps
       ├── Detection details
       └── MITRE context
       │
       ▼
OpenAI API
       │
       ▼
GPT-4o-mini
       │
       ├── Plain-English incident summary
       ├── Key indicators
       ├── Investigation context
       └── MITRE ATT&CK mapping
       │
       ▼
Sentinel Incident Comment
```

The AI output will be treated as **analyst assistance**, not as an autonomous final security decision.

Documentation:

* [`logic-app/README.md`](logic-app/README.md)
* [`logic-app/ai-triage-prompt.md`](logic-app/ai-triage-prompt.md)
* [`notes/08-ai-triage.md`](notes/08-ai-triage.md)

---

## 🧪 Investigation Workflow

The investigation will follow a simplified SOC workflow:

```text
Alert
  ↓
Triage
  ↓
Validate
  ↓
Investigate
  ↓
Correlate
  ↓
Identify IOCs
  ↓
MITRE ATT&CK Mapping
  ↓
AI-Assisted Summary
  ↓
Document Findings
```

---

## 📸 Screenshots & Evidence

Evidence will be organized by project phase.

* [Lab Setup](screenshots/01-lab-setup/)
* [Active Directory](screenshots/02-active-directory/)
* [Domain Join](screenshots/03-domain-join/)
* [Microsoft Sentinel](screenshots/04-sentinel/)
* [Log Collection](screenshots/05-log-collection/)
* [Password Spray](screenshots/06-password-spray/)
* [Detection](screenshots/07-detection/)
* [AI Triage](screenshots/08-ai-triage/)

---

## 📚 Documentation

| Phase            | Documentation                                            |
| ---------------- | -------------------------------------------------------- |
| Lab Setup        | [`01-lab-setup.md`](notes/01-lab-setup.md)               |
| Active Directory | [`02-active-directory.md`](notes/02-active-directory.md) |
| Domain Join      | [`03-domain-join.md`](notes/03-domain-join.md)           |
| Sentinel Setup   | [`04-sentinel-setup.md`](notes/04-sentinel-setup.md)     |
| Log Collection   | [`05-log-collection.md`](notes/05-log-collection.md)     |
| Password Spray   | [`06-password-spray.md`](notes/06-password-spray.md)     |
| KQL Detection    | [`07-kql-detection.md`](notes/07-kql-detection.md)       |
| AI Triage        | [`08-ai-triage.md`](notes/08-ai-triage.md)               |

---

## 🛠️ Technologies Used

* Windows Server
* Windows 10
* Active Directory Domain Services
* DNS
* Kali Linux
* VirtualBox
* Microsoft Azure
* Microsoft Sentinel
* Log Analytics
* Azure Monitor Agent
* Kusto Query Language (KQL)
* Microsoft Sentinel Analytics Rules
* Azure Logic Apps
* OpenAI API
* GPT-4o-mini
* MITRE ATT&CK
* Git
* GitHub

---

## 💡 What I Learned

This section will be completed throughout the project.

Topics will include:

* Active Directory fundamentals
* AD DS and Domain Controllers
* DNS and domain authentication
* Domain joining
* Windows Security Event Logs
* Azure Monitor Agent
* Microsoft Sentinel
* KQL
* Detection engineering
* Password-spray detection
* Sentinel Analytics Rules
* SOAR automation
* Logic Apps
* AI-assisted SOC triage
* MITRE ATT&CK mapping
* Security automation considerations

---

## ⚠️ Security & Cost Considerations

This is a controlled lab environment.

No real user accounts or production systems are used.

Azure resources will be monitored carefully because Microsoft Sentinel, Log Analytics ingestion, Logic Apps, and related Azure services can incur charges depending on usage and current pricing.

API credentials and secrets will **never** be committed to GitHub.

Examples:

```text
.env
API keys
Azure credentials
passwords
tokens
client secrets
```

will remain outside the repository.

---

## 📈 Project Progress

* [x] GitHub project structure created
* [ ] Windows Server VM
* [ ] Windows 10 VM
* [ ] Active Directory Domain Services
* [ ] Domain Controller
* [ ] Test accounts
* [ ] Windows 10 domain join
* [ ] Azure account
* [ ] Log Analytics Workspace
* [ ] Microsoft Sentinel
* [ ] Azure Monitor Agent
* [ ] Security Event ingestion
* [ ] Password spray simulation
* [ ] KQL detection
* [ ] Sentinel Analytics Rule
* [ ] Sentinel incident
* [ ] Logic App
* [ ] OpenAI API integration
* [ ] AI-assisted triage
* [ ] MITRE ATT&CK mapping
* [ ] Final documentation

---

## 🎓 Key Takeaway

> This project demonstrates an end-to-end SOC workflow: generating an attack, collecting telemetry, detecting the behavior with KQL, creating a SIEM alert, automating incident response, and using an LLM to assist with initial incident triage and MITRE ATT&CK mapping.

---

## 👤 Author

**Ajaydev S**

GitHub:
https://github.com/Ajaydevs007
