# Module 9: Azure Load Balancer

## Phase 6: Layer 4 Load Balancing
_Time: 4-5 hours • Priority: 🔴_

## Module Focus

- [ ] Azure Load Balancer overview
- [ ] Public vs Internal Load Balancer
- [ ] Load Balancer SKUs (Basic vs Standard)
- [ ] Frontend IP configuration
- [ ] Backend pools
- [ ] Health probes (TCP, HTTP, HTTPS)
- [ ] Load balancing rules
- [ ] Inbound NAT rules
- [ ] Outbound rules
- [ ] Session persistence (source IP affinity)
- [ ] Floating IP (Direct Server Return)
- [ ] HA Ports

#### 🎯 Key Exam Points - Phase 6

| Topic | Must Remember |
|-------|---------------|
| Basic vs Standard | Standard = zone-redundant, SLA, required for availability zones |
| Health probes | HTTP probe needs 200 response; TCP just needs connection |
| HA Ports | Load balance **all** ports simultaneously (Standard SKU only) |
| Session persistence | None (default), Client IP, Client IP + Protocol |
| Backend pool | Standard LB = NIC or IP-based; Basic = availability set only |

#### ⚠️ Common Exam Traps

- Basic LB has **no SLA**
- Standard LB backends must be in **same VNet**
- Cannot mix Basic and Standard SKUs
- Load Balancer is **L4** only; HTTP routing belongs to App Gateway/Front Door

#### 🧪 Hands-On Lab 5

```powershell
# Create Standard Load Balancer
az network lb create --name MyLoadBalancer --resource-group MyRG \
  --sku Standard --frontend-ip-name MyFrontEnd --backend-pool-name MyBackEndPool

# Create health probe
az network lb probe create --lb-name MyLoadBalancer --resource-group MyRG \
  --name MyHealthProbe --protocol Http --port 80 --path /health

# Create load balancing rule
az network lb rule create --lb-name MyLoadBalancer --resource-group MyRG \
  --name MyLBRule --frontend-ip-name MyFrontEnd --backend-pool-name MyBackEndPool \
  --protocol Tcp --frontend-port 80 --backend-port 80 --probe-name MyHealthProbe
```
