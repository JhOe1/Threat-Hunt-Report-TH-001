#  Threat Hunt Report — TH-001  
**Hypothesis-Led Threat Hunting | Microsoft Defender XDR Lab**



##  Overview
This project demonstrates a **hypothesis-led threat hunting investigation** conducted in a custom-built Security Operations lab using:

- Microsoft Defender for Endpoint (EDR)
- Microsoft Sentinel (SIEM)
- Sysmon (Enhanced Logging)
- KQL (Advanced Hunting)

The objective was to simulate a **realistic multi-stage cyber attack** and validate detection capabilities across the environment.



##  Objective
To proactively identify attacker behaviour using **threat intelligence and MITRE ATT&CK techniques**, then:

- Detect malicious activity across telemetry
- Reconstruct the attack timeline
- Improve detection capabilities
- Develop actionable security controls



##  Threat Hunting Hypothesis
> Based on threat intelligence, an attacker may use PowerShell execution, scheduled task persistence, and living-off-the-land techniques to establish and maintain access within a Windows environment.



##  Lab Environment

| Component | Technology |
|----------|-----------|
| SIEM | Microsoft Sentinel |
| EDR | Microsoft Defender for Endpoint |
| Logging | Sysmon (SwiftOnSecurity config) |
| Endpoint | Windows 11 Enterprise , Windows 2022 server|
| Scripting | PowerShell with ScriptBlock Logging |
| Attack Simulation | Atomic Red Team |


##  Lab Setup — Microsoft Security Environment

To simulate a realistic enterprise security environment, I built a cloud-based lab using Microsoft technologies.

###  Step 1 — Identity & Tenant Setup
- Created a Microsoft account (Outlook)
- Registered for a **Microsoft 365 E5 Developer tenant**
- Provisioned a custom domain:
  - `jhoelab1.onmicrosoft.com`
- Created administrative user accounts for testing and simulation

  <img width="3389" height="1402" alt="Image" src="https://github.com/user-attachments/assets/de9a6d8f-dfac-4640-b300-6dbd9dc46f2b" />

---

###  Step 2 — Azure Environment
- Signed up for a **Microsoft Azure free trial**
- Received **$200 free credits**
- Configured Azure subscription for lab deployment

---

###  Step 3 — Virtual Machine Deployment
Deployed two virtual machines to simulate a small enterprise environment:

#### 1. Windows Server 2022 (Infrastructure)
- Role: Simulated enterprise services (e.g. domain services, central management)
- Used for:
  - Identity and access simulation
  - Potential attack surface
 <img width="573" height="263" alt="Image" src="https://github.com/user-attachments/assets/c70b69c4-b909-4707-8491-5f1e983600a8" />


#### 2. Windows 11 Endpoint
- Role: User workstation (target machine)
- Used for:
  - Attack simulation
  - Threat detection and analysis

<img width="605" height="257" alt="Image" src="https://github.com/user-attachments/assets/3df53195-fa1e-45c6-8649-b9e3def5476a" />


---

###  Step 4 — Security Stack Configuration

Configured a full detection and monitoring stack:

- **Microsoft Defender for Endpoint (EDR)**
  - Installed and onboarded endpoint
  - Enabled real-time protection and telemetry
 <img width="3390" height="1404" alt="Image" src="https://github.com/user-attachments/assets/777b8e11-ddff-450b-bf1c-a17582db59ef" />
  

- **Microsoft Sentinel (SIEM)**
  - Connected to Defender XDR
  - Enabled log ingestion and correlation

- **Sysmon (Enhanced Logging)**
  - Installed with SwiftOnSecurity configuration
  - Captures detailed system activity

- **PowerShell Logging**
  - Enabled ScriptBlock logging via Group Policy / registry
  - Provides visibility into attacker commands

---

###  Step 5 — Attack Simulation
- Used **Atomic Red Team** to simulate real-world attacker techniques
- Executed MITRE ATT&CK-based scenarios including:
  - PowerShell execution
  - Persistence mechanisms
  - Discovery techniques
  - Credential access attempts

---

###  Outcome
This lab provides:
- End-to-end visibility across **endpoint, identity, and network activity**
- A controlled environment for **threat hunting and incident response**
- Realistic telemetry for **hypothesis-led investigations**




<img width="3443" height="804" alt="Image" src="https://github.com/user-attachments/assets/1ce34552-9270-4df5-8186-9467a4886fd8" />

##  MITRE ATT&CK Techniques Simulated

| Technique | ID | Tactic |
|----------|----|-------|
| PowerShell Execution | T1059.001 | Execution |
| Scheduled Task Persistence | T1053.005 | Persistence |
| Account Discovery | T1087.001 | Discovery |
| Network Connections Discovery | T1049 | Discovery |
| LSASS Credential Dump | T1003.001 | Credential Access |

---

##  Methodology

1. **Simulate Attack**
   - Used Atomic Red Team to execute techniques sequentially
  
   <img width="1295" height="1178" alt="Image" src="https://github.com/user-attachments/assets/934a2292-5bc6-4a00-a014-378118921e2e" />  

<img width="3383" height="1402" alt="Image" src="https://github.com/user-attachments/assets/5a63465f-f046-4fff-ad59-9567fe21a0b1" />

2. **Query Telemetry**
   - Leveraged KQL in Defender Advanced Hunting
  
     

3. **Analyse Behaviour**
   - Identified anomalies such as:
     - Unusual parent-child processes
     - Off-hours activity
     - Known attacker tools

4. **Build Timeline**
   - Reconstructed attack chain from execution to persistence

5. **Improve Detection**
   - Created custom detection rules based on findings

---

##  Key Findings

###  Suspicious PowerShell Execution (T1059.001)
- `powershell.exe` launched via `explorer.exe`
- Occurred at **02:56 AM (off-hours)**
- Indicates potential post-exploitation activity

---

###  Scheduled Task Persistence (T1053.005)
- Task created with:
