# Module 2: Virtual Network NAT

## Phase 1: Core Networking Fundamentals
_Time: 4-5 hours • Priority: 🔴_

## Module Focus

- [ ] Azure NAT Gateway overview
- [ ] Outbound connectivity
- [ ] SNAT port exhaustion prevention
- [ ] NAT Gateway association
- [ ] Public IP / Public IP prefix with NAT

#### 🎯 Key Exam Points - Phase 1

| Topic | Must Remember |
|-------|---------------|
| Reserved IPs | 5 per subnet: .0 (network), .1 (gateway), .2-.3 (Azure DNS), .255 (broadcast) |
| Smallest subnet | /29 = 8 addresses - 5 reserved = **3 usable** |
| Basic vs Standard IP | Standard = zone-redundant, static only, secure by default |
| NAT Gateway | Prevents SNAT exhaustion, supports up to 16 public IPs |

#### 🧪 Hands-On Lab 1

```powershell
# Create VNet with multiple subnets
az network vnet create --name MyVNet --resource-group MyRG --address-prefix 10.0.0.0/16
az network vnet subnet create --name WebSubnet --vnet-name MyVNet --resource-group MyRG --address-prefix 10.0.1.0/24
az network vnet subnet create --name AppSubnet --vnet-name MyVNet --resource-group MyRG --address-prefix 10.0.2.0/24
az network vnet subnet create --name DbSubnet --vnet-name MyVNet --resource-group MyRG --address-prefix 10.0.3.0/24

# Create NAT Gateway
az network nat gateway create --name MyNatGateway --resource-group MyRG --public-ip-addresses MyPublicIP
```
