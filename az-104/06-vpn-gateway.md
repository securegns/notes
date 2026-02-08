# Module 6: VPN Gateway

## Phase 4: VNet Connectivity
_Time: 6-8 hours • Priority: 🔴_

## Module Focus

- [ ] VPN Gateway overview
- [ ] Gateway SKUs and performance (VpnGw1-5, VpnGw1AZ-5AZ)
- [ ] Site-to-Site (S2S) VPN
- [ ] Point-to-Site (P2S) VPN
- [ ] VNet-to-VNet connections
- [ ] Gateway subnet requirements (**/27 or larger recommended**)
- [ ] Local network gateway
- [ ] Connection types (IPsec/IKE)
- [ ] Active-active vs active-standby
- [ ] BGP with VPN Gateway
- [ ] VPN Gateway reset and troubleshooting

#### 🎯 Key Exam Points - Phase 4

| Topic | Must Remember |
|-------|---------------|
| Peering is non-transitive | VNet A ↔ B ↔ C: A cannot reach C without direct peering or NVA |
| Gateway transit | Hub VNet shares gateway with spoke VNets |
| Gateway subnet name | Must be exactly **GatewaySubnet** |
| Gateway subnet size | /27 or larger (supports ExpressRoute coexistence) |
| P2S authentication | Azure AD, RADIUS, or certificate-based |
| S2S requirements | Public IP + Local Network Gateway + shared key |

#### ⚠️ Common Exam Traps

- Peering must be created on **both** VNets
- Cannot peer VNets with **overlapping address spaces**
- "Use remote gateways" = spoke side; "Allow gateway transit" = hub side
- VPN Gateway is **not** the same as VNet peering; peering is simpler and lower latency

#### 🧪 Hands-On Lab 4

```powershell
# Create hub-spoke peering
# Hub to Spoke
az network vnet peering create --name HubToSpoke --resource-group MyRG \
  --vnet-name HubVNet --remote-vnet SpokeVNet \
  --allow-vnet-access true --allow-gateway-transit true

# Spoke to Hub
az network vnet peering create --name SpokeToHub --resource-group MyRG \
  --vnet-name SpokeVNet --remote-vnet HubVNet \
  --allow-vnet-access true --use-remote-gateways true
```
