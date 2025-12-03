# suspicious-powershell-execution

Azure Sentinel Detection & Incident Response Project Suspicious PowerShell Execution – End-to-End Threat Detection, Investigation & Containment

**Project Overview**

In this project, I designed and implemented an end-to-end detection and response workflow using Azure Sentinel, Microsoft Defender for Endpoint, Log Analytics Workspace, and an Azure Windows VM.
The goal was to simulate malicious PowerShell behavior, detect it using a scheduled analytics rule, investigate the resulting incident, and contain the threat using endpoint isolation.

1️⃣ Detection Engineering – Creating a Custom Scheduled Analytics Rule
Objective

Detect suspicious PowerShell activity involving the “Invoke” parameter, commonly used for downloading payloads (Invoke-WebRequest, Invoke-Expression).

**Configuration**

* Rule Type: Scheduled Analytics Rule
* Query Frequency: Every 4 hours
* Lookup Period: Last 24 hours
* Data Source: Microsoft Defender / DeviceEvents
* Entity Mapping: Device, Account, Process Command Line

<img width="1000" height="402" alt="creating scheduled analytics rule" src="https://github.com/user-attachments/assets/227a85b5-e7d4-4bc1-a0e8-d9ea4e9c19cc" />

**Detection Logic (KQL Used)**

<img width="1000" height="331" alt="Screen Shot 2025-12-03 at 4 29 53 pm" src="https://github.com/user-attachments/assets/d5c60e51-e5c5-4fab-9d4b-ca0c7aa0969c" />



This rule was designed to trigger anytime a PowerShell command utilizing "Invoke" is executed.


**2️) Attack Simulation – Executing Suspicious PowerShell Command**

To trigger the detection, I executed the following command on my Azure Windows VM:

powershell.exe -ExecutionPolicy Bypass -Command Invoke-WebRequest -Uri https://raw.githubusercontent.com/joshmadakor1/lognpacific-public/refs/heads/main/cyber-range/entropy-gorilla/eicar.ps1 -OutFile C:\programdata\eicar.ps1

Observed Behavior

* PowerShell was launched with ExecutionPolicy Bypass
* A remote script (eicar.ps1) was downloaded from GitHub
* The file was written to C:\ProgramData, a location often used by attackers
* This successfully triggered the analytics rule and created an incident in Sentinel.

**3)Incident Response – Investigation in Microsoft Sentinel**

After the incident fired, I performed a full SOC-style investigation:

* Assigned Incident to Myself
* Changed incident status to Active and began triage.
* Verified Affected Assets
    * Device: win-ameer-projec
    * User Account: ameer

* Suspicious Command Executed:

* Included Invoke-WebRequest with URL

* Used ExecutionPolicy Bypass

✔ Deep-Dive Investigation

Using Defender for Endpoint and Log Analytics:

Reviewed process execution chain

Confirmed that the downloaded script was not executed

Checked for:

Persistence mechanisms

Lateral movement

Additional downloads

New processes spawned

Registry changes

No additional malicious activity was detected.

4️⃣ MITRE ATT&CK Mapping
Tactic	Technique	Description
Execution	T1059.001 – PowerShell	Suspicious PowerShell command execution
Command & Control	T1105 – Ingress Tool Transfer	Downloading remote script using web request
Execution	T1059 – Command and Scripting Interpreter	Script-based execution
5️⃣ Containment – Isolating the Compromised Machine

To prevent any further malicious activity:

✔ Isolated the VM using Microsoft Defender for Endpoint

This action:

Blocks all inbound/outbound communication

Keeps only secure channels active

Prevents lateral movement or remote control

Isolation successfully contained the threat.

6️⃣ Final Outcome
✔ Detection Rule Worked as Intended

The custom Sentinel rule accurately identified the suspicious PowerShell behavior.

✔ Incident Response Completed End-to-End

From initial detection to investigation to containment.

✔ Environment Left Fully Secured

No malicious execution occurred beyond the test download.

🔎 Key Skills Demonstrated
Detection Engineering

Custom KQL query development

Scheduled analytics rule creation

Entity mapping in Sentinel

Threat Hunting & Investigation

Process chain analysis in Defender

Log Analytics workspace investigation

MITRE ATT&CK mapping

Incident Response & Containment

Incident assignment and triage

Evidence collection

Endpoint isolation using EDR

📂 Final Notes (Portfolio Highlight)

This project showcases my ability to:
✔ Build custom detection rules
✔ Simulate realistic threat behavior
✔ Perform hands-on SOC investigations
✔ Apply MITRE ATT&CK techniques
✔ Contain threats using enterprise EDR tooling
