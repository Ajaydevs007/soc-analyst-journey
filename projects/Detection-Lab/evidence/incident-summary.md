# Sentinel Incident Summary

## Incident

**Name:** SOC-LAB - Kerberos Password Spray Detection

**Severity:** Medium

**Category:** Credential Access

**MITRE ATT&CK:** T1110.003 — Password Spraying

## Detection

The Sentinel Analytics Rule detected multiple failed Kerberos
pre-authentication attempts against different user accounts from
the same source IP within a 15-minute window.

## Source

`10.10.10.30`

## Target Accounts

- analyst01
- user01
- user02

## Event

**Event ID:** 4771 — Kerberos Pre-Authentication Failure

## Detection Logic

The KQL detection groups Event ID 4771 events by source IP and
identifies sources targeting at least three unique accounts.

## Result

The detection successfully generated a Microsoft Sentinel incident.

## Analyst Assessment

This activity is consistent with a controlled Kerberos password-spray
attack performed inside the lab environment.

## Investigation Status

Validated — controlled lab activity.

## Notes

No production systems or accounts were involved.