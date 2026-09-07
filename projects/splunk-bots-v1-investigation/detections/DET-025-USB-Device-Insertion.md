# DET-025 — Suspicious USB Device Insertion

## Objective

Detect removable USB devices inserted into an endpoint and identify
potentially suspicious device names.

## Detection Logic

1. Identify the endpoint.
2. Search Windows device/removable-media events.
3. Identify USB insertion activity.
4. Extract device metadata.
5. Review the device name and associated user.

## Investigation Target

User:

Bob Smith

Workstation:

we8105desk

## Finding

USB device name:

MALWARE

## Analyst Note

USB device insertion should be correlated with:

- User
- Hostname
- Timestamp
- Device ID
- Serial number
- Drive letter
- Subsequent file activity
- Process execution

A suspicious device name alone should not be considered proof of malicious
activity.