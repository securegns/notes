# Module 16: Network Security Best Practices

## Phase 10: Advanced Security
_Time: 4-5 hours • Priority: 🟡_

## Module Focus

- [ ] Defense in depth
- [ ] Network segmentation
- [ ] Zero trust model
- [ ] Azure Bastion (secure VM access)
- [ ] Just-in-time VM access
- [ ] DDoS Protection (Basic vs Standard)
- [ ] Network Virtual Appliances (NVAs)

#### 🎯 Key Exam Points - Phase 10

| Topic | Must Remember |
|-------|---------------|
| Firewall rule order | DNAT → Network → Application |
| Firewall subnet name | **AzureFirewallSubnet** (minimum /26) |
| Premium features | TLS inspection, IDPS, URL filtering, Web categories |
| Bastion | No public IP on VM needed, uses port 443 |
| DDoS Standard | Adaptive tuning, cost protection, metrics/alerts |
| Azure Firewall vs NSG | Firewall = centralized L3-7; NSG = L3-4 per subnet/NIC |

#### 🧪 Hands-On Lab 8

```powershell
# Create Bastion subnet and host
az network vnet subnet create --name AzureBastionSubnet --vnet-name MyVNet \
  --resource-group MyRG --address-prefix 10.0.255.0/26

az network bastion create --name MyBastion --resource-group MyRG \
  --vnet-name MyVNet --public-ip-address MyBastionIP
```
