# Investigation: Special Hex Code Associated with Customized Malware

## Investigation ID

INV-012

---

# Objective

Identify the special hexadecimal code associated with the customized malware discussed in the previous investigation.

---

# Question

> What special hex code is associated with the customized malware discussed in question 11?
>
> Hint: It's not in Splunk.

---

# Background

The previous investigation identified malware associated with Po1s0n1vy's attack infrastructure.

Known malware:

```
MirandaTateScreensaver.scr.exe
```

Known SHA256:

```
9709473ab351387aab9e816eff3910b9f28a7a70202e250ed46dba8f820f34a8
```

The question explicitly states that the answer is not contained in Splunk, so the investigation requires external threat-intelligence research.

---

# Data Sources

- VirusTotal
- Historical VirusTotal Community data
- Historical BOTS v1 walkthroughs / research

---

# Investigation Steps

## Step 1 – Pivot from Previous Malware IOC

The SHA256 identified in the previous investigation was used as the pivot:

```
9709473ab351387aab9e816eff3910b9f28a7a70202e250ed46dba8f820f34a8
```

The corresponding malware was:

```
MirandaTateScreensaver.scr.exe
```

---

## Step 2 – Review VirusTotal

The malware's VirusTotal record was investigated.

The current VirusTotal record provides the following file information:

- Filename: `MirandaTateScreensaver.scr.exe`
- File type: Win32 EXE
- SHA256: `9709473ab351387aab9e816eff3910b9f28a7a70202e250ed46dba8f820f34a8`

The current VirusTotal Community section contains hexadecimal content.

---

## Step 3 – Identify Historical Evidence

The current VirusTotal Community comment is:

```
6c 61 73 63 69 61 74 65 20 6f 67 6e 69 20 73 70 65 72 61 6e 7a 61 2c 20 76 6f 69 20 63 68 27 69 6e 74 72 61 74 65
```

Decoding this produces:

```
lasciate ogni speranza, voi ch'intrate
```

However, this is **not** the hexadecimal string expected by the original BOTS v1 challenge.

Historical BOTS v1 research documents a different VirusTotal Community comment associated with the same malware.

---

# Historical BOTS v1 Evidence

The hexadecimal string expected by the original BOTS v1 answer key is:

```
53 74 65 76 65 20 42 72 61 6e 74 27 73 20 42 65 61 72 64 20 69 73 20 61 20 70 6f 77 65 72 66 75 6c 20 74 68 69 6e 67 2e 20 46 69 6e 64 20 74 68 69 73 20 6d 65 73 73 61 67 65 20 61 6e 64 20 61 73 6b 20 68 69 6d 20 74 6f 20 62 75 79 20 79 6f 75 20 61 20 62 65 65 72 21 21 21
```

---

# Hexadecimal Decoding

The hexadecimal sequence can be decoded as ASCII.

Decoded message:

```
Steve Brant's Beard is a powerful thing. Find this message and ask him to buy you a beer!!!
```

---

# Findings

The special hexadecimal code associated with the customized malware, according to the original BOTS v1 challenge evidence, is:

```text
53 74 65 76 65 20 42 72 61 6e 74 27 73 20 42 65 61 72 64 20 69 73 20 61 20 70 6f 77 65 72 66 75 6c 20 74 68 69 6e 67 2e 20 46 69 6e 64 20 74 68 69 73 20 6d 65 73 73 61 67 65 20 61 6e 64 20 61 73 6b 20 68 69 6d 20 74 6f 20 62 75 79 20 79 6f 75 20 61 20 62 65 65 72 21 21 21
```

Decoded value:

```text
Steve Brant's Beard is a powerful thing. Find this message and ask him to buy you a beer!!!
```

---

# Important Evidence Limitation

The current VirusTotal Community content does not match the historical content expected by the original BOTS v1 challenge.

Therefore, the historical hex string should **not** be represented as something independently observed on the current VirusTotal page.

The answer is based on historical BOTS v1 evidence preserved in challenge walkthroughs / answer-key material.

---

# Investigation Flow

```text
Previous Investigation
        │
        ▼
MirandaTateScreensaver.scr.exe
        │
        ▼
SHA256
9709473ab351387aab9e816eff3910b9f28a7a70202e250ed46dba8f820f34a8
        │
        ▼
VirusTotal
        │
        ▼
Community / Historical Data
        │
        ▼
Historical Hex Code
        │
        ▼
ASCII Decoding
        │
        ▼
"Steve Brant's Beard is a powerful thing..."
```

---

# Conclusion

The original BOTS v1 challenge associates the following hexadecimal code with the customized malware:

```text
53 74 65 76 65 20 42 72 61 6e 74 27 73 20 42 65 61 72 64 20 69 73 20 61 20 70 6f 77 65 72 66 75 6c 20 74 68 69 6e 67 2e 20 46 69 6e 64 20 74 68 69 73 20 6d 65 73 73 61 67 65 20 61 6e 64 20 61 73 6b 20 68 69 6d 20 74 6f 20 62 75 79 20 79 6f 75 20 61 20 62 65 65 72 21 21 21
```

The decoded message is:

```text
Steve Brant's Beard is a powerful thing. Find this message and ask him to buy you a beer!!!
```