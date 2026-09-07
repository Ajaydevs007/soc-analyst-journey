# REP-022 — Cerber Redirect FQDN

## Executive Summary

DNS activity from the infected workstation was analyzed to identify the
domain associated with the final stage of the Cerber ransomware encryption
process.

## Victim

we8105desk

## IP

192.168.250.100

## Malware

Cerber ransomware

## Finding

cerberhhyed5frqa.xmfir0.win

## Relevant Timestamp

2016-08-24 17:15:12.668

## Assessment

The FQDN was queried by the infected workstation during the ransomware
activity and corresponds to the destination Cerber attempted to direct the
victim toward following encryption.

## Conclusion

The Cerber redirect FQDN was:

cerberhhyed5frqa.xmfir0.win