# Module 4: Azure DNS

## Phase 3: Name Resolution
_Time: 3-4 hours • Priority: 🟡_

## Module Focus

- [ ] Azure DNS zones (public)
- [ ] Azure Private DNS zones
- [ ] DNS record types (A, AAAA, CNAME, MX, NS, PTR, SOA, SRV, TXT)
- [ ] Record sets
- [ ] Alias records
- [ ] DNS zone delegation
- [ ] Auto-registration with Private DNS
- [ ] Virtual network links

#### 🎯 Key Exam Points - Phase 3

| Topic | Must Remember |
|-------|---------------|
| Private DNS zone naming | Use `privatelink.<service>.core.windows.net` for Private Endpoints |
| Auto-registration | Only works with **one** VNet link per zone |
| Alias records | Point to Azure resource, auto-update when IP changes |
| Record types | A = IPv4, AAAA = IPv6, CNAME = alias, MX = mail |
| Azure DNS vs Private DNS | Public zones serve internet names; Private zones are VNet-only |

#### 🧪 Hands-On Lab 3

```powershell
# Create Private DNS zone
az network private-dns zone create --name contoso.internal --resource-group MyRG

# Link to VNet with auto-registration
az network private-dns link vnet create --zone-name contoso.internal \
  --resource-group MyRG --name MyVNetLink --virtual-network MyVNet \
  --registration-enabled true

# Add A record
az network private-dns record-set a add-record --zone-name contoso.internal \
  --resource-group MyRG --record-set-name webserver --ipv4-address 10.0.1.4
```
