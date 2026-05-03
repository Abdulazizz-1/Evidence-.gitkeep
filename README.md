Cloud-Native SOC Operations: Detection & IR Lab

# This project demonstrates the deployment of a fully functional Security Operations Center (SOC) environment in a cloud infrastructure. The objective was to engineer a pipeline for telemetry ingestion, design custom detection logic for brute-force attacks, and simulate a real-world Command and Control (C2) attack to validate the defense stack's effectiveness.

## 🏗️ Architecture & Lab Infrastructure
The environment was hosted on **Vultr** Cloud Infrastructure to simulate enterprise networking and attacker activity:
* **Victim Machine:** Windows Server 2022 (Configured with Sysmon & Elastic Agent).
* **Attacker Machine:** Kali Linux (Hosting Mythic C2).
* **SIEM & Ingestion:** Elastic Stack (ELK, Kibana, Fleet Server).
* **Ticketing System:** osTicket.

![SOC Architecture]<img width="1470" height="956" alt="SOC dia" src="https://github.com/user-attachments/assets/8a69a67a-ad33-495b-a963-984b308c29b6" />




---

# Project Phases & Implementation

### Phase 1: Infrastructure Deployment and Telemetry Ingestion
* Deployed cloud virtual machines on Vultr (`Vultr.jpg`).<img width="1470" height="956" alt="Vultr" src="https://github.com/user-attachments/assets/dc0e129c-9cd2-48d3-a820-357342ff93f3" />

* Installed Elastic Fleet Server and enrolled the Windows machine using Elastic Agent (`11:1.jpg`).<img width="1470" height="956" alt="11:1" src="https://github.com/user-attachments/assets/49eb103e-f2f5-40f3-bd5d-737d29805fa6" />
* Configured Sysmon to monitor detailed process creation and network connections.

---

### Phase 2: Log Analysis & Dashboard Creation
Created custom Kibana dashboards to track authentication and brute-force activities over RDP and SSH. 
* Monitored `Event ID 4624` (Successful Login) and `Event ID 4625` (Failed Login).
* Geolocation parsing for threat analysis.

![Authentication Dashboard](RDP_jdwl.jpg)<img width="1920" height="1080" alt="SSH jdwl" src="https://github.com/user-attachments/assets/02e16eb8-22be-40c6-8577-266fa42eaa08" /><img width="1920" height="1080" alt="11:5" src="https://github.com/user-attachments/assets/a4962412-a2a4-4af8-9ae0-d4ed6979561f" />



### Phase 3: Attack Simulation (Red Team Activity)

Attack Phase I: RDP Brute-Force (Network Vector):
 Executed attack scenarios to validate the SOC detection capabilities:
To simulate a realistic initial access attempt, I executed a password-guessing attack from the Kali Linux (Attacker) machine using **Hydra**.<img width="1920" height="1080" alt="6 ," src="https://github.com/user-attachments/assets/46495928-d74e-4840-ac9a-f49185ce422a" />

* **Tool:** Hydra (Network Logon Cracker).
* **Protocol:** RDP (Remote Desktop Protocol) on Port 3389.
* **Objective:** Crack local administrator credentials for target system access.

 
 Attack Phase II: C2 Establishment (Endpoint Vector):
* Generated malicious payloads using the **Mythic C2 Framework** (Apollo C2 profile).
* Delivered and executed the payload on the victim machine via PowerShell (`7...png`).<img width="1920" height="1080" alt="7" src="https://github.com/user-attachments/assets/3dec3767-9006-4baa-9110-448b732fe35e" />
* Established a successful command-and-control callback (`777...png`).<img width="1920" height="1080" alt="Mythic" src="https://github.com/user-attachments/assets/441ebc9d-9cce-4a8c-a263-2a46f722909d" />


### Phase 4: Threat Hunting & Detection (Blue Team Activity)
Investigated host and network telemetry in Elastic Security:
* Identified the anomalous execution of the Mythic Apollo agent payload (Sysmon Event ID 1).<img width="1920" height="1080" alt="11:3" src="https://github.com/user-attachments/assets/af531213-7538-427d-928e-d41eedca8fc8" />

* Investigated Windows Defender logs indicating tampering and malware detection.<img width="1920" height="1080" alt="11:3" src="https://github.com/user-attachments/assets/303efc7b-0388-429d-b0e0-8214534b9599" />


![Malware Detection](Mythic_detect.jpg)<img width="1920" height="1080" alt="15 :" src="https://github.com/user-attachments/assets/0d8dcac8-b787-413e-a941-8cb926f01a0f" />
<img width="1920" height="1080" alt="Malware" src="https://github.com/user-attachments/assets/4fb6ae44-6281-47af-80b2-64679f5abafb" />


### Phase 5: Incident Management & Ticketing
Managed the incident response lifecycle using an internal ticketing system:
* Created tickets in osTicket documenting the attack phases, affected assets (`AURAFARMER-WINDOWS`), and remediation strategies.

![osTicket Integration](OSt.jpg)<img width="1920" height="1080" alt="OSt" src="https://github.com/user-attachments/assets/5b719f3c-b7c4-415f-b754-a025ffb5f201" />


---

## 📂 Evidence Archive
All 26 screenshots and detailed logs are stored in the `Evidence` directory for verification.
