# Detection Rule


---

# 1. Rule Information

| Field | Value |
|--------|-------|
| Rule ID | SDR-2026-001 |
| Rule Name | Windows Firewall Network Reconnaissance Detection |
| Severity | Low |
| Status | Active |
| Author | Ajaydev S |
| Last Updated | 05 July 2026 |

---

# 2. Objective

Detect potential network reconnaissance activity targeting Windows endpoints by identifying repeated inbound TCP connection attempts recorded by Windows Defender Firewall.

This detection is designed to identify early-stage network service discovery attempts before an attacker progresses to authentication attacks or exploitation.

The rule uses Windows Firewall telemetry centralized in Splunk Enterprise to identify repeated blocked TCP connection attempts originating from the same source IP address.

---

# 3. ATT&CK Mapping

| Field | Value |
|--------|-------|
| ATT&CK Tactic | Reconnaissance |
| ATT&CK Technique | Network Service Discovery |
| Technique ID | T1046 |
---

# 4. Detection Logic

This detection identifies potential network reconnaissance activity by monitoring Windows Defender Firewall logs for repeated blocked inbound TCP connection attempts originating from the same source IP address.

The rule focuses on identifying reconnaissance behavior rather than individual connection attempts.

## Detection Conditions

The detection triggers when:

- Multiple inbound TCP connection attempts are blocked by Windows Defender Firewall.
- The connection attempts originate from the same source IP address.
- The activity occurs within a defined time window.
- One or more destination ports are targeted.

## Correlation Logic

Correlate the following attributes:

- Source IP Address
- Destination IP Address
- Destination Port
- Firewall Action (`DROP`)
- Time Window

Repeated blocked TCP connection attempts may indicate reconnaissance or port scanning activity.

If the activity is followed by authentication failures (Event ID 4625), successful authentication (Event ID 4624), or process creation events (Event ID 4688 / Sysmon Event ID 1), the alert should be escalated for further investigation.

---

# 5. Required Telemetry

| Source | Required | Purpose |
|---------|:--------:|---------|
| Windows Security Log | Optional | Correlate later authentication activity. |
| Windows Defender Firewall | ✅ Yes | Primary telemetry for blocked inbound TCP connections. |
| Sysmon | Optional | Correlate successful local network connections when applicable. |
| Splunk Enterprise | ✅ Yes | Centralized collection, correlation, and alerting. |

---

# 6. SPL Detection Query

## Investigation Query

The following query displays Windows Defender Firewall events related to blocked inbound TCP connection attempts.

```spl
sourcetype=windows:firewall action=DROP protocol=TCP
| table _time src_ip dest_ip dest_port action protocol
| sort _time
```

Purpose:

- Review individual firewall events.
- Validate the alert.
- Identify the source IP, destination IP, and targeted ports.

---

## SPL Queries

The following query detects repeated blocked inbound TCP connection attempts originating from the same source IP within a five-minute time window.

```spl
sourcetype=windows:firewall action=DROP protocol=TCP
| stats count values(dest_port) AS TargetPorts values(dest_ip) AS TargetHosts earliest(_time) AS FirstSeen latest(_time) AS LastSeen BY src_ip
| where count >= 10
| convert ctime(FirstSeen) ctime(LastSeen)
| rename src_ip AS SourceIP
```

Purpose:

- Aggregate repeated firewall events.
- Identify potential reconnaissance or port scanning activity.
- Reduce alert noise by using a threshold rather than alerting on every dropped packet.

---

# 7. Expected Detection

When the detection rule triggers, the analyst should expect to see:

| Field | Description |
|--------|-------------|
| SourceIP | System initiating the reconnaissance activity |
| TargetHosts | Windows systems targeted by the scan |
| TargetPorts | Ports targeted during the scan |
| Count | Number of blocked connection attempts |
| FirstSeen | Timestamp of the first observed event |
| LastSeen | Timestamp of the most recent observed event |

Example:

| Field | Value |
|--------|-------|
| SourceIP | 10.10.10.10 |
| TargetHosts | 10.10.10.20 |
| TargetPorts | 445 |
| Count | 18 |

---

# 8. Investigation Guidance

When this detection rule triggers, the analyst should validate the alert before determining whether the activity represents legitimate network scanning or malicious reconnaissance.

## Step 1 — Validate the Alert

Confirm:

- Source IP Address
- Destination IP Address
- Destination Port(s)
- Number of blocked connection attempts
- Time window of the activity

---

## Step 2 — Review Windows Firewall Logs

Verify that Windows Defender Firewall recorded repeated `DROP TCP` events from the identified source IP.

Example:

```text
DROP TCP 10.10.10.10 10.10.10.20 40738 445 RECEIVE
```

---

## Step 3 — Review Windows Security Logs

Determine whether the reconnaissance activity was followed by:

- Event ID 4625 (Failed Logon)
- Event ID 4624 (Successful Logon)

The presence of these events may indicate progression to the next stage of the attack.

---

## Step 4 — Review Sysmon

Check for:

- Event ID 3 (Network Connection)
- Event ID 1 (Process Creation)

If Event ID 3 is absent, verify whether the Windows Defender Firewall blocked the connection before a TCP session was established.

---

## Step 5 — Correlate in Splunk

Correlate:

- Source IP
- Destination IP
- Destination Port
- Firewall Action
- Event Timeline

Determine whether:

- Multiple ports were scanned.
- Multiple hosts were targeted.
- Additional malicious activity occurred after reconnaissance.

---

## Step 6 — Determine Alert Disposition

Classify the activity as:

- Benign (authorized scanning)
- Suspicious (unknown source performing reconnaissance)
- Malicious (reconnaissance followed by additional attack stages)

Refer to the **Network Reconnaissance Investigation Playbook** for the complete investigation workflow.
---

# 9. False Positives

The following legitimate activities may trigger this detection rule:

- Authorized penetration testing
- Internal vulnerability scanning
- Asset discovery tools
- Network inventory solutions
- Security compliance assessments

Before escalating the alert, verify whether the source IP belongs to an approved security tool or authorized administrator.

If the activity is expected and documented, classify the alert as a benign true positive and record the justification.
---

# 10. Detection Limitations

This detection rule is effective for identifying blocked inbound TCP connection attempts recorded by Windows Defender Firewall. However, several limitations should be considered.

## Telemetry Dependencies

The rule depends on:

- Windows Defender Firewall logging being enabled.
- Firewall logs being forwarded to Splunk.
- Accurate time synchronization between monitored systems.

If any of these components fail, reconnaissance activity may not be detected.

---

## Network Visibility

This rule detects reconnaissance observed by the monitored Windows endpoint.

It does not provide visibility into:

- Network scans targeting unmanaged systems.
- Traffic that bypasses the monitored host.
- Reconnaissance detected only by network-based security devices.

---

## Attack Variations

Sophisticated attackers may evade this detection by:

- Performing low-and-slow scans over extended periods.
- Distributing scans across multiple source IP addresses.
- Targeting ports below the configured detection threshold.

These techniques may require additional correlation rules or network-based monitoring.

---

## False Negative Considerations

Reconnaissance activity may not trigger this rule if:

- Windows Defender Firewall logging is disabled.
- Firewall events are not ingested into Splunk.
- The scan generates fewer events than the configured threshold.
- The targeted service accepts the connection without generating blocked firewall events.
---

# 11. Detection Tuning

The following improvements can increase the effectiveness of this detection rule.

## Threshold Optimization

Adjust the event threshold based on normal network activity.

Example:

- Small environment: `count >= 5`
- Enterprise environment: `count >= 25`

Thresholds should balance detection capability with alert volume.

---

## Multi-Port Correlation

Enhance the rule to identify scans targeting multiple destination ports rather than a single service.

This improves detection of broad reconnaissance activity.

---

## Multi-Host Correlation

Generate higher-priority alerts when a single source IP scans multiple hosts within a defined time window.

This behavior may indicate automated reconnaissance.

---

## Risk-Based Alerting

Increase the alert severity when reconnaissance is followed by:

- Event ID 4625 (Failed Logon)
- Event ID 4624 (Successful Logon)
- Event ID 4688 (Process Creation)
- Sysmon Event ID 1 (Process Create)

Correlating multiple attack stages reduces false positives and improves detection confidence.

---

## Threat Intelligence Integration

Compare source IP addresses against internal blocklists or external threat intelligence feeds.

Escalate alerts when known malicious infrastructure is identified.

---

# 12. Related Documents

| Document | Reference |
|----------|-----------|
| Investigation | `logs-analysis/recon-analysis.md` |
| MITRE Mapping | `mitre-mapping/recon-mitre.md` |
| Incident Report | `incident-report/recon-incident-report.md` |
| Playbook | `playbooks/network-reconnaissance-playbook.md` |

---

# References

- Microsoft Windows Defender Firewall Documentation
- Splunk Search Reference
- MITRE ATT&CK – T1046 Network Service Discovery
- Sysinternals Sysmon Documentation
- Reconnaissance Detection & Telemetry Analysis Investigation

---

Template Version: 2.0

Last Updated: 2026-07-05

Author: Ajaydev S