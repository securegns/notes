# Module 10: Application Gateway

## Phase 7: Layer 7 Load Balancing
_Time: 5-6 hours • Priority: 🔴_

## Module Focus

- [ ] Application Gateway overview
- [ ] Layer 7 load balancing
- [ ] SKUs (Standard_v2, WAF_v2)
- [ ] Frontend IP configuration
- [ ] Listeners (Basic vs Multi-site)
- [ ] Backend pools
- [ ] HTTP settings
- [ ] Routing rules (Basic vs Path-based)
- [ ] Health probes
- [ ] SSL/TLS termination
- [ ] End-to-end SSL
- [ ] Web Application Firewall (WAF)
- [ ] WAF policies and rules
- [ ] Autoscaling
- [ ] Zone redundancy

#### 🎯 Key Exam Points - Phase 7

| Topic | Must Remember |
|-------|---------------|
| Layer 7 | HTTP/HTTPS, URL-based routing, cookie affinity |
| Multi-site listener | Host multiple sites on same public IP |
| Path-based routing | /images/* → pool1, /api/* → pool2 |
| WAF modes | Detection (log only) vs Prevention (block) |
| Subnet requirement | Dedicated subnet, no other resources |
| App Gateway vs Front Door | App Gateway = regional L7; Front Door = global L7 anycast |

#### 🧪 Hands-On Lab 6

```powershell
# Create Application Gateway subnet (dedicated)
az network vnet subnet create --name AppGwSubnet --vnet-name MyVNet \
  --resource-group MyRG --address-prefix 10.0.10.0/24

# Create Application Gateway with WAF
az network application-gateway create --name MyAppGateway --resource-group MyRG \
  --vnet-name MyVNet --subnet AppGwSubnet --sku WAF_v2 \
  --http-settings-cookie-based-affinity Enabled \
  --frontend-port 80 --http-settings-port 80 --http-settings-protocol Http
```
