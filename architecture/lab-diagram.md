# Lab Architecture

## Network Topology

    +------------------+          +------------------+
    |   Kali Linux     |          |  Windows 11 Home |
    |   (Attacker)     | -------> |  (Target + SIEM) |
    |                  |          |                  |
    | VMware VM        |          | Splunk Enterprise|
    | IP: 192.168.127.x|          | IP: 192.168.1.234|
    +------------------+          +------------------+

## Component Roles

| Component | Role | IP |
|-----------|------|----|
| Windows 11 Host | Target machine + Splunk SIEM | 192.168.1.xxx |
| Kali Linux VM | Attacker machine | 192.168.127.x |

## Traffic Flow
1. Kali runs attack tools (Nmap, Hydra) against Windows host
2. Windows logs all events to Windows Event Log
3. Splunk ingests Windows Event Log in real time
4. Analyst investigates events in Splunk Search & Reporting

## Limitations
- Single host setup — SIEM and target on same machine
- Windows 11 Home — no inbound RDP, SMB signing blocks Hydra
- No dedicated network tap or firewall logging
- Kali IP not captured in Windows SSH logs (4625 events)

## Planned Improvements
- Add second laptop as dedicated target machine
- Deploy Sysmon for network connection logging
- Enable Windows Firewall logging for nmap detection
