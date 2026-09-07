# TL-028 — Cerber Process Chain

| Sequence | Activity |
|---|---|
| 1 | Initial Cerber infection identified |
| 2 | Malicious VBScript execution identified |
| 3 | VBScript launches 121214.tmp |
| 4 | Sysmon process creation event identified |
| 5 | Initial 121214.tmp launch examined |
| 6 | ParentProcessId identified as 2568 |

## Key Finding

121214.tmp initial ParentProcessId:

2568