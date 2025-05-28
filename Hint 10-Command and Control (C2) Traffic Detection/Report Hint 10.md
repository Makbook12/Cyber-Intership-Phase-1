# Hint 10 – Command and Control (C2) Traffic Detection

## ✅ Objective

Simulate and detect C2-style beaconing behavior from a Windows machine to a Kali Linux listener. The goal is to identify patterns like regular outbound traffic intervals using Sysmon and visualize them using Kibana.

---

## 🧰 Tools Used

* Windows 10 VM (beacon source)
* Kali Linux VM (listener)
* PowerShell (to simulate beacon)
* Sysmon (logs network connections)
* Winlogbeat (forwards logs)
* Kibana (visualizes logs)

---

## 🔍 Steps Performed

### 1. Started Netcat Listener on Kali

```bash
nc -lvp 5555
```

This listens for incoming connections on port 5555.

---

### 2. Created a PowerShell Beacon Script on Windows

```powershell
while ($true) {
    try {
        $client = New-Object System.Net.Sockets.TcpClient("192.168.43.200", 5555)
        $stream = $client.GetStream()
        $writer = New-Object System.IO.StreamWriter($stream)
        $writer.WriteLine("beacon at $(Get-Date)")
        $writer.Flush()
        $writer.Close()
        $stream.Close()
        $client.Close()
    } catch {}
    Start-Sleep -Seconds 30
}
```

This creates a beacon every 30 seconds to the Kali IP on port 5555.

---

### 3. Observed Incoming Connections on Kali

Kali terminal shows repeating incoming connections every 30 seconds, indicating beaconing behavior.

---

### 4. Searched Logs in Kibana

**Kibana Query:**

```kibana
event.code: "3" AND destination.port: "5555"
```

This showed regular outbound connections from the Windows IP to the listener, all triggered by `powershell.exe`.

---

## 📊 Observations

* Consistent interval (30s) outbound traffic was captured via Sysmon Event ID 3
* Destination IP and port remained constant
* Source process was `powershell.exe`, which is unusual for network communication
* Pattern matches known C2 traffic (e.g., beaconing)

---

## ✅ Conclusion

The simulation demonstrated how basic C2 communication can be modeled using PowerShell and Netcat. Sysmon effectively logged the behavior, and Kibana provided clear visibility of the repeated outbound connections. This validates the environment’s ability to detect C2-like patterns for threat hunting and response.

---

## 🔗 Next Steps

* Add Sigma rule or ELK detection rule for beaconing intervals
* Test with real frameworks (Empire, Cobalt Strike) in a safe lab
* Extend simulation using DNS or HTTP callbacks
* Alert on process-name-based outbound connections on uncommon ports

