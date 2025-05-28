# 🔐 Hint 08: Privilege Escalation Attempt

## 🎯 Objective
Simulate and detect activities where a user attempts to elevate privileges on a Windows system — including creating new admin accounts, modifying registry keys for persistence, or installing services.

---

## 🧪 Simulations Performed

### ✅ 1. Created a New Admin User
```powershell
net user attacker P@ssword123 /add
net localgroup administrators attacker /add
```
### Tests Performed
| Source   |Event ID | Description                     |
| -------- | -------- | ------------------------------- |
| Security | 4720     | User account creation           |
| Security | 4732     | User added to Admin group       |
| System   | 7045     | New service installed           |
| Sysmon   | 13       | Registry key value modification |
| Sysmon   | 1        | Process creation (`whoami.exe`) |


