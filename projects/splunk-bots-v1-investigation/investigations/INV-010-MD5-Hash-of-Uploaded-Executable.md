# Investigation: MD5 Hash of Uploaded Executable

## Investigation ID

INV-010

---

# Objective

Identify the MD5 hash of the executable uploaded by the attacker during the compromise of the Joomla web server.

---

# Question

> What is the MD5 hash of the executable uploaded?

---

# Initial Information

Previous investigations identified the uploaded executable:

```
3791.exe
```

The next objective was to identify the file hash for malware identification and threat intelligence correlation.

---

# Data Sources

- Index: `botsv1`
- Sourcetype: `XmlWinEventLog:Microsoft-Windows-Sysmon/Operational`

---

# Investigation Steps

## Step 1 – Search for the Uploaded Executable

Initially, the investigation searched for events where the executable appeared as the created file or executing process.

```spl
index=botsv1
sourcetype="XmlWinEventLog:Microsoft-Windows-Sysmon/Operational"
(TargetFilename="*3791.exe" OR Image="*3791.exe")
```

### Observation

```
0 Events Found
```

The executable was not present in the expected fields.

---

## Step 2 – Perform a Broad Search

Instead of assuming which Sysmon field contained the filename, a broader search was performed.

```spl
index=botsv1
sourcetype="XmlWinEventLog:Microsoft-Windows-Sysmon/Operational"
3791.exe
```

### Observation

```
69 Events Found
```

---

## Step 3 – Analyze Process Creation Events

One of the returned events was Sysmon Event ID 1 (Process Creation).

Important fields included:

| Field | Value |
|--------|-------|
| Event ID | 1 |
| Image | C:\Windows\SysWOW64\cmd.exe |
| ParentImage | C:\inetpub\wwwroot\joomla\3791.exe |

This confirms that the uploaded executable launched **cmd.exe**, demonstrating that it was not only uploaded but also executed.

---

## Step 4 – Extract File Hash

The Sysmon `Hashes` field contained multiple hashes.

```
SHA1=F5CFD4070EA7D2B40A29F21F9E29AF23341C59EC

MD5=59A1D4FACD7B333F76C4142CD42D3ABA

SHA256=E1A080E61FB1BAF0DA629D34BAEE6F0F9D0E0337BF6CED9F4B3AB9B1C23D91BA

IMPHASH=5B13496CE269DF7709AAB6B1BBF99CD3
```

---

# Findings

The uploaded executable:

```
3791.exe
```

was executed on the compromised server.

Sysmon Process Creation logs recorded the executable as the parent process of **cmd.exe**, allowing extraction of its cryptographic hashes.

---

# Investigation Flow

```
Known Executable
        │
        ▼
Search Sysmon
        │
        ▼
No Direct Match
        │
        ▼
Broad Search
        │
        ▼
Process Creation Event
        │
        ▼
ParentImage = 3791.exe
        │
        ▼
Extract Hashes
```

---

# Evidence Collected

| Artifact | Value |
|----------|-------|
| Uploaded Executable | 3791.exe |
| Event ID | 1 |
| Child Process | cmd.exe |
| Parent Process | 3791.exe |
| MD5 | 59A1D4FACD7B333F76C4142CD42D3ABA |
| SHA1 | F5CFD4070EA7D2B40A29F21F9E29AF23341C59EC |
| SHA256 | E1A080E61FB1BAF0DA629D34BAEE6F0F9D0E0337BF6CED9F4B3AB9B1C23D91BA |

---

# Lessons Learned

The initial investigation assumed that the executable would appear in the `Image` field.

When no results were returned, the search was broadened to all Sysmon fields, revealing that **3791.exe** appeared in the `ParentImage` field.

This approach demonstrates the importance of avoiding assumptions about field placement during investigations.

---

# Conclusion

The MD5 hash of the uploaded executable is:

```
59A1D4FACD7B333F76C4142CD42D3ABA
```