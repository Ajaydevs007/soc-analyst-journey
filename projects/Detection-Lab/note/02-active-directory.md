# 02 — Active Directory

## Objective

Configure the Windows Server VM as the Domain Controller for the SOC Detection Lab and join the Windows 10 endpoint to the domain.

## Active Directory Architecture

```text
SOC-LAB.LOCAL
│
├── SOC-LAB-DC
│   ├── Active Directory Domain Services
│   ├── DNS
│   └── Global Catalog
│
└── SOC-LAB-WIN10
    └── Domain Member


    | Component               | Configuration       |
| ----------------------- | ------------------- |
| Forest                  | SOC-LAB.LOCAL       |
| Domain                  | SOC-LAB.LOCAL       |
| NetBIOS Name            | SOC-LAB             |
| Domain Controller       | SOC-LAB-DC          |
| DC Internal IP          | 10.10.10.10         |
| Windows 10              | SOC-LAB-WIN10       |
| Windows 10 Internal IP  | 10.10.10.20         |
| Forest Functional Level | Windows Server 2016 |
| Domain Functional Level | Windows Server 2016 |
| DNS                     | Enabled             |
| Global Catalog          | Enabled             |


Domain Controller

The Windows Server 2022 VM was promoted to the first Domain Controller in a new forest.

The server provides:

Active Directory Domain Services
DNS
Global Catalog
Domain authentication
Centralized identity management


DNS Configuration

The internal lab interface uses:

10.10.10.10

as the Domain Controller/DNS server.

The NAT interface is used for Internet connectivity and is not intended to provide AD DNS records.

The AD DNS zone was cleaned to prevent the NAT address 10.0.2.15 from being returned for the domain.

Final IPv4 DNS records include:

SOC-LAB.LOCAL       → 10.10.10.10
SOC-LAB-DC          → 10.10.10.10
DomainDnsZones      → 10.10.10.10
ForestDnsZones      → 10.10.10.10


Windows 10 Domain Join

Windows 10 was joined to:

SOC-LAB.LOCAL

The full computer name is:

SOC-LAB-WIN10.SOC-LAB.LOCAL
Authentication Verification

After restarting Windows 10, domain authentication was verified with:

whoami

Result:

soc-lab\administrator

Computer membership was verified with:

Get-CimInstance Win32_ComputerSystem |
Select-Object Name,Domain,PartOfDomain

Result:

Name          Domain        PartOfDomain
----          ------        ------------
SOC-LAB-WIN10 SOC-LAB.LOCAL True
Result

The Windows 10 endpoint is successfully joined to the SOC-LAB.LOCAL Active Directory domain and can authenticate using domain credentials.

Status
[x] Windows Server 2022 installed
[x] AD DS installed
[x] Domain Controller promoted
[x] New forest created
[x] SOC-LAB.LOCAL domain created
[x] DNS configured
[x] DNS records verified
[x] Windows 10 joined to domain
[x] Domain authentication verified
[x] Domain membership verified
[ ] Organizational Units
[ ] Domain users
[ ] Group Policy
[ ] Windows security auditing
[ ] Sentinel log collection





## Organizational Units

Three dedicated OUs were created to organize the lab environment:

```text
SOC-LAB.LOCAL
├── SOC-Admins
├── SOC-Computers
│   └── SOC-LAB-WIN10
└── SOC-Users


SOC-Users

Used for normal domain users participating in the security testing scenarios.

SOC-Computers

Used for domain-joined endpoint computers.

The Windows 10 endpoint was moved from the default Computers container into:

OU=SOC-Computers,DC=SOC-LAB,DC=LOCAL
SOC-Admins

Used to separate administrative identities from normal lab users.

Why OUs Matter

The OUs provide a controlled structure for applying Group Policy later.

For example:

SOC-Computers
      │
      ▼
   Group Policy
      │
      ▼
Windows Security Auditing
      │
      ▼
Security Events
      │
      ▼
Microsoft Sentinel

This structure will allow the lab to demonstrate how centralized Windows security policies can generate telemetry for SOC detection.

## Lab User Accounts

Three standard domain accounts were created inside `SOC-Users` for controlled SOC testing.

| Account | OU | Purpose |
|---|---|---|
| analyst01 | SOC-Users | SOC analyst/test account |
| user01 | SOC-Users | Password-spray test account |
| user02 | SOC-Users | Password-spray test account |

The accounts are standard domain users and are not members of administrative groups.

These accounts will later provide controlled authentication activity for the password-spray detection scenario.


## Security Auditing GPO

A dedicated Group Policy Object was created and linked to the
`Domain Controllers` OU:

```text
SOC-LAB - DC Security Auditing


The following Advanced Audit Policies were configured for failure auditing:

Audit Policy	Setting	Purpose
Logon	Failure	Records failed logon attempts
Kerberos Authentication Service	Failure	Records failed Kerberos authentication
Credential Validation	Failure	Records failed credential validation

The policy was applied using:

gpupdate /force

The effective policy was verified with:

auditpol /get /subcategory:"Logon"

Result:

Logon → Failure

The Account Logon policies were verified with:

auditpol /get /category:"Account Logon"

Result:

Kerberos Authentication Service → Failure
Credential Validation            → Failure
Why this matters for the SOC lab

These audit policies provide Windows security telemetry that can later be
collected by the monitoring platform.

The intended detection flow is:

Failed Authentication
        ↓
Windows Security Event
        ↓
Log Collection
        ↓
Microsoft Sentinel
        ↓
KQL Detection
        ↓
Incident
        ↓
SOAR / AI-assisted Triage





## Authentication Failure Validation

A controlled failed authentication was performed from the Windows 10 domain-joined workstation against the lab domain.

### Test

```powershell
runas /user:SOC-LAB\user01 cmd

An intentionally incorrect password was entered.

Result

The Domain Controller generated Windows Security Event ID 4771.

Key telemetry:

Field	          Value
Event ID	      4771
Account	          user01
Service	          krbtgt/SOC-LAB
Client Address	  10.10.10.20
Failure Code	  0x18
Pre-Authentication   Type	2
Timestamp	      2026-09-29 14:02:04 IST


Investigation Flow


SOC-LAB-WIN10
      ↓
Incorrect domain password
      ↓
SOC-LAB-DC
      ↓
Kerberos pre-authentication failure
      ↓
Event ID 4771
      ↓
Account: user01
      ↓
Client: 10.10.10.20

This validates that the Domain Controller is generating authentication failure telemetry that can later be collected by Microsoft Sentinel and used for password-spray detection.




## Windows 10 Endpoint Security Auditing

A dedicated GPO named `SOC-LAB - Windows Endpoint Security Auditing` was created and linked to the `SOC-Computers` OU.

The GPO configures:

- Audit Credential Validation → Failure
- Audit Logon → Failure

The GPO was applied to `SOC-LAB-WIN10` using:

```powershell
gpupdate /force

The effective audit policy was verified with auditpol:

Credential Validation → Failure
Logon → Failure

This provides endpoint-side authentication failure telemetry for later collection and correlation in Microsoft Sentinel.