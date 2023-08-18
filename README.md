# CloudHoneynet
In this project, I designed and implemented an Azure Cloud Honeynet environment that emulates real vulnerable systems to **capture**, **analyze** and **respond** to live malicious traffic. The goal was to enhance my understanding of cybersecurity threats and incident response through practical, real-world experience.

---

![image](https://github.com/Muneer44/CloudHoneynet/assets/117259069/3fb58841-b63d-4241-b5de-28ec40305a53)

---

# Methodology 
- <ins>**Azure Infrastructure Setup:**</ins> Deployed an Azure environment consisting of virtual machines, SQL Server, Azure Active Directory, storage accounts, Key Vault, and more.
  
- <ins>**SIEM Integration with Azure Sentinel:**</ins> Leveraged Sentinel (SIEM) to monitor and analyze the captured data from various sources and respond to the incidents, all within a central hub.
  
- <ins>**Analytic Rules and Alerts:**</ins> I crafted custom analytic rules using *Kusto Query Language* to detect and trigger specific attack patterns like brute force attempts, manipulation of firewall, identification of malicious files and other potential threats.
  
- <ins>**Threat Mapping with Workbooks:**</ins> Using Azure Sentinel's workbooks, I transformed threat data into maps that outlines the origin of attacks. This provides a comprehensive overview of the threat landscape.

- <ins>**Incident Response Simulation:**</ins> Simulated incident response on triggered alerts by analyzing the collected data, identifying attack vectors, and applying appropriate mitigation strategies by developing a sample Playbook based on *NIST 800-61 - Incident Response* guidelines.
  
- <ins>**Baseline and Remediation Metric Comparison:**</ins> Observed the environment for 24 hours in its vulnerable state, capturing essential security metrics as a baseline for comparison against the 24-hour metrics of the environment after implementing remediation measures.
    
- <ins>**Regulatory Compliance:**</ins> Ensured regulatory compliance by enabling *NIST 800-53* and *PCI DSS* security policies alongside hardening the environment's security posture.

--- 

# Initial Architecture Before Implementing Security Measures

<img src="https://github.com/Muneer44/CloudHoneynet/assets/117259069/6ece485f-2c8a-4cb5-8c62-346fb71dacff" alt = "Initial Architecture diagram" width="480" height="465">  

  
> Initially, resources were intentionally set up for public exposure to attract potential threat actors and stimulate malicious activity. This deployment involved VMs with open Network Security Groups (NSGs) and built-in firewalls, allowing unrestricted access from any source.  In addition, resources like key vaults and storage accounts were also publicly accessible, lacking appropriate restrictions or private endpoint security controls.

---  

# Architecture After Implementation of Security Measures

<img src="https://github.com/Muneer44/CloudHoneynet/assets/117259069/76572122-d284-4c29-af8c-7fd4ab053cda" alt = "Initial Architecture diagram" width="510" height="565">

> Here, I enhanced security by implementing strict Network Security Group (NSG) configurations, allowing only authorized traffic from trusted IP address. I optimized built-in firewalls on virtual machines and replaced public endpoints with Private Endpoints for sensitive resources like Key Vaults and Storage Accounts, ensuring limited access within the virtual network.  
> Additionally, I implemented *'SC-7: Boundary Protection controls from NIST 800-53'* to further enhance protection against other threats and adhere to regulatory compliance.

---  

# Visualizing Attacks: Mapping the Source of Attacks
![NSG map](https://github.com/Muneer44/CloudHoneynet/assets/117259069/3b6ae72a-1a22-4c05-a89b-b41f08b0156f)

<details>
<summary><h1>🚩Toggle for more maps</h1></summary>
  
![Linux Map](https://github.com/Muneer44/CloudHoneynet/assets/117259069/a3003e0f-3a9c-494e-9c36-e9a4daf9d7f7)

![Win map ](https://github.com/Muneer44/CloudHoneynet/assets/117259069/6495fed2-bc49-428d-b0be-1da893909db7)

![Win RDP](https://github.com/Muneer44/CloudHoneynet/assets/117259069/79b75435-4774-4094-aeec-23b4bed15ec7)

</details>

These maps were crafted by ingesting IP Geolocation databases into Sentinel's watchlist. This data was then used in KQL queries to build custom workbooks, which would ultimately showcase the maps highlighting the countries from which attacks originated.
> Note: The above maps were generated while the environment was in a vulnerable state. 

---

# Metric Comparison: Pre and Post Security Enhancements

## Pre-Enhancement :

| **Event type**  | **Count** |
| ------------- | ------------- |
Security Events (Windows VMs) |	127457
Syslog (Linux VMs) |	1879
SecurityAlert (Microsoft Defender for Cloud) |	3
SecurityIncident (Sentinel Incidents) |	181
NSG Inbound Malicious Flows Allowed |	2713

## Post-Enhancement :

| **Event type**  | **Count** |
| ------------- | ------------- |
Security Events (Windows VMs) |	783
Syslog (Linux VMs) |	23
SecurityAlert (Microsoft Defender for Cloud) |	0
SecurityIncident (Sentinel Incidents) |	0
NSG Inbound Malicious Flows Allowed |	0

- The comparison of metrics between the environment's pre and post security enhancements over a 24-hour period demonstrates the effectiveness of the implemented security controls, resulting in zero incidents Post Security Enhancement.
- The Security Events (783) and Syslog (23) counts are identified as **false positives** originating from internal systems like (NT-Authority).

---  
<details>
<summary><h1>Highlevel Walkthrough</h1></summary>

## Azure Ecosystem Utilized:

- Virtual Machines (2x Windows, 1x Linux)
- Azure Network Security Group (NSG)
- Azure Virtual Network (VNet)
- Log Analytics Workspace with Kusto Query Language (KQL) Queries
- Azure Key Vault for Secure Secrets Management
- Azure Storage Account for Data Storage
- Microsoft Sentinel for Security Information and Event Management (SIEM)
- Microsoft Defender for Cloud to Protect Cloud Resources
- PowerShell for Automation
- NIST SP 800-61 Revision 2 for Incident Handling Guidance
- NIST SP 800-53 Revision 5 for Security Controls

---  

# Phase 1 : Deployment 
- Create Windows VM
  - Instal SQL server and SQL Server Management Studio
  - Enable logs from SQL Server to be ingested to Win Event Viewer -[[Ref](https://learn.microsoft.com/en-us/sql/relational-databases/security/auditing/write-sql-server-audit-events-to-the-security-log?view=sql-server-ver16)]
- Create Linux VM
- Create Attacker (Win) VM
- Apply open Network Security Groups (NSG) configuration
  
![VMs](https://github.com/Muneer44/CloudHoneynet/assets/117259069/c4959815-4dbd-4777-86ee-6b83cc3a20c6)
![image](https://github.com/Muneer44/CloudHoneynet/assets/117259069/3faa39cd-7a04-4114-9b16-432ebe40cfa3)

---

# Phase 2 : Setup Logging and Monitoring
## Log collection 
- Create Storage account
- Create Log Analytics Workspace (LAW)
- Enable MS Defender for Cloud
- Create flow logs for NSG - (Vnet Traffic analytics)
- Create and configure Data Collection Rule for Win and Linux VMs
- Create another custom rule for Win Firewall and Defender Logs
- Install Log Agents on VMs
- Setup Azure Tenant level logging (Azure Active Directory logs)
- Setup Azure Subscription level logging (Azure Activity logs)
- Setup Azure Resource level logging. (Azure Resource Manipulation logs)

---

## MS Sentinel (SIEM) Configuration  

![MS SIEM](https://github.com/Muneer44/CloudHoneynet/assets/117259069/c72ea948-ee55-440d-b6c7-21a8171bab92)

- Download Geo-IP Databases - [[Ref](https://github.com/AndiDittrich/GeoIP-Country-Lists)]
- Create Container and upload Geo-Ip Databases
  
  ![Container](https://github.com/Muneer44/CloudHoneynet/assets/117259069/6a315143-d5d5-49c1-90be-7dce01c0df30)

- Create MS Sentinel Watchlists
  
  ![Watchlists](https://github.com/Muneer44/CloudHoneynet/assets/117259069/63923683-f613-47fe-a335-9167750c1c9e)  

- Create Workbooks to generate [MAPs](#visualizing-attacks-mapping-the-source-of-attacks)

---

## Analytics, Alerts and Incident Generation
- Create MS Sentinel Analytics (Alert Rules)

  ![Analytics](https://github.com/Muneer44/CloudHoneynet/assets/117259069/dc6c03b6-f93e-4b52-a41f-be8cab6dbf7b)

---
 
## Simulate Attack Attempts: Trigger Alerts
 ### Powershell Scripts
 - [AAD_Brute_Force_Simulator]()
 - [SQL-Brute-Force-Simulator]()
 - [EICAR-Malware-Generator]()
   > Note: It's not a malicious malware. EICAR is a test file used to check antivirus softwares. [Read more](https://www.eicar.org/download-anti-malware-testfile)

 ### KQL Query Cheat Sheet 
 - [Windows Security Event Log]()
 - [Linux Syslog]()
 - [Azure Active Directory]()
 - [Azure Storage Account]()
 - [Azure Key Vault]()
 - [Network Security Groups]()

---

## Incident Response  
![NIST IR](https://github.com/Muneer44/CloudHoneynet/assets/117259069/b67747ff-89db-47f7-84e2-384758054727)  

> Incidents occured (Alerts Triggered)
  
![Incidents](https://github.com/Muneer44/CloudHoneynet/assets/117259069/127bb98c-d67e-4964-94f4-f8bb79fd0c86)  

> investigation the incidents
  
![image](https://github.com/Muneer44/CloudHoneynet/assets/117259069/2b71eaea-9697-4801-b3fb-603e878b1305)
![Investigation 2](https://github.com/Muneer44/CloudHoneynet/assets/117259069/a5ae2674-b233-4a4e-b415-0cef77011861)

---

## Security Enhancements
- Limit Resources public exposure [IMG](#architecture-after-implementation-of-security-measures)
  
  -Disable public access
  - Create Private endpoint access
- Implement secure NSG configuration
  - Delete wide open all traffic NSG entry
  - Allow RDP from specific host     
- Deploy NSG on subnet
- Fulfill *NIST 800-53 R5 - Boundary Protection*

---


</details>




