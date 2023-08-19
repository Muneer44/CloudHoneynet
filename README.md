# CloudHoneynet
In this project, I designed and implemented an Azure Cloud Honeynet environment that emulates real vulnerable systems to **capture**, **analyze** and **respond** to live malicious traffic. The goal was to enhance my understanding of cybersecurity threats and incident response through practical, real-world experience. Additionally, I conducted a metric comparison of the outcomes before and after implementing security measures for an accurate evaluation of the effectiveness of the employed security enhancements.

---

![image](https://github.com/Muneer44/CloudHoneynet/assets/117259069/3fb58841-b63d-4241-b5de-28ec40305a53)

---

# Methodology 
- <ins>**Azure Infrastructure Setup:**</ins> Deployed an Azure environment consisting of virtual machines, SQL Server, Azure Active Directory, storage accounts, Key Vault, and more.
  
- <ins>**SIEM Integration with Azure Sentinel:**</ins> Utilized Sentinel (SIEM) to monitor and analyze the captured data from various sources and respond to the incidents, all within a central hub.
  
- <ins>**Analytic Rules and Alerts:**</ins> I crafted custom analytic rules using *Kusto Query Language* to detect and trigger specific attack patterns like brute force attempts, manipulation of firewall, identification of malicious files and other potential threats.
  
- <ins>**Threat Mapping with Workbooks:**</ins> Using Azure Sentinel's workbooks, I transformed threat data into maps that outline the origin of attacks. This provides a comprehensive overview of the threat landscape.

- <ins>**Incident Response Simulation:**</ins> Simulated incident response for triggered alerts by analyzing the collected data, identifying attack vectors, and applying appropriate mitigation strategies by developing a sample Playbook based around ***NIST 800-61 - Incident Response*** guidelines.
  
- <ins>**Baseline and Remediation Metric Comparison:**</ins> Observed the environment in its vulnerable state for 24 hours, capturing essential security metrics as a baseline for comparison against the 24-hour metrics of the environment after implementing remediation measures.
    
- <ins>**Regulatory Compliance:**</ins> Ensured regulatory compliance by enabling ***NIST 800-53*** security policy alongside hardening the environment's security posture.

--- 

# Architecture Before Implementing Security Measures

<img src="https://github.com/Muneer44/CloudHoneynet/assets/117259069/6ece485f-2c8a-4cb5-8c62-346fb71dacff" alt = "Initial Architecture diagram" width="480" height="465">  

  
> Initially, resources were intentionally set up for public exposure to attract potential threat actors and stimulate malicious activity. This deployment involved VMs with open Network Security Groups (NSGs) and disabled built-in firewalls, which allowed unrestricted access from any source.  In addition, resources like key vaults and storage accounts were also made publicly accessible, lacking appropriate restrictions or private endpoint security controls.

---  

# Architecture After Implementation of Security Measures

<img src="https://github.com/Muneer44/CloudHoneynet/assets/117259069/76572122-d284-4c29-af8c-7fd4ab053cda" alt = "Initial Architecture diagram" width="510" height="565">

> Here, I enhanced security by implementing strict Network Security Group (NSG) configurations, allowing only authorized traffic from trusted IP addresses. I optimized the built-in firewalls on virtual machines and replaced public endpoints with Private Endpoints for sensitive resources like Key Vaults and Storage Accounts, ensuring limited access within the virtual network.  
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

# Metric Comparison of Incidents: 

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

- The comparison of metrics between the environment's pre- and post-security enhancements over a 24-hour period demonstrates the effectiveness of the implemented security controls, resulting in zero incidents after the  Security Enhancement.
- The Security Events (783) and Syslog (23) counts are identified as **false positives** originating from internal systems like NT-Authority.
- Note: The absence of active users on these systems reduces threat visibility. The presence of active users could potentially attract more threats.
---  
<details>
<summary><h1>🌟 Comprehensive Walkthrough</h1> </summary>
 
<details>
<summary><h1>Azure Components Utilized: </h1></summary>
  
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

</details>

<details>
<summary><h1>Phase 1 : Initial Setup </h1></summary>
  
- Create Windows VM
  - Install SQL server and SQL Server Management Studio
  - Enable logs from SQL Server to be ingested to Windows' Event Viewer - [[Reference](https://learn.microsoft.com/en-us/sql/relational-databases/security/auditing/write-sql-server-audit-events-to-the-security-log?view=sql-server-ver16)]
- Create Linux VM
- Create Attacker Windows VM
- Create OPEN (insecure) Network Security Group (NSG) entries
  
![VMs](https://github.com/Muneer44/CloudHoneynet/assets/117259069/c4959815-4dbd-4777-86ee-6b83cc3a20c6)
![image](https://github.com/Muneer44/CloudHoneynet/assets/117259069/3faa39cd-7a04-4114-9b16-432ebe40cfa3)

---

</details>

<details>
<summary><h1>Phase 2 : Setup Logging and Monitoring</h1></summary>
   
## Log Collection 
- Create Storage account
- Create Log Analytics Workspace (LAW)
- Enable MS Defender for Cloud
- Create flow logs for NSG - (Vnet Traffic analytics)
- Create and configure Data Collection Rule for Win and Linux VMs
- Create another custom rule for Win Firewall and Defender Logs
- Install Log Agents on VMs
- Setup Azure Tenant-level logging (Azure Active Directory logs)
- Setup Azure Subscription level logging (Azure Activity logs)
- Setup Azure Resource-level logging. (Azure Resource Manipulation logs)

## Microsoft Sentinel (SIEM) for Monitoring, Alerting and Analysis     

![MS SIEM](https://github.com/Muneer44/CloudHoneynet/assets/117259069/c72ea948-ee55-440d-b6c7-21a8171bab92)

## Setup SIEM :
- Download Geo-IP Databases - [[Ref](https://github.com/AndiDittrich/GeoIP-Country-Lists)]
- Create Container and upload Geo-Ip Databases
  
  ![Container](https://github.com/Muneer44/CloudHoneynet/assets/117259069/6a315143-d5d5-49c1-90be-7dce01c0df30)

- Create MS Sentinel Watchlists
  
  ![Watchlists](https://github.com/Muneer44/CloudHoneynet/assets/117259069/63923683-f613-47fe-a335-9167750c1c9e)  

- Create Workbooks to generate [Maps](#visualizing-attacks-mapping-the-source-of-attacks)

- Create MS Sentinel Analytics (Alert Rules)

  ![Analytics](https://github.com/Muneer44/CloudHoneynet/assets/117259069/dc6c03b6-f93e-4b52-a41f-be8cab6dbf7b)

---

</details>

<details>
<summary><h1>Phase 3 : Simulate Attacks and Examine Logs</h1></summary> 
  
 ### Powershell Scripts
 - [AAD_Brute_Force_Simulator](https://github.com/Muneer44/CloudHoneynet/blob/main/Powershell%20Scripts/AAD_Brute_Force_Simulator.ps1)
 - [SQL-Brute-Force-Simulator](https://github.com/Muneer44/CloudHoneynet/blob/main/Powershell%20Scripts/SQL-Brute-Force-Simulator.ps1)
 - [EICAR-Malware-Generator](https://github.com/Muneer44/CloudHoneynet/blob/main/Powershell%20Scripts/EICAR-Malware-Generator.ps1)
   > Note: It's not a malicious malware. EICAR is a test file used to check antivirus softwares. [Read more](https://www.eicar.org/download-anti-malware-testfile)

 ### KQL Query Cheat Sheet 
 > Verify simulated attack logs using KQL queries in Log Analytics Workspace
 - [Windows Security Event Log](https://github.com/Muneer44/CloudHoneynet/blob/main/KQL%20Query%20Cheat%20Sheet/Network%20Security%20Groups.md)
 - [Linux Syslog](https://github.com/Muneer44/CloudHoneynet/blob/main/KQL%20Query%20Cheat%20Sheet/Linux%20Syslog.md)
 - [Azure Active Directory](https://github.com/Muneer44/CloudHoneynet/blob/main/KQL%20Query%20Cheat%20Sheet/Azure%20Active%20Directory.md)
 - [Azure Storage Account](https://github.com/Muneer44/CloudHoneynet/blob/main/KQL%20Query%20Cheat%20Sheet/Azure%20Storage%20Account.md)
 - [Azure Key Vault](https://github.com/Muneer44/CloudHoneynet/blob/main/KQL%20Query%20Cheat%20Sheet/Azure%20Key%20Vault%20Queries.md)
 - [Network Security Groups](https://github.com/Muneer44/CloudHoneynet/blob/main/KQL%20Query%20Cheat%20Sheet/Network%20Security%20Groups.md)


> Note: AI and the internet were utilized to a moderate extent for the creation of above scripts and queries.

---

</details>

<details>
<summary><h1>Phase 4 :  Incident Response</h1></summary> 
 
![NIST IR](https://github.com/Muneer44/CloudHoneynet/assets/117259069/b2f79c25-4555-4b91-a3cb-996ab1be9957)

- **Preparation :** Centralizing logs, crafting analytics by ingesting logs from varied systems.
- **Detection and Analysis :** Utilizing MS Sentinel to prompt alerts and initiate investigation.
- **Containment, Eradication, and Recovery :** Applying playbook-guided response as needed.
- **Post-Incident Activity :** Thoroughly documenting investigation findings for future reference.

---

## IR Sample Playbook :
![image](https://github.com/Muneer44/CloudHoneynet/assets/117259069/1596013b-9c43-4365-bee4-ad55df928c7f)
![image](https://github.com/Muneer44/CloudHoneynet/assets/117259069/978593cc-ae29-4ddb-a7bd-e96957b28b86)
![image](https://github.com/Muneer44/CloudHoneynet/assets/117259069/322ede2d-fb06-44ac-9ea4-586c38d9052c)

## Triggered Alerts (Incidents) :
![Incidents](https://github.com/Muneer44/CloudHoneynet/assets/117259069/127bb98c-d67e-4964-94f4-f8bb79fd0c86)  

## Investigation :
![image](https://github.com/Muneer44/CloudHoneynet/assets/117259069/2b71eaea-9697-4801-b3fb-603e878b1305)
![Investigation 2](https://github.com/Muneer44/CloudHoneynet/assets/117259069/a5ae2674-b233-4a4e-b415-0cef77011861)

---

</details>

<details>
<summary><h1>Phase 5 :  Security Enhancements</h1></summary> 

- Restrict public exposure of resources - [[IMG](#architecture-after-implementation-of-security-measures)]
  - Disable public access
  - Create Private endpoint access
- Implement secure NSG configuration
  - Delete wide open all traffic NSG entry
  - Allow RDP from specific host     
- Deploy NSG on subnet
- Fulfill *NIST 800-53 R5 - Boundary Protection*

  ![Final_SC-7](https://github.com/Muneer44/CloudHoneynet/assets/117259069/fa963e5e-a62b-4ca1-b839-731a4592cb63)

  ## MiTRE Threat Vector Analysis :
   ![MiTRE](https://github.com/Muneer44/CloudHoneynet/assets/117259069/56f3f90f-5228-47ae-97e1-434e6df46cdf)

  ## Virtual network Topology :
   ![Vnet Topology](https://github.com/Muneer44/CloudHoneynet/assets/117259069/a8e8a748-fbfa-4076-9738-865ec8d5614f)

---

</details>

</details>

### ↑ Toggle for Environment Setup, SIEM Configuration, KQL Queries, NIST-IR Utilization, and Other Details 

---

# Conclusion

The **CloudHoneynet project** demonstrated the practical application of cybersecurity techniques. By constructing an Azure-based environment emulating vulnerable systems, I engaged with real malicious traffic, enhancing my understanding of vulnerabilities, threats and incident response. By actively engaging with tools like **Azure Sentinel**, crafting **Kusto Query Language** queries, and following **NIST** guidelines, I not only enhanced my skills but also gained a profound understanding of practical security measures. 
I continue to strive to educate myself further each day, and this is just one of the many practical projects I've worked on. You can view my portfolio [here](https://github.com/Muneer44)










