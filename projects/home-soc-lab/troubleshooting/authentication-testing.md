# Authentication Testing Troubleshooting

---

## Troubleshooting — Microsoft Account vs Local Account Authentication

### Problem

During preparation for the brute-force authentication lab, the existing Windows account (`ajayd`) was used for SMB authentication testing. Although the account could be used to log into Windows, authentication consistently failed when accessed through SMB and other remote authentication tools.

Observed behavior:

- `runas /user:ajayd cmd` returned:

```text
RUNAS ERROR: Unable to run - cmd
1326: The user name or password is incorrect.
```

- `smbclient` returned:

```text
NT_STATUS_LOGON_FAILURE
```

- `rpcclient` returned:

```text
NT_STATUS_LOGON_FAILURE
```

Attempting to change the password using:

```cmd
net user ajayd NewPassword
```

returned:

```text
System error 8646 has occurred.

The system is not authoritative for the specified account.
```

### Investigation

Further investigation revealed that the Windows account was associated with a Microsoft Account rather than being a standalone local account.

Evidence included:

```cmd
whoami /all
```

showing:

```text
MicrosoftAccount\<email_address>
```

Because the account was managed by Microsoft's identity provider, Windows did not allow the password to be managed using the local Security Accounts Manager (SAM).

This prevented predictable authentication testing using SMB.

### Resolution

A dedicated local account was created specifically for security testing.

```cmd
net user socuser Lab@12345 /add
```

The new account was verified using:

```cmd
runas /user:socuser cmd
```

which successfully launched a new command prompt.

SMB authentication was then verified using:

```bash
smbclient -L //10.10.10.20 -U socuser
```

The SMB shares were successfully enumerated, confirming that authentication was working correctly.

### Lesson Learned

For repeatable authentication testing, create a dedicated local account instead of using a Microsoft-linked Windows account.

Local accounts provide predictable authentication behavior, easier password management, and cleaner security event generation for SOC investigations.

---

## Troubleshooting — Verifying SMB Authentication Before Hydra

### Problem

Hydra initially failed to authenticate against the Windows SMB service and returned:

```text
[ERROR] invalid reply from target smb://10.10.10.20:445/
```

At first glance, the error appeared to indicate an issue with Hydra or the SMB service.

### Investigation

Rather than assuming Hydra was the problem, authentication was tested independently using native SMB tools.

The following verification steps were performed:

1. Verify local credentials

```cmd
runas /user:socuser cmd
```

Result:

Authentication succeeded.

---

2. Verify SMB authentication

```bash
smbclient -L //10.10.10.20 -U socuser
```

Result:

```text
ADMIN$
C$
IPC$
```

The SMB server successfully authenticated the user and returned the available administrative shares.

Although Samba displayed an SMB1 workgroup warning, authentication itself had already succeeded.

---

3. Verify network connectivity

```bash
nmap -Pn -p445 --script smb-protocols 10.10.10.20
```

Result:

```text
445/tcp open

SMB Dialects:
2.0.2
2.1
3.0
3.0.2
3.1.1
```

This confirmed that:

- TCP port 445 was reachable.
- SMB negotiation succeeded.
- Modern SMB protocols were supported.

### Resolution

The authentication path was verified before retrying Hydra.

This isolated the problem and confirmed that:

- Windows credentials were valid.
- SMB authentication worked correctly.
- Network connectivity was functioning.

Only after these checks was Hydra used to generate authentication events.

### Recommended Verification Workflow

```text
Verify Local Credentials
        │
        ▼
runas
        │
        ▼
Verify SMB Authentication
        │
        ▼
smbclient
        │
        ▼
Verify Network Connectivity
        │
        ▼
Nmap
        │
        ▼
Run Hydra
```

### Lesson Learned

When password attacks fail, verify the authentication mechanism independently before troubleshooting the attack tool.

Testing credentials with `runas` and `smbclient` helps distinguish between credential issues, SMB configuration problems, and tool-specific errors, resulting in faster and more accurate troubleshooting.