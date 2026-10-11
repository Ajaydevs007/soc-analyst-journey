# 02 — Active Directory

## Objective

Configure the Windows Server VM as the Active Directory Domain Controller for the SOC Detection Lab, establish the `SOC-LAB.LOCAL` domain, join the Windows 10 endpoint, create the lab OU and user structure, and configure Windows security auditing.

---

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
```

| Component | Configuration |
|---|---|
| Forest | `SOC-LAB.LOCAL` |
| Domain | `SOC-LAB.LOCAL` |
| NetBIOS Name | `SOC-LAB` |
| Domain Controller | `SOC-LAB-DC` |
| DC Internal IP | `10.10.10.10` |
| Windows 10 | `SOC-LAB-WIN10` |
| Windows 10 Internal IP | `10.10.10.20` |
| Forest Functional Level | Windows Server 2016 |
| Domain Functional Level | Windows Server 2016 |
| DNS | Enabled |
| Global Catalog | Enabled |

---

## Domain Controller

The Windows Server 2022 VM was promoted to the first Domain Controller in a new forest.

The Domain Controller provides:

- Active Directory Domain Services
- DNS
- Global Catalog
- Domain authentication
- Centralized identity management

The Domain Controller is named:

```text
SOC-LAB-DC
```

and uses the internal lab address:

```text
10.10.10.10
```

---

## DNS Configuration

The internal lab interface uses:

```text
10.10.10.10
```

as the Domain Controller and DNS server.

The NAT interface provides Internet connectivity and is not intended to provide the Active Directory DNS records.

During configuration, the NAT address `10.0.2.15` was incorrectly registered in the Active Directory DNS zone. The stale records were removed so that the domain resolves through the internal lab interface.

Final IPv4 DNS records include:

```text
SOC-LAB.LOCAL       → 10.10.10.10
SOC-LAB-DC          → 10.10.10.10
DomainDnsZones      → 10.10.10.10
ForestDnsZones      → 10.10.10.10
```

This ensures that domain clients use the isolated SOC lab network for Active Directory and DNS communication.

---

## Windows 10 Domain Join

The Windows 10 endpoint was joined to:

```text
SOC-LAB.LOCAL
```

Full computer name:

```text
SOC-LAB-WIN10.SOC-LAB.LOCAL
```

Domain authentication was verified with:

```powershell
whoami
```

Result:

```text
soc-lab\administrator
```

Computer membership was verified with:

```powershell
Get-CimInstance Win32_ComputerSystem |
Select-Object Name,Domain,PartOfDomain
```

Result:

```text
Name            Domain          PartOfDomain
----            ------          ------------
SOC-LAB-WIN10   SOC-LAB.LOCAL   True
```

### Result

The Windows 10 endpoint is successfully joined to the `SOC-LAB.LOCAL` Active Directory domain and can authenticate using domain credentials.

---

## Organizational Units

Three dedicated Organizational Units were created:

```text
SOC-LAB.LOCAL
│
├── SOC-Admins
├── SOC-Computers
│   └── SOC-LAB-WIN10
└── SOC-Users
```

### SOC-Users

Used for normal domain users participating in controlled security-testing scenarios.

### SOC-Computers

Used for domain-joined endpoint computers.

The Windows 10 endpoint was moved from the default Computers container into:

```text
OU=SOC-Computers,DC=SOC-LAB,DC=LOCAL
```

### SOC-Admins

Used to separate administrative identities from normal lab users.

### Why OUs Matter

The OU structure provides a controlled scope for applying Group Policy.

For example:

```text
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
```

This structure allows the lab to demonstrate how centralized Windows security policies generate telemetry that can later be used for SOC detection.

---

## Lab User Accounts

Three standard domain accounts were created inside `SOC-Users` for controlled SOC testing.

| Account | OU | Purpose |
|---|---|---|
| `analyst01` | `SOC-Users` | SOC analyst/test account |
| `user01` | `SOC-Users` | Password-spray test account |
| `user02` | `SOC-Users` | Password-spray test account |

The accounts are standard domain users and are not members of administrative groups.

These accounts provide controlled authentication activity for the password-spray detection scenario.

---

## Security Auditing GPO

A dedicated Group Policy Object was created and linked to the Domain Controllers OU:

```text
SOC-LAB - DC Security Auditing
```

The following Advanced Audit Policies were configured for failure auditing:

| Audit Policy | Setting | Purpose |
|---|---|---|
| Audit Logon | Failure | Records failed logon attempts |
| Audit Kerberos Authentication Service | Failure | Records failed Kerberos authentication |
| Audit Credential Validation | Failure | Records failed credential validation |

The policy was applied using:

```powershell
gpupdate /force
```

The effective policy was verified with:

```powershell
auditpol /get /subcategory:"Logon"
```

and:

```powershell
auditpol /get /category:"Account Logon"
```

Verified results included:

```text
Logon → Failure

Kerberos Authentication Service → Failure
Credential Validation → Failure
```

Microsoft documents that the **Audit Kerberos Authentication Service** policy generates Kerberos authentication audit events, including Event ID 4771 for failed pre-authentication.

---

## Windows 10 Endpoint Security Auditing

A separate GPO was created for the Windows endpoint:

```text
SOC-LAB - Windows Endpoint Security Auditing
```

The GPO was linked to:

```text
SOC-Computers
```

The following settings were configured:

```text
Audit Credential Validation → Failure
Audit Logon               → Failure
```

The GPO was applied using:

```powershell
gpupdate /force
```

The effective audit policy was verified with:

```powershell
auditpol
```

Verified results:

```text
Credential Validation → Failure
Logon                 → Failure
```

This provides endpoint-side authentication failure telemetry for collection and investigation.

---

## Authentication Failure Validation

A controlled failed domain authentication was performed from the Windows 10 domain-joined workstation.

### Test

```powershell
runas /user:SOC-LAB\user01 cmd
```

An intentionally incorrect password was entered.

### Result

The Domain Controller generated Windows Security Event ID `4771`.

Microsoft identifies Event ID 4771 as **Kerberos pre-authentication failed** and documents failure code `0x18` as invalid pre-authentication information caused by an incorrect password.

### Key Telemetry

| Field | Value |
|---|---|
| Event ID | `4771` |
| Account | `user01` |
| Service | `krbtgt/SOC-LAB` |
| Client Address | `10.10.10.20` |
| Failure Code | `0x18` |
| Pre-Authentication Type | `2` |
| Timestamp | `2026-09-29 14:02:04 IST` |

### Investigation Flow

```text
SOC-LAB-WIN10
      │
      ▼
Incorrect domain password
      │
      ▼
SOC-LAB-DC
      │
      ▼
Kerberos pre-authentication failure
      │
      ▼
Event ID 4771
      │
      ├── Account: user01
      └── Client: 10.10.10.20
```

This validated that the Domain Controller was generating authentication-failure telemetry that could subsequently be collected by Microsoft Sentinel and used for password-spray detection.

---

## Security Telemetry Flow

The completed Active Directory foundation provides the following telemetry path:

```text
Domain Authentication Activity
            │
            ▼
Windows Security Events
            │
            ▼
Azure Monitor Agent
            │
            ▼
Log Analytics
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

## Status

- [x] Windows Server 2022 installed
- [x] Active Directory Domain Services installed
- [x] Domain Controller promoted
- [x] New forest created
- [x] `SOC-LAB.LOCAL` domain created
- [x] DNS configured
- [x] DNS records verified and cleaned
- [x] Windows 10 joined to domain
- [x] Domain authentication verified
- [x] Domain membership verified
- [x] `SOC-Users` OU created
- [x] `SOC-Computers` OU created
- [x] `SOC-Admins` OU created
- [x] Lab users created
- [x] Domain Controller security auditing configured
- [x] Windows endpoint security auditing configured
- [x] Event ID 4771 authentication failure validated
- [x] Kerberos failure code `0x18` validated
- [x] Security telemetry path established for Sentinel collection