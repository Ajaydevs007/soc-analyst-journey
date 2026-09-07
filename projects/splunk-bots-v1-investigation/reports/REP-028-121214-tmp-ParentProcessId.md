# REP-028 — 121214.tmp Parent Process

## Executive Summary

Sysmon process creation telemetry was analyzed to identify the process
responsible for the initial execution of 121214.tmp during the Cerber
ransomware infection.

## Payload

121214.tmp

## Data Source

Microsoft Sysmon

## Process Creation

The initial process creation event for 121214.tmp was identified and its
ParentProcessId field was examined.

## Finding

ParentProcessId:

2568

## Assessment

The ParentProcessId provides a pivot for reconstructing the process tree
associated with the Cerber infection.

## Conclusion

The ParentProcessId of the initial 121214.tmp launch was:

2568