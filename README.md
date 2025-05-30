# Phase 1: Reflection & Evaluation Questions

### 1. What is the role of SIEM in modern cybersecurity?

SIEM (Security Information and Event Management) platforms like Wazuh and ELK Stack help collect, analyze, and correlate security event logs across systems. They are critical in detecting threats like brute force attacks, malware, and lateral movement in real-time, aiding both proactive defense and forensic investigations.

---

### 2. What challenges did you face while setting up your lab?

Some of the challenges I faced included:
- Properly configuring Sysmon and Winlogbeat to forward logs.
- Ensuring the SIEM (Wazuh/ELK) received data from the Windows VM.
- Managing limited system resources during virtualization.
- Troubleshooting firewall settings and connectivity issues between the Windows VM and the SIEM platform.

---

### 3. What are the differences between Sysmon logs and Windows Security logs?

- **Sysmon Logs**: Provide granular system-level telemetry such as process creation (Event ID 1), network connections (Event ID 3), and registry modifications (Event ID 13).
- **Windows Security Logs**: Focus on high-level security events like logon attempts (Event ID 4624, 4625), group membership changes, and user account activity.

Sysmon offers richer context for threat hunting, while Windows Security logs are better for audit trails.

---

### 4. How does a brute force attack appear in logs? Mention specific Event IDs.

Brute force attacks appear as repeated failed logon attempts:
- **Windows Security Event ID 4625**: Indicates a failed login.
  - Fields: TargetUsername, WorkstationName, Source IP
- A high number of 4625 events from a single IP over a short period indicates a brute force attempt.

---

### 5. How would you detect a login outside normal business hours?

- Review **Event ID 4624** (successful logon) and check the **Logon Time** field.
- Correlate timestamps with organization’s business hours (e.g., 9 AM – 6 PM).
- Flag logons occurring at night or on weekends.
- SIEM rules or Kibana dashboards can be used to automate such detection.

---

### 6. Describe how RDP lateral movement is tracked in event logs.

RDP lateral movement can be tracked by:
- **Windows Event ID 4624** (Type 10 - RemoteInteractive): Indicates remote desktop logon.
- **Sysmon Event ID 3**: Captures network connections made during the session.
- Monitoring frequent logons between systems within a short time can indicate lateral movement.

---

### 7. What is the risk of log tampering, and how can we detect it?

**Risk**:
- Attackers may delete or modify logs to cover tracks after a breach.

**Detection**:
- Monitor for gaps in log timelines or sudden drops in log volume.
- SIEM alerts on **event log clear** activities (e.g., **Windows Event ID 1102**).
- Use immutable log storage or forward logs in real-time to a centralized SIEM.

---

### 8. What improvements would you make in your lab setup if given more time?

- Automate the entire environment setup using scripts (PowerShell, Bash).
- Add more attack simulation tools like Atomic Red Team or Caldera.
- Improve SIEM dashboards for easier correlation.
- Include threat intelligence feeds for enrichment.

---

### 9. How will this phase help you in real-world interviews or jobs?

- Gained hands-on experience with SIEM tools and threat detection.
- Learned how to interpret logs and correlate events to real-world attacks.
- Improved ability to explain technical setups and detection logic—crucial in interviews and security analyst roles.
- Developed a GitHub portfolio showcasing practical skills.

---

### 10. What was your biggest takeaway from Phase 1?

Understanding how every attack leaves traces in logs and how to detect those patterns using SIEM was the biggest takeaway. The hands-on experience bridged the gap between theory and practice, enhancing both technical knowledge and investigative thinking.

---
