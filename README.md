# CloudHoneynet
In this project, I designed and implemented an Azure Cloud Honeynet environment that emulates real vulnerable systems to **capture**, **analyze** and **respond** to live malicious traffic. The goal was to enhance my understanding of cybersecurity threats and incident response through practical, real-world experience.

## TLDR;
- <ins>**Azure Infrastructure Setup:**</ins> Deployed an Azure environment consisting of virtual machines, SQL Server, Azure Active Directory, storage accounts, Key Vault, and more.
  
- <ins>**SIEM Integration with Azure Sentinel:**</ins> Leveraged Sentinel (SIEM) to monitor and analyze the captured data from various sources and respond to the incidents, all within a central hub.
  
- <ins>**Analytic Rules and Alerts:**</ins> I crafted custom analytic rules using *Kusto Query Language* to detect and trigger specific attack patterns like brute force attempts, manipulation of firewall, identification of malicious files and other potential threats.
  
- <ins>**Threat Mapping with Workbooks:**</ins> Using Azure Sentinel's workbooks, I transformed threat data into maps that outlines the origin of attacks. This provides a comprehensive overview of the threat landscape.

- <ins>**Incident Response Simulation:**</ins> Simulated incident response on triggered alerts by analyzing the collected data, identifying attack vectors, and applying appropriate mitigation strategies by developing a sample Playbook based on *NIST 800-61 - Incident Response* guidelines.
  
- <ins>**Baseline and Remediation Metric Comparison:**</ins> Observed the environment for 24 hours in its vulnerable state, capturing essential security metrics as a baseline for comparison against the 24-hour metrics of the environment after implementing remediation measures.
    
- <ins>**Regulatory Compliance:**</ins> Ensured regulatory compliance by enabling *NIST 800-53* and *PCI DSS* security policies alongside hardening the environment's security posture.

--- 

