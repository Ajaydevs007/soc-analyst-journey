# PB-021 — Cerber Suricata Alert Analysis

## Trigger

Suricata alerts associated with Cerber ransomware.

## Step 1 — Identify Suricata Events

Search the BOTS v1 index for:

sourcetype=suricata

## Step 2 — Filter for Cerber

Search the signature information for the malware name:

Cerber

## Step 3 — Extract Signature IDs

Review:

alert.signature_id

## Step 4 — Count Alerts

Use:

stats count by alert.signature_id

## Step 5 — Sort by Frequency

Sort the resulting signatures by alert count.

## Step 6 — Investigate the Least Frequent Signature

Review the associated:

- Signature name
- Source IP
- Destination IP
- Timestamp
- Protocol
- Network connection
- Related Suricata alerts

## BOTS v1 Finding

Least frequent Cerber signature:

2816763

Signature:

ETPRO TROJAN Ransomware/Cerber Checkin 2