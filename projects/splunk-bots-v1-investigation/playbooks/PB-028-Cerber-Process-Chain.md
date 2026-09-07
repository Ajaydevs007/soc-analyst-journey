# PB-028 — Cerber Process Chain Investigation

## Trigger

A suspicious executable is launched by a script during a ransomware
investigation.

## Step 1 — Identify Payload

Payload:

121214.tmp

## Step 2 — Search Sysmon

Search Event ID 1 process creation events.

## Step 3 — Establish Initial Launch

Sort matching events chronologically and identify the first execution
of 121214.tmp.

## Step 4 — Extract Parent Information

Review:

- ParentProcessId
- ParentImage
- CommandLine
- ProcessId
- User

## Step 5 — Correlate With VBScript

Determine whether the parent process is part of the previously identified
VBScript execution chain.

## Step 6 — Continue the Process Tree

Pivot from the parent and child processes to identify:

- Script interpreter
- Payload
- Additional child processes
- Network connections
- Persistence
- File modification

## BOTS v1 Finding

Payload:

121214.tmp

Initial ParentProcessId:

2568