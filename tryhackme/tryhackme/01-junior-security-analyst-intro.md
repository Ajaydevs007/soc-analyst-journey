# Room: Junior Security Analyst Intro
**Platform:** TryHackMe  
**Date:** 06 May 2026  
**Difficulty:** Easy  
**Points:** 32

## What I Learned
- SOC has three analyst levels — L1, L2, and L3
- L1 monitors alerts, triages, identifies false positives
- L2 investigates confirmed incidents in deeper detail
- L3 handles advanced threats, threat hunting, forensics
- NOC focuses on network availability, SOC focuses on security
- SIEM collects logs from all systems and correlates events

## Key Concepts
- **L1 Analyst** — first line of defense, monitors SIEM alerts
- **L2 Analyst** — deeper investigation, escalation handling
- **L3 Analyst** — threat hunting, forensics, rule creation
- **False Positive** — alert triggered but no real threat
- **True Positive** — confirmed real security incident
- **Escalation** — passing alert to higher level when confirmed threat

## SOC Workflow
Alert triggered → L1 triages → False positive? Close it.  
True positive? → Investigate → Escalate to L2 → L2 confirms  
→ Incident response begins → Document everything

## One Thing to Remember
As L1 your job is not to solve everything.  
Your job is to correctly identify what is real  
and escalate it to the right person quickly.  
Speed and accuracy in triage is everything.

## Tools/Platforms Mentioned
- SIEM — central log collection and alert platform
- Ticketing system — for documenting and tracking incidents
- Splunk — most common SIEM tool in industry
