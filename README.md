
# SOC Home Lab

## Environment
- Host: Windows (Splunk SIEM)
- Attacker VM: Kali Linux (VMware)

## Status
- [x] Phase 1: Environment setup
- [x] Phase 2: Splunk installed, Windows logs ingesting
- [x] Phase 3: Attack simulation (Nmap scan + SSH brute force complete)
- [x] Phase 4: Detection + incident reports
- [ ] Phase 5: Full documentation

## Log Sources
- WinEventLog:Security (35,743+ events)
- WinEventLog:System
- WinEventLog:Application

## Tools & Techniques
- Nmap 7.98 — network reconnaissance, port scanning
- Hydra v9.6 — SSH brute force simulation
- SPL — custom detection queries and automated alerts
- Windows Event Logs — EventCodes 4624, 4625, 4648, 4688

## Findings Log
### Nmap Scan — 2026-06-18
- 9 open ports discovered on Windows host
- Notable: SMB (445), MySQL (3306), Splunkd (8089) exposed
- OS fingerprinting returned incorrect result (XP/7/2012)

### SSH Brute Force — 2026-06-28
- Simulated brute force using Hydra from Kali Linux
- 12 failed logon attempts detected (EventCode 4625, Logon Type 8)
- Detection rule built in SPL, saved as automated Splunk alert
- Incident report written: reports/brute-force-report.md
