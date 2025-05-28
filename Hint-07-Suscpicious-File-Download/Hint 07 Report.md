# 🚩 Hint 07: Suspicious File Download

## 🎯 Objective

To simulate a suspicious file download using PowerShell and detect the associated events using:

- Sysmon (Event ID 1 and 3)
- PowerShell Logging (Script Block, Event ID 4104)
- Winlogbeat forwarding
- SIEM searches in Kibana/ELK

This exercise reflects real-world tactics used by attackers during initial access or malware delivery via living-off-the-land binaries like `Invoke-WebRequest`.

---

## 🛠️ Lab Setup and Prerequisites

- Windows 10 Virtual Machine (target system)
- Sysmon installed with SwiftOnSecurity config
- Winlogbeat installed and configured to forward logs to ELK
- PowerShell logging (ScriptBlockLogging and ModuleLogging) enabled
- Network connectivity (or localhost web server for simulation)
- ELK stack running (on Kali Linux in my setup)

---

## 🧪 Step-by-Step Simulation

### ✅ Enable PowerShell Script Block Logging
Use:Invoke-WebRequest -Uri "http://example.com" -OutFile "testfile.html"

Command (run as admin):

```powershell
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows\PowerShell\ScriptBlockLogging" /v EnableScriptBlockLogging /t REG_DWORD /d 1 /f
