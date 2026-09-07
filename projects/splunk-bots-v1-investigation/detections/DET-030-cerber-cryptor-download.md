# DET-030 — Cerber Cryptor Payload Download

## Detection Objective

Detect suspicious downloads of files associated with Cerber ransomware activity.

## Detection Logic

Look for HTTP GET requests from known compromised hosts to suspicious infrastructure, especially downloads using image extensions that may disguise executable or malicious content.

## Key Indicators

### Host

`192.168.250.100`

### Suspicious Domain

`solidaritedeproximite.org`

### Downloaded File

`mhtr.jpg`

### Related IP

`92.222.104.182`

## Why This Is Suspicious

A JPEG extension normally indicates an image. In this incident, however, `mhtr.jpg` was associated with the Cerber cryptor payload.

This is an example of masquerading/obfuscation where a malicious payload is given an apparently harmless file extension.

## Detection Idea

Alert when:

- A compromised endpoint contacts known malicious infrastructure.
- An image file is downloaded from suspicious infrastructure.
- The downloaded file is associated with known ransomware activity.
- The file is subsequently executed or loaded by a suspicious process.