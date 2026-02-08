# Module 3: NSGs Deep Dive

## Phase 2: Network Security Groups
_Time: 5-6 hours • Priority: 🔴_

## Module Focus

- [ ] NSG overview and purpose
- [ ] Inbound security rules
- [ ] Outbound security rules
- [ ] Default security rules (AllowVNetInBound, AllowAzureLoadBalancerInBound, DenyAllInBound)
- [ ] Rule priority (**100-4096**, lower = higher priority)
- [ ] Service tags (VirtualNetwork, Internet, AzureLoadBalancer, etc.)
- [ ] Application Security Groups (ASGs)
- [ ] NSG association (subnet vs NIC level)
- [ ] Effective security rules
- [ ] NSG flow logs

#### 🎯 Key Exam Points - Phase 2

| Topic | Must Remember |
|-------|---------------|
| Rule priority | 100-4096, **lower number = processed first** |
| Default rules | Priority 65000-65500, cannot delete, can override |
| NSG association | Subnet-level = all resources; NIC-level = specific VM |
| Effective rules | NIC rules processed **after** subnet rules |
| Service tags | VirtualNetwork includes peered VNets and on-prem (via gateway) |

#### ⚠️ Common Exam Traps

- NSG at **both** subnet and NIC? Traffic must pass **both**
- Default outbound = **Allow** (DenyAllOutBound doesn't exist by default)
- ASGs can only be used within the **same VNet**
- NSGs are **stateful** (return traffic is allowed automatically)

#### 🧪 Hands-On Lab 2

```powershell
# Create NSG with rules
az network nsg create --name WebNSG --resource-group MyRG

# Allow HTTP inbound (priority 100)
az network nsg rule create --nsg-name WebNSG --resource-group MyRG \
  --name AllowHTTP --priority 100 --direction Inbound \
  --access Allow --protocol Tcp --destination-port-ranges 80

# Deny all other inbound (priority 200)
az network nsg rule create --nsg-name WebNSG --resource-group MyRG \
  --name DenyAll --priority 200 --direction Inbound \
  --access Deny --protocol '*' --destination-port-ranges '*'

# Associate with subnet
az network vnet subnet update --name WebSubnet --vnet-name MyVNet \
  --resource-group MyRG --network-security-group WebNSG
```
