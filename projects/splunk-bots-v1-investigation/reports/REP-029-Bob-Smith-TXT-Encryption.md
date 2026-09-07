# REP-029 — Bob Smith TXT File Encryption

## Executive Summary

Sysmon telemetry from Bob Smith's workstation was analyzed to determine
the number of .txt files encrypted by Cerber.

## Workstation

we8105desk

## User

Bob Smith

## Profile

C:\Users\bob.smith.WAYNECORPINC\

## Data Source

Microsoft Sysmon Event ID 2

## Finding

406 distinct .txt files were encrypted.

## Assessment

The file modification activity is consistent with Cerber ransomware
encryption activity during the BOTS v1 ransomware scenario.

## Conclusion

Total distinct encrypted .txt files:

406