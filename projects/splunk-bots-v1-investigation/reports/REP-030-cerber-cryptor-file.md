# REP-030 — Cerber Cryptor Payload

## Incident

Cerber ransomware activity against the Wayne Corporation environment.

## Affected Host

`we8105desk`

IP:

`192.168.250.100`

## Finding

The compromised host downloaded a file named:

`mhtr.jpg`

from:

`solidaritedeproximite.org`

The same filename was also observed from:

`92.222.104.182`

## Assessment

Despite having a `.jpg` extension, the file was associated with the Cerber ransomware cryptor code.

The use of an image extension helped disguise the malicious payload.

## Severity

High

## Recommended Response

1. Isolate the compromised endpoint.
2. Block the associated domain and IP.
3. Search the environment for `mhtr.jpg`.
4. Search for connections to the associated infrastructure.
5. Investigate subsequent process execution.
6. Determine the extent of ransomware encryption.
7. Preserve forensic evidence.