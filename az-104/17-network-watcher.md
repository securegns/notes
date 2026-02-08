# Module 17: Network Watcher

## Phase 11: Monitoring & Troubleshooting
_Time: 3-4 hours • Priority: 🟡_

## Module Focus

- [ ] Network Watcher overview
- [ ] IP flow verify
- [ ] Next hop
- [ ] Connection troubleshoot
- [ ] NSG diagnostics
- [ ] VPN troubleshoot
- [ ] Packet capture
- [ ] Connection monitor
- [ ] Traffic analytics
- [ ] NSG flow logs
- [ ] Diagnostic logs

#### 🎯 Key Exam Points - Phase 11

| Tool | Use Case |
|------|----------|
| IP flow verify | Check if NSG allows/denies specific traffic |
| Next hop | Determine routing path for traffic |
| Connection troubleshoot | Test connectivity between resources |
| NSG diagnostics | View effective security rules |
| Packet capture | Capture network traffic for analysis |
| Traffic analytics | Visualize NSG flow logs |

#### 🔍 Troubleshooting Flow (Bulletproof)

1. **Check NSG effective rules** (subnet + NIC).  
2. **Check route tables** (UDR/BGP/system).  
3. **Validate DNS resolution** (Private DNS links, record sets).  
4. **Confirm service health** (LB/AppGW probes).  
5. **Validate gateways** (VPN/ER status and routes).  

#### 🧪 Hands-On Lab 9

```powershell
# Enable Network Watcher
az network watcher configure --resource-group MyRG --locations eastus --enabled true

# IP flow verify
az network watcher test-ip-flow --direction Inbound --protocol Tcp \
  --local 10.0.1.4:80 --remote 10.0.2.4:* --vm MyVM --resource-group MyRG

# Next hop
az network watcher show-next-hop --vm MyVM --resource-group MyRG \
  --source-ip 10.0.1.4 --dest-ip 10.0.2.4
```

---

## 📋 Quick Reference: Numbers to Memorize

| Item | Value |
|------|-------|
| Reserved IPs per subnet | **5** |
| NSG rule priority range | **100-4096** |
| Default NSG rules priority | **65000-65500** |
| GatewaySubnet minimum size | **/27** (recommended), /29 (absolute minimum) |
| AzureFirewallSubnet minimum | **/26** |
| AzureBastionSubnet minimum | **/26** |
| VPN Gateway deployment time | **30-45 minutes** |
| Max VNets per peering | Direct peering only (non-transitive) |
| Standard LB max backend pools | **5000** VMs |
| Application Gateway max instances | **125** |

---

## ✅ Exam Readiness Checklist (Bulletproof)

- [ ] I can design a hub-spoke network from scratch and defend the design.
- [ ] I can explain **when to use** VNet peering vs VPN Gateway vs ExpressRoute.
- [ ] I can map **any** requirement to the correct load balancer (LB/AppGW/Front Door/Traffic Manager).
- [ ] I can troubleshoot a connectivity issue in under 10 minutes using Network Watcher.
- [ ] I can explain Service Endpoints vs Private Endpoints without notes.
- [ ] I know subnet size requirements for Gateway/Bastion/Firewall subnets.
- [ ] I can read effective NSG rules and identify the blocking rule.
- [ ] I can explain Basic vs Standard SKUs for Public IPs, LB, and WAF.

---

## 🧪 Final Comprehensive Lab (3-4 hours)

### Scenario: Build Enterprise Hub-Spoke Network

```
                    ┌─────────────┐
                    │   Internet  │
                    └──────┬──────┘
                           │
                    ┌──────▼──────┐
                    │  Azure      │
                    │  Firewall   │
                    └──────┬──────┘
                           │
┌──────────────────────────┼──────────────────────────┐
│                    Hub VNet                          │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  │
│  │ Firewall    │  │   Gateway   │  │   Bastion   │  │
│  │ Subnet      │  │   Subnet    │  │   Subnet    │  │
│  └─────────────┘  └─────────────┘  └─────────────┘  │
└──────────────────────────┬──────────────────────────┘
                           │ Peering
         ┌─────────────────┼─────────────────┐
         │                 │                 │
   ┌─────▼─────┐     ┌─────▼─────┐     ┌─────▼─────┐
   │  Spoke 1  │     │  Spoke 2  │     │  Spoke 3  │
   │  (Web)    │     │  (App)    │     │  (Data)   │
   │           │     │           │     │           │
   │ App GW +  │     │ Internal  │     │ Private   │
   │ VMs + NSG │     │ LB + VMs  │     │ Endpoint  │
   └───────────┘     └───────────┘     └───────────┘
```

### Tasks

1. **Create VNets** (Hub: 10.0.0.0/16, Spokes: 10.1.0.0/16, 10.2.0.0/16, 10.3.0.0/16)
2. **Configure peering** with gateway transit from hub
3. **Deploy Azure Firewall** with DNAT and network rules
4. **Create NSGs** for each spoke subnet
5. **Deploy Application Gateway** with WAF in Spoke 1
6. **Deploy Internal Load Balancer** in Spoke 2
7. **Create Private Endpoint** to Storage in Spoke 3
8. **Configure UDRs** to route spoke traffic through firewall
9. **Deploy Bastion** for secure VM access
10. **Enable Network Watcher** and test connectivity

---

## ✅ Study Checklist

- [ ] Completed all 11 phases
- [ ] Finished all 9 hands-on labs
- [ ] Built final hub-spoke lab
- [ ] Can explain Service Endpoints vs Private Endpoints
- [ ] Know all SKU differences (Basic vs Standard)
- [ ] Memorized key numbers (reserved IPs, priorities, subnet sizes)
- [ ] Practiced with Azure CLI and PowerShell
- [ ] Taken at least 2 practice exams (MeasureUp/Whizlabs)

---

## 📚 Recommended Resources

| Resource | Link |
|----------|------|
| Microsoft Learn | https://learn.microsoft.com/en-us/training/paths/az-104-manage-virtual-networks/ |
| John Savill's YouTube | AZ-104 Networking Study Cram |
| Practice Tests | MeasureUp (official), Whizlabs |
| Azure Free Account | $200 credit for hands-on labs |

---

**Good luck with AZ-104!** 🎉
