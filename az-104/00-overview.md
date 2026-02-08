# AZ-104 Study Plan: Configure and Manage Virtual Networks

> **Total Estimated Time:** 45-55 hours | **Exam Weight:** ~25-30% of AZ-104

---

## 📊 Priority Legend

| Icon | Meaning | Exam Frequency |
|------|---------|----------------|
| 🔴 | **High Priority** | Frequently tested, must know |
| 🟡 | **Medium Priority** | Moderately tested |
| 🟢 | **Low Priority** | Occasionally tested |

---

## Modules Index

- [1 Virtual Network Fundamentals](01-virtual-network-fundamentals.md)
- [2 Virtual Network NAT](02-virtual-network-nat.md)
- [3 NSGs Deep Dive](03-nsgs-deep-dive.md)
- [4 Azure DNS](04-azure-dns.md)
- [5 VNet Peering](05-vnet-peering.md)
- [6 VPN Gateway](06-vpn-gateway.md)
- [7 ExpressRoute](07-expressroute.md)
- [8 Virtual WAN](08-virtual-wan.md)
- [9 Azure Load Balancer](09-azure-load-balancer.md)
- [10 Application Gateway](10-application-gateway.md)
- [11 Traffic Manager](11-traffic-manager.md)
- [12 Azure Front Door](12-azure-front-door.md)
- [13 Network Routing](13-network-routing.md)
- [14 Service Endpoints & Private Link](14-service-endpoints-private-link.md)
- [15 Azure Firewall](15-azure-firewall.md)
- [16 Network Security Best Practices](16-network-security-best-practices.md)
- [17 Network Watcher](17-network-watcher.md)

---

## How to Use This Guide (Bulletproof Mode)

- **Pass 1 (Concepts):** Read each phase once without labs. Aim for definitions and boundaries.  
- **Pass 2 (Hands-on):** Execute the labs for each phase using Azure CLI or Portal.  
- **Pass 3 (Recall):** Use the “Key Exam Points,” “Common Exam Traps,” and checklists to self-test.  
- **Pass 4 (Teach-back):** Explain each phase out loud in 2–3 minutes; if you stumble, revisit.  
- **Evidence of mastery:** You can answer *why* a service is chosen, not just *what* it is.  

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
