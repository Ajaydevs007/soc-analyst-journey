# INV-029 — Bob Smith TXT File Encryption

## Question

The Cerber ransomware encrypts files located in Bob Smith's Windows
profile. How many .txt files does it encrypt?

## Answer

406

## Host

we8105desk

## User

Bob Smith

## Profile

C:\Users\bob.smith.WAYNECORPINC\

## Malware

Cerber ransomware

## Investigation

Sysmon Event ID 2 events were examined because Cerber modifies the file
creation timestamp during the encryption process.

The search was restricted to .txt files located inside Bob Smith's
Windows profile.

Duplicate events were removed logically by using Splunk's distinct-count
function against TargetFilename.

## SPL

index=botsv1
host=we8105desk
sourcetype="XmlWinEventLog:Microsoft-Windows-Sysmon/Operational"
EventCode=2
TargetFilename="C:\\Users\\bob.smith.WAYNECORPINC\\*.txt"
| stats dc(TargetFilename) as encrypted_txt_files

## Result

406 distinct .txt files.

## Conclusion

Cerber encrypted 406 distinct .txt files in Bob Smith's Windows profile.

## Evidence Type

Direct BOTS v1 Sysmon evidence.