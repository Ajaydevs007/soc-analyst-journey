## Troubleshooting 4 — Missing Sysmon Event IDs

### Problem

Some expected events did not appear.

Examples:

- ping google.com did not create Event ID 3.
- Browser activity produced Event IDs 1, 13 and 22.

### Investigation

Reviewed the SwiftOnSecurity Sysmon configuration.

Observed:

- Include rules
- Exclude rules
- High-signal filtering

### Root Cause

The configuration intentionally reduces telemetry noise.

Not every network activity generates Event ID 3.

### Resolution

Reviewed the XML configuration instead of modifying it immediately.

### Result

Confirmed that missing events were expected behavior.

### Lesson Learned

Missing events do not always indicate failures.

Understand filtering logic before changing configurations.
