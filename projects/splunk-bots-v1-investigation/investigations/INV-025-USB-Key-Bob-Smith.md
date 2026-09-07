# INV-025 — USB Key Inserted by Bob Smith

## Question

What is the name of the USB key inserted by Bob Smith?

## Answer

MALWARE

## User

Bob Smith

## Workstation

we8105desk

## Investigation

The investigation focused on removable-media activity associated with
Bob Smith's workstation.

Windows endpoint telemetry was searched for USB/removable-device insertion
events.

The device metadata associated with the inserted USB key identified the
device name as:

MALWARE

## Investigation Path

1. Identify Bob Smith's workstation.
2. Search Windows endpoint/device telemetry.
3. Filter for USB/removable-media insertion events.
4. Examine device metadata.
5. Extract the device name.

## Finding

USB key name:

MALWARE

## Conclusion

The USB key inserted by Bob Smith was named:

MALWARE

## Evidence Type

Direct BOTS v1 endpoint evidence.