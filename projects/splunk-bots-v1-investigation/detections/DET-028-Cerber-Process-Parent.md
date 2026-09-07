# DET-028 — Cerber Process Parent Identification

## Objective

Identify the parent process responsible for launching a suspicious
temporary executable during the Cerber infection.

## Detection Logic

1. Search Sysmon Event ID 1 process creation events.
2. Identify 121214.tmp.
3. Sort matching events chronologically.
4. Select the initial launch.
5. Extract ParentProcessId and ParentImage.
6. Correlate the parent with the preceding VBScript execution.

## SPL

index=botsv1
sourcetype="XmlWinEventLog:Microsoft-Windows-Sysmon/Operational"
"121214.tmp"
| table _time Computer Image CommandLine ProcessId ParentProcessId ParentImage
| sort _time

## Finding

Child process:

121214.tmp

ParentProcessId:

2568

## Analyst Note

Parent-child process relationships are important when investigating
script-based malware because they can reveal the execution chain from
initial script execution to payload execution.