# INV-028 — 121214.tmp Parent Process ID

## Question

The VBScript found in question 204 launches 121214.tmp.
What is the ParentProcessId of this initial launch?

## Answer

2568

## Malware

Cerber ransomware

## Investigation Context

The previous investigation identified the malicious VBScript associated
with the initial Cerber infection.

The VBScript launches:

121214.tmp

The process creation event for 121214.tmp was investigated using
Microsoft Sysmon Event ID 1 telemetry.

The ParentProcessId field identifies the process responsible for launching
121214.tmp.

## Investigation Path

1. Identify the malicious VBScript.
2. Determine that it launches 121214.tmp.
3. Search Sysmon process creation events for 121214.tmp.
4. Locate the initial execution event.
5. Examine the ParentProcessId field.

## SPL

index=botsv1
sourcetype="XmlWinEventLog:Microsoft-Windows-Sysmon/Operational"
"121214.tmp"
| table _time Computer Image CommandLine ProcessId ParentProcessId ParentImage
| sort _time

## Finding

Process:

121214.tmp

ParentProcessId:

2568

## Conclusion

The ParentProcessId of the initial launch of 121214.tmp was:

2568

## Evidence Type

Direct BOTS v1 Sysmon evidence.