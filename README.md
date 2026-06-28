# SOC Home Lab: Splunk SIEM & Attack Detection
> A hands-on blue team lab simulating real-world attack detection using Splunk SIEM on a Windows 11 host with Kali Linux as the attacker machine.

**Executive Summary**
Deployed a Splunk SIEM environment to ingest Windows Event Logs, simulate network reconnaissance and brute-force attacks, and write custom SPL alerts for active intrusion detection. This lab demonstrates practical skills in log analysis, threat hunting, and SIEM rule creation.

## Lab Architecture
See [architecture/lab-diagram.md](architecture/lab-diagram.md)

**Quick Overview:**
- Host: Windows 11 Home — Target machine + Splunk SIEM
- Attacker: Kali Linux (VMware) — Attack simulation
- SIEM: Splunk Enterprise (Free License)

## Tools & Versions
- Splunk Enterprise 10.4.0 — Free License
- Nmap 7.98
- Hydra v9.6
- VMware Workstation
- Kali Linux 2025.4

## Repository Structure
    SOC-Home-Lab/
    ├── architecture/
    │   └── lab-diagram.md
    ├── setup/
    │   └── 01-splunk-install.md
    ├── investigations/
    │   ├── 2026-06-18-nmap-scan.md
    │   └── 2026-06-28-ssh-brute-force.md
    ├── splunk-queries/
    │   └── spl-library.md
    └── README.md

## Status
- [x] Phase 1: Environment setup
- [x] Phase 2: Splunk installed, Windows logs ingesting
- [x] Phase 3: Attack simulation (Nmap scan + SSH brute force)
- [x] Phase 4: Detection + incident reports
- [x] Phase 5: Full documentation

## Log Sources
- WinEventLog:Security — 35,743+ events and growing
- WinEventLog:System — 37,307+ events and growing
- WinEventLog:Application — 22,491+ events and growing

## Investigations
| Date | Attack | Severity | Report |
|------|--------|----------|--------|
| 2026-06-18 | Nmap SYN Scan | Low | [View](investigations/2026-06-18-nmap-scan.md) |
| 2026-06-28 | SSH Brute Force | Medium | [View](investigations/2026-06-28-ssh-brute-force.md) |

## Detection Rules Built
| Rule | EventCode | Trigger |
|------|-----------|---------|
| Brute Force — Network | 4625 | Type 8 failures > 3 |
| Brute Force — Local | 4625 | Type 2 failures > 5 |

## Key Findings
- Nmap SYN scan leaves zero traces in default Windows Event Logs
- SSH brute force detected via EventCode 4625 flood (12 attempts/second)
- Windows 11 Home has significant logging limitations vs Enterprise
- Sysmon required for complete attack visibility

## SPL Queries
Full query library: [splunk-queries/spl-library.md](splunk-queries/spl-library.md)

## Setup Guide
Full setup instructions: [setup/01-splunk-install.md](setup/01-splunk-install.md)

## Planned Improvements
- Add second laptop as dedicated target machine
- Deploy Sysmon for network connection logging
- Enable Windows Firewall logging
- Simulate additional attacks: malware traffic, lateral movement
