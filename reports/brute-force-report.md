# Incident Report — SSH Brute Force Attempt
**Date:** 2026-06-28  
**Analyst:** Abhinav Vijayvergia  
**Severity:** Medium  

## Summary
A brute force attack was detected against the SSH service (port 22) 
on host ECO (192.168.1.234). 12 failed logon attempts were recorded 
within a 1-second window.

## Detection
- Tool: Splunk SIEM
- EventCode: 4625 (Failed Logon)
- Logon Type: 8 (Network Cleartext)
- Failure Reason: Unknown user name or bad password

## SPL Query Used
index=main source="WinEventLog:Security" EventCode=4625 Logon_Type=8
| stats count by Account_Name, Source_Network_Address, Failure_Reason
| where count > 3

## Timeline
- 16:16:20 — 6 failed SSH authentication attempts recorded
- All attempts within 1 second — consistent with automated tool

## Indicators of Compromise (IOCs)
- Source IP: Not captured (SSH logging limitation on Windows Home)
- Target Account: ECO$ (machine account)
- Target Port: 22 (SSH)
- Tool Used (lab): Hydra v9.6

## Limitations
- Windows Home SSH does not log source IP in 4625 events
- Machine account (ECO$) logged instead of target username (Acer)
- Sysmon required for complete SSH attack visibility

## Recommendation
- Deploy Sysmon for network connection logging
- Set Splunk alert: trigger if Type 8 failures > 3 in 5 minute window
- Consider disabling SSH if not required, or restrict to specific IPs
