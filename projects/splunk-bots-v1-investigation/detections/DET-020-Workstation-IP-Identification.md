# DET-020 — Workstation IP Identification

## Objective

Identify the IP address associated with a specific workstation during a
known investigation period.

## Detection Logic

1. Search events associated with the hostname.
2. Restrict the time range to the relevant date.
3. Aggregate events by source IP.
4. Identify the dominant IP address.
5. Validate using protocol-specific telemetry such as LDAP.

## SPL

index=botsv1 host=we8105desk
| stats count by src_ip
| sort - count

## Finding

Hostname:

we8105desk

Date:

24 AUG 2016

IP:

192.168.250.100

## Analyst Note

A workstation may have multiple IP addresses over time. Therefore, the
date/time constraint is important when identifying the most likely address
for an incident.