# INV-024 — Cerber VBScript Field Length

## Question

During the initial Cerber infection a VB script is run. The entire script
from this execution, pre-pended by the name of the launching .exe, can be
found in a field in Splunk. What is the length in characters of the value
of this field?

## Answer

4490

## Host

we8105desk

## Investigation

The infected workstation's Sysmon process creation events were searched
for executions containing a `.vbs` extension.

The CommandLine field contained the launching executable followed by the
VBScript content.

The length of the CommandLine field was calculated using Splunk's len()
function.

## SPL

index=botsv1
sourcetype="XmlWinEventLog:Microsoft-Windows-Sysmon/Operational"
host="we8105desk"
*.vbs
| eval field_length=len(CommandLine)
| table _time CommandLine field_length
| sort - field_length

## Result

Longest CommandLine field:

4490 characters

## Important Note

The requested value is the length of the Splunk field, not merely the
length of the VBScript.

## Conclusion

The length of the relevant field was:

4490

## Evidence Type

Direct BOTS v1 Splunk evidence.