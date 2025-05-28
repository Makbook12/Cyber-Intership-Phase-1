# Hint 6 Report: Unauthorized Access Attempt

## Objective
Simulate unauthorized access behavior on a Windows VM and detect login attempts from unusual geolocations, disabled accounts, or activity outside of business hours using event logs and SIEM alerts.

## Environment Setup
- **Windows 10 VM** with Sysmon and Winlogbeat installed
- **SIEM Platform**: Wazuh/Kibana to collect and analyze logs
- Time and account-based monitoring enabled

## Steps Performed
1. Configure a test user account and disable it.
2. Attempt to log in using the disabled user account.
3. Log in using valid credentials outside of typical working hours.
4. Optionally simulate a login attempt from a different IP/location (e.g., VPN or change system timezone/IP).

## Observations
- **Windows Event ID 4625**: Logged for failed logon attempts with disabled user account.
- **Windows Event ID 4624**: Logged for successful logons, showing timestamp and IP.

## Detection in SIEM
- Use the following filters in Kibana/Wazuh:
  - `event.code:"4625"` → Failed login
  - `event.code:"4624"` → Successful login
- Correlate with user status (disabled), geolocation (if available), and logon timestamps.
- Alert triggered for login by a disabled user and for activity during off-hours.

## Indicators of Attack
- Login attempts using disabled or inactive user accounts.
- Logins from geolocations/IPs not usually associated with the user.
- Logins during off-hours (e.g., early morning or late night activity).

## Conclusion
Unauthorized access attempts are early warning signs of credential abuse or insider threats. By monitoring failed and successful login patterns, especially involving inactive accounts or odd hours, organizations can identify and stop threats before damage is done.
