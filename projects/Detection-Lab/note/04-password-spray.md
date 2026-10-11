# 06 - Kerberos Password Spray Simulation

## Objective

The objective of this phase was to simulate a controlled Kerberos password-spraying attack against the Active Directory environment and verify that the activity generated security telemetry that could be detected by Microsoft Sentinel.

The attack was performed only inside the isolated Detection Lab environment.

---

# Lab Architecture

| Component | Details |
|---|---|
| Domain Controller | SOC-LAB-DC |
| Domain | SOC-LAB.LOCAL |
| Domain Controller IP | 10.10.10.10 |
| Attacker | Kali Linux |
| Attacker IP | 10.10.10.30 |
| Authentication Protocol | Kerberos |
| Security Event | Event ID 4771 |
| Failure Code | 0x18 |
| Tool | Kerbrute |
| Detection Platform | Microsoft Sentinel |

---

# Attack Scenario

The simulated attack represents a password-spraying technique.

Instead of repeatedly attacking a single account with many passwords, password spraying attempts a common password against multiple accounts.

For this lab, three domain accounts were used:

- `analyst01`
- `user01`
- `user02`

A deliberately incorrect password was used so that no account would be compromised.

---

# User List

The Kali machine contained the following lab-only account list:

```text
analyst01
user01
user02


Kerbrute

Kerbrute was used from the Kali Linux attacker machine to generate Kerberos authentication attempts against the domain controller.
Kerbrute version:
v1.0.3

Binary location:
~/tools/kerbrute_linux_amd64


Password Spray Command
The following command was executed from Kali Linux:
~/tools/kerbrute_linux_amd64 passwordspray \
  -d SOC-LAB.LOCAL \
  --dc 10.10.10.10 \
  ~/tools/soc-lab-users.txt \
  'WrongLabPassword!2026'

Command Breakdown
Parameter	            Purpose
passwordspray	        Performs a password-spray operation
-d SOC-LAB.LOCAL	    Specifies the Active Directory domain
--dc 10.10.10.10	    Specifies the domain controller
soc-lab-users.txt	    Contains the target usernames
WrongLabPassword!2026	Deliberately incorrect lab password




Attack Result
The password spray was intentionally unsuccessful.
Result:
3 accounts tested
0 successful logins

Target accounts:
analyst01
user01
user02

Source:
10.10.10.30

Domain Controller:
10.10.10.10

No legitimate account credentials were compromised during this simulation.


Windows Security Event Generation
The password-spray activity generated:
Event ID: 4771

Event ID 4771 represents:
Kerberos pre-authentication failed

The event is generated on the Domain Controller when the Key Distribution Center fails to issue a Kerberos Ticket Granting Ticket.
In this lab, the failed authentication attempts used an incorrect password, resulting in:
Failure Code: 0x18

Microsoft documents 0x18 as:
KDC_ERR_PREAUTH_FAILED

which indicates invalid pre-authentication information, such as an incorrect password.
Observed Event Details
The generated events contained information including:
Event ID: 4771
Target User: analyst01 / user01 / user02
Service: krbtgt/SOC-LAB.LOCAL
Source IP: 10.10.10.30
Failure Code: 0x18

The important detection fields were:
- TargetUserName
- IpAddress
- ServiceName
- EventID
Why Event ID 4771 Matters
A single Event ID 4771 does not necessarily mean that a password spray is occurring.
A legitimate user can generate a Kerberos pre-authentication failure by entering an incorrect password.
The important signal in this lab was the combination of:
Multiple failed Kerberos authentications
        +
Same source IP
        +
Multiple target accounts
        +
Short time window

This behavior is much more suspicious than a single failed authentication.
Sentinel Detection Logic
The generated events were collected into Microsoft Sentinel through the Windows Security Events data pipeline.
The detection logic searched for Event ID 4771 events within a 15-minute window.
The query grouped events by source IP and calculated:
- Total failed attempts
- Number of unique accounts
- Accounts targeted
- First observed event
- Last observed event
The detection threshold was:
3 or more unique accounts

from the same source IP.
KQL Detection Query
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

Detection Result
The detection identified the password-spray behavior from:
Source IP:
10.10.10.30

The targeted accounts were:
analyst01
user01
user02

The detection confirmed:
Unique Accounts: 3

This satisfied the detection threshold.
The behavior was therefore identified as potential password-spraying activity.
MITRE ATT&CK Mapping
This activity maps to:
T1110 - Brute Force

Specifically:
T1110.003 - Password Spraying

The simulated attack demonstrates how an attacker can attempt the same password against multiple accounts instead of repeatedly attacking a single account.
Investigation Flow
The investigation followed this workflow:
Kali Linux
     ↓
Kerbrute Password Spray
     ↓
Active Directory / Kerberos
     ↓
Domain Controller
     ↓
Event ID 4771
     ↓
Azure Monitor Agent
     ↓
Log Analytics Workspace
     ↓
Microsoft Sentinel
     ↓
KQL Detection
     ↓
Password Spray Detection


Evidence
Attack evidence:
../evidence/attack-results.txt

Detection evidence:
../evidence/detection-results.txt

Incident summary:
../evidence/incident-summary.md



Security Outcome
The simulation successfully demonstrated the complete attack-to-detection workflow.
The lab was able to:
1. Generate controlled Kerberos authentication failures.
2. Produce Event ID 4771 on the Domain Controller.
3. Identify the attacking source IP.
4. Identify multiple targeted accounts.
5. Collect the security events in Microsoft Sentinel.
6. Detect the behavior using KQL.
7. Identify the activity as potential password spraying.
8. Generate a Sentinel detection/incident.
Key SOC Analyst Lessons
1. One failed login is not necessarily an attack
A single failed authentication can be normal user behavior.
2. Behavior is more important than a single event
Multiple failed authentication attempts against multiple accounts from the same source are more suspicious.
3. Source IP is important
The IpAddress field helps identify where the authentication attempts originated.
4. Account diversity matters
A password spray commonly targets multiple accounts rather than repeatedly attacking one account.
5. SIEM detection combines multiple events
The KQL detection does not simply search for Event ID 4771.
It analyzes:
Event ID
+
Source IP
+
Number of accounts
+
Time window

to identify suspicious behavior.
Status
COMPLETED

The controlled Kerberos password-spray simulation and Sentinel detection were successfully completed.