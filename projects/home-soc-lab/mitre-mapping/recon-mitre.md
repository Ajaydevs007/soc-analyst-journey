# MITRE ATT&CK Mapping



---

# 1. ATT&CK Technique Overview

| Field | Value |
|--------|-------|
| Technique ID | T1046 |
| Technique Name | Network Service Discovery |
| ATT&CK Tactic | Reconnaissance |
| ATT&CK Platform | Windows |
| Data Sources | Windows Firewall, Splunk, Sysmon, Windows Security Log |

---

# 2. Technique Description

MITRE ATT&CK Technique **T1046 – Network Service Discovery** describes activity in which an attacker probes a target system to identify exposed network services.

Attackers commonly perform this technique during the reconnaissance phase to determine which services are accessible and to identify potential entry points for subsequent attacks.

Typical tools used for this technique include:

- Nmap
- Masscan
- Netcat
- Custom network scanners

In this investigation, the attacker used Nmap to perform a TCP Connect Scan against TCP port 445 (SMB) on the Windows endpoint to determine whether the service was accessible.

---

# 3. Why This Technique Applies

The activity observed during this investigation aligns with **MITRE ATT&CK Technique T1046 – Network Service Discovery** because the attacker attempted to identify the availability of a network service exposed by the Windows endpoint.

The attacker used **Nmap** to perform a TCP Connect Scan against TCP port **445 (SMB)** on the target system. The purpose of the scan was to determine whether the SMB service was accessible and to gather information that could support future attack stages.

The investigation confirmed that:

- The Nmap scan targeted TCP port 445 (SMB).
- Windows Defender Firewall received the inbound TCP connection attempts.
- The firewall blocked each connection attempt before a TCP session could be established.
- Nmap reported the port as **filtered**.
- Splunk successfully correlated the Windows Firewall logs associated with the scan.

These observations demonstrate that the attacker was collecting information about exposed network services rather than attempting authentication, privilege escalation, or command execution.

Although the reconnaissance activity did not result in successful access to the target system, it represents an important early-stage technique within the ATT&CK framework because it enables attackers to identify potential attack surfaces before attempting exploitation or credential-based attacks.

Based on the observed behavior and supporting evidence, **MITRE ATT&CK Technique T1046 – Network Service Discovery** is the most appropriate mapping for this activity.


## ATT&CK Mapping Confidence

| Assessment | Value |
|------------|-------|
| Confidence | High |
| Justification | Mapping is supported by Nmap execution, Windows Firewall telemetry, and Splunk correlation. |

---

# 4. Supporting Evidence

## Kali Linux

The attacker executed an Nmap TCP Connect Scan against the Windows 10 endpoint using the following command:

```bash
nmap -Pn -p 445 -sT 10.10.10.20
```

The scan reported TCP port **445 (SMB)** as **filtered**, indicating that the target host was reachable but the service could not be accessed.

---

## Windows Security Log

No Windows Security events were generated for the reconnaissance activity.

This was expected because the activity involved inbound network probing rather than authentication, account management, or process creation.

---

## Windows Firewall

Windows Defender Firewall recorded multiple inbound TCP connection attempts from the Kali attacker.

Example:

```text
DROP TCP 10.10.10.10 10.10.10.20 40738 445 RECEIVE
```

These log entries confirmed that the reconnaissance traffic reached the endpoint and was successfully blocked before a TCP session could be established.

---

## Sysmon

No Sysmon Event ID 3 (Network Connection) was generated.

This was expected because Sysmon records network connections initiated by the local Windows host. Since the inbound TCP SYN packets were blocked by Windows Defender Firewall before a connection was established, no Event ID 3 was created.

---

## Splunk

Splunk successfully ingested the Windows Firewall logs and provided centralized visibility into the reconnaissance activity.

Correlation of the firewall logs confirmed repeated blocked TCP connection attempts originating from the Kali attacker against TCP port 445.

---

# 5. Observed Telemetry

| Telemetry Source | Coverage | Evidence |
|------------------|----------|----------|
| Windows Security Log | ❌ None | No Security events generated for the reconnaissance activity. |
| Windows Firewall | ✅ High | Multiple `DROP TCP` entries confirmed blocked inbound TCP connection attempts. |
| Sysmon | ⚠️ Partial | No Event ID 3 generated because no local outbound or established network connection existed. |
| Splunk | ✅ High | Successfully centralized Windows Firewall telemetry and enabled event correlation. |
---

# 6. Detection Opportunities

The investigation identified several opportunities to detect **MITRE ATT&CK Technique T1046 – Network Service Discovery** before an attacker progresses to later stages of the attack lifecycle.

## Detection Logic

Monitor for repeated inbound TCP connection attempts from the same source IP targeting one or more ports within a short time window.

Generate an alert when Windows Defender Firewall records multiple `DROP TCP` events originating from the same source system.

---

## Primary Detection Sources

- Windows Defender Firewall
- Splunk Enterprise

---

## Supporting Detection Sources

- Windows Security Log (when applicable)
- Sysmon (for successful local network connections)
- Network IDS/IPS (if deployed)

---

## Correlation Opportunities

Correlate the following attributes:

- Source IP Address
- Destination IP Address
- Destination Port
- Connection Count
- Time Window

Repeated blocked connection attempts across multiple ports may indicate reconnaissance or port scanning activity.

---

## Example Detection Strategy

Alert when:

- More than 10 dropped TCP connection attempts
- From the same source IP
- Within 60 seconds
- Against one or more destination ports

Escalate the alert if the reconnaissance activity is followed by:

- Authentication failures (Event ID 4625)
- Successful authentication (Event ID 4624)
- Process creation (Event ID 4688)

---

# 7. Detection Limitations

The investigation identified several limitations when detecting **T1046 – Network Service Discovery** using endpoint telemetry alone.

## Windows Security Log

Windows Security logs did not record the reconnaissance activity because no authentication, process creation, or account management events occurred.

---

## Sysmon

Sysmon Event ID 3 was not generated because the Windows endpoint did not initiate or establish the network connection.

This represents an expected telemetry limitation rather than a configuration issue.

---

## Windows Firewall

Windows Defender Firewall successfully recorded the blocked inbound TCP connection attempts.

However, firewall logs alone cannot identify:

- The attacker's intent.
- The scanning tool used.
- Whether the reconnaissance is part of a larger attack campaign.

Additional correlation is required.

---

## Splunk

Splunk provides centralized visibility but depends entirely on the telemetry received from configured log sources.

If Windows Firewall logging is disabled or not forwarded, reconnaissance activity of this type may not be detected.

---

# 8. Defensive Recommendations

The investigation identified several opportunities to strengthen defenses against **MITRE ATT&CK Technique T1046 – Network Service Discovery**.

## Network Hardening

- Disable unnecessary network services.
- Restrict access to administrative services such as SMB (TCP/445) using Windows Defender Firewall.
- Limit service exposure to trusted hosts and networks.

---

## Firewall Configuration

- Continue logging both dropped and allowed connections.
- Regularly review Windows Defender Firewall logs for repeated blocked connection attempts.
- Configure alerts for excessive inbound connection attempts from a single source.

---

## Logging & Monitoring

- Ensure Windows Firewall logs are continuously forwarded to Splunk.
- Verify Sysmon is configured and operating correctly to capture endpoint telemetry.
- Regularly validate that telemetry sources are functioning as expected.

---

## Detection Engineering

- Develop correlation rules that identify repeated blocked inbound connection attempts.
- Correlate reconnaissance activity with subsequent authentication failures or successful logins.
- Implement threshold-based alerts to reduce false positives while maintaining visibility.

---

## Security Operations

- Investigate repeated reconnaissance activity even when the firewall successfully blocks the traffic.
- Treat reconnaissance as an early indicator of potential follow-on attacks.
- Document observed attacker behavior to improve future detection and response capabilities.

---

# 9. Related Investigation

| Document | Status |
|----------|--------|
| Investigation ID | HSL-2026-001 |
| Investigation | Reconnaissance Detection & Telemetry Analysis |
| Detection Rule | recon-detection.md |
| Incident Report | recon-incident-report.md |
| Playbook | network-reconnaissance-playbook.md |
---

# References

## MITRE ATT&CK

- T1046 – Network Service Discovery

---

## Documentation

- Microsoft Windows Defender Firewall Documentation
- Microsoft Windows Event Logging Documentation
- Splunk Documentation
- Sysinternals Sysmon Documentation

---

## Investigation Evidence

- Reconnaissance Detection & Telemetry Analysis
- Kali terminal output
- Windows Firewall (`pfirewall.log`)
- Sysmon Event Viewer
- Splunk search results
- Project screenshots

---

Template Version: 2.0

Last Updated: 2026-07-05

Author: Ajaydev S