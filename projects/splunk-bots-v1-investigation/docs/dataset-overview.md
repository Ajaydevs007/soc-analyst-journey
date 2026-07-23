# Dataset Overview

## Dataset

- Dataset: Splunk BOTS v1
- Index: botsv1
- Total Events: 33,413,777
- Hosts: 1,764
- Sourcetypes: 22
- Sources: 24
- Time Range:
  - First Event: 01 Aug 2016 05:30:00
  - Last Event: 29 Aug 2016 11:28:59

## Investigation Goal

The objective of this investigation is to reconstruct the complete attack lifecycle by analyzing multiple log sources, correlating events across systems, identifying attacker behavior, developing detection opportunities, mapping activity to the MITRE ATT&CK framework, and producing professional incident response documentation.

## Initial Observations

- The dataset contains approximately one month of telemetry.
- Multiple log sources are available for correlation.
- The environment appears to represent an enterprise network rather than a single host.
- The investigation will focus on reconstructing the attack timeline from initial access through data exfiltration.