# CompTIA Network+ N10-009 Study Index

**Exam Code:** N10-009 (V9) | **Passing Score:** 720/900 | **Duration:** 90 min | **Max Questions:** 90

---

## Domain 1: Networking Concepts (23%)

- [ ] **OSI Model Layers** — Physical (bits), Data Link (frames/MAC), Network (packets/IP), Transport (segments/TCP/UDP), Session, Presentation (encryption/compression), Application
- [ ] **Networking Appliances** — Routers, Switches (L2/L3), Firewalls (stateful/stateless), IDS/IPS, Load Balancers, Proxies, NAS, SAN, Wireless APs, VPN concentrators, Voice gateways, Media converters, Modems
- [ ] **Cloud Concepts** — NFV, VPC, Network Security Groups, Cloud Gateways, Deployment models (Public/Private/Hybrid/Community), Service models (SaaS/IaaS/PaaS), Elasticity, Scalability, Multitenancy
- [ ] **SDN & Automation** — Control plane/Data plane/Application plane separation, SDN controllers, SD-WAN, Network automation, REST APIs, Infrastructure as Code (IaC), Playbooks/Runbooks, CI/CD for networking
- [ ] **VXLAN & Overlay Networks** — VXLAN encapsulation, VTEP (VXLAN Tunnel Endpoint), Overlay vs Underlay networks *(NEW in N10-009)*
- [ ] **Traffic Types** — Unicast, Multicast, Anycast, Broadcast
- [ ] **Traffic Flows** — North-South (client-to-datacenter), East-West (server-to-server within datacenter)
- [ ] **Transmission Media** — Wireless (802.11a/b/g/n/ac/ax/be, Cellular 4G/5G, Satellite), Wired (Fiber single-mode/multimode, Coaxial RG-6/RG-59, DAC, Twisted pair Cat5e/Cat6/Cat6a/Cat7/Cat8)
- [ ] **Transceivers & Connectors** — SFP, SFP+ (10G), QSFP (40G), QSFP+ | Fiber: SC, LC, ST, MPO | Copper: RJ-11 (phone), RJ-45 (Ethernet), F-type (coax), BNC (legacy coax)
- [ ] **Network Topologies** — Mesh (full/partial), Hybrid, Star/Hub-and-spoke, Spine-and-leaf, Point-to-point, Three-tier (Core/Distribution/Access), Collapsed core (Two-tier)
- [ ] **IPv4 Addressing** — Public vs Private, APIPA (169.254.x.x), RFC1918 (10.x.x.x, 172.16-31.x.x, 192.168.x.x), Loopback (127.0.0.1), Subnetting (VLSM, CIDR), Address classes (A–E), Binary conversions
- [ ] **IPv6 Addressing** — Address format (128-bit), Link-local (fe80::), Global unicast (2000::/3), Unique local (fc00::/7), EUI-64, Dual-stack, Tunneling (6to4, Teredo), NAT64, NDP, SLAAC

### ⚠️ Ports & Protocols Quick Reference (MUST MEMORIZE)

| Port | Protocol | Description | TCP/UDP |
|------|----------|-------------|---------|
| 20/21 | FTP | File Transfer (Data/Control) | TCP |
| 22 | SSH/SFTP/SCP | Secure Shell/File Transfer | TCP |
| 23 | Telnet | Unsecure Remote Terminal | TCP |
| 25 | SMTP | Email Sending | TCP |
| 53 | DNS | Name Resolution | TCP/UDP |
| 67/68 | DHCP | Dynamic IP (Server/Client) | UDP |
| 69 | TFTP | Trivial File Transfer | UDP |
| 80 | HTTP | Web Traffic | TCP |
| 110 | POP3 | Email Retrieval | TCP |
| 123 | NTP | Time Synchronization | UDP |
| 143 | IMAP | Email Retrieval | TCP |
| 161/162 | SNMP | Network Monitoring (Agent/Trap) | UDP |
| 389 | LDAP | Directory Services | TCP/UDP |
| 443 | HTTPS | Secure Web Traffic | TCP |
| 445 | SMB/CIFS | File Sharing | TCP |
| 465/587 | SMTPS | Secure Email Submission | TCP |
| 514 | Syslog | Logging | UDP |
| 636 | LDAPS | Secure LDAP | TCP |
| 993 | IMAPS | Secure IMAP | TCP |
| 995 | POP3S | Secure POP3 | TCP |
| 1433 | SQL Server | Microsoft Database | TCP |
| 1521 | Oracle SQL | Oracle Database | TCP |
| 3306 | MySQL | MySQL Database | TCP |
| 3389 | RDP | Remote Desktop | TCP/UDP |
| 5060/5061 | SIP | VoIP Signaling | TCP/UDP |

**Other Protocols:** ICMP (ping/traceroute), GRE (tunneling), IPsec (AH for integrity, ESP for encryption)

### 📡 Wireless Standards Reference (COMMONLY TESTED)

| Standard | Wi-Fi Gen | Frequency | Max Speed | Notes |
|----------|-----------|-----------|-----------|-------|
| 802.11a | — | 5 GHz | 54 Mbps | Legacy |
| 802.11b | — | 2.4 GHz | 11 Mbps | Legacy |
| 802.11g | — | 2.4 GHz | 54 Mbps | Legacy |
| 802.11n | Wi-Fi 4 | 2.4/5 GHz | 600 Mbps | MIMO |
| 802.11ac | Wi-Fi 5 | 5 GHz | 6.9 Gbps | MU-MIMO |
| 802.11ax | Wi-Fi 6/6E | 2.4/5/6 GHz | 9.6 Gbps | OFDMA, BSS coloring |
| 802.11be | Wi-Fi 7 | 2.4/5/6 GHz | 46 Gbps | Multi-link operation |

---

## Domain 2: Network Implementation (20%)

- [ ] **Routing Technologies** — Static/Dynamic routing, Administrative distance, Route aggregation/summarization
- [ ] **Dynamic Routing Protocols** — OSPF (link-state, cost metric), EIGRP (hybrid, composite metric), BGP (path-vector, AS path), RIP (distance-vector, hop count)
- [ ] **Address Translation** — NAT (Static/Dynamic), PAT (Port Address Translation), NAT64
- [ ] **First Hop Redundancy** — FHRP, HSRP, VRRP, GLBP, VIP (Virtual IP)
- [ ] **Router Configuration** — Subinterfaces (router-on-a-stick), Default routes, Static routes
- [ ] **Switching Technologies** — VLANs, 802.1Q tagging, Native VLAN, Voice VLAN, VLAN trunking, Interface config (speed/duplex)
- [ ] **Spanning Tree Protocol** — STP, RSTP, MSTP, Root bridge election, Port states/roles, BPDU guard, PortFast
- [ ] **Link Aggregation** — LACP, 802.3ad, EtherChannel, Port channeling
- [ ] **Layer 2 Concepts** — MTU, Jumbo frames (9000 bytes), MAC address table, CAM table overflow
- [ ] **PoE Standards** — 802.3af (15.4W), 802.3at (PoE+, 25.5W), 802.3bt (PoE++, up to 90W)
- [ ] **Wireless Devices** — Channels (1, 6, 11 for 2.4GHz), Frequency bands (2.4/5/6 GHz), Channel bonding, Band steering, MU-MIMO, OFDMA
- [ ] **Wireless Networks** — SSID, BSSID, ESSID, BSS, ESS, Infrastructure mode, Ad hoc/Mesh, Point-to-point, Guest networks, Captive portals
- [ ] **Wireless Security** — WPA2 (AES/CCMP), WPA3 (SAE), 802.1X/EAP, PSK (Pre-shared key), Enterprise authentication, EAP-TLS, PEAP
- [ ] **Antenna Types** — Omnidirectional, Directional (Yagi, parabolic), Patch antenna, Gain (dBi)
- [ ] **Physical Installations** — MDF, IDF, Hot/Cold aisles, Cable management, Patch panels, Rack diagrams
- [ ] **Power Considerations** — UPS, PDU, Generator backup, Dual power supplies
- [ ] **Environmental Factors** — HVAC, Temperature monitoring, Humidity control, Fire suppression

---

## Domain 3: Network Operations (19%)

- [ ] **Documentation** — Physical vs Logical diagrams, Rack diagrams, Cable maps/Wiring schematics, Network topology maps, Site survey reports, Asset inventory, Asset tagging, IPAM, SLA (uptime guarantees, response times), Wireless heat maps
- [ ] **Life-Cycle Management** — EOL (End of Life), EOS (End of Support), Firmware/Software management, Patching schedules, Decommissioning procedures
- [ ] **Change Management** — Request process, Approval workflows, Documentation requirements, Rollback plans, Maintenance windows
- [ ] **Configuration Management** — Production configs, Backup configs, Baseline configs, Golden images, Version control

### 📊 Network Monitoring

- [ ] **SNMP** — SNMPv1 (community strings, no encryption), SNMPv2c (bulk retrieval, no encryption), SNMPv3 (authentication + encryption), MIBs, OIDs, SNMP traps
- [ ] **Flow Data** — NetFlow, sFlow, IPFIX, Traffic analysis
- [ ] **Packet Capture** — Wireshark, tcpdump, Port mirroring/SPAN
- [ ] **Logging** — Syslog (port 514), Event logs, Log aggregation, SIEM integration
- [ ] **Baseline Metrics** — Bandwidth utilization, Latency, Jitter, Packet loss, Error rates
- [ ] **API Integration** — REST APIs, JSON/XML responses, Automation scripts

### 🔄 Disaster Recovery

| Metric | Definition |
|--------|------------|
| **RPO** | Recovery Point Objective — Maximum acceptable data loss (time) |
| **RTO** | Recovery Time Objective — Maximum acceptable downtime |
| **MTTR** | Mean Time To Repair — Average repair duration |
| **MTBF** | Mean Time Between Failures — Average uptime between failures |

- [ ] **Site Types** — Cold site (empty, days to recover), Warm site (partial equipment, hours), Hot site (fully operational, minutes)
- [ ] **Redundancy** — Active-active, Active-passive, Failover mechanisms
- [ ] **DR Testing** — Tabletop exercises, Simulation testing, Parallel testing, Full interruption testing

### 🌐 Network Services (CRITICAL)

**DHCP:**
- [ ] DORA Process — Discover → Offer → Request → Acknowledge
- [ ] DHCP pools, Address reservations, Lease time, DHCP options
- [ ] DHCP relay agent/IP helper (for crossing subnets)
- [ ] Address pool exhaustion troubleshooting

**DNS:**
- [ ] Forward lookups (name → IP) vs Reverse lookups (IP → name)
- [ ] Recursive vs Iterative queries
- [ ] DNS caching, TTL, DNS hierarchy (root → TLD → authoritative)
- [ ] DNSSEC (DNS Security Extensions)

### 📋 DNS Record Types (MUST KNOW)

| Record | Purpose | Example |
|--------|---------|---------|
| **A** | IPv4 address mapping | www.example.com → 192.168.1.1 |
| **AAAA** | IPv6 address mapping | www.example.com → 2001:db8::1 |
| **CNAME** | Canonical name (alias) | mail.example.com → server1.example.com |
| **MX** | Mail exchange server | Priority + mail server hostname |
| **PTR** | Reverse lookup | IP → hostname |
| **TXT** | Text records | SPF, DKIM, DMARC for email auth |
| **NS** | Name server | Authoritative DNS server |
| **SOA** | Start of Authority | Zone information, serial numbers |
| **SRV** | Service locator | _sip._tcp.example.com |

**Time Protocols:**
- [ ] NTP (Network Time Protocol) — Port 123, Stratum levels
- [ ] NTS (Network Time Security) — Secure NTP *(NEW in N10-009)*
- [ ] PTP (Precision Time Protocol) — Sub-microsecond accuracy

### 🔐 Access & Management

- [ ] **VPN Types** — Client-to-site (remote access), Site-to-site, Clientless VPN (SSL/TLS), Full tunnel vs Split tunnel
- [ ] **Remote Access** — SSH (port 22), GUI/Web-based management, API integration, Console/Serial connections
- [ ] **Jump Box/Bastion Host** — Secure intermediary for accessing internal resources
- [ ] **Management Types** — In-band management (same network), Out-of-band management (dedicated management network)

---

## Domain 4: Network Security (14%)

### 🔒 Authentication & Authorization

- [ ] **Encryption** — Data at rest (stored), Data in transit (network), PKI (Public Key Infrastructure), Digital certificates, Certificate authorities (CA)
- [ ] **Authentication Protocols** — RADIUS, TACACS+, LDAP, SAML, Kerberos, EAP types (EAP-TLS, PEAP, EAP-FAST)
- [ ] **Identity Management** — IAM, MFA (Something you know/have/are), SSO (Single Sign-On), 802.1X (port-based authentication)
- [ ] **Authorization Models** — Least privilege, RBAC (Role-based), ABAC (Attribute-based), Time-based authentication, Geofencing

### 🛡️ Security Frameworks & Concepts

- [ ] **Zero Trust Architecture** — Never trust, always verify, Microsegmentation, Policy-based access *(Expanded in N10-009)*
- [ ] **SASE** — Secure Access Service Edge (cloud-delivered security) *(NEW in N10-009)*
- [ ] **Security Terminology** — Risk (likelihood × impact), Vulnerability (weakness), Exploit (attack method), Threat (potential danger), CIA triad (Confidentiality, Integrity, Availability)

### 🏢 Physical Security

- [ ] **Surveillance** — Cameras/CCTV, Motion detection
- [ ] **Access Control** — Biometric locks, Keycard/Badge readers, Cipher locks, Mantraps/Airlocks, Security guards
- [ ] **Deception Technologies** — Honeypot (single decoy), Honeynet (network of decoys)

### 📜 Audits & Compliance

- [ ] **Regulations** — PCI DSS (payment cards), GDPR (EU privacy), HIPAA (healthcare), SOX (financial)
- [ ] **Data Considerations** — Data locality requirements, Data sovereignty, Data classification

### 🔀 Network Segmentation

- [ ] **Specialized Networks** — IoT networks, IIoT (Industrial IoT), SCADA, ICS (Industrial Control Systems), OT (Operational Technology)
- [ ] **Access Networks** — Guest networks, BYOD networks, Quarantine networks
- [ ] **Security Zones** — Screened subnet (DMZ), Trusted vs Untrusted zones, Extranet, Intranet

### ⚔️ Types of Attacks (COMMONLY TESTED)

**Network Attacks:**
| Attack | Description | Mitigation |
|--------|-------------|------------|
| **DoS/DDoS** | Overwhelm resources with traffic | Rate limiting, IPS |
| **VLAN hopping** | Switch spoofing, Double tagging | Disable DTP, Native VLAN |
| **MAC flooding** | Overflow CAM table | Port security |
| **ARP poisoning/spoofing** | Manipulate ARP cache | DAI, Static ARP |
| **DNS poisoning/spoofing** | Redirect DNS queries | DNSSEC |
| **Rogue DHCP** | Unauthorized DHCP server | DHCP snooping |
| **Rogue devices** | Unauthorized network devices | NAC, 802.1X |
| **Evil twin** | Malicious duplicate AP | Wireless IDS |
| **On-path attack** | Man-in-the-middle | Encryption, PKI |
| **Deauthentication** | Force wireless disconnect | WPA3, 802.11w |

**Social Engineering:**
- [ ] Phishing (email), Vishing (voice), Smishing (SMS)
- [ ] Spear phishing (targeted), Whaling (executive targeting)
- [ ] Dumpster diving, Shoulder surfing, Tailgating/Piggybacking, Pretexting

**Malware Types:**
- [ ] Ransomware, Viruses, Worms, Trojans, Rootkits, Keyloggers, Spyware

### 🛠️ Security Features & Defense

- [ ] **Device Hardening** — Disable unused ports/services, Change default credentials, Firmware updates, MAC filtering, Secure SNMP (v3)
- [ ] **Network Access Control (NAC)** — 802.1X, Posture assessment, Agent vs Agentless, Quarantine VLAN
- [ ] **Access Control Lists (ACLs)** — Standard ACLs (source IP only), Extended ACLs (source/dest IP, ports), Implicit deny at end
- [ ] **Firewall Concepts** — Stateful vs Stateless, URL/Content filtering, Application-aware filtering, Next-gen firewalls (NGFW)
- [ ] **Key Management** — Key rotation, Key escrow, Certificate lifecycle

---

## Domain 5: Network Troubleshooting (24%)

### 📋 Troubleshooting Methodology (MEMORIZE EXACT STEPS)

| Step | Action | Details |
|------|--------|---------|
| 1 | **Identify the problem** | Gather info, question users, identify symptoms, determine scope, duplicate if possible |
| 2 | **Establish a theory** | Question the obvious, consider multiple approaches, top-to-bottom/bottom-to-top OSI |
| 3 | **Test the theory** | Confirm theory; if not confirmed, re-establish new theory |
| 4 | **Establish a plan** | Plan resolution steps, identify potential effects, get approval if needed |
| 5 | **Implement solution** | Make changes or escalate as appropriate |
| 6 | **Verify functionality** | Confirm resolution, test full system, implement preventive measures |
| 7 | **Document findings** | Document findings, actions, outcomes, and root cause |

### 🔌 Cabling & Physical Interface Issues

**Cable Problems:**
- [ ] Incorrect cable type (straight-through vs crossover)
- [ ] Signal degradation/attenuation, Distance limitations
- [ ] Improper termination, Bad crimps
- [ ] TX/RX transposed (fiber polarity)
- [ ] EMI (Electromagnetic Interference), Crosstalk
- [ ] Opens, Shorts, Cable category mismatch

**Interface Issues:**
- [ ] Increasing error counters (CRC errors, runts, giants)
- [ ] Drops, Discards, Collisions
- [ ] Port status (up/down, error disabled)
- [ ] Speed/duplex mismatch
- [ ] PoE standards mismatch
- [ ] Transceiver mismatch, Bad SFP modules
- [ ] Signal strength (dBm) — Know that -30 dBm is strong, -80 dBm is weak

### 🌐 Network Services Issues

**Switching Issues:**
- [ ] STP problems (loops, blocked ports, incorrect root bridge)
- [ ] Incorrect VLAN assignment, VLAN mismatch
- [ ] Trunk/access port misconfiguration
- [ ] Native VLAN mismatch

**Routing Issues:**
- [ ] Missing routes in routing table
- [ ] Incorrect default gateway
- [ ] Routing loops, Asymmetric routing
- [ ] High administrative distance

**IP Issues:**
- [ ] Incorrect IP address/subnet mask/gateway
- [ ] Duplicate IP addresses
- [ ] Address pool exhaustion
- [ ] Expired DHCP leases

**ACL Issues:**
- [ ] Implicit deny blocking traffic
- [ ] Rules in wrong order
- [ ] Missing return traffic rules

### 📉 Performance Issues

- [ ] Network congestion, Bandwidth bottlenecks
- [ ] High latency, Jitter (VoIP issues)
- [ ] Packet loss, Retransmissions
- [ ] Broadcast storms

**Wireless-Specific:**
- [ ] Channel overlap/interference (use 1, 6, 11 for 2.4GHz)
- [ ] Co-channel interference (same channel, different APs)
- [ ] Adjacent channel interference
- [ ] Low RSSI/signal strength
- [ ] Roaming issues, AP handoff problems
- [ ] Capacity/client density issues

### 💻 Command-Line Tools (KNOW SYNTAX & OUTPUT)

| Command | Purpose | Key Options |
|---------|---------|-------------|
| `ping` | Test connectivity (ICMP) | `-t` (continuous), `-n` (count) |
| `tracert`/`traceroute` | Path discovery | Shows each hop |
| `pathping` | Combines ping + traceroute | Statistics per hop |
| `nslookup`/`dig` | DNS queries | Query specific record types |
| `ipconfig`/`ifconfig`/`ip` | IP configuration | `/all`, `/release`, `/renew`, `/flushdns` |
| `netstat` | Connection statistics | `-an` (all numeric), `-b` (process) |
| `arp` | ARP table | `-a` (display), `-d` (delete) |
| `route` | Routing table | `print`, `add`, `delete` |
| `nmap` | Port scanning | `-sS` (SYN scan), `-O` (OS detect) |
| `tcpdump` | Packet capture | `-i` (interface), `-w` (write to file) |

**Network Device Commands:**
- [ ] `show running-config` / `show config` — Current configuration
- [ ] `show interface` — Interface status and statistics
- [ ] `show ip route` / `show route` — Routing table
- [ ] `show vlan` — VLAN information
- [ ] `show mac-address-table` — MAC/CAM table

### 🔧 Hardware Tools & Analyzers

| Tool | Purpose |
|------|---------|
| **Cable tester** | Verify cable continuity, pinout |
| **Tone generator/probe** | Trace cables through walls |
| **Loopback adapter** | Test ports without network |
| **TDR** | Time Domain Reflectometer — Find cable faults (copper) |
| **OTDR** | Optical TDR — Find fiber faults/breaks |
| **Multimeter** | Test voltage, resistance |
| **Spectrum analyzer** | Analyze RF interference |
| **Wi-Fi analyzer** | Channel utilization, signal strength |
| **Protocol analyzer (Wireshark)** | Deep packet inspection |
| **TAP/Port mirror** | Copy traffic for analysis |

---

## Study Resources

| Resource | Type | Cost |
|----------|------|------|
| [CompTIA N10-009 Objectives PDF](https://www.comptia.org) | Official checklist | Free |
| Professor Messer | Video course | Free |
| Jason Dion / Dion Training | Practice exams & videos | Paid |
| Packet Tracer / GNS3 | Lab environment | Free |
| subnettingpractice.com | Subnetting drills | Free |
| Boson ExSim | Practice exams (PBQ practice) | Paid |
| Anki / Quizlet | Flashcard apps for ports | Free |

---

## 🎯 PBQ Practice Scenarios (Performance-Based Questions)

Expect 3–5 PBQs at the start of the exam. Practice these hands-on scenarios:

| Scenario | Skills Tested |
|----------|--------------|
| **VLAN Configuration** | Assign ports to VLANs, configure trunks, verify with `show vlan` |
| **Subnetting Worksheet** | Calculate network/broadcast/host range, determine subnet mask |
| **Network Troubleshooting** | Use CLI commands to diagnose connectivity issues |
| **IP Addressing** | Assign correct IPs to devices based on requirements |
| **Routing Configuration** | Add static routes, verify routing table |
| **Wireless Setup** | Configure SSID, security settings, channels |
| **ACL Creation** | Write permit/deny rules, apply to interfaces |
| **Network Diagram Analysis** | Identify devices, trace paths, find misconfigurations |
| **Cable Selection** | Match cable types to scenarios |
| **DNS/DHCP Troubleshooting** | Fix common configuration errors |

---

## Study Tips

1. **Study Priority** — Focus most time on Domain 1 (23%) and Domain 5 (24%) as they carry the highest weight
2. **Hands-on Practice** — CompTIA recommends 9–12 months of practical networking experience; consider labs with Packet Tracer or GNS3
3. **Prerequisite** — CompTIA A+ is recommended before taking Network+
4. **PBQ Strategy** — Flag PBQs and return to them after completing multiple choice; they're time-consuming
5. **Subnetting Drills** — Practice 10+ subnetting problems daily; mastering this is essential for passing
6. **Memorize Ports** — Create flashcards for ALL ports in the table above; expect 5-10 port questions
7. **Know Your Acronyms** — The exam loves testing acronym meanings (SLAAC, FHRP, SASE, etc.)
8. **Wireless Standards** — Memorize frequencies and speeds for each 802.11 standard
9. **Troubleshooting Method** — Know all 7 steps in exact order; this WILL be on the exam
10. **New N10-009 Topics** — Pay special attention to VXLAN, SD-WAN, Zero Trust, SASE, NTS, Wi-Fi 6/6E

---

## ⚡ Quick Review Checklist (Night Before Exam)

- [ ] All port numbers and their protocols
- [ ] OSI model — which protocols/devices at each layer
- [ ] 7-step troubleshooting methodology in order
- [ ] IPv4 private ranges (10.x, 172.16-31.x, 192.168.x)
- [ ] Subnetting: /24 = 256 IPs, /25 = 128, /26 = 64, /27 = 32, /28 = 16
- [ ] Wireless channels: 2.4 GHz = 1, 6, 11 non-overlapping
- [ ] DHCP DORA process
- [ ] DNS record types
- [ ] WPA2 vs WPA3 differences
- [ ] Recovery metrics: RPO, RTO, MTTR, MTBF
