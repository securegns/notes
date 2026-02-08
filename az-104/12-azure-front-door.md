# Module 12: Azure Front Door

## Phase 8: Global Traffic Routing
_Time: 3-4 hours • Priority: 🟡_

## Module Focus

- [ ] Front Door overview
- [ ] Global load balancing
- [ ] URL-based routing
- [ ] Session affinity
- [ ] SSL offloading
- [ ] WAF integration
- [ ] Caching
- [ ] URL redirect/rewrite

#### 🎯 Key Exam Points - Phase 8

| Topic | Must Remember |
|-------|---------------|
| Traffic Manager | DNS-based, returns IP/FQDN (Layer 7 DNS) |
| Front Door | HTTP-based, Anycast, global Layer 7 |
| Performance routing | Routes to lowest latency endpoint |
| Geographic routing | Route users by geographic location (GDPR compliance) |
| Nested profiles | Combine routing methods |

#### Comparison: Traffic Manager vs Front Door vs Load Balancer vs App Gateway

| Feature | Traffic Manager | Front Door | Load Balancer | App Gateway |
|---------|----------------|------------|---------------|-------------|
| Layer | DNS (L7) | HTTP (L7) | L4 | L7 |
| Scope | Global | Global | Regional | Regional |
| Protocol | Any | HTTP/HTTPS | TCP/UDP | HTTP/HTTPS |
| WAF | ❌ | ✅ | ❌ | ✅ |
| SSL Offload | ❌ | ✅ | ❌ | ✅ |
| Caching | ❌ | ✅ | ❌ | ❌ |

#### ✅ Quick Decision Guide

- **Need global HTTP/HTTPS + WAF + caching:** Front Door  
- **Need DNS-based failover/geo routing:** Traffic Manager  
- **Need regional TCP/UDP load balancing:** Load Balancer  
- **Need regional L7 routing + WAF for VMs/AKS:** Application Gateway
