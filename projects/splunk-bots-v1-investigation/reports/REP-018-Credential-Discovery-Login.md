# REP-018 — Credential Discovery to Compromised Login

## Executive Summary

Analysis of Joomla authentication traffic identified two uses of the
correct administrative password, `batman`.

The events were separated by:

92.17 seconds

## Timeline

Password discovered:

2016/08/10 21:46:33.689

Compromised login:

2016/08/10 21:48:05.858

## Calculation

92.169084 seconds

Rounded:

92.17 seconds

## Assessment

The short interval demonstrates that the attacker used the discovered
credential shortly after the brute-force process identified it.

## Severity

High

Administrative credential compromise can allow further modification and
execution on the compromised CMS.