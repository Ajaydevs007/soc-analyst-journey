# REP-024 — Cerber VBScript Field Length

## Executive Summary

Sysmon process creation telemetry from the infected workstation
we8105desk was examined for VBScript execution.

The malicious execution contained the launching executable followed by
the VBScript content in the CommandLine field.

## Host

we8105desk

## Data Source

Microsoft Sysmon

## Field

CommandLine

## Finding

CommandLine length:

4490 characters

## Assessment

The unusually long CommandLine value is consistent with the execution of
the malicious VBScript associated with the initial Cerber infection.

## Conclusion

The required field length is:

4490