# DET-021 — Cerber Suricata Signature Detection

## Objective

Identify Suricata signatures associated with Cerber ransomware and
determine which signature generated the fewest alerts.

## Detection Logic

1. Search Suricata events.
2. Filter events associated with Cerber.
3. Extract alert.signature_id.
4. Count alerts per signature.
5. Sort by alert count.
6. Identify the least frequent signature.

## SPL

index=botsv1
sourcetype=suricata
event_type=alert
signature=*Cerber*
| stats count by alert.signature_id
| sort count

## Finding

Signature ID:

2816763

Signature:

ETPRO TROJAN Ransomware/Cerber Checkin 2

## Analyst Note

A signature with a low alert count can still be highly significant.
Alert frequency should not be used as the sole measure of severity.