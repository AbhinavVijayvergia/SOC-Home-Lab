# SPL Query Library

## Baseline Queries

### All Security Events
    index=main source="WinEventLog:Security"

### All Sources Summary
    index=main | stats count by source

### Process Creation Events (excluding Splunk noise)
    index=main source="WinEventLog:Security" EventCode=4688 Account_Name!=Splunkd
    | table _time, Account_Name, New_Process_Name, Process_Command_Line

### EventCode Frequency Check
    index=main source="WinEventLog:Security" EventCode IN (4624, 4625, 4648, 4720, 4688)
    | stats count by EventCode

## Authentication Queries

### Successful Logons
    index=main source="WinEventLog:Security" EventCode=4624
    | table _time, Account_Name, Logon_Type, Source_Network_Address

### Failed Logons
    index=main source="WinEventLog:Security" EventCode=4625
    | table _time, Account_Name, Logon_Type, Source_Network_Address, Failure_Reason

### Interactive Logons Only (Type 11)
    index=main source="WinEventLog:Security" EventCode=4624 Logon_Type=11
    | table _time, Account_Name, Account_Domain, Logon_Type, Source_Network_Address

### Failed Logon Summary by Type
    index=main source="WinEventLog:Security" EventCode=4625
    | stats count by Account_Name, Failure_Reason, Logon_Type

## Time-Bounding (Incident Investigation)

### Search Within Specific Time Window
    index=main source="WinEventLog:Security" EventCode=4688
    earliest="MM/DD/YYYY:HH:MM:SS" latest="MM/DD/YYYY:HH:MM:SS"
    | table _time, Account_Name, New_Process_Name, Process_Command_Line

Note: Replace earliest/latest with actual incident timestamps when investigating.

## Detection Rules

### Brute Force Detection (Type 8 — Network Cleartext)
    index=main source="WinEventLog:Security" EventCode=4625 Logon_Type=8
    | stats count by Account_Name, Source_Network_Address, Failure_Reason
    | where count > 3

Saved as Splunk alert: runs every 15 minutes, triggers if results > 0.

### Brute Force Detection (Type 2 — Interactive/Local)
    index=main source="WinEventLog:Security" EventCode=4625 Logon_Type=2
    | stats count by Account_Name, Source_Network_Address, Failure_Reason
    | where count > 5
