# Troubleshooting — Windows Firewall Logging

## Problem

During the reconnaissance phase, the Windows Firewall log (`pfirewall.log`) appeared empty even though firewall logging had been enabled.

The initial assumption was that firewall logging was not functioning correctly.

## Investigation

Verified the following:

* Windows Defender Firewall was enabled.
* Logging of both dropped packets and successful connections was enabled.
* The configured log path was:

```
C:\Windows\System32\LogFiles\Firewall\pfirewall.log
```

Checked the active firewall profile using:

```cmd
netsh advfirewall show currentprofile
```

Verified logging settings for all profiles:

```cmd
netsh advfirewall show allprofiles
```

Initially, the log appeared empty when opened in Notepad.

Later inspection confirmed that the log file was being updated.

## Validation

Executed a controlled TCP connection attempt from Kali:

```bash
nmap -Pn -p 445 -sT 10.10.10.20
```

and verified the firewall log using:

```cmd
findstr "10.10.10.10" C:\Windows\System32\LogFiles\Firewall\pfirewall.log
```

Observed entries such as:

```
DROP TCP 10.10.10.10 10.10.10.20 ... 445 RECEIVE
```

## Root Cause

Firewall logging was functioning correctly.

The initial empty file was observed before traffic matching the logging configuration had been generated.

The controlled Nmap scan confirmed that Windows Firewall was logging dropped inbound TCP packets.

## Lesson Learned

An empty firewall log does not necessarily indicate a configuration problem.

Always generate controlled traffic and verify the log contents before concluding that logging is not working.
