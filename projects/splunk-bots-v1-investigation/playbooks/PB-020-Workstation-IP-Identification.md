# PB-020 — Workstation IP Identification

## Trigger

An investigation identifies a workstation hostname but its IP address
is unknown.

## Step 1 — Search Hostname

Search the SIEM for events containing the workstation hostname.

## Step 2 — Restrict Time

Apply the relevant incident date/time window.

## Step 3 — Aggregate IPs

Use:

stats count by src_ip

## Step 4 — Identify Dominant IP

Sort the results by event count.

## Step 5 — Validate

Confirm the address using reliable network telemetry such as:

- LDAP
- SMB
- Sysmon network events
- Windows authentication
- DNS

## BOTS v1 Finding

Host:

we8105desk

IP:

192.168.250.100

Date:

24 AUG 2016