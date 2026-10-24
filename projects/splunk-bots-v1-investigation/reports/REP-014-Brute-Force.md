# Incident Report — Joomla Brute-Force Activity

## Report ID

REP-014

## Executive Summary

Analysis of HTTP authentication traffic identified repeated login attempts against the Joomla administrator interface. The first attempted credential was identified from the chronological request sequence; its value is redacted from this public portfolio.

## Findings

| Item | Value |
|---|---|
| Source IP | `23.22.63.114` |
| Destination IP | `192.168.250.70` |
| Target | Joomla Administrator |
| Endpoint | `/joomla/administrator/index.php` |
| First password value | [REDACTED] |

## Assessment

Repeated HTTP POST authentication attempts are consistent with automated password brute-forcing.

## Recommendations

- Block the attacking source if confirmed malicious.
- Reset affected credentials.
- Review successful authentication events.
- Investigate post-authentication activity.
- Implement authentication rate limiting.

## Evidence Handling

The underlying training dataset may contain credential values. They are intentionally omitted from this public report.
