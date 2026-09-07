# INV-021 — Cerber Suricata Signature Analysis

## Question

Amongst the Suricata signatures that detected the Cerber malware,
which one alerted the fewest number of times?

## Answer

2816763

## Malware

Cerber ransomware

## Data Source

Suricata

## Investigation

The investigation focused on Suricata alerts associated with the Cerber
malware.

First, Suricata events containing "Cerber" were identified.

The alert.signature_id field was then aggregated to determine how many
times each Cerber-related signature alerted.

## SPL

index=botsv1
sourcetype=suricata
event_type=alert
signature=*Cerber*
| stats count by alert.signature_id
| sort count

## Result

The signature with the fewest alerts was:

2816763

## Signature

ETPRO TROJAN Ransomware/Cerber Checkin 2

## Conclusion

Suricata signature ID 2816763 generated the fewest alerts among the
signatures detecting Cerber.

## Evidence Type

Direct BOTS v1 Splunk evidence.