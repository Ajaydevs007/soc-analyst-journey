# 07 - KQL Detection

## Objective

The objective of this phase was to convert the raw Kerberos authentication failure events into a behavioral detection using Kusto Query Language (KQL) in Microsoft Sentinel.

The detection focuses on identifying potential Kerberos password-spraying activity by looking for multiple failed authentication attempts against multiple accounts from the same source IP within a short time window.

---

# Detection Scenario

The previous phase generated controlled Kerberos authentication failures using Kerbrute.

The Domain Controller generated:

```text
Event ID: 4771

The important detection characteristics were:
Multiple failed Kerberos authentications
        +
Same source IP
        +
Multiple target accounts
        +
Short time window

This behavior can indicate password spraying.
Data Source
The detection uses the Microsoft Sentinel:
SecurityEvent

table.
The primary event used by the detection is:
Event ID 4771

Event ID 4771 represents a failed Kerberos pre-authentication attempt.
Detection Requirements
The detection was designed with the following requirements:
Requirement	Value
Data Source	SecurityEvent
Event ID	4771
Detection Window	15 minutes
Grouping Field	IpAddress
Account Field	TargetUserName
Minimum Unique Accounts	3
Detection Type	Behavioral
MITRE Technique	T1110.003 - Password Spraying


KQL Query
The following query was created and tested in Microsoft Sentinel:
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

Query Explanation
1. Select Security Events
SecurityEvent

The query searches the SecurityEvent table containing Windows security events collected from the lab machines.
2. Restrict the Time Window
| where TimeGenerated > ago(15m)

Only events generated during the previous 15 minutes are considered.
This creates the detection window.
3. Select Kerberos Failures
| where EventID == 4771

Only Kerberos pre-authentication failure events are analyzed.
4. Require a Target Account
| where isnotempty(TargetUserName)

Events without a target username are excluded.
This allows the detection to identify how many different accounts were targeted.
5. Aggregate the Events
| summarize
    FailedAttempts = count(),
    UniqueAccounts = dcount(TargetUserName),
    Accounts = make_set(TargetUserName),
    FirstSeen = min(TimeGenerated),
    LastSeen = max(TimeGenerated)
    by IpAddress

The events are grouped by:
IpAddress

For each source IP, the query calculates:
FailedAttempts
count()

Counts the total number of Event ID 4771 events.
UniqueAccounts
dcount(TargetUserName)

Counts the number of different accounts targeted by the source IP.
Accounts
make_set(TargetUserName)

Creates a list of the targeted accounts.
FirstSeen
min(TimeGenerated)

Identifies the first event observed in the detection window.
LastSeen
max(TimeGenerated)

Identifies the most recent event observed in the detection window.
Detection Threshold
The query applies the following condition:
| where UniqueAccounts >= 3

This means the detection only returns a source IP when it has generated failed Kerberos authentication attempts against at least three different accounts.
The threshold was selected for this controlled lab because the password-spray simulation targeted three accounts.
Detection Result
The controlled password spray originated from:
10.10.10.30

The targeted accounts were:
analyst01
user01
user02

The query identified:
Unique Accounts: 3

The source therefore satisfied the detection condition:
UniqueAccounts >= 3

The activity was identified as potential password-spraying behavior.
Why This Detection Works
The detection does not rely on a single failed login.
Instead, it correlates several characteristics:
Event ID 4771
        ↓
Same source IP
        ↓
Multiple target accounts
        ↓
Within 15 minutes
        ↓
3+ unique accounts
        ↓
Potential password spray

This is a behavioral detection rather than a simple event search.
Example Detection Logic
Normal User Mistake
10.10.10.20
      ↓
user01
      ↓
1 failed authentication

This is not enough to trigger the password-spray detection.
Suspicious Behavior
10.10.10.30
      ↓
analyst01
user01
user02
      ↓
Multiple Event ID 4771
      ↓
Within 15 minutes
      ↓
3 unique accounts
      ↓
Detection triggered

This matches the behavior the rule was designed to identify.
Microsoft Sentinel Analytics Rule
The KQL query was converted into a Microsoft Sentinel Analytics Rule.
Rule name:
SOC-LAB - Kerberos Password Spray Detection

Rule type:
Scheduled query rule

Severity:
Medium

MITRE tactic:
Credential Access

MITRE technique:
T1110.003 - Password Spraying

Analytics Rule Configuration
The rule was configured with:
Setting	Value
Rule Name	SOC-LAB - Kerberos Password Spray Detection
Rule Type	Scheduled
Severity	Medium
Query Frequency	5 minutes
Query Period	15 minutes
Alert Threshold	Greater than 0
Event Grouping	Group all events into a single alert
Incident Creation	Enabled
Alert Grouping	Disabled
Automation Rule	None


The 5-minute query frequency means Sentinel periodically evaluates the KQL query, while the 15-minute query period defines the data lookback window.
Analytics Rule Query
The same detection logic was used in the Sentinel Analytics Rule:
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

Alert and Incident
The Analytics Rule successfully generated a Sentinel detection.
Incident:
SOC-LAB - Kerberos Password Spray Detection

Severity:
Medium

Category:
Credential Access

MITRE technique:
T1110.003 - Password Spraying

The resulting Sentinel incident confirmed that the KQL detection was successfully connected to an operational detection rule.
Investigation Flow
The complete detection chain was:
Kali Linux
     ↓
Kerbrute Password Spray
     ↓
Kerberos Authentication
     ↓
Domain Controller
     ↓
Event ID 4771
     ↓
Azure Monitor Agent
     ↓
Log Analytics Workspace
     ↓
SecurityEvent
     ↓
KQL Detection
     ↓
Sentinel Analytics Rule
     ↓
Alert
     ↓
Incident

Evidence
KQL detection evidence:
../evidence/detection-results.txt

Incident evidence:
../evidence/incident-summary.md

Password-spray evidence:
../evidence/attack-results.txt

Screenshots
KQL detection:
../screenshots/06-password-spray/password-spray-kql-detection.png

Sentinel incident:
../screenshots/07-detection/password-spray-sentinel-incident.png

MITRE ATT&CK
The detection maps to:
T1110 - Brute Force

Specifically:
T1110.003 - Password Spraying

The detection identifies a pattern consistent with an attacker attempting the same password against multiple accounts.
SOC Analyst Perspective
From a SOC analyst perspective, the important indicators are:
Source IP
10.10.10.30

Identifies the machine generating the authentication attempts.
Target Accounts
analyst01
user01
user02

Shows that multiple accounts were targeted.
Event ID
4771

Shows Kerberos pre-authentication failures.
Time Window
15 minutes

Provides temporal context for correlating the authentication failures.
Account Count
3 unique accounts

Meets the detection threshold.
Detection Limitations
This detection is intentionally simple and designed for the Detection Lab.
Potential improvements for a production environment could include:
- Excluding known service accounts.
- Excluding known administrative systems.
- Using additional authentication events.
- Adding workstation and user context.
- Mapping the source IP as an entity.
- Adding custom alert details.
- Adding suppression or tuning logic.
- Correlating with other suspicious activity.
- Reducing false positives through baseline analysis.
These improvements were not implemented as part of this project.
Project Outcome
The Detection Lab successfully demonstrated the complete detection workflow:
BUILD
  ↓
ATTACK
  ↓
GENERATE LOGS
  ↓
COLLECT LOGS
  ↓
WRITE KQL
  ↓
CREATE ANALYTICS RULE
  ↓
GENERATE ALERT
  ↓
CREATE INCIDENT
  ↓
INVESTIGATE

The password-spray simulation was successfully detected using Microsoft Sentinel.
Status
COMPLETED

The KQL detection and Sentinel Analytics Rule were successfully implemented and validated against the controlled Kerberos password-spray simulation.