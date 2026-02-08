# Module 14: Service Endpoints & Private Link

## Phase 9: Routing & Endpoints
_Time: 5-6 hours • Priority: 🔴_

## Module Focus

- [ ] Virtual Network service endpoints
- [ ] Service endpoint policies
- [ ] Azure Private Link overview
- [ ] Private endpoints
- [ ] Private DNS integration
- [ ] Private Link Service (for custom services)
- [ ] **Difference: Service Endpoints vs Private Endpoints**

#### 🎯 Key Exam Points - Phase 9

| Topic | Must Remember |
|-------|---------------|
| UDR priority | UDR > BGP > System routes |
| Virtual appliance | Requires IP forwarding enabled on NIC |
| Forced tunneling | Route 0.0.0.0/0 to on-prem |
| Route propagation | Disable to prevent gateway routes in route table |
| Service endpoints | Public endpoint remains; traffic stays on backbone |
| Private endpoints | Private IP in VNet + Private DNS zone |

#### Service Endpoints vs Private Endpoints

| Feature | Service Endpoints | Private Endpoints |
|---------|-------------------|-------------------|
| Traffic path | Over Microsoft backbone | Via private IP in your VNet |
| Public endpoint | Still accessible | Can disable completely |
| IP address | Uses service public IP | Gets private IP from your subnet |
| DNS | No change needed | Requires Private DNS zone |
| On-prem access | ❌ No | ✅ Yes (via VPN/ExpressRoute) |
| Cost | Free | Per-hour + data processing |
| Cross-region | ❌ Same region only | ✅ Any region |

#### 🧪 Hands-On Lab 7

```powershell
# Create route table with UDR
az network route-table create --name MyRouteTable --resource-group MyRG

# Add route to NVA
az network route-table route create --route-table-name MyRouteTable \
  --resource-group MyRG --name ToNVA --address-prefix 10.0.2.0/24 \
  --next-hop-type VirtualAppliance --next-hop-ip-address 10.0.1.4

# Associate with subnet
az network vnet subnet update --name AppSubnet --vnet-name MyVNet \
  --resource-group MyRG --route-table MyRouteTable

# Create Private Endpoint for Storage
az network private-endpoint create --name MyPrivateEndpoint --resource-group MyRG \
  --vnet-name MyVNet --subnet DbSubnet --private-connection-resource-id <storage-id> \
  --group-id blob --connection-name MyConnection
```
