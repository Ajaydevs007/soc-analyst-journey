# PB-018 — Credential Discovery to Login Analysis

## Trigger

A credential appears repeatedly during a brute-force investigation.

## Step 1 — Identify Repeated Credential

Extract passwords and identify values appearing more than once.

## Step 2 — Establish Timeline

Sort matching events chronologically.

## Step 3 — Calculate Time Delta

Use:

| delta _time

or:

| transaction <credential>

## Step 4 — Identify Login Context

Compare:

- Source IP
- User-Agent
- HTTP endpoint
- HTTP status
- Request timing

## Step 5 — Investigate Post-Login Activity

Search for:

- Administrative actions
- File uploads
- Configuration changes
- Web shells
- Command execution
- Suspicious outbound traffic

## BOTS v1 Finding

Correct password:

batman

Time between discovery and compromised login:

92.17 seconds