# DET-024 — Cerber VBScript Execution

## Objective

Identify suspicious VBScript execution associated with the initial Cerber
ransomware infection.

## Detection Logic

1. Search Sysmon process creation events.
2. Identify CommandLine values containing `.vbs`.
3. Calculate the length of CommandLine.
4. Review unusually long command lines.
5. Investigate the associated parent process and executable.

## SPL

index=botsv1
sourcetype="XmlWinEventLog:Microsoft-Windows-Sysmon/Operational"
host="we8105desk"
*.vbs
| eval field_length=len(CommandLine)
| table _time CommandLine field_length
| sort - field_length

## Finding

CommandLine field length:

4490

## Analyst Note

Extremely long command lines containing scripts can be a useful behavioral
indicator, particularly when launched from Office applications or other
user-facing processes.