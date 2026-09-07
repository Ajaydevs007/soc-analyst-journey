# DET-022 — Cerber Redirect Domain Detection

## Objective

Identify DNS requests associated with Cerber ransomware activity.

## Detection Logic

1. Identify the infected workstation.
2. Search DNS traffic originating from the workstation.
3. Filter for Cerber-related queries.
4. Correlate the DNS request with Suricata alerts.
5. Investigate the destination domain.

## SPL

index=botsv1
sourcetype="stream:dns"
src_ip="192.168.250.100"
query="*cerber*"
| table _time query
| sort _time

## Finding

Suspicious FQDN:

cerberhhyed5frqa.xmfir0.win

## Associated Malware

Cerber ransomware

## Analyst Note

DNS activity should be correlated with IDS alerts and endpoint telemetry
to establish whether the domain was contacted as part of ransomware
execution.