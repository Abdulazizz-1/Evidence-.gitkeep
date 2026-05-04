#  Threat Hunting & Incident Investigation

This document outlines the chronological timeline, Indicators of Compromise (IoCs), and KQL queries used during the threat hunting phase to uncover the multi-vector attack (Brute-Force & Mythic C2).

##  Attack Execution Timeline (Mythic C2)
*All timestamps are recorded on Apr 24, 2026.*

* **11:43:10.340** - RDP Compromise: Attacker successfully authenticated via RDP using the `Administrator` account.
* **12:37:15.086** - Network Activity: Initial outbound network connection established towards the C2 IP.
* **12:39:39.436** - File Creation: Malicious payload `svchost-AZOZ.exe` dropped in `\users\public\Downloads\`.
* **12:43:40.043** - C2 Beaconing: Additional network interaction detected.
* **12:52:19.050** - C2 Beaconing: Continued telemetry exchange with the Mythic server.
* **12:59:02.000** - Process Execution: Payload executed in the system under PID `2568`.

---

##  Indicators of Compromise (IoCs)
* **Malicious File Name:** `svchost-AZOZ.exe`
* **Original File Name:** `Apollo.exe` (Mythic Framework Profile)
* **File Path:** `\users\public\Downloads\svchost-AZOZ.exe`
* **Process ID (PID):** `2568`
* **File Hash (SHA-1):** `28E89662C46AC2563BFBB482D322B7AEF277AE17`
* **C2 Server IP:** `45.77.220.25`
* **Attacker IP (SSH Brute-Force):** `129.121.84.193`
* **Attacker IP (RDP Brute-Force):** `141.98.11.59`

---

## 🎯 Elastic KQL Detection Queries

### 1. Failed Authentication & Brute-Force
**Linux SSH Failed Attempts:**
`system.auth.ssh.event: * and agent.name: AURAFARMER-Linux-AZO and system.auth.ssh.event: Failed`

**Windows RDP Failed Attempts:**
`event.code: "4625" and agent.name: AURAFARMER-windows`

**Targeted Administrator RDP Brute-Force:**
`event.code: "4625" and agent.name: AURAFARMER-windows and user.name: Administrator`

**Successful RDP Compromise:**
`event.code: 4624 and (winlog.event_data.LogonType: 10 or winlog.event_data.LogonType: 7)`

### 2. Payload Execution & Suspicious Activity
**Execution Detection by Hash or Original Name (Event ID 1):**
`event.code: 1 and (winlog.event_data.Hashes: *32713E907B55A240693A9C26D1C14D9DE35781E8278E96B8148533E1DE70E3C8* or winlog.event_data.OriginalFileName:"Apollo.exe")`

**Suspicious Command-Line (LOLBins):**
`event.code: 1 and event.provider: Microsoft-Windows-Sysmon and (powershell or cmd or rundll32)`

### 3. Network & Defense Evasion
**Malicious Outbound Connections (Event ID 3):**
`event.code: 3 and event.provider: Microsoft-Windows-Sysmon and winlog.event_data.Initiated: true`

**Windows Defender Alerts / Tampering (Event ID 5001):**
`event.code: 5001 and event.provider: Microsoft-Windows-Windows Defender`
