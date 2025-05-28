# Hint 1 Report: Brute Force Login Attempt

## Objective
Simulate a brute force login scenario on a Windows VM to observe and capture failed login events in the SIEM platform.

## Environment Setup
- **Windows 10 VM** with Sysmon and Winlogbeat installed
- **SIEM Platform**: Wazuh/Kibana configured to receive logs
- **PowerShell** used to simulate login attempts

## Steps Performed
1. Open PowerShell on the Windows VM.
2. Execute the following script to simulate multiple failed login attempts:

```powershell
for ($i=1; $i -le 10; $i++) {
    net use \\127.0.0.1\IPC$ /user:FakeUser WrongPassword
}
'''
## Observations
Windows Event ID 4625 was logged for each failed login attempt.

The logs included details such as:

Username: FakeUser

IP Address: 127.0.0.1

Failure Reason: Unknown username or bad password

## Detection in SIEM
In Kibana/Wazuh, Event ID 4625 appeared multiple times.

Filter used: event.code:"4625"

The SIEM dashboard showed a spike in failed login attempts over a short time window.
