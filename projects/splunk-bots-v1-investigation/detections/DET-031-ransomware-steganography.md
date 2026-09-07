# DET-031 — Suspicious Image-Based Payload

## Detection Objective

Identify potentially malicious payloads disguised as legitimate image files.

## Technique

Steganography

## Detection Indicators

Look for:

- Image files downloaded from suspicious infrastructure.
- Image files associated with malware execution.
- Unexpectedly large image files.
- Image files containing anomalous executable data.
- Network downloads followed by suspicious process activity.
- Image files accessed by scripts or executable processes.

## Relevant BOTS v1 Artifact

`mhtr.jpg`

## Associated Malware

Cerber ransomware

## Detection Logic

An image file should be investigated when:

1. It originates from suspicious infrastructure.
2. It is downloaded immediately before malicious activity.
3. Its file characteristics do not match a normal image.
4. It is accessed or executed by an unusual process.

## False Positive Considerations

Legitimate applications frequently download images.

Therefore, the image extension alone should not trigger a high-confidence alert.

Correlate:

- Source reputation
- Destination reputation
- Process activity
- File metadata
- File type/magic bytes
- User activity
- Endpoint telemetry