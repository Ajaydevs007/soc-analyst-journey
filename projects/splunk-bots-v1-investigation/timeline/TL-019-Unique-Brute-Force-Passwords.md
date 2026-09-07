# TL-019 — Brute-Force Password Volume

| Sequence | Activity |
|---|---|
| 1 | HTTP authentication traffic identified |
| 2 | Password values extracted from form_data |
| 3 | Duplicate password values removed logically using distinct count |
| 4 | Unique password count calculated |
| 5 | Result = 412 |

## Key Finding

412 unique passwords were attempted.