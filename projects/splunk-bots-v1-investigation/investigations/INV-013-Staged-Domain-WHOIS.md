# Investigation: Po1s0n1vy Staged Domain WHOIS Analysis

## Investigation ID

INV-013

---

# Objective

Identify the two disjointed hexadecimal codes contained in the WHOIS information of one of Po1s0n1vy's staged domains and concatenate them into a single answer.

---

# Question

> One of Po1s0n1vy's staged domains has some disjointed "unique" WHOIS information. Concatenate the two codes together and submit as a single answer.

---

# Initial Information

Previous investigations identified Po1s0n1vy infrastructure:

```text
23.22.63.114
```

A domain previously associated with this infrastructure was:

```text
prankglassinebracket.jumpingcrab.com
```

However, the question refers to **one of Po1s0n1vy's staged domains**, so the investigation must enumerate related infrastructure rather than assume that the previously identified domain is the target.

---

# Investigation Method

This question is an external OSINT / WHOIS investigation.

The question does not require Splunk data for the final answer.

Investigation flow:

```text
Known Po1s0n1vy IP
        |
        v
23.22.63.114
        |
        v
Identify associated domains
        |
        v
waynecorinc.com
        |
        v
Historical WHOIS
        |
        v
Unusual WHOIS fields
        |
        +-------------------+
        |                   |
        v                   v
Company Name         Mailing Address
        |                   |
        v                   v
Hex Fragment #1      Hex Fragment #2
        |                   |
        +---------+---------+
                  |
                  v
             Concatenate
                  |
                  v
             Final Answer
```

---

# Step 1 - Identify Associated Domain

Historical infrastructure research identified:

```text
waynecorinc.com
```

as one of the domains associated with Po1s0n1vy's infrastructure.

---

# Step 2 - Perform Historical WHOIS Lookup

A historical WHOIS lookup was required because the information belongs to the historical BOTS v1 scenario.

The relevant WHOIS record contained unusual values in the administrative contact fields.

---

# Step 3 - Analyze WHOIS Information

The administrative contact contained:

```text
Full Name:
LILLIAN ROSE
```

```text
Company Name:
31 73 74 32 66 69 6E 64 67 65 74 73 66 72 65 65 62 65 65 72
```

```text
Mailing Address:
66 72 6F 6D 72 79 61 6E 66 69 6E 64 68 69 6D 74 6F 67 65 74
```

The company name and mailing address contain two separate hexadecimal strings.

---

# Step 4 - Extract First Code

Company Name:

```text
31 73 74 32 66 69 6E 64 67 65 74 73 66 72 65 65 62 65 65 72
```

---

# Step 5 - Extract Second Code

Mailing Address:

```text
66 72 6F 6D 72 79 61 6E 66 69 6E 64 68 69 6D 74 6F 67 65 74
```

---

# Step 6 - Concatenate the Two Codes

The question specifically instructs:

> Concatenate the two codes together.

Therefore:

```text
31 73 74 32 66 69 6E 64 67 65 74 73 66 72 65 65 62 65 65 72
+
66 72 6F 6D 72 79 61 6E 66 69 6E 64 68 69 6D 74 6F 67 65 74
```

Produces:

```text
31 73 74 32 66 69 6E 64 67 65 74 73 66 72 65 65 62 65 65 72 66 72 6F 6D 72 79 61 6E 66 69 6E 64 68 69 6D 74 6F 67 65 74
```

---

# Step 7 - Optional Validation

Decoding the hexadecimal as ASCII produces:

```text
1st2findgetsfreebeerfromryanfindhimtoget
```

This confirms that the two fragments form a single coherent message.

---

# Findings

## Staged Domain

```text
waynecorinc.com
```

## WHOIS Administrative Contact

```text
LILLIAN ROSE
```

## Code Fragment 1

```text
31 73 74 32 66 69 6E 64 67 65 74 73 66 72 65 65 62 65 65 72
```

## Code Fragment 2

```text
66 72 6F 6D 72 79 61 6E 66 69 6E 64 68 69 6D 74 6F 67 65 74
```

## Concatenated Answer

```text
31 73 74 32 66 69 6E 64 67 65 74 73 66 72 65 65 62 65 65 72 66 72 6F 6D 72 79 61 6E 66 69 6E 64 68 69 6D 74 6F 67 65 74
```

---

# Evidence Limitation

This investigation relies on **historical WHOIS information** associated with the BOTS v1 scenario.

Current WHOIS/RDAP services may not reproduce the historical administrative contact information because WHOIS data can change, be redacted, or be replaced by modern RDAP records.

Therefore, the historical WHOIS evidence should be clearly distinguished from current domain-registration data.

---

# Conclusion

The two hexadecimal fragments were found in the historical WHOIS administrative contact information for:

```text
waynecorinc.com
```

The fragments were concatenated to produce:

```text
31 73 74 32 66 69 6E 64 67 65 74 73 66 72 65 65 62 65 65 72 66 72 6F 6D 72 79 61 6E 66 69 6E 64 68 69 6D 74 6F 67 65 74
```