# PB-016 — Joomla CMS Admin Compromise

## Trigger

Suspicious authentication against the Joomla administrator interface.

## Step 1 — Identify Source

Determine the source IP submitting credentials.

## Step 2 — Extract Credentials

Extract username and password from HTTP form_data.

## Step 3 — Compare Against Brute Force

Determine whether the source IP is:

- the known brute-force source
- a separate host
- a legitimate administrator
- another potentially compromised system

## Step 4 — Validate Login

Correlate:

- HTTP status
- repeated credential submission
- source IP
- user agent
- subsequent administrative activity

## Step 5 — Investigate Post-Login Activity

Search for:

- Joomla configuration changes
- uploaded files
- web shells
- new administrator accounts
- suspicious HTTP requests
- command execution
- outbound connections

## Step 6 — Containment

If the activity is malicious:

1. Disable compromised credentials.
2. Isolate the affected host if required.
3. Preserve relevant logs.
4. Block confirmed malicious infrastructure.
5. Search for persistence.
6. Review all administrative actions.

## BOTS v1 Finding

The correct password was:

batman

The associated non-brute-force source observed in the dataset was:

40.80.148.42