# Splunk Installation & Configuration

## Environment
- Host OS: Windows 11 Home
- Splunk Version: Splunk Enterprise (Free License)
- Installation path: C:\Program Files\Splunk

## Installation Steps
1. Download Splunk Enterprise from https://www.splunk.com/en_us/download/splunk-enterprise.html
2. Run the .msi installer
3. Set admin username and password during setup
4. Default web port: 8000
5. Access Splunk at http://localhost:8000

## Log Sources Configured
Added via Settings → Add Data → Monitor → Local Event Logs:
- WinEventLog:Security
- WinEventLog:System
- WinEventLog:Application

## Process Auditing Setup
Windows Home does not have Local Security Policy (secpol.msc).
Enabled via Command Prompt (Administrator):

    auditpol /set /subcategory:"Process Creation" /success:enable /failure:enable

    reg add "HKLM\Software\Microsoft\Windows\CurrentVersion\Policies\System\Audit" /v ProcessCreationIncludeCmdLine_Enabled /t REG_DWORD /d 1 /f

Enables EventCode 4688 (Process Creation) with full command line logging.

## RDP Configuration Attempt
Windows 11 Home does not support inbound RDP, so OpenSSH was installed as an alternative for attack simulation:
The following was attempted but did not produce results:

    netsh advfirewall firewall add rule name="RDP" dir=in action=allow protocol=TCP localport=3389

    reg add "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server" /v fDenyTSConnections /t REG_DWORD /d 0 /f

    Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -Name "UserAuthentication" -Value 0

Hydra could not authenticate via RDP — Windows 11 Home blocks inbound RDP at the OS level regardless of firewall rules.

## OpenSSH Server Setup
Windows 11 Home does not support inbound RDP.
OpenSSH installed as alternative for attack simulation:

    Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0
    Start-Service sshd
    Set-Service -Name sshd -StartupType 'Automatic'

## Verification
Confirm log ingestion:

    index=main | stats count by source

Expected output: WinEventLog:Security, WinEventLog:System, WinEventLog:Application
