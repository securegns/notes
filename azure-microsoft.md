# Imp Links
- [New security improvements from microsoft blog](https://www.microsoft.com/en-us/security/blog/2023/01/23/microsoft-security-innovations-from-2022-to-help-you-create-a-safer-world-today/)
- [Microsoft Cybersecurity Reference Architectures](https://learn.microsoft.com/en-gb/security/adoption/mcra)
- [Microsoft Security Adoption Framework (SAF)](https://learn.microsoft.com/en-us/security/adoption/adoption)
- [Log analytics workspace demo](https://aka.ms/lademo)
- [Microsoft graph explorer demo](https://developer.microsoft.com/graph/graph-explorer)

# Azure/Sec Labs

# Azure Sec tools
- Purview, Priva, 

# CIS benchmark
- [How to Enforce CIS Benchmark policies using Azure Blueprints](https://www.youtube.com/watch?v=FS874tASMTw)
- [CIS Benchmark PDF](https://www.csiltd.co.uk/wp-content/uploads/2021/12/CIS_Microsoft_Azure_Foundations_Benchmark_v1.4.0.pdf)


# Interactive guides
- Sentinel and how SOC works - https://mslearn.cloudguides.com/guides/Investigate%20security%20incidents%20in%20a%20hybrid%20environment%20with%20Azure%20Sentinel

# Defense in Depth approach
- Identity and Access - PIM
- Perimeter - Azure Firewall [Application FDQN Filtering, Network Traffic filtering rules, FDQN Tags, Outbound SNAT, Inbound DNAT support, L3-L7 Connectivity policie, seperate firewall subnet, static public IP address, Forced tunnelling], Firewall manager, Firewall policies
- Network - Vnets, Application security groups, Network security groups, JIT, Route Tables, 
- Compute - Host secuirty, Container security
- Application - Azure WAF, Azure WAf Policies, 
- Data - Defender for Cloud/storage/SQL, Azure Information Protection
   
### Ninja trainings 
- All Ninja trainings list     -   https://rodtrent.substack.com/p/all-the-microsoft-ninja-training
- M365 Defender Ninja          -   https://techcommunity.microsoft.com/t5/microsoft-365-defender-blog/become-a-microsoft-365-defender-ninja/ba-p/1789376
- Microsoft Sentinel ninja     -   https://techcommunity.microsoft.com/t5/microsoft-sentinel-blog/become-a-microsoft-sentinel-ninja-the-complete-level-400/ba-p/1246310
- 

### KQL notes
- Practice KQL in azure demo env - https://dataexplorer.azure.com/clusters/help/databases/SecurityLogs
- Must learn KQL blog - https://github.com/rod-trent/MustLearnKQL
- 
### Notes
- Stateful firewall is VNETs and for stateless firewall, we need to use Azure Firewall
- Types of Azure logs - Activity logs(control-plane), Metrics, 

### Things to try
- Upload a reverse shell to storage account, download the file in a windows that has defender for endpoint connected and check for alert. Use the file hash to scan whole orginisation using microsoft sentinel
- 
