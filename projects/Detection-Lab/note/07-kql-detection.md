## Detection Query Validation

A controlled Kerberos password-spray attack was performed from the Kali attacker VM.

### Attack Details

- Source IP: `10.10.10.30`
- Target Domain Controller: `SOC-LAB-DC`
- Event ID: `4771`
- Accounts tested:
  - `analyst01`
  - `user01`
  - `user02`
- Result: `0 successes`

### KQL Validation

The detection query was tested against the last 24 hours of SecurityEvent data.

The query identified:

- Source IP: `10.10.10.30`
- Failed attempts: `9`
- Unique accounts: `3`
- Accounts:
  - `user01`
  - `user02`
  - `analyst01`

This confirmed that the KQL logic successfully identifies multiple failed Kerberos pre-authentication attempts against different accounts from the same source IP.

### MITRE ATT&CK

- Tactic: Credential Access
- Technique: T1110 – Brute Force
- Sub-technique: T1110.003 – Password Spraying