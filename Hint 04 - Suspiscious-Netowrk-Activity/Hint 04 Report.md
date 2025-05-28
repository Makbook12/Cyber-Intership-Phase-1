# Hint 4 – Suspicious Network Activity

## ✅ Objective

Simulate and detect suspicious network behavior, specifically a port scan using Nmap, and observe how the activity is captured and visualized using Sysmon, Winlogbeat, and Kibana.

---

## 🧰 Tools Used

* **Attacker Machine:** Kali Linux VM
* **Target Machine:** Windows 10 VM
* **Logging:** Sysmon (Event ID 3 for network connections)
* **Log Forwarding:** Winlogbeat
* **Log Visualization:** Kibana (via ELK stack)

---

## 🔍 Steps Performed

### Step 1: Identified Windows VM IP Address

On the Windows VM, the following PowerShell command was executed to find the IP address:

```powershell
Get-NetIPAddress -AddressFamily IPv4 | Select-Object IPAddress
```

**Result:**

* IP Address: `192.168.43.206`

*(See screenshot: `powershell_get_ip.png`)*

---

### Step 2: Performed a Stealth Port Scan from Kali Linux

On Kali, Nmap was used with SYN scan to target the Windows VM:

```bash
nmap -sS -T4 192.168.43.206
```

**Result:**

* Host is up.
* Open port found: `5357/tcp` (wsdapi)
* 999 ports filtered (no-response)

*(See screenshot: `nmap_scan_kali.png`)*

---

### Step 3: Checked Sysmon Configuration on Windows

Sysmon was installed and running on Windows. It was started using:

```powershell
Sysmon64.exe -accepteula -i sysmonconfig.xml
```

**Note:** Sysmon is configured to log Event ID 3 (network connections).

---

### Step 4: Logged and Forwarded Events via Winlogbeat

Winlogbeat forwarded Windows event logs, including Sysmon logs, to Logstash/Elasticsearch.

### Step 5: Detected Activity in Kibana

Kibana Discover query used:

```kibana
winlog.event_id: 3 AND source.ip: "192.168.43.206"
```

**Result:**

* Multiple events showing network connection attempts.
* Source IP: `192.168.43.206` (Windows VM)
* Event details captured: timestamps, ports, and process info.


---

## 📸 Screenshots Included

* `powershell_get_ip.png`: PowerShell output showing Windows VM IP
* `nmap_scan_kali.png`: Nmap results from Kali showing scanned ports
* `kibana_event3_scan.png`: Kibana logs showing Event ID 3 (network connect)

---

## 📊 Observations

* Nmap scan from Kali triggered network connections that were detected by Sysmon on Windows.
* Event ID 3 logs captured connection attempts even for filtered ports.
* Winlogbeat successfully forwarded logs to ELK.
* Kibana displayed logs in near real-time, showing key attributes:

  * source/destination IP
  * port numbers
  * process name (e.g., `powershell.exe`, `nmap` if on Windows)

---

## 📅 Log Example (Sysmon Event ID 3)

```json
{
  "event.code": "3",
  "source.ip": "192.168.43.206",
  "destination.ip": "192.168.43.200",
  "destination.port": 4444,
  "process.name": "powershell.exe",
  "agent.hostname": "DESKTOP-9HIEGDP"
}
```

---

## ✅ Conclusion

This exercise simulated reconnaissance activity using Nmap from Kali to a Windows 10 VM. The scan was accurately detected and logged by Sysmon and visualized in Kibana. This proves the system is capable of identifying potentially malicious network behavior, supporting effective threat detection and incident response.

---

## 🔗 Next Steps / Recommendations

* Tune alert rules to trigger on high-frequency Event ID 3 logs to detect port scans.
* Correlate with failed login or lateral movement attempts.
* Expand detection with Suricata or Zeek for packet-level inspection.
* Test EDR visibility for advanced recon tools (e.g., Masscan, Unicornscan).

