# 🛡️ Cloud-Native SOC Operations: Detection & IR Lab

## 📌 Project Overview

This project demonstrates the deployment of a fully functional Security Operations Center (SOC) environment in a cloud infrastructure. The objective was to engineer a pipeline for telemetry ingestion, design custom detection logic for brute-force attacks, and simulate a real-world Command and Control (C2) attack to validate the defense stack's effectiveness.

---

## 🏗️ Architecture & Lab Infrastructure

The environment was hosted on **Vultr** Cloud Infrastructure to simulate enterprise networking and attacker activity:

* **Victim Machine:** Windows Server 2022 (Configured with Sysmon & Elastic Agent).
* **Attacker Machine:** Kali Linux (Hosting Mythic C2).
* **SIEM & Ingestion:** Elastic Stack (ELK, Kibana, Fleet Server).
* **Ticketing System:** osTicket.

<img width="1000" alt="SOC Architecture" src="https://github.com/user-attachments/assets/8a69a67a-ad33-495b-a963-984b308c29b6" />


* Attack Flow
<img width="1000" alt="Attack Diagram" src="https://github.com/user-attachments/assets/dae83160-ba9e-4009-87f7-da8d5c108e55" />


---

# Project Phases & Implementation

### Phase 1: Infrastructure Deployment and Telemetry Ingestion

* Deployed cloud virtual machines on Vultr.

<img width="1000" alt="Vultr" src="https://github.com/user-attachments/assets/dc0e129c-9cd2-48d3-a820-357342ff93f3" />

* Installed Elastic Fleet Server and enrolled the Windows machine using Elastic Agent.

<img width="1000" alt="Elastic Agent Enrollment" src="https://github.com/user-attachments/assets/49eb103e-f2f5-40f3-bd5d-737d29805fa6" />

* Configured Sysmon to monitor detailed process creation and network connections.

---

### Phase 2: Log Analysis & Dashboard Creation

Created custom Kibana dashboards to track authentication and brute-force activities over RDP and SSH.

* Monitored `Event ID 4624` (Successful Login) and `Event ID 4625` (Failed Login).
* Geolocation parsing for threat analysis.

<img width="1000" alt="Authentication Dashboard" src="https://github.com/user-attachments/assets/02e16eb8-22be-40c6-8577-266fa42eaa08" />

<img width="1000" alt="Dashboard" src="https://github.com/user-attachments/assets/a4962412-a2a4-4af8-9ae0-d4ed6979561f" />

---

### Phase 3: Attack Simulation (Red Team Activity)

#### Attack Phase I — RDP Brute-Force (Network Vector)

Executed attack scenarios to validate the SOC detection capabilities.

To simulate a realistic initial access attempt, I executed a password-guessing attack from the Kali Linux (Attacker) machine using **Hydra**.

<img width="1000" alt="Hydra Attack" src="https://github.com/user-attachments/assets/46495928-d74e-4840-ac9a-f49185ce422a" />

* **Tool:** Hydra (Network Logon Cracker).
* **Protocol:** RDP (Remote Desktop Protocol) on Port 3389.
* **Objective:** Crack local administrator credentials for target system access.

#### Attack Phase II — C2 Establishment (Endpoint Vector)

* Generated malicious payloads using the **Mythic C2 Framework** (Apollo C2 profile).

* Delivered and executed the payload on the victim machine via PowerShell.

<img width="1000" alt="Payload Execution" src="https://github.com/user-attachments/assets/3dec3767-9006-4baa-9110-448b732fe35e" />

* Established a successful command-and-control callback.

<img width="1000" alt="Mythic Callback" src="https://github.com/user-attachments/assets/441ebc9d-9cce-4a8c-a263-2a46f722909d" />

---

### Phase 4: Threat Hunting & Detection (Blue Team Activity)

Investigated host and network telemetry in Elastic Security.

* Identified the anomalous execution of the Mythic Apollo agent payload (Sysmon Event ID 1 for Process Creation).

<img width="1000" alt="Mythic Detection" src="https://github.com/user-attachments/assets/96e4197b-f74c-4268-98e9-bcb9b83d75b0" />



* Investigated Windows Defender logs indicating tampering and malware detection.

<img width="1000" alt="Defender Logs" src="https://github.com/user-attachments/assets/68e73371-2fcd-4785-890d-f7d3362e4b94" />

<img width="1000" alt="Malware Detection" src="https://github.com/user-attachments/assets/0d8dcac8-b787-413e-a941-8cb926f01a0f" />

<img width="1000" alt="Malware Analysis" src="https://github.com/user-attachments/assets/4fb6ae44-6281-47af-80b2-64679f5abafb" />

---

### Phase 5: Incident Management & Ticketing

Managed the incident response lifecycle using an internal ticketing system.

* Created tickets in osTicket documenting the attack phases, affected assets (`AURAFARMER-WINDOWS`), and remediation strategies.

<img width="1000" alt="osTicket Integration" src="https://github.com/user-attachments/assets/5b719f3c-b7c4-415f-b754-a025ffb5f201" />

---

## 📂 Evidence Archive

All 26 screenshots and detailed logs are stored in the `Evidence` directory for verification.
