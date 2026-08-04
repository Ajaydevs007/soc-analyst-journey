# Detection: Executable Execution from Web Root

## Detection ID

DET-010

---

# Objective

Detect executables launched from web server directories.

---

# Data Source

- Sysmon Event ID 1

---

# SPL Detection Rule

```spl
index=botsv1
sourcetype="XmlWinEventLog:Microsoft-Windows-Sysmon/Operational"
EventCode=1
ParentImage="*\\inetpub\\wwwroot\\*"
| table _time Computer ParentImage Image CommandLine Hashes
```

---

# Detection Logic

Alert when executables located inside the IIS web root spawn new processes.

Legitimate web applications rarely execute binaries directly from the web root.

---

# MITRE ATT&CK

| Technique | ID |
|-----------|----|
| User Execution | T1204 |
| Command and Scripting Interpreter: Windows Command Shell | T1059.003 |

---

# Severity

Critical

---

# Analyst Response

1. Verify executable origin.
2. Review parent-child process chain.
3. Extract hashes.
4. Submit hashes to VirusTotal or internal sandbox.
5. Isolate affected server.