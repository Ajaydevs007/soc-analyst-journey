# Room: Log Analysis - Sysmon
**Platform:** Blue Team Labs Online  
**Date:** 06 May 2026  
**Difficulty:** Medium  

## What I Learned
- Understood how Sysmon Event ID 1 logs process creation activity
- Learned the difference between ParentImage, Image, and CommandLine
- Identified malware behavior through self-spawning processes
- Learned how attackers establish reverse shells for remote access
- Understood how LOLBINs are abused during attacks
- Learned how DLL dependencies can reveal malware programming language

## Key Concepts
- ParentImage = process that launched another process
- Image = currently running process
- CommandLine = exact command executed
- Reverse shell allows victim machine to connect back to attacker
- LOLBIN = legitimate Windows binary abused by attackers
- DLL dependencies can reveal malware language

## Tool/Command Used

### Process Creation Analysis
```spl
host="sysmon-events" Event.System.EventID=1
| table Event.EventData.ParentImage Event.EventData.Image Event.EventData.CommandLine
```

### Reverse Shell Detection
```spl
host="sysmon-events" Event.System.EventID=1
| search "nc.exe"
| table Event.EventData.CommandLine
```

### Malware Dependency Analysis
```spl
host="sysmon-events"
| search supply.exe
| table Event.EventData.TargetFilename Event.EventData.Image
```

## Important Findings
- Initial malware file: `supply.exe`
- Reverse shell port: `9898`
- LOLBIN used: `ftp.exe`
- Malware language identified: `Python`
- PowerShell cmdlet used for malware download: `Invoke-WebRequest`

## One Thing to Remember
Always analyze ParentImage, Image, and CommandLine together.
Repeated self-spawning behavior is a strong malware indicator.

## Difficulty I Faced
- Understanding why ParentImage and Image were both `supply.exe`
- Understanding how DLLs reveal malware language
- Understanding reverse shell command structure

Resolved by:
- Breaking down process execution flow step-by-step
- Learning how malware loads runtime DLLs like `python27.dll`
- Reading CommandLine values carefully and analyzing attack flow
