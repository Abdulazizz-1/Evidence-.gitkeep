Cloud-Native SOC Operations: Detection & IR Lab

# This project demonstrates the deployment of a fully functional Security Operations Center (SOC) environment in a cloud infrastructure. The objective was to engineer a pipeline for telemetry ingestion, design custom detection logic for brute-force attacks, and simulate a real-world Command and Control (C2) attack to validate the defense stack's effectiveness.

## 🏗️ Architecture & Lab Infrastructure
The environment was hosted on **Vultr** Cloud Infrastructure to simulate enterprise networking and attacker activity:
* **Victim Machine:** Windows Server 2022 (Configured with Sysmon & Elastic Agent).
* **Attacker Machine:** Kali Linux (Hosting Mythic C2).
* **SIEM & Ingestion:** Elastic Stack (ELK, Kibana, Fleet Server).
* **Ticketing System:** osTicket.

![SOC Architecture](SOC_dia.jpg)



---

# Project Phases & Implementation

### Phase 1: Infrastructure Deployment and Telemetry Ingestion
* Deployed cloud virtual machines on Vultr (`Vultr.jpg`).
* Installed Elastic Fleet Server and enrolled the Windows machine using Elastic Agent (`11:1.jpg`).
* Configured Sysmon to monitor detailed process creation and network connections.

### Phase 2: Log Analysis & Dashboard Creation
Created custom Kibana dashboards to track authentication and brute-force activities over RDP and SSH. 
* Monitored `Event ID 4624` (Successful Login) and `Event ID 4625` (Failed Login).
* Geolocation parsing for threat analysis.

![Authentication Dashboard](RDP_jdwl.jpg)

### Phase 3: Attack Simulation (Red Team Activity)
Executed attack scenarios to validate the SOC detection capabilities:
* Generated malicious payloads using the **Mythic C2 Framework** (Apollo C2 profile).
* Delivered and executed the payload on the victim machine via PowerShell (`7...png`).
* Established a successful command-and-control callback (`777...png`).

### Phase 4: Threat Hunting & Detection (Blue Team Activity)
Investigated host and network telemetry in Elastic Security:
* Identified the anomalous execution of the Mythic Apollo agent payload (Sysmon Event ID 1).
* Investigated Windows Defender logs indicating tampering and malware detection.

![Malware Detection](Mythic_detect.jpg)

### Phase 5: Incident Management & Ticketing
Managed the incident response lifecycle using an internal ticketing system:
* Created tickets in osTicket documenting the attack phases, affected assets (`AURAFARMER-WINDOWS`), and remediation strategies.

![osTicket Integration](OSt.jpg)

---

## 📂 Evidence Archive
All 26 screenshots and detailed logs are stored in the `Evidence` directory for verification.
