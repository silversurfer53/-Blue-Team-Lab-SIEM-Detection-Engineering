# SIEM Home Lab: Wazuh Threat Detection & Incident Response

<p align="center">
  <img src="https://img.shields.io/badge/SIEM-Wazuh-blue?style=for-the-badge&logo=wazuh">
  <img src="https://img.shields.io/badge/OS-Windows_%26_Linux-informational?style=for-the-badge&logo=windows">
  <img src="https://img.shields.io/badge/Focus-Threat_Hunting-red?style=for-the-badge&logo=target">
</p>

---

## 1. Executive Summary
This project involved the deployment of a functional **Security Information and Event Management (SIEM)** environment utilizing the **Wazuh** open-source platform. The objective was to establish a centralized logging and monitoring system, simulate an adversary attack (**Brute Force**), and analyze resulting security telemetry to validate detection capabilities.

## 2. Technical Stack & Skills
* **SIEM:** Wazuh (Manager, Indexer, Dashboard)
* **Virtualization:** Oracle VM VirtualBox
* **Operating Systems:** Linux (Ubuntu/CentOS), Windows 10/11
* **Telemetry:** Windows Event Logs, Sysmon, Wazuh Agent
* **Skills:** Network Segmentation, Log Analysis, Adversary Simulation, Incident Documentation, Port Configuration.

## 3. Network Architecture
<p align="center">
  <img width="418" height="397" alt="Image" src="https://github.com/user-attachments/assets/592abff2-eb82-400c-b40a-5a5a36786c8f" />

| Component | Operating System | IP Address | Role |
| :--- | :--- | :--- | :--- |
| **Wazuh Manager** | Linux (Ubuntu) | 192.168.1.50 | Centralized Log Analysis & Alerting |
| **Windows Endpoint** | Windows 10/11 | 192.168.1.60 | Telemetry Source (Agent Installed) |
| **Virtual Network** | NAT / Bridged | 192.168.1.0/24 | Lab Infrastructure |

---

## 4. Implementation Stages

### **Phase 1: Infrastructure Provisioning**
* Deployed the Wazuh Manager OVA within VirtualBox to ensure a pre-configured, hardened analysis environment.
* Configured network adapters to ensure connectivity between the Manager and the Endpoint.

### **Phase 2: Agent Deployment & Telemetry Ingestion**
* Initiated agent deployment via the Wazuh Dashboard.
* Executed a PowerShell command with **Administrative privileges** on the Windows Endpoint to install the Wazuh Agent.
* Verified the connection via the Manager Dashboard, ensuring active status and log heartbeat.

---

## 5. Security Incident Simulation (Brute Force)

* **Objective:** To simulate an **External Brute Force Attack (T1110)** on the Windows RDP/Login service and verify if the SIEM triggers the appropriate alerts.
* **Execution:** Attempted 10+ failed login attempts on the Windows Victim VM using incorrect credentials.
* **Detection & Analysis:** Wazuh successfully identified the malicious activity. The following alert was triggered:
    * **Rule ID:** 5710 (or similar)
    * **Description:** Windows: Multiple failed attempts to log in.
    * **Severity:** Level 10 (High Severity)

<details>
<summary><b>Click to View Evidence (Screenshots)</b></summary>
<br>
<p align="center">
  <img width="505" height="513" alt="Image" src="https://github.com/user-attachments/assets/4060bc1f-a62e-42b7-9528-c5242102a31c" />
  <br>
  <i>Figure 1: Wazuh Dashboard capturing the Brute Force attempt in real-time.</i>
</p>
</details>

---

## 6. Reflections & Lessons Learned
Through this lab, I gained a deep understanding of the **Log Pipeline**:

1.  **Generation:** The OS creates an event log (**Event ID 5710**).
2.  **Collection:** The Wazuh Agent picks up the log and encrypts it.
3.  **Ingestion:** The Manager receives the log and compares it against pre-defined rules.
4.  **Alerting:** The Dashboard visualizes the threat for a SOC Analyst to investigate.

**Key Takeaway:** This setup reinforces the importance of log correlation, as a single failed login is a non-event, but 10 failed logins within a minute is a security incident.

---
