# REP-021 — Cerber Suricata Signature Analysis

## Executive Summary

Suricata alerts associated with Cerber ransomware were analyzed to identify
the signature that generated the fewest alerts.

The least frequent signature was:

2816763

## Malware

Cerber ransomware

## Detection Technology

Suricata IDS/IPS

## Signature

ETPRO TROJAN Ransomware/Cerber Checkin 2

## Methodology

Cerber-related Suricata alerts were filtered and grouped by
alert.signature_id.

The resulting signature IDs were sorted by alert frequency.

## Finding

Signature ID 2816763 generated the fewest alerts.

## Assessment

The signature represents network detection associated with Cerber
ransomware activity.

The low alert count does not reduce the significance of the detection,
as a single alert may represent important attacker or malware activity.

## Conclusion

The required signature ID is:

2816763