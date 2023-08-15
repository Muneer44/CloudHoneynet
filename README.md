# CloudHoneynet
In this project, I designed and implemented an Azure Cloud Honeynet environment that emulates real vulnerable systems to **capture**, **analyze** and **respond** to live malicious traffic. The goal was to enhance my understanding of cybersecurity threats and incident response through practical, real-world experience.

---

![image](https://github.com/Muneer44/CloudHoneynet/assets/117259069/3fb58841-b63d-4241-b5de-28ec40305a53)

---

## TLDR;
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
> Note: The above maps were generated while the environment was in vulnerable state.

---

# Metric Comparison: Pre and Post Security Enhancements








