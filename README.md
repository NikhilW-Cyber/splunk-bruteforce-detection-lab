# Splunk Brute-Force Detection Lab (Windows + Kali)

This project demonstrates a Blue Team SIEM workflow using Splunk:
- Collect Windows Security logs using Splunk Universal Forwarder
- Simulate failed network logons from a Kali attacker
- Detect brute-force behavior using Splunk SPL
- Trigger a Splunk alert when repeated failed logons occur from the same source IP

---

## Lab Architecture

Kali (Attacker) ➜ Windows 10 (Victim) ➜ Splunk Enterprise (Ubuntu SIEM)

- Splunk Server: Ubuntu (Splunk Enterprise)
- Log Source: Windows 10 (Windows Security Event Logs)
- Attacker: Kali Linux (SMB authentication attempts)

---

## What I Built

- Splunk receives Windows Security logs in `index=main`  
- Kali generates failed network logons against Windows  
- Windows logs EventCode **4625 (Failed Logon)** with **Logon_Type = 3 (Network Logon)**  
- Splunk detection aggregates failures by attacker IP  
- Splunk alert triggers when the threshold is met  

---

## Key Detection Logic (SPL)

### Confirm failed network logons (4625, Logon_Type=3)
````markdown
```spl
index=main source="WinEventLog:Security" EventCode=4625 Logon_Type=3
```

## Project Evidence (Screenshots)

### 1. Splunk server running and receiving connections
![Splunk Running](/screenshots/01_splunk_running_and ports.png)

### 2. Windows Splunk Universal Forwarder service running
![Windows Forwarder](screenshots/02_windows_forwarder_running.png)

### 3. Windows Forwarder configured to send logs to Splunk
![Forward Server Config](screenshots/03_forward_server_config.png)

### 4. inputs.conf configured to collect Windows Event Logs
![Inputs Config](screenshots/04_inputs_conf_wineventlog.png)

### 5. Forwarder restarted after configuration
![Forwarder Restart](screenshots/05_forwarder_restart.png)

### 6. Windows logs successfully appearing in Splunk
![Windows Logs](screenshots/06_windows_logs_success.png)

### 7. Splunk showing available log sources
![Sources Breakdown](screenshots/07_sources_breakdown.png)

### 8. Search for failed login events
![Failed Login Search](screenshots/08_failed_login_search.png)

### 9. Count of failed login attempts per user
![Failed Login Count](screenshots/09_failed_login_count_by_user.png)

### 10. Kali attacker IP detected in Splunk logs
![Kali Detection](screenshots/10_kali_failed_network_log_detected.png)

### 11. Windows network profile changed to private
![Network Private](screenshots/11_network_changed_to_private.png)

### 12. Brute force detection triggered
![Bruteforce Trigger](screenshots/12_bruteforce_detection_triggered.png)

### 13. Splunk alert configuration
![Alert Configuration](screenshots/13_alert_configuration.png)

### 14. Multiple failed login attempts generated
![Multiple Attempts](screenshots/14_multiple_failed_attempts.png)

### 15. Final brute force detection query result
![Detection Query](screenshots/15_bruteforce_detection.png)

### 16. Splunk alert triggered proof
![Alert Triggered](screenshots/16_alert_triggered_proof.png)
