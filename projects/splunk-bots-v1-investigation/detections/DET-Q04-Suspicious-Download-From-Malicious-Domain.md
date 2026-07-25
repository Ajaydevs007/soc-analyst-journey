# Detection: Suspicious Download from Malicious Domain

## Detection ID

DET-Q04

---

# Objective

Detect outbound HTTP requests from internal hosts to known malicious domains that may indicate malware retrieval, website defacement resources, or command-and-control activity.

---

# Data Source

- Splunk Enterprise
- Sourcetype: `fgt_utm`

---

# SPL Detection Rule

## Detect Downloads from Malicious Websites

```spl
index=botsv1 sourcetype=fgt_utm
catdesc="Malicious Websites"
| table _time srcip dstip hostname url action
```

---

## Detect Downloads from External Hosts

```spl
index=botsv1 sourcetype=fgt_utm
action="passthrough"
catdesc="Malicious Websites"
| stats count by srcip dstip hostname url
```

---

## Detect Downloads by Internal Web Servers

```spl
index=botsv1 sourcetype=fgt_utm
srcip="192.168.250.70"
| table _time hostname url catdesc action
```

---

# Detection Logic

This detection identifies outbound HTTP requests made by internal systems to domains categorized as malicious.

Such activity may indicate:

- Malware downloads
- Website defacement resources
- Command and Control communication
- Payload retrieval

---

# MITRE ATT&CK

| Technique | ID |
|-----------|----|
| Ingress Tool Transfer | T1105 |
| Application Layer Protocol | T1071.001 |

---

# Severity

High

---

# False Positives

- Authorized malware analysis
- Security research
- Threat intelligence validation

---

# Analyst Response

1. Verify destination domain.
2. Review downloaded file.
3. Investigate process responsible for the request.
4. Determine whether the file was executed or deployed.
5. Isolate compromised host if required.

---

# References

Downloaded File:

```
poisonivy-is-coming-for-you-batman.jpeg
```

Malicious Host:

```
prankglassinebracket.jumpingcrab.com
```