# Reconnaissance Detection & Telemetry Analysis

---

# 1. Investigation Overview

## Objective

Investigate repeated dropped inbound TCP connection attempts detected by Splunk to determine whether they represent reconnaissance activity against the Windows 10 endpoint.

The investigation aims to identify the attack technique, correlate evidence from Windows Firewall logs, Sysmon, and Splunk Enterprise, evaluate the effectiveness of the endpoint's defensive controls, and document the telemetry generated throughout the attack lifecycle.

---

## Scope

### Investigation Status

| Field | Value |
|--------|-------|
| Investigation ID | HSL-2026-001 |
| Status | Closed |
| Severity | Low |
| Disposition | Benign Security Testing |
| Analyst | Ajaydev S |



### Investigation Trigger

Splunk identified repeated Windows Firewall `DROP TCP` events originating from the Kali Linux attacker (`10.10.10.10`) and targeting the Windows 10 victim (`10.10.10.20`) over TCP port 445.

### Investigation Goals

- Determine whether reconnaissance activity occurred.
- Identify which telemetry sources detected the activity.
- Explain why Sysmon Event ID 3 was not generated.
- Assess the effectiveness of Windows Firewall in blocking the connection attempts.
- Develop detection opportunities for future reconnaissance activity.

### Time Window

26 June 2026

### Systems Involved

- Kali Linux (Attacker)
- Windows 10 (Victim)
- Splunk Enterprise
- Splunk Universal Forwarder
- Sysmon
- Windows Defender Firewall

---

# 2. Lab Environment

| Component | Details |
|-----------|---------|
| Hypervisor | VirtualBox |
| Attacker | Kali Linux (10.10.10.10) |
| Victim | Windows 10 (10.10.10.20) |
| SIEM | Splunk Enterprise |
| Log Forwarder | Splunk Universal Forwarder |
| Endpoint Telemetry | Sysmon (SwiftOnSecurity Configuration) |
| Network | Internal Network (`soc-lab-net`) |

## Telemetry Sources

- Windows Security Log
- Windows System Log
- Windows Application Log
- Sysmon Operational Log
- Windows Firewall Log (`pfirewall.log`)
- Splunk Enterprise

---

# 3. Attack Overview

## Attack Scenario

A reconnaissance activity was simulated from the Kali Linux attacker (`10.10.10.10`) against the Windows 10 victim (`10.10.10.20`) to evaluate the organization's ability to detect early-stage network discovery attempts.

The attacker performed a TCP Connect Scan targeting the SMB service (TCP port 445) using Nmap. The objective was to determine whether the target exposed the service and to observe the telemetry generated across Windows Firewall, Sysmon, and Splunk Enterprise.

---

## Attack Details

| Field | Value |
|--------|-------|
| Attack Phase | Reconnaissance |
| ATT&CK Tactic | Reconnaissance |
| ATT&CK Technique | T1046 – Network Service Discovery |
| Attacker | Kali Linux |
| Target | Windows 10 |
| Source IP | 10.10.10.10 |
| Destination IP | 10.10.10.20 |
| Tool | Nmap |
| Attack Type | TCP Connect Scan (`-sT`) |
| Target Port | TCP/445 (SMB) |

---

## Attack Execution

### Tool

Nmap 7.98

---

### Command

```bash
nmap -Pn -p 445 -sT 10.10.10.20
```

---

### Command Output

```text
Starting Nmap 7.98 ...

PORT    STATE     SERVICE
445/tcp filtered  microsoft-ds

Nmap done: 1 IP address (1 host up) scanned in 2.55 seconds
```

---

### Initial Observation

The TCP Connect Scan successfully reached the target host. Nmap reported TCP port 445 as **filtered**, indicating that the connection attempt did not complete successfully.

At this stage of the investigation, the reason for the filtered response had not yet been determined and required further investigation using Windows Firewall logs, Sysmon telemetry, and Splunk Enterprise.

---



---

# 4. Evidence Collection

Document all evidence collected during the investigation.

---

## 4.1 Attacker Evidence (Kali Linux)

### Commands Executed

```bash
nmap -Pn -p 445 -sT 10.10.10.20
```

---

### Terminal Output

```text
PORT    STATE     SERVICE
445/tcp filtered microsoft-ds
```

---

### Screenshots

- step35-nmap-scan.png 

---

### Initial Observations

- Target host responded successfully.
- TCP port 445 was reported as filtered.
- The TCP connection was not successfully established.

---

## 4.2 Windows Security Evidence

### Relevant Event IDs

No relevant Windows Security Event IDs were generated for this reconnaissance activity.

---

### Event Details

No Security log events directly associated with the inbound TCP connection attempts were observed.

---

### Observations

Windows Security logs did not provide visibility into this reconnaissance activity.

---

## 4.3 Windows Firewall Evidence

### Log Entries

Example firewall log entry:

```text
DROP TCP 10.10.10.10 10.10.10.20 40738 445 RECEIVE
```

Multiple similar entries were recorded for repeated TCP SYN packets targeting TCP port 445.

---

### Screenshots

- step41-pfirewall-drop-events.png

---

### Observations

- Windows Defender Firewall received inbound TCP connection attempts.
- The firewall blocked each connection attempt.
- Repeated DROP TCP entries confirmed that the scan reached the Windows endpoint.

---

## 4.4 Sysmon Evidence

### Relevant Event IDs

No Sysmon Event ID 3 (Network Connection) was generated.

---

### Event Details

Sysmon Event ID 3 was not present in Event Viewer or Splunk for the inbound TCP scan.

---

### Screenshots



---

### Observations

The absence of Sysmon Event ID 3 was investigated and determined to be expected behavior because the Windows host did not initiate or establish the network connection.

---

## 4.5 Splunk Evidence

### Search Queries

```spl
sourcetype=windows:firewall
```

Additional searches included verification of Sysmon events:

```spl
source="WinEventLog:Microsoft-Windows-Sysmon/Operational"
```

---

### Search Results

Splunk successfully ingested Windows Firewall log entries showing repeated dropped TCP connections from the Kali attacker.

No Sysmon Event ID 3 corresponding to the reconnaissance scan was observed.

---

### Screenshots

- step45-firewall-events-in-splunk.png

---

### Observations

Splunk successfully centralized Windows Firewall telemetry and confirmed repeated inbound connection attempts against TCP port 445.

---

# 5. Evidence Summary

| Source   | Evidence         | Value                                   |
| -------- | ---------------- | --------------------------------------- |
| Kali     | Nmap output      | Confirmed attack execution              |
| Firewall | DROP TCP         | Confirmed blocked reconnaissance        |
| Sysmon   | No Event ID 3    | Confirmed expected telemetry limitation |
| Splunk   | Centralized logs | Enabled correlation                     |


---

# 6. Investigation

## 6.1 Telemetry Analysis

### Kali Linux

The attacker initiated a TCP Connect Scan against the Windows 10 endpoint using Nmap.

The scan targeted TCP port 445 (SMB), a commonly targeted service during reconnaissance because it may expose file sharing or administrative access.

Nmap reported the port as **filtered**, indicating that the target host was reachable but the TCP connection could not be successfully established.

---

### Windows Security Log

No Windows Security events related to the reconnaissance activity were generated.

This behavior is expected because the Windows Security log primarily records authentication, authorization, account management, and process auditing events rather than blocked inbound network connection attempts.

---

### Windows Firewall

Windows Defender Firewall recorded multiple **DROP TCP** entries corresponding to repeated inbound TCP SYN packets from the Kali attacker.

Example:

```text
DROP TCP 10.10.10.10 10.10.10.20 40738 445 RECEIVE
```

These entries confirmed that:

- The reconnaissance packets reached the Windows network stack and were processed by Windows Defender Firewall before being dropped.
- The Windows Firewall inspected the packets.
- The inbound connection attempts were blocked before a TCP session could be established.

---

### Sysmon

No Sysmon Event ID 3 (Network Connection) was generated during the reconnaissance activity.

This behavior was investigated and determined to be expected.

Sysmon Event ID 3 records **network connections initiated by processes running on the local Windows host**.

In this scenario:

- The connection was initiated from the Kali attacker.
- Windows did not establish the TCP session.
- The Windows Firewall blocked the inbound SYN packets.

Because no local outbound or established network connection existed, Sysmon did not generate Event ID 3.

---

### Splunk Enterprise

Splunk successfully ingested telemetry from multiple sources, including Windows Firewall logs and Sysmon.

The Windows Firewall log entries provided clear visibility into the reconnaissance activity by recording repeated dropped TCP connection attempts.

Splunk centralized the available telemetry, enabling the analyst to correlate attacker activity with endpoint defensive controls.

---

## 6.2 Event Correlation

The investigation correlated evidence collected from Kali Linux, Windows Defender Firewall, Sysmon, and Splunk Enterprise to reconstruct the reconnaissance activity.

### Step 1 — Reconnaissance Initiated

The attacker initiated a TCP Connect Scan (`-sT`) from the Kali Linux system (`10.10.10.10`) targeting TCP port 445 on the Windows 10 endpoint (`10.10.10.20`).

The scan attempted to establish a standard TCP three-way handshake with the target service.

Evidence:

- Kali terminal output
- Nmap command execution

---

### Step 2 — Traffic Reached the Target

Windows Defender Firewall received the inbound TCP SYN packets generated by the Nmap scan.

Multiple firewall log entries confirmed that the packets reached the Windows endpoint.

Example:

```text
DROP TCP 10.10.10.10 10.10.10.20 40738 445 RECEIVE
```

This confirmed that the attacker had network connectivity to the target and that the reconnaissance traffic successfully reached the Windows host.

---

### Step 3 — Windows Firewall Blocked the Connection

Windows Defender Firewall evaluated the inbound connection attempts against its configured security policy.

The firewall blocked each TCP connection attempt before the TCP three-way handshake could complete.

As a result:

- The SMB service remained inaccessible.
- Nmap reported the port as **filtered**.
- No TCP session was successfully established.

---

### Step 4 — Sysmon Behavior

During the investigation, no Sysmon Event ID 3 (Network Connection) was observed.

This was determined to be expected behavior rather than a logging failure.

Sysmon Event ID 3 records network connections initiated by processes running on the local Windows endpoint.

Since:

- the attacker initiated the connection,
- Windows Firewall blocked the inbound SYN packets, and
- no local outbound or established connection existed,

Sysmon correctly did not generate Event ID 3.

---

### Step 5 — Centralized Visibility Through Splunk

Splunk successfully ingested the Windows Firewall logs generated during the attack.

Although Sysmon did not provide network connection telemetry for this activity, the Windows Firewall logs provided sufficient evidence to identify and investigate the reconnaissance attempt.

By correlating telemetry from multiple sources, Splunk enabled reconstruction of the attack sequence and verification of the defensive controls.

---

## 6.3 Root Cause Analysis

The investigation determined that the observed telemetry accurately reflected the behavior of the attack and the defensive controls protecting the Windows endpoint.

### Root Cause

The attacker performed a TCP Connect Scan against TCP port 445 (SMB) on the Windows 10 endpoint using Nmap.

The Windows Defender Firewall was configured to block unsolicited inbound SMB connections. As each TCP SYN packet reached the endpoint, the firewall evaluated the traffic against its active inbound rules and rejected the connection attempt before the TCP three-way handshake could be completed.

Because the TCP session was never successfully established, Nmap classified the port as **filtered**.

---

### Why Windows Firewall Logged the Activity

Windows Defender Firewall logging was configured to record dropped packets.

As a result, every blocked TCP SYN packet generated a corresponding `DROP TCP` entry in `pfirewall.log`.

These log entries confirmed:

- The attacker had network connectivity to the target.
- The reconnaissance traffic successfully reached the Windows endpoint.
- The firewall inspected and blocked the connection attempts.
- The defensive control functioned as intended.

---

### Why Sysmon Event ID 3 Was Not Generated

Initially, the absence of Sysmon Event ID 3 appeared unusual because network-related activity had occurred.

Further investigation confirmed that this was expected behavior.

Sysmon Event ID 3 records network connections initiated by processes running on the local Windows system.

In this investigation:

- The connection originated from the external attacker.
- Windows did not initiate the connection.
- The firewall blocked the inbound SYN packets.
- No TCP session was established.

Since no local network connection was created, Sysmon correctly did not generate Event ID 3.

This behavior represents a telemetry limitation for detecting blocked inbound reconnaissance rather than a configuration or logging failure.

---

### Defensive Control Assessment

The Windows Defender Firewall successfully prevented access to the SMB service by blocking every inbound connection attempt.

The investigation also demonstrated that relying on a single telemetry source could lead to incomplete conclusions.

Although Sysmon did not record the reconnaissance activity, Windows Firewall logs provided sufficient visibility to detect and investigate the attack.

Correlating multiple telemetry sources through Splunk allowed the complete sequence of events to be reconstructed and verified.

---

### Investigation Conclusion

The reconnaissance activity successfully reached the Windows endpoint but was prevented from establishing a TCP connection by Windows Defender Firewall.

The absence of Sysmon Event ID 3 was determined to be expected system behavior rather than a telemetry failure.

The investigation confirmed that Windows Firewall logs were the primary telemetry source for identifying this reconnaissance activity, while Splunk provided centralized visibility and correlation across the available data sources.

---

# 7. Timeline Reconstruction

| Time     | Activity                        | Telemetry Source  | Evidence      |
| -------- | ------------------------------- | ----------------- | ------------- |
| 15:17:00 | Nmap TCP Connect Scan initiated | Kali              | Nmap terminal |
| 15:17:12 | Inbound TCP SYN received        | Windows Firewall  | DROP TCP      |
| 15:17:12 | Firewall blocks connection      | Windows Firewall  | DROP TCP      |
| 15:17:13 | No TCP session established      | Sysmon            | No Event ID 3 |
| 15:17:13 | Firewall logs forwarded         | Splunk UF         | Forwarder     |
| 15:17:14 | Events searchable               | Splunk Enterprise | SPL search    |

---

# 8. Findings

## Confirmed Findings

### Finding 1 — Reconnaissance Activity Confirmed

The investigation confirmed that reconnaissance activity was performed against the Windows 10 endpoint using an Nmap TCP Connect Scan targeting TCP port 445 (SMB).

---

### Finding 2 — Windows Defender Firewall Successfully Blocked the Scan

Windows Defender Firewall inspected and blocked each inbound TCP connection attempt before a TCP session could be established.

As a result, the SMB service remained inaccessible to the attacker.

---

### Finding 3 — Sysmon Event ID 3 Was Not Expected

No Sysmon Event ID 3 (Network Connection) was generated during the reconnaissance activity.

The investigation determined this to be expected behavior because Windows did not initiate or establish the network connection.

---

### Finding 4 — Windows Firewall Provided Primary Detection Visibility

Windows Firewall logs were the primary telemetry source for identifying the reconnaissance activity.

Repeated `DROP TCP` entries provided clear evidence that inbound TCP SYN packets reached the endpoint and were successfully blocked.

---

### Finding 5 — Splunk Successfully Correlated Available Telemetry

Splunk Enterprise successfully centralized Windows Firewall telemetry, allowing the reconnaissance activity to be investigated from a single platform.

Although Sysmon did not generate network connection events for this activity, Splunk still provided sufficient visibility through correlated firewall logs.

---

## Supporting Evidence

| Finding | Supporting Evidence |
|----------|---------------------|
| Reconnaissance Confirmed | Nmap terminal output |
| Firewall Blocked Connection | Windows Firewall `DROP TCP` entries |
| No Sysmon Event ID 3 | Event Viewer and Splunk verification |
| Centralized Investigation | Splunk Firewall Events |


---


# 9. Detection & Telemetry Coverage

| Telemetry Source | Coverage | Notes |
|------------------|----------|------|
| Windows Security Log | ❌ None | No relevant Security events generated. |
| Windows Firewall | ✅ High | Successfully recorded dropped inbound TCP connection attempts. |
| Sysmon | ⚠️ Partial | Event ID 3 not generated because the connection was not initiated by the local host. |
| Splunk | ✅ High | Successfully centralized Windows Firewall telemetry for investigation. |

---

# 10. Detection Opportunities

The investigation identified several opportunities to improve the detection of reconnaissance activity.

## Detection Logic

Generate an alert when a single source IP performs multiple inbound TCP connection attempts against one or more ports within a short time window and the Windows Firewall records repeated `DROP TCP` events.

---

## Primary Telemetry

- Windows Defender Firewall
- Splunk Enterprise

---

## Supporting Telemetry

- Sysmon (when applicable)
- Windows Security Log

---

## Detection Strategy

Correlate:

- Source IP Address
- Destination IP Address
- Destination Port
- Number of dropped connections
- Time interval

Repeated blocked connection attempts targeting administrative services such as SMB (TCP/445) may indicate reconnaissance activity.

---

## Detection Improvements

Future improvements may include:

- Detect scans across multiple destination ports.
- Apply threshold-based alerting.
- Correlate reconnaissance with later authentication failures.
- Assign higher risk scores when reconnaissance is followed by brute-force attempts.
- Integrate threat intelligence to identify known malicious IP addresses.

---

# 11. Analyst Reflection

At the beginning of the investigation, I expected Sysmon Event ID 3 to be generated during the Nmap TCP Connect Scan.

As the investigation progressed, I discovered that Windows Defender Firewall blocked the inbound TCP SYN packets before a TCP session could be established. This explained why Sysmon Event ID 3 was not generated.

Initially, I considered the absence of Event ID 3 to be a possible logging issue. By correlating evidence from Windows Firewall logs, Sysmon, Splunk, and the Nmap output, I confirmed that the telemetry accurately reflected the behavior of the attack and the defensive controls.

This investigation reinforced the importance of correlating multiple telemetry sources rather than relying on a single log source.

The experience also demonstrated that the absence of an expected event can itself become valuable investigative evidence when interpreted within the correct technical context.

---

# 12. Lessons Learned

- Reconnaissance activity may not generate the same telemetry across all logging sources.
- Windows Defender Firewall provides valuable visibility into blocked inbound network activity.
- Sysmon Event ID 3 records network connections initiated by the local host and should not be expected for blocked inbound connection attempts.
- Correlating multiple telemetry sources provides significantly greater investigative confidence than relying on a single log source.
- Splunk enables centralized investigation by combining endpoint telemetry from multiple Windows logging sources.
- Unexpected or missing events should be investigated before assuming a configuration or logging problem.
- Understanding how telemetry is generated is as important as collecting the telemetry itself.

---

# 13. References

## Documentation References

- Microsoft Windows Defender Firewall Documentation
- Microsoft Windows Event Logging Documentation
- Splunk Documentation
- MITRE ATT&CK
- Sysinternals Sysmon Documentation

---

## Investigation Evidence

- Kali terminal output
- Windows Firewall log (`pfirewall.log`)
- Sysmon Event Viewer
- Splunk search results
- Project screenshots

---

## Related Project Documents

- `mitre-mapping/recon-mitre.md`
- `splunk-detections/recon-detection.md`
- `incident-report/recon-incident-report.md`
- `playbooks/network-reconnaissance-playbook.md`

---

## Next Investigation

Brute Force Detection & Authentication Analysis