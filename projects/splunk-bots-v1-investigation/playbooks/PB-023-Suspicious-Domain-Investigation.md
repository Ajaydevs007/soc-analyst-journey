# PB-023 — Suspicious Domain Investigation

## Trigger

A workstation generates network traffic to an unusual external domain.

## Step 1 — Identify the Host

Determine the workstation IP and hostname.

BOTS v1:

we8105desk
192.168.250.100

## Step 2 — Restrict the Time Range

Focus on the known incident date:

24 AUG 2016

## Step 3 — Examine Network Telemetry

Review:

- DNS
- HTTP
- Suricata
- Proxy logs
- Endpoint network connections

## Step 4 — Remove Known Legitimate Traffic

Exclude explainable:

- Microsoft domains
- Windows connectivity checks
- Internal corporate domains
- Known enterprise infrastructure

## Step 5 — Threat Intelligence

Investigate suspicious domains using reputation and IOC sources.

## Step 6 — Establish Timeline

Sort events chronologically and identify the first suspicious domain.

## BOTS v1 Finding

solidaritedeproximite.org