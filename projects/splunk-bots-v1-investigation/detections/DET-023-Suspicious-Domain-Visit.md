# DET-023 — Suspicious Domain Visit

## Objective

Identify suspicious external domains visited by an infected workstation.

## Detection Logic

1. Identify the workstation IP.
2. Restrict the investigation to the incident date.
3. Search HTTP/DNS telemetry.
4. Extract visited hostnames.
5. Remove known legitimate infrastructure.
6. Investigate remaining domains using threat intelligence.
7. Sort chronologically to identify the first suspicious domain.

## SPL

index=botsv1
sourcetype=suricata
src_ip="192.168.250.100"
event_type=http
| dedup http.hostname
| table _time http.hostname
| sort _time

## Finding

Suspicious domain:

solidaritedeproximite.org

## Analyst Note

Domain reputation and contextual investigation should be used before
classifying an unusual domain as malicious.