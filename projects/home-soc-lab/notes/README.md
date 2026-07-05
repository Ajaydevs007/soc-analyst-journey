# Day 1

## Step 1 — Network Connectivity Validation

Configured VirtualBox networking:

- Adapter 1: NAT
- Adapter 2: Internal Network (soc-lab-net)

Static IP assignments:

- Kali eth1: 10.10.10.10
- Windows Ethernet 2: 10.10.10.20

Connectivity validation:

- Kali → Windows successful
- Windows → Kali successful
- 0% packet loss observed

Observation:

Both VMs received 10.0.2.15 on their NAT adapters. This is expected because VirtualBox creates isolated NAT networks for each VM.

Conclusion:

The dedicated attack network (10.10.10.0/24) is functioning correctly and is ready for telemetry generation.


## Step 2 — Windows Audit Policy Configuration

Configured Advanced Audit Policy using Local Group Policy.

Enabled:

- Audit Logon (Success + Failure)
- Audit Process Creation (Success)
- Audit User Account Management (Success + Failure)
- Include command line in process creation events

Applied policies using:

gpupdate /force

Purpose:

Prepare Windows to generate the telemetry required for later attack simulations and investigations.


## Step 3 — Native Process Creation Verification

Executed:

- whoami
- hostname
- ipconfig

Verified Event ID 4688 in:

Windows Logs → Security

Observed:

- Process name information present.
- Command-line information present.

Conclusion:

Windows native auditing is functioning correctly and can provide process execution telemetry for later investigations.


## Step 4 — Sysmon Installation

Installed Sysmon using the original SwiftOnSecurity configuration.

Verified:

- Sysmon service running.
- Sysmon Operational log created.
- Event generation visible in Event Viewer.

Purpose:

Enhance Windows telemetry with process, network, DNS, registry, and file activity events for later SOC investigations.

Additional verification:

Confirmed Sysmon Event ID 1 (Process Create).

Observed fields:

- Image
- CommandLine
- ProcessId
- User

Example:

Image:
C:\Windows\System32\mmc.exe

CommandLine:
"C:\Windows\system32\mmc.exe" "C:\Windows\system32\eventvwr.msc"

Conclusion:

Enhanced process telemetry is functioning correctly.


## Step 5 — Additional Sysmon Verification

Using the SwiftOnSecurity configuration, Sysmon applies filtering rules to reduce telemetry noise.

Observations:

- Event ID 1 generated for process execution.
- Event ID 22 generated during DNS lookups.
- Event ID 13 generated for registry activity.
- Event ID 3 was generated only for connections matching the Sysmon configuration rules.

Conclusion:

The SwiftOnSecurity configuration intentionally prioritizes high-signal telemetry instead of logging every event.


Observation:

The SwiftOnSecurity Sysmon configuration uses include/exclude rules to reduce telemetry noise.

Examples observed:

- Event ID 1 → Process creation
- Event ID 13 → Registry modifications
- Event ID 22 → DNS queries
- Event ID 3 → Only selected network connections

Not every network activity generated Event ID 3 because the XML intentionally prioritizes high-signal events.

Lesson learned:

Missing events do not necessarily indicate failures. Sysmon filtering rules should be investigated before modifying configurations.


## Step 6A — Splunk Receiving Configuration

Configured Splunk Enterprise to receive forwarded events.

Enabled:

- TCP port 9997

Verified:

netstat -ano | findstr 9997

Result:

Splunk is listening for incoming Universal Forwarder connections.


## Step 6B — Universal Forwarder Installation

Installed Splunk Universal Forwarder on the Windows 10 VM.

Configured receiving indexer:

192.168.29.237:9997

Verified:

- SplunkForwarder service running.
- Windows VM can communicate with the Splunk host.

Purpose:

Prepare the victim machine to forward telemetry to Splunk Enterprise.


## Step 6C — Forwarder Inputs Configuration

Configured Universal Forwarder to collect:

- Application logs
- Security logs
- System logs
- Sysmon Operational logs

Restarted the SplunkForwarder service.

Purpose:

Enable Windows telemetry collection for Splunk analysis.


Observation:

WinEventLog inputs do not appear in `splunk list monitor`.

Verification was instead performed using:

- splunk list inputstatus

Observed:

splunk-winevtlog.exe actively reading Windows Event Logs.

Lesson learned:

Windows Event Log inputs are handled differently from file monitors inside Splunk Universal Forwarder.


## Step 6D — Splunk Ingestion Verification

Verified end-to-end telemetry pipeline.

Observed:

- WinEventLog:Security
- WinEventLog:System
- WinEventLog:Application
- WinEventLog:Microsoft-Windows-Sysmon/Operational

Confirmed that logs generated in Windows Event Viewer successfully reached Splunk Enterprise.

Conclusion:

Telemetry ingestion pipeline is functioning correctly.


## Phase 1 — Reconnaissance Detection & Telemetry Analysis

Executed:

sudo nmap -sT 10.10.10.20

Purpose:

Simulate attacker reconnaissance activity.

Checkpoint:

Investigate Event Viewer and Splunk before proceeding with additional scans.


## Phase 1 Observation — Nmap TCP Connect Scan

Executed:

sudo nmap -sT 10.10.10.20

Result:

- Host reachable.
- All 1000 scanned TCP ports reported as filtered.
- No Sysmon Event ID 3 observed.
- No Event ID 3 in Splunk.

Analysis:

The scan did not result in successful TCP connection establishment. The Windows firewall silently filtered incoming connection attempts. Because Sysmon Event ID 3 records network connections initiated by the local host, no Event ID 3 was generated for this inbound filtered scan.

Lesson Learned:

Expected telemetry depends on how the attack interacts with the target system. The absence of an expected event can itself provide useful information when interpreted alongside network behavior.



