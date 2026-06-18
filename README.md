
# SOC Home Lab

## Environment
- Host: Windows (Splunk SIEM)
- Attacker VM: Kali Linux (VMware)

## Status
- [x] Phase 1: Environment setup
- [x] Phase 2: Splunk installed, Windows logs ingesting
- [x] Phase 3: Attack simulation (Nmap port scan complete)
- [ ] Phase 4: Detection + incident reports
- [ ] Phase 5: Full documentation

## Log Sources
- WinEventLog:Security (35,743+ events)
- WinEventLog:System
- WinEventLog:Application

## Findings Log
### Nmap Scan — 2026-06-18
- 9 open ports discovered on Windows host
- Notable: SMB (445), MySQL (3306), Splunkd (8089) exposed
- OS fingerprinting returned incorrect result (XP/7/2012)
