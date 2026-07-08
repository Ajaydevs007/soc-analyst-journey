## Troubleshooting — EventCode Field Ambiguity Across Windows Logs

### Problem

Searching only by `EventCode=4625` returned events from the Windows Application log instead of Windows Security log events.

### Cause

The `EventCode` field is not unique across all Windows log channels. Different Windows logs can contain events with the same EventCode value.

Searching only by EventCode returns matching events from every indexed Windows log.

### Resolution

Scope searches to the appropriate log source.

Incorrect:

```spl
index=* EventCode=4625