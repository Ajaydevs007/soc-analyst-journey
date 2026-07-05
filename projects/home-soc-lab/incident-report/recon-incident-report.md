# Incident Report


---

# 1. Incident Information

| Field | Value |
|--------|-------|
| Incident ID | HSL-IR-2026-001 |
| Severity | Low |
| Status | Closed |
| Date | 26 June 2026 |
| Analyst | Ajaydev S |

---

# 2. Executive Summary

A simulated reconnaissance attack was performed against the Windows 10 endpoint as part of the Home SOC Lab to evaluate the organization's ability to detect early-stage attacker activity.

The attacker used Nmap to perform a TCP Connect Scan targeting TCP port 445 (SMB). Windows Defender Firewall successfully blocked all inbound connection attempts before a TCP session could be established.

Although Sysmon did not generate Event ID 3 for the activity, Windows Defender Firewall logs provided sufficient telemetry to identify the reconnaissance attempt. Splunk Enterprise successfully centralized and correlated the available evidence, allowing the activity to be investigated and confirmed.

No unauthorized access to the target system occurred, and the incident was classified as **Benign Security Testing** after investigation.
---

# 3. Incident Overview

| Field | Value |
|--------|-------|
| Attack Phase | Reconnaissance |
| ATT&CK Technique | T1046 – Network Service Discovery |
| Source IP | 10.10.10.10 |
| Target | Windows 10 (10.10.10.20) |
| Detection Source | Windows Defender Firewall & Splunk Enterprise |

---

# 4. Timeline

| Time | Activity |
|------|----------|
| 15:17:00 | Nmap TCP Connect Scan initiated from Kali Linux. |
| 15:17:12 | Windows Defender Firewall received inbound TCP connection attempts. |
| 15:17:12 | Firewall blocked the TCP connections and recorded `DROP TCP` events. |
| 15:17:13 | Splunk Universal Forwarder collected the firewall logs. |
| 15:17:14 | Splunk Enterprise ingested the events and the activity was investigated. |
| 15:17:30 | Reconnaissance activity confirmed and incident classified as Benign Security Testing. |

---

# 5. Investigation Summary

The investigation confirmed that the attacker performed a TCP Connect Scan against TCP port 445 (SMB) on the Windows endpoint.

Windows Defender Firewall successfully blocked each inbound connection attempt before a TCP session could be established, preventing access to the target service.

Windows Firewall logs provided the primary evidence for the investigation, while Splunk Enterprise centralized and correlated the available telemetry.

The absence of Sysmon Event ID 3 was determined to be expected behavior because the Windows endpoint did not initiate or establish the network connection.
---

# 6. Impact Assessment

## Systems Affected

- Windows 10 Endpoint

---

## Business Impact

No business impact occurred.

The reconnaissance activity was detected during an authorized Home SOC Lab simulation.

---

## Security Impact

The Windows Defender Firewall successfully prevented access to the SMB service.

No unauthorized authentication, command execution, or persistence activity was observed.

---

# 7. Containment & Response

No containment actions were required because the activity was part of an authorized security exercise.

During the investigation, the following actions were performed:

- Verified Windows Defender Firewall logs.
- Correlated firewall telemetry within Splunk Enterprise.
- Validated the absence of Sysmon Event ID 3.
- Confirmed that Windows Defender Firewall functioned as expected.
- Closed the investigation as Benign Security Testing.

---

# 8. Recommendations

The investigation identified the following recommendations:

- Continue forwarding Windows Defender Firewall logs to Splunk Enterprise.
- Periodically validate Sysmon telemetry and log ingestion.
- Develop automated detection rules for repeated blocked TCP connection attempts.
- Correlate reconnaissance alerts with authentication and process creation events.
- Continue validating defensive controls through controlled security exercises.
---

# 9. Lessons Learned

- Early-stage reconnaissance can be detected through Windows Defender Firewall telemetry.
- Correlating multiple telemetry sources provides greater investigative confidence.
- Missing telemetry should be investigated before assuming a logging failure.
- Defensive controls should be continuously validated using controlled attack simulations.

---

# 10. Related Documents

| Document | Reference |
|----------|-----------|
| Investigation | `logs-analysis/recon-analysis.md` |
| MITRE Mapping | `mitre-mapping/recon-mitre.md` |
| Detection Rule | `splunk-detections/recon-detection.md` |
| Playbook | `playbooks/network-reconnaissance-playbook.md` |
---

Template Version: 2.0

Last Updated: 2026-07-05

Author: Ajaydev S