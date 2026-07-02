# Reconnaissance Analysis

## Objective

Simulate reconnaissance against the Windows victim using an Nmap TCP Connect Scan and investigate the resulting telemetry.

## Attack

Attacker:

* Kali Linux (10.10.10.10)

Target:

* Windows 10 (10.10.10.20)

Command executed:

```bash
nmap -Pn -p 445 -sT 10.10.10.20
```

Result:

```
445/tcp filtered microsoft-ds
```

## Investigation

### Sysmon

No Event ID 3 was generated.

Analysis:

Sysmon Event ID 3 records network connections initiated by processes on the local Windows host.

Because Windows Firewall silently dropped the incoming SYN packets before a TCP connection was established, no local network connection event was created.

### Windows Firewall

The Windows Firewall log recorded multiple dropped TCP packets.

Example:

```
DROP TCP 10.10.10.10 10.10.10.20 40738 445 RECEIVE
```

This confirms that the reconnaissance traffic reached the target but was blocked by the firewall.

## Findings

* The reconnaissance scan reached the Windows host.
* Windows Firewall successfully blocked the incoming TCP connection attempts.
* Sysmon Event ID 3 was not generated because no outbound or established connection existed on the Windows system.

## Conclusion

Reconnaissance activity was confirmed through Windows Firewall logging rather than Sysmon network connection events. This demonstrates that different telemetry sources provide visibility into different stages of network activity.
