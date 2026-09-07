# PB-024 — VBScript Infection Analysis

## Trigger

Suspicious VBScript execution on an endpoint.

## Step 1 — Identify Host

Determine the infected workstation.

BOTS v1:

we8105desk

## Step 2 — Search Sysmon

Search process creation events for `.vbs`.

## Step 3 — Examine CommandLine

Review the full CommandLine field for:

- Launching executable
- VBScript content
- Obfuscation
- Encoded commands
- Suspicious functions

## Step 4 — Calculate Field Length

Use:

eval field_length=len(CommandLine)

## Step 5 — Identify the Longest Entry

Compare the matching events and investigate the longest command line.

## BOTS v1 Finding

Relevant field length:

4490 characters.