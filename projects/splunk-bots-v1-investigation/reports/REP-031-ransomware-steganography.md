# REP-031 — Ransomware Steganography Finding

## Incident

Cerber ransomware activity within the Wayne Corporation environment.

## Finding

The ransomware cryptor was identified as:

`mhtr.jpg`

The file uses an image extension despite being associated with malicious cryptor code.

## Likely Obfuscation Technique

`Steganography`

## Explanation

Steganography is a technique where information is concealed within another seemingly legitimate medium.

In this scenario, malicious content is concealed within or disguised as an image file.

## Confidence

High for the BOTS v1 scenario based on the historical investigation context.

## Important Analyst Note

A `.jpg` extension by itself does not establish steganography. File-type validation and malware analysis would be required to prove the technique in a real-world investigation.

## Related Infrastructure

- `solidaritedeproximite.org`
- `92.222.104.182`

## Related Artifact

`mhtr.jpg`