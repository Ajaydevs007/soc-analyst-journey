# KQL Queries

This directory contains the Kusto Query Language (KQL) queries developed
and tested during the Detection Lab project.

The queries are used with Microsoft Sentinel to analyze Windows Security
events and identify suspicious authentication behavior.

---

## Password Spray Detection

### File

`password-spray-detection.kql`

### Purpose

Detect potential Kerberos password-spray activity by identifying
multiple failed Kerberos pre-authentication attempts against different
user accounts from the same source IP address within a 15-minute window.

### Data Source

- Microsoft Sentinel
- Log Analytics
- `SecurityEvent` table
- Windows Security Events via Azure Monitor Agent (AMA)

Microsoft documents that Windows Security Events collected through AMA
are written to the `SecurityEvent` table. :chatgpt-content-reference{index="0"}

### Event ID

`4771` — Kerberos pre-authentication failure.

### Detection Logic

The query:

1. Looks at the last 15 minutes of events.
2. Filters for Event ID `4771`.
3. Requires a valid `TargetUserName`.
4. Groups failed authentication attempts by source IP.
5. Counts total failed attempts.
6. Counts unique targeted accounts.
7. Builds a list of targeted accounts.
8. Records the first and last observed timestamps.
9. Triggers the detection when at least 3 unique accounts are targeted.

### Detection Pattern

```text
Same Source IP
       │
       ├── Failed authentication → user01
       ├── Failed authentication → user02
       └── Failed authentication → analyst01
                    │
                    ▼
        Multiple accounts targeted
                    │
                    ▼
          Potential Password Spray