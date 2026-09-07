# PB-022 — Cerber DNS Investigation

## Trigger

Cerber-related Suricata alert or suspicious DNS activity.

## Step 1 — Identify Victim

Determine the source IP associated with the ransomware activity.

BOTS v1 victim:

192.168.250.100

## Step 2 — Search DNS

Search stream:dns events originating from the victim.

## Step 3 — Identify Suspicious Domain

Look for:

- Cerber-related strings
- High-entropy domains
- Unusual TLDs
- Domains temporally associated with ransomware alerts

## Step 4 — Correlate With Suricata

Correlate DNS activity with:

- alert.signature
- alert.signature_id
- flow_id
- timestamp

## Step 5 — Investigate Domain

Determine:

- Domain reputation
- DNS history
- Related IPs
- Other victims
- Related malware

## BOTS v1 Finding

Cerber redirect FQDN:

cerberhhyed5frqa.xmfir0.win