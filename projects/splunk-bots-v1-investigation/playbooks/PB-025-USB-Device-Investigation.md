# PB-025 — USB Device Investigation

## Trigger

A removable USB device is inserted into an endpoint.

## Step 1 — Identify User

Determine which user was logged into the endpoint.

BOTS v1:

Bob Smith

## Step 2 — Identify Workstation

BOTS v1:

we8105desk

## Step 3 — Search Device Events

Search Windows endpoint telemetry for removable-media and USB insertion
events.

## Step 4 — Extract Device Information

Collect:

- Device name
- Device ID
- Serial number
- Vendor
- Product
- Drive letter
- Insertion timestamp

## Step 5 — Investigate Subsequent Activity

Search for activity occurring immediately after insertion:

- File creation
- File execution
- Script execution
- Process creation
- Malware execution
- Network connections

## BOTS v1 Finding

USB key:

MALWARE