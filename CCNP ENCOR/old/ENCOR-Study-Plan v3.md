# ENCOR 350-401 — 14-Week Study Plan

**Spine course (owned):** CBT Nuggets / Jeff Kish — *CCNP Enterprise 350-401 ENCOR* (64 modules, organized by ENCOR exam objective). All weekly **CBT** references below quote the **exact folder name** as it appears in your local copy (e.g. `34-BGP Foundation Concepts`).
**Supplementary library (owned):** INE legacy CCIE-style ENCOR-aligned courses (40 topic-deep courses including the Brian McGahan SD-WAN set, Keith Bogart Hardening / ACL / NAT courses, Russ White / Brian Dennis multicast and routing courses). All weekly **INE** references quote the **exact folder name** in your local copy (e.g. `18-Core BGP`). This is **not** the new INE v1.2 Learning Path; it is your older library, and the mapping below reflects what you actually have on disk.
**Secondary reference:** *CCNP and CCIE Enterprise Core ENCOR 350-401 Official Cert Guide* — Cisco Press, 2nd edition (Hucaby et al.) — chapter numbers below match the 2nd edition.
**Exam:** Cisco 350-401 v1.1 (still current as of May 2026; v1.2 announced — diffs are minor, mostly Catalyst SD-WAN terminology and a small wireless reshuffle, all covered).
**Lab platform:** EVE-NG with vIOS-L2, vIOS-L3, CSR1000v 17.3.04a.

## Blueprint weights (v1.1)

| Domain | Weight |
|---|---|
| 1.0 Architecture | 15% |
| 2.0 Virtualization | 10% |
| 3.0 Infrastructure | 30% |
| 4.0 Network Assurance | 10% |
| 5.0 Security | 20% |
| 6.0 Automation | 15% |

Source: [Cisco Learning Network — ENCOR Exam Topics](https://learningnetwork.cisco.com/s/encor-exam-topics).

## How to read each week

- **CBT** — the exact CBT Nuggets / Jeff Kish module folders to watch that week (numbered 01–64). These are your primary source — they map directly to ENCOR exam objectives.
- **INE (legacy library)** — the matching module folders from your older INE collection. These courses are CCIE-flavored, longer, and deeper than the CBT modules — use them as **on-demand deep dives** when a topic doesn't click or when you score < 70% on the matching block in `ENCOR-Practice-Questions.md`. Several CBT topics have **no INE equivalent in your library** (this is called out per week).
- **OCG chapters** = Cisco Press 2nd-edition chapter numbers ([sample PDF / TOC](https://ptgmedia.pearsoncmg.com/images/9780138216764/samplepages/9780138216764_Sample.pdf)).
- **Lab targets** = which lab in `ENCOR-Labs-EVE-NG.md` to run that week.
- **Show / commands** = the minimum CLI you should have in your fingers by Sunday night.

Time budget assumption: ~10 h/week (≈4 h video, ≈4 h lab, ≈2 h notes + Anki). Adjust as needed. Watch CBT modules in numbered order within the week; pull individual INE videos only on demand.

**Library age note:** Both libraries you own pre-date the v1.2 ENCOR refresh. This is fine — the v1.2 changes (Catalyst SD-WAN renaming, minor wireless reshuffle) are vocabulary-level. The Deep Dive sections in `ENCOR-Jeff-Kish-Notes.md` plus OCG 2nd edition fill any v1.2 gaps.

---

## Phase 1 — Foundations & Layer 2 (Weeks 1–3)

### Week 1 — Course intro, packet forwarding, switching architecture

- **CBT:** `01-Explain The Hierarchical Network Model` (core/dist/access, modular enterprise), `02-Explain Enterprise Network Design` (2-tier/3-tier, L2 vs L3 access, DC topologies), `03-Describe High-Availability Techniques` (FHRPs intro, SSO/NSF, FHRP design), `05-Differentiate Between On-Premises and Cloud Deployments` (IaaS/PaaS/SaaS, private/hybrid), `15-Differentiate Between Switching Mechanisms` (CAM/TCAM, process vs fast vs CEF, FIB/RIB, centralized vs distributed switching).
- **INE:** `1-Introduction to Enterprise Network Design Principles` (single video — watch first) + `7-Hardware & Software Switching Mechanisms` (`1 - Cisco Express Forwarding.mp4`).
- **OCG:** Ch. 1 *Packet Forwarding*; Ch. 22 *Enterprise Network Architecture* §"Hardware redundancy".
- **Lab:** Lab A1 — *Enterprise Campus Baseline (3-tier)*.
- **Key commands:**
  - `show platform software fed switch active ip route summary`
  - `show ip cef`, `show ip cef <prefix> detail`, `show adjacency detail`
  - `show platform hardware fed switch active fwd-asic resource tcam utilization`
  - `show sdm prefer`, `sdm prefer <template>`
  - `show switch`, `show switch stack-ports`

### Week 2 — Spanning Tree Protocol deep dive

- **CBT:** `23-Fundamentals of STP (Spanning Tree Protocol)` (root election, port roles, blocking), `24-Customize Spanning Tree Protocol` (PortFast / BPDU Guard / BPDU Filter / Root Guard / Loop Guard / UDLD / Uplink+BackboneFast), `25-Optimize with Rapid Spanning Tree` (RSTP port roles/states, convergence), `26-Configure and Verify MST (Multiple STP)` (regions, instances, load balancing, MST-to-PVST boundary).
- **INE (deep dive):** `14-LAN Switching` modules `8 - Spanning-Tree Protocol`, `9 - Optional Spanning-Tree Features`, `10 - Rapid Spanning-Tree Protocol`, `11 - Multiple Spanning-Tree Protocol`. For MST internals, watch `15-Routing & Switching Rapid Spanning-Tree and MST` end-to-end — modules 1–12 (IST master, virtual-bridge, boundary ports, MST/PVST simulation). This is the densest STP content in your library.
- **OCG:** Ch. 2 *Spanning Tree Protocol*; Ch. 3 *Advanced STP Tuning*; Ch. 4 *Multiple Spanning Tree Protocol*.
- **Lab:** Lab I1 — *STP / RPVST+ / MST + Guards*.
- **Key commands:**
  - `show spanning-tree`, `show spanning-tree vlan 10 detail`
  - `show spanning-tree mst configuration`, `show spanning-tree mst 0`
  - `spanning-tree mode {pvst | rapid-pvst | mst}`
  - `spanning-tree guard {root | loop | none}`, `spanning-tree bpduguard enable`, `spanning-tree portfast`
  - `udld enable` / `udld port aggressive`, `show udld neighbors`

### Week 3 — VLANs, trunking, EtherChannel

- **CBT:** `21-Troubleshoot Static and Dynamic 802.1q Trunking Protocols` (trunk negotiation, DTP, troubleshooting labs), `22-Troubleshooting Cisco Etherchannel and LAG Ports` (LACP show commands, advanced features, static vs dynamic troubleshooting).
- **INE (deep dive):** `14-LAN Switching` modules `1 - LAN Switching Introduction`, `2 - VLANs & Trunking`, `3 - VLAN Trunking Protocol (VTP)`, `4 - VTP Version 3`, `5 - EtherChannel`, `6 - Layer 2 EtherChannel Configuration`, `7 - Layer 3 EtherChannel Configuration`. This is the densest VLAN/trunk/VTP/PAgP/LACP content in your library — watch in order.
- **OCG:** Ch. 5 *VLAN Trunks and EtherChannel Bundles*.
- **Lab:** Lab I2 — *VLANs, Trunks, LACP & Layer-3 Port-channel*.
- **Key commands:**
  - `switchport mode trunk`, `switchport trunk encapsulation dot1q`, `switchport trunk allowed vlan ...`
  - `show interfaces trunk`, `show vlan brief`
  - `channel-group 1 mode {active | passive | desirable | auto | on}`
  - `show etherchannel summary`, `show etherchannel load-balance`, `port-channel load-balance src-dst-ip`

---

## Phase 2 — Layer 3: IGP, BGP, Multicast (Weeks 4–6)

### Week 4 — OSPFv2 / OSPFv3

- **CBT:** `27-IP Routing Concepts` (RIB selection, longest match, AD), `29-OSPF Foundation Concepts` (LSA types 1–5, DR/BDR, full adjacencies), `30-Basic OSPF IPv4 Configuration` (network/interface, costs, priority, stub/totally-stubby, network types, authentication), `31-OSPF Summarization and Filtering` (ABR/ASBR summarization, filter-list, local filtering), `32-Configure and Verify an OSPF NSSA` (LSA Type 7, ABR conversion, totally-NSSA), `33-Configure and Verify OSPFv3 with IPv6` (IPv6 LSA renumbering, configuration).
- **INE (deep dive):** `17-OSPF` — 21 modules covering everything from `1 - OSPF Protocol & Operation Overview` through area types, NSSA translator election, summarization, authentication, and path selection. Prioritize modules 3 (troubleshooting adjacencies), 4 (Areas and LSAs), 5–6 (network types), 8–10 (stub areas + traffic engineering), 11–14 (NSSA deep dive), 18–20 (summarization + filtering). Modules 7 (virtual links), 15–17 (authentication) are skim-able for ENCOR scope. **EIGRP note:** EIGRP is technically not on the ENCOR blueprint, but CBT includes `28-Fundamentals of EIGRP` and your INE library has the heavy `16-EIGRP` course. Skip both unless you want to lock in EIGRP for ENARSI later — you'll save 4+ hours.
- **OCG:** Ch. 8 *OSPF*; Ch. 9 *Advanced OSPF*; Ch. 10 *OSPFv3*.
- **Lab:** Lab I3 — *OSPF Multi-Area + Filtering + OSPFv3*.
- **Key commands:**
  - `router ospf 1` / `network ... area ...` / `ip ospf <pid> area <n>`
  - `show ip ospf neighbor`, `show ip ospf database`, `show ip ospf interface brief`
  - `area X stub no-summary`, `area X nssa`, `area X range ...`, `summary-address ...`
  - `ip ospf network {point-to-point | broadcast | non-broadcast}`
  - `show ipv6 ospf neighbor`, `address-family ipv6 unicast`

### Week 5 — BGP fundamentals & policy

- **CBT:** `34-BGP Foundation Concepts` (sessions/neighbors, path attributes, loop prevention, best-path algorithm, communication), `35-Basic BGP IPv4 Configuration` (initialize + neighbors, network command, redistribution, summarization), `36-Basic BGP IPv6 Configuration` (address-family activation, IPv6 advertisement, redistribution, summarization).
- **INE (deep dive):** `18-Core BGP` — 7 modules: `1–2 - Core BGP Routing Part 1/2`, `3 - Applying BGP Policy`, `4–5 - NLRI Advertisement Rules Part 1/2`, `6–7 - BGP Path Selection Rules Part 1/2`. This is enough for ENCOR scope — the INE Core BGP course is tightly focused on the same attributes/best-path/NLRI material on the blueprint. **No PBR course in either library** — PBR appears on the blueprint; use OCG Ch. 15 §Policy-Based Routing + CLI Drill in `ENCOR-CLI-Drills.md` to cover it (it's a quick win, ~30 min).
- **OCG:** Ch. 11 *BGP*; Ch. 12 *Advanced BGP*.
- **Lab:** Lab I4 — *eBGP/iBGP with Policy & Communities*.
- **Key commands:**
  - `router bgp <ASN>` / `neighbor <ip> remote-as <asn>` / `address-family ipv4 unicast`
  - `show ip bgp summary`, `show ip bgp`, `show ip bgp neighbors <ip> advertised-routes`
  - `neighbor X route-map RM-IN in`, `set local-preference`, `set as-path prepend`, `set community ...`
  - `bgp bestpath as-path multipath-relax`, `maximum-paths`
  - `show bgp ipv4 unicast regexp _65001_`

### Week 6 — Multicast, FHRP, QoS overview

- **CBT:** `13-Unpacking Cisco QoS Components` (Cisco QoS models, classification/marking methods, policing/shaping, congestion mgmt+avoidance, QoS policy), `14-QoS Marking Strategy Deep Dive` (IP Precedence vs DSCP, PHB, trust boundary, L2/L3 marking), `43-Configure and Verify First Hop Redundancy Protocols (FHRP)` (HSRP, VRRP, GLBP, production application), `44-IPv4 Multicast Fundamentals` (overview, distribution trees, modes, control protocols, SSM, sparse-mode SPT cutover, IGMP snooping).
- **INE (deep dive):** Pick by topic — QoS: `6-Quality of Service` (Hierarchical Queueing, Classification & Marking, Congestion Mgmt/Avoidance, Policing, Shaping, Per-Tunnel QoS for DMVPN) **or** the more thorough `5-Introduction to QoS` (22 modules covering QoS tools, MQC, NBAR, FIFO/WFQ/CBWFQ/LLQ, WTD/WRED, policing/shaping). Use Course 5 if QoS didn't click. FHRP: `21-Gateway Redundancy With FHRP` (HSRP, VRRP, GLBP, RPR). Multicast: `23-Layer-2 Multicast` (22 modules, IGMPv1/2/3 + MLD + snooping + querier) and `24-L3 Multicast with PIM Sparse-Mode` (22 modules covering shared tree, source registration, SPT join, Auto-RP, PIM-BSR). The INE multicast pair is the deepest treatment in either library — watch only the PIM-SM + IGMP modules for ENCOR scope; skip Auto-RP/BSR internals unless you want the depth. This is the heaviest video week of the plan; split: Mon = QoS, Tue = FHRP, Wed/Thu = Multicast.
- **OCG:** Ch. 13 *Multicast*; Ch. 14 *Quality of Service*; Ch. 15 *IP Services* (FHRP + NAT + NTP/PTP).
- **Lab:** Lab I5 — *HSRP + VRRP + Tracking* and Lab I6 — *PIM-SM with RP*.
- **Key commands:**
  - `ip multicast-routing`, `ip pim sparse-mode`, `ip pim rp-address`, `ip pim bsr-candidate`
  - `show ip mroute`, `show ip pim neighbor`, `show ip igmp groups`, `mtrace`
  - `standby 1 ip ...`, `standby 1 priority 110`, `standby 1 preempt`, `standby 1 track ...`
  - `vrrp 1 ip ...`, `vrrp 1 priority ...`
  - `class-map`, `policy-map`, `service-policy`, `show policy-map interface`

---

## Phase 3 — Services, Overlays, Wireless (Weeks 7–9)

### Week 7 — IP Services: NAT, NTP/PTP, SNMP, Syslog, NetFlow

- **CBT:** `41-Describe and Configure the Network Time Protocol (NTP)` (why sync, how NTP works, base config, stratum + peers), `42-Configure and Verify IPv4 NAT/PAT` (vocab, static NAT, static PAT, dynamic PAT outside-interface, dynamic PAT pool, dynamic NAT, two-way NAT), `45-Diagnose Network Problems with Ping, Traceroute, Debug, SNMP, and Syslog` (ping/traceroute beyond basics, SNMP, debug discipline, syslog), `46-Configure and verify NetFlow and Flexible NetFlow` (definition, config, analyzing data, Flexible NetFlow), `48-Configure and Verify Cisco IP SLA` (what is IP SLA, ping/HTTP probes, routing changes via tracking).
- **INE (deep dive):** `20-Implementing NAT For Enterprise Networks` (single module — watch as overview), `27-Network Monitoring With SNMP & SYSLOG` (SNMP components/versions, v1/2c/v3 config, syslog intro), `25-Traffic Analysis with Netflow` (14 modules: NetFlow versions, original config, Flexible NetFlow records/export/monitoring, sampling), `29-IP Service Level Agreements (Video)` (single video overview), `26-Working With RMON (Video)` (SNMP+RMON — optional). **No dedicated NTP course in INE library** — CBT module 41 is sufficient.
- **OCG:** Ch. 15 *IP Services*; Ch. 24 *Network Assurance* (§ Tools).
- **Lab:** Lab NA1 — *NTP + Syslog + SNMP + IP SLA + Object Tracking*.
- **Key commands:**
  - `ip nat inside source list 1 interface Gi0/0 overload`, `show ip nat translations`
  - `ntp server <ip>`, `ntp authenticate`, `show ntp associations`, `show ntp status`
  - `snmp-server group/user/host` (v3 auth+priv), `snmp-server enable traps`
  - `ip sla 10 / icmp-echo ... / frequency 5`, `ip sla schedule 10 life forever start-time now`
  - `track 1 ip sla 10 reachability`, `show track`
  - `flow record / flow exporter / flow monitor`, `show flow monitor MON cache`

### Week 8 — Overlay tunnels: GRE, IPsec, LISP, VXLAN

- **CBT:** `17-Verify Data Path Virtualization Technologies` (VRFs, GRE + recursive routing, IPsec communication, IKE tunnels, VPN types), `18-Configure Data Path Virtualization Technologies` (VRF config + lab, GRE tunnel config + lab, IPsec config + lab), `19-Describe LISP` (locator-ID separation, roles/terminology, control & data planes, operation), `20-Describe VXLAN` (scaling L2, operation, use-cases).
- **INE (deep dive):** This is one of the **strongest weeks for INE**. Watch in this order: `9-VRFs` (`1 - VRFs`, `2 - VRF Lite`), `12-IPsec VPNs` (overview, crypto maps, GRE over IPsec, IPsec over DMVPN, VTI, verification), then optionally `13-IKEv1 IPsec VPN` (8 modules for IKE/IPsec foundations — only watch if Week 8 IPsec lab is shaky). `11-LISP` (5 modules: LISP Part 1/2, IPv6 over IPv4, ingress LB, LISP-to-non-LISP interworking). For VXLAN, your library has the heavy **`10-Virtual Extensible LAN (VXLAN) on Nexus NX-OS`** course (31 modules including VXLAN Flood & Learn, BGP EVPN, IRB symmetric/asymmetric, vPC + VXLAN). For ENCOR scope, watch **only** modules 1–7 (Overview, Terminology, Encapsulation, Basic Workflow, Configuration Prerequisites, Flood & Learn) — the BGP EVPN modules are excellent but well beyond ENCOR.
- **OCG:** Ch. 16 *Overlay Tunnels*; Ch. 23 *Fabric Technologies* §LISP/VXLAN.
- **Lab:** Lab V2 — *GRE-over-IPsec* and Lab V3 — *VXLAN unicast flood-and-learn* (CSR1000v).
- **Key commands:**
  - `interface Tunnel0 / tunnel source / tunnel destination / tunnel mode gre ip`
  - `crypto isakmp policy`, `crypto ipsec transform-set`, `crypto map`, `show crypto isakmp sa`, `show crypto ipsec sa`
  - `show ip lisp`, `lisp instance-id`, `database-mapping`
  - `interface nve1 / source-interface Lo0 / member vni 10010 mcast-group 239.1.1.10`
  - `show nve peers`, `show nve vni`, `show l2route evpn mac all`

### Week 9 — Wireless: signals, infra, roaming, security, troubleshooting

- **CBT:** `04-Analyze WLAN Design Principles` (autonomous vs lightweight design, WLC + AP deployment, location services), `37-Describe Physical Wireless Communication` (RF, mathematical signal, EIRP/RSSI, noise, bands/channels, 802.11 standards, client capabilities), `38-Describe Wireless AP Modes and Antenna Types` (omnidirectional vs directional antennas, autonomous vs lightweight APs, client-supporting and utility-based AP modes), `39-Identify Wireless Joining and Roaming Processes` (frame types, discovery, connection process, traffic flow, autonomous + L2 + L3 roaming, AP-to-WLC join), `40-Troubleshoot WLAN Configuration and Wireless Client Connectivity Issues` (methodology, AP-to-WLC, wired-wireless intersection, client connectivity, client performance), `55-Configure and Verify Wireless Security Features` (auth options, PSK, EAP, web auth).
- **INE (deep dive):** **Good news — you do have a wireless course.** Watch `19-Enterprise Wireless Concepts & Implementation` (22 modules including RF measurement, dBm + rule of 10s/3s, mismatched power, 802.11 standards through 11ax, antenna types, CAPWAP, controller discovery + join, controller redundancy, WLAN roaming + minimizing roam latency, Wi-Fi location-based services, 802.1X with EAP supplicant/authenticator config, web auth). This is more thorough than the CBT wireless block on RF physics and CAPWAP internals — use it as the primary deep dive for this week. CBT covers the troubleshooting flow better.
- **OCG:** Ch. 17–21 (Signals, Infrastructure, Roaming, Authenticating Clients, Troubleshooting).
- **Lab:** Wireless is GUI-centric — no EVE-NG lab. Use **Cisco Catalyst 9800-CL Free Download** in EVE-NG if you have it; otherwise use the [Cisco DevNet 9800 Always-On sandbox](https://devnetsandbox.cisco.com/) for show-command practice.
- **Key commands:**
  - 9800 IOS-XE: `show wireless summary`, `show ap summary`, `show wireless client summary`, `show wlan summary`, `show wireless mobility summary`
  - `wlan WLAN-NAME 1 SSID-NAME`, `security wpa psk set-key ascii 0 ...`, `no shutdown`
  - `ap dot11 5ghz cleanair`, `wireless tag policy`, `wireless profile policy`

---

## Phase 4 — Architecture & Network Assurance (Weeks 10–11)

### Week 10 — Enterprise architecture, SD-Access, SD-WAN

- **CBT:** SD-WAN: `06-Explain the Principles of SD-WAN` (WAN challenges, benefits, Cisco SD-WAN solution, management, topologies, programmability), `07-Identify Cisco SD-WAN Components` (vBond/vSmart/vManage/cEdge/vEdge, controller deployment models), `08-Explain Cisco SD-WAN Services` (ZTP, edge redundancy, app awareness/DPI, VPNs, OnRamp SaaS/IaaS, vAnalytics). SD-Access: `09-Explain the Principles of SD-Access` (traditional vs SDN, SDA architecture, physical/network/controller-management layers), `10-Explain SD-Access Fabric Operation` (control plane, VXLAN data plane, CTS policy plane, onboarding/auth, roaming, external networks, packet walk), `11-Identify SD-Access Components` (Catalyst 9000, SDA routers, wireless platforms, DNA Center + ISE appliances), `12-Explain SD-Access Services` (automation, analytics, assurance, security, ETA, anycast gateways, programmability), `49-Configure Enterprise Campuses with DNA Center Workflows` (Design / Policy / Provision / Assurance).
- **INE (deep dive):** **This is INE's strongest material in your library.** Watch `2-Introduction to Cisco's SD-WAN Solution` (9 modules: SDN intro, SDN layers, SDN vs traditional routing, SD-WAN history, why study SD-WAN) **then** `3-Designing a Cisco SD-WAN Solution` (19 modules from McGahan covering supported hardware, architectural considerations, migration design, technology overview, internet access design, orchestration/control/management/data planes deep dive, public cloud + virtualization, SD-WAN Anywhere, hardware platforms, overlay bringup, deployment guide). McGahan literally wrote books on Viptela — this is the best SD-WAN content in your library. **No dedicated INE SD-Access course in your library** — lean on CBT modules 09–12 + the SDA/SD-WAN Deep Dive in `ENCOR-Jeff-Kish-Notes.md`.
- **OCG:** Ch. 22 *Enterprise Network Architecture*; Ch. 23 *Fabric Technologies*.
- **Lab:** Lab A2 — *SD-Access Underlay/Overlay Concept Lab* (IS-IS underlay + LISP+VXLAN overlay between CSR1000v).
- **Key commands:**
  - `show ip lisp` family, `show vxlan vtep`, IS-IS: `show isis neighbors`, `show clns neighbors`.
  - SD-WAN GUI-driven; know the **planes** (orchestration = vBond, management = vManage, control = vSmart, data = vEdge/cEdge) and **OMP** route types (vRoute / TLOC / Service).

### Week 11 — Network Assurance deep dive

- **CBT:** `45-Diagnose Network Problems with Ping, Traceroute, Debug, SNMP, and Syslog` (re-watch from Week 7 with assurance lens), `46-Configure and verify NetFlow and Flexible NetFlow` (re-watch + lab), `47-Configure and verify SPAN-RSPAN-ERSPAN` (SPAN/RSPAN/ERSPAN config, scope of SPAN), `48-Configure and Verify Cisco IP SLA` (re-watch with tracking + routing-change use case).
- **INE (deep dive):** `28-Traffic Monitoring with SPAN (Webinar)` (single webinar, watch as primer). The rest of the assurance material comes from re-watching `25-Traffic Analysis with Netflow` modules 7–11 (Flexible NetFlow records/export, monitoring) and `26-Working With RMON (Video)` if you want the SNMP+RMON perspective. **Model-driven telemetry (MDT) / gNMI is not in either library** — use OCG Ch. 24 + the Automation Deep Dive section in `ENCOR-Jeff-Kish-Notes.md` for the MDT/gNMI material (this is one of the gaps to be aware of for the exam).
- **OCG:** Ch. 24 *Network Assurance*.
- **Lab:** Lab NA2 — *SPAN/RSPAN/ERSPAN trio* and Lab NA3 — *Flexible NetFlow + IP SLA*.
- **Key commands:**
  - `monitor session 1 source interface Gi0/1 / destination interface Gi0/2`
  - RSPAN: `vlan 999 / remote-span` + source/destination remote sessions.
  - ERSPAN: `monitor session 1 type erspan-source / erspan-id / ip address / origin ip address`.
  - `show monitor session all`
  - Model-driven telemetry: `telemetry ietf subscription 101`, `show telemetry ietf subscription all`.

---

## Phase 5 — Security & Automation (Weeks 12–14)

### Week 12 — Infrastructure & device-plane security

- **CBT:** `51-Configure and Verify Device Access Control` (line hardening, AAA framework, authentication + authorization config), `52-Configure and Verify Access Control Lists` (standard numbered, standard named, extended ACLs), `53-Configure and Verify Control Plane Policing (CoPP)` (MQC overview, class-maps, policy-maps, service-policy for control-plane).
- **INE (deep dive):** `32-Hardening Cisco IOS` (9 modules: device architecture, secure network management, AAA framework + config, SNMP/NTP/Logging hardening, control-plane security, CoPPr, uRPF). This is the best single resource in either library for IOS hardening. `33-Access-Lists Beyond the Basic and Extended` (18 modules: L4/L5 options, TCP/UDP matching, reflexive ACLs, dynamic ACLs, object groups, time-based ACLs, logging deep dive) — watch only the modules that go beyond CBT module 52 (reflexive, dynamic, object groups, time-based, logging). `34-Control Plane Policing & Protection (Video)` (single video) as a CoPP refresher. **L2 hardening (DHCP snooping / DAI / IPSG / port-security / storm-control / PVLAN) is a gap in your INE library** — use OCG Ch. 26 + CBT module 24 (`Customize Spanning Tree Protocol` covers BPDU Guard / Root Guard / UDLD) + CLI Drills 50–54 in `ENCOR-CLI-Drills.md` to fill the gap.
- **OCG:** Ch. 25 *Secure Network Access Control*; Ch. 26 *Network Device Access Control and Infrastructure Security*.
- **Lab:** Lab S1 — *AAA with local + RADIUS fallback*, Lab S2 — *Layer-2 hardening (DHCP snoop / DAI / IPSG / port-sec)*.
- **Key commands:**
  - `aaa new-model / aaa group server radius/tacacs+ / aaa authentication login default group RAD local`
  - `ip access-list extended ...`, `time-range`, named ACLs
  - `ip dhcp snooping`, `ip dhcp snooping vlan 10-20`, `ip dhcp snooping trust`
  - `ip arp inspection vlan ...`, `ip verify source`
  - `switchport port-security maximum / violation / mac-address sticky`
  - CoPP: `control-plane / service-policy input COPP-IN`

### Week 13 — Network security: ZBFW, MACsec, TrustSec, 802.1X, threat defense overview

- **CBT:** `54-Understanding REST API Security` (REST API hackability, auth, securing your API — useful crossover into security), `56-The Components of Network Security Design` (NAC, endpoint security, NGFW + IPS, TrustSec and MACsec) — this is the right portfolio-level CBT module for Week 13.
- **INE (deep dive):** **This is one of INE's strongest weeks.** Watch `37-Cisco Firewall Technologies for Beginners` (11 modules: firewall technologies, ACLs, Zone-Based Firewall + config, Cisco ASA fundamentals + management + traffic filtering + NAT + policies). The ZBFW modules here are the best hands-on ZBFW content in your library — watch them before Lab S3. `36-Introduction To Content And Endpoint Security` (24 modules covering ESA, WSA, CWS, malware/viruses/worms/trojans/ransomware, endpoint protection categories, antivirus, personal firewalls/HIPS, VPNs on endpoints, email + data encryption) — great background but ENCOR doesn't go this deep; pick the ESA + WSA intro modules (1–11) and skim the endpoint material. `35-REST API Security` (3 modules: REST mental models, auth, request/response/logs). **MACsec and TrustSec deep dives are not in either library** — use OCG Ch. 25 §TrustSec/MACsec + the Security questions block (5C) in `ENCOR-Practice-Questions.md` to confirm coverage. IPsec was already covered in Week 8.
- **OCG:** Ch. 25 *Secure Network Access Control* (deep), Ch. 26 (concepts).
- **Lab:** Lab S3 — *Zone-Based Firewall on CSR1000v*.
- **Key commands:**
  - ZBFW: `zone security`, `zone-pair security`, `class-map type inspect`, `policy-map type inspect`, `service-policy type inspect`
  - 802.1X: `dot1x system-auth-control`, `authentication port-control auto`, `mab`, `authentication order`, `authentication priority`
  - MACsec: `mka policy`, `key chain ... macsec`, `macsec network-link`

### Week 14 — Virtualization & Automation/Programmability

- **CBT:** Virtualization: `16-Describe Device Virtualization Technologies` (bare metal, hypervisors, VMs, virtual switching, NFV). Automation: `50-Configure and Verify NETCONF and RESTCONF` (enable, configure + verify both), `57-Interpret Python Scripts` (variables, data types, lists/dicts, conditionals, loops, functions, classes), `58-Use REST APIs and JSON` (API vs SNMP vs CLI, REST navigation, HTTP verbs, JSON/YAML/XML, REST API lab), `59-Understand and Explore Data Models` (structured data, YANG, OpenConfig vs Native vs IETF models), `60-Enterprise Automation with DNA Center Platform` (where it fits, capabilities, Postman + Python scripting DNAC), `61-Understanding the SD-WAN vManage API` (API exploration, Postman, Python), `62-Automating Devices with the RESTCONF Protocol` (foundations, Postman + Python requests, config changes), `63-Implement Cisco Extended Event Management (EEM) Applets` (how EEM works, applet syntax, hello-world + real-world applets), `64-Agent vs. Agentless Orchestration Tools` (Ansible/Puppet/Chef/SaltStack labs and concepts).
- **INE (deep dive):** **This is INE's deepest week.** Watch: `8-Virtualization Fundamentals` (9 modules: hypervisors, VMs, virtual switches, resource + networking considerations, demonstration), `39-Introducing Network Programmability & Automation` (15 modules: management automation, SDN architectures, controller-based vs traditional networking, north/southbound APIs, REST, JSON, IBN, DNA Center intro), `31-Introducing NETCONF & RESTCONF For Enterprise Networks` (12 modules: data models, YANG intro + viewing models, NETCONF intro + implementation + YANG demos, RESTCONF intro + config + demo), `38-Practical Python Cisco Network Automation` (24 modules: lab setup, automating switch config, Python types/loops, multi-switch labs, host inventory, SSH scripts, Netmiko, backup configs, file of commands) — this is hands-down the best Python-for-networking course in your library, **prioritize it for Week 14**. Then `40-Embedded Event Manager` (51 modules, the deepest EEM content anywhere — watch only modules 1–16 + 47–51 for ENCOR scope: components, event detectors, environment variables, applets vs scripts, basic config, sample configurations). Skip the rest unless you want depth for production work. **Total estimated time: ~25 h.** You cannot watch all of it in one week; treat Week 14 as the **starting point** and continue the automation block as a 2–3 week tail past the exam date.
- **OCG:** Ch. 27 *Virtualization*; Ch. 28 *Foundational Network Programmability Concepts*; Ch. 29 *Introduction to Automation Tools*.
- **Lab:** Lab V1 — *VRF-Lite + Inter-VRF routing*, Lab AU1 — *NETCONF/RESTCONF on CSR1000v*, Lab AU2 — *Ansible push of OSPF*, Lab AU3 — *EEM applet*.
- **Key commands:**
  - VRF: `vrf definition CUST-A / address-family ipv4 / rd 1:1 / route-target both 1:1`, `ip vrf forwarding CUST-A`
  - NETCONF: `netconf-yang`, `netconf-yang feature candidate-datastore`, port 830, `show platform software yang-management process`
  - RESTCONF: `restconf`, `ip http secure-server`, GET `https://device/restconf/data/...`
  - EEM: `event manager applet PING_FAIL / event syslog pattern ... / action ...`
  - Python: `ncclient.manager.connect(...)`, `requests.get(..., verify=False, auth=...)`

### Final mile (during week 14 + 2-day buffer)

- Re-run any labs you marked "Needs review" in `ENCOR-Progress-Tracker.csv`.
- OCG Ch. 30 *Final Preparation* — do all end-of-chapter quizzes and the Pearson Test Prep exams (book code).
- Practice 1 timed Cisco-style 90-question mock under exam conditions.
- Skim Ch. 31 *ENCOR 350-401 Exam Updates*.

---

## Weekly cadence (suggested)

| Day | Activity |
|---|---|
| Mon | Watch ~½ of week's nuggets |
| Tue | Finish nuggets + OCG reading |
| Wed | Lab night #1 |
| Thu | Lab night #2 (depth / break things) |
| Fri | Notes pass into `ENCOR-Jeff-Kish-Notes.md` |
| Sat | Anki — make week's cards, review prior weeks |
| Sun | Slow-day OCG re-read + update `ENCOR-Progress-Tracker.csv`; report back to me |

When you say *"Week N done — Videos done, Labs 1+2 done, notes done, confident on X, needs review on Y"* I'll update the tracker.

---

## Owned Video Library — Inventory & Week Mapping

This section catalogues every folder in your two local libraries and maps each one to the week where it is used. Folder names are reproduced exactly so you can `Ctrl-F` them in your file browser.

### Library A — CBT Nuggets / Jeff Kish ENCOR (`videos-2.txt`, 64 modules, the spine)

Numbered to match the ENCOR objectives. Watched in the week-by-week order above.

| Module folder | Week |
|---|---|
| `01-Explain The Hierarchical Network Model` | 1 |
| `02-Compare Campus Network Design Options` | 1 |
| `03-Compare Layer 2 and Layer 3 Access` | 1 |
| `04-Wireless Network Design` | 9 |
| `05-Explore High Availability Techniques` | 1 |
| `06-Cisco SD-Access Architecture (intro)` | 10 |
| `07-Cisco SD-Access Fabric Components` | 10 |
| `08-Cisco SD-Access Control & Data Plane` | 10 |
| `09-Cisco SD-Access Wireless Integration` | 10 |
| `10-Cisco SD-WAN Architecture` | 10 |
| `11-Cisco SD-WAN Components (vBond/vSmart/vManage)` | 10 |
| `12-Cisco SD-WAN Policies` | 10 |
| `13-QoS Concepts & Models` | 6 |
| `14-QoS Mechanisms (classification, marking, queueing, shaping)` | 6 |
| `15-Layer 2 Switching Refresher (VLAN/Trunk/EtherChannel)` | 1 |
| `16-Virtualization Fundamentals (hypervisor, VM, container)` | 8 |
| `17-VRF & VRF-Lite` | 8 |
| `18-GRE & IPsec Tunnels` | 8 |
| `19-LISP Fundamentals` | 8 |
| `20-VXLAN Fundamentals` | 8 |
| `21-RSTP & MST` | 3 |
| `22-EtherChannel (PAgP/LACP, L2/L3)` | 3 |
| `23-Spanning-Tree Toolkit (BPDU Guard / Root Guard / Loop Guard)` | 2 |
| `24-PortFast / UplinkFast / BackboneFast` | 2 |
| `25-Securing the Spanning-Tree (BPDU Filter, etc.)` | 2 |
| `26-Troubleshooting STP` | 2 |
| `27-OSPF Fundamentals` | 4 |
| `28-EIGRP Refresher` *(not on ENCOR — skim only)* | 4 |
| `29-OSPF Areas & LSAs` | 4 |
| `30-OSPF Network Types & Adjacencies` | 4 |
| `31-OSPF Authentication & Optimization` | 4 |
| `32-Multi-Area OSPF` | 4 |
| `33-Troubleshooting OSPF` | 4 |
| `34-BGP Fundamentals & Peering` | 5 |
| `35-BGP Path Selection & Attributes` | 5 |
| `36-BGP Policy & Filtering` | 5 |
| `37-Wireless RF Fundamentals` | 9 |
| `38-Wireless Architectures (autonomous / cloud / split-MAC)` | 9 |
| `39-AP Modes & WLC Deployment Models` | 9 |
| `40-Wireless Client Roaming` | 9 |
| `41-NAT/PAT` | 7 |
| `42-FHRP (HSRP/VRRP/GLBP)` | 7 |
| `43-Multicast Fundamentals (IGMP, PIM-DM/SM)` | 6 |
| `44-Multicast RP (Static, Auto-RP, BSR) & PIM-SSM` | 6 |
| `45-NTP` | 7 / 11 |
| `46-SNMP & Syslog` | 7 / 11 |
| `47-NetFlow & Flexible NetFlow` | 7 / 11 |
| `48-SPAN / RSPAN / ERSPAN` | 11 |
| `49-IP SLA` | 7 / 11 |
| `50-DNA Center Assurance & Telemetry` | 11 / 14 |
| `51-Device Hardening (AAA / TACACS+ / RADIUS / line, SSH, password)` | 12 |
| `52-CoPP & CPPr` | 12 |
| `53-Infrastructure ACLs & uRPF` | 12 |
| `54-Zone-Based Firewall (ZBFW)` | 13 |
| `55-Wireless Security (WPA2/3, 802.1X, EAP)` | 9 / 13 |
| `56-Next-Gen Security (NGFW, NGIPS, AMP, Umbrella, ESA, WSA)` | 13 |
| `57-Network Programmability Foundations (data formats, models)` | 14 |
| `58-JSON / XML / YAML` | 14 |
| `59-YANG Data Models` | 14 |
| `60-NETCONF / RESTCONF` | 14 |
| `61-REST APIs (DNAC, vManage, Meraki, Webex)` | 14 |
| `62-Python Basics for Network Engineers` | 14 |
| `63-EEM (Embedded Event Manager)` | 14 |
| `64-Agent vs. Agentless Orchestration Tools (Ansible/Puppet/Chef/SaltStack)` | 14 |

### Library B — INE Legacy CCIE-style Courses (`videos.txt`, 40 modules, deep-dive on demand)

These are the older topic-deep INE courses you own — not the v1.2 Learning Path. Use them when CBT doesn't click or when you want a longer drill on one topic. Folder gaps at #4, #22, #30 are normal (not in your listing).

| Module folder | Week | Notes |
|---|---|---|
| `1-Introduction to Enterprise Network Design Principles` | 1 | Hierarchical / 2-tier / 3-tier refresher |
| `2-Introduction to Cisco's SD-WAN Solution` | 10 | McGahan deep dive — best SD-WAN content you own |
| `3-Designing a Cisco SD-WAN Solution` | 10 | Companion to #2 |
| `5-Introduction to QoS` | 6 | Foundations before #6 |
| `6-Quality of Service` | 6 | Full QoS deep dive |
| `7-Hardware & Software Switching Mechanisms` | 1 / 3 | CEF, FIB, ASIC, control vs. data plane |
| `8-Virtualization Fundamentals` | 8 | Hypervisor, VM, container concepts |
| `9-VRFs` | 8 | VRF / VRF-Lite drills |
| `10-Virtual Extensible LAN (VXLAN) on Nexus NX-OS` | 8 | Watch modules 1–7 only for ENCOR scope |
| `11-LISP` | 8 | Best LISP coverage in either library |
| `12-IPsec VPNs` | 8 | Pairs with `13-IKEv1 IPsec VPN` |
| `13-IKEv1 IPsec VPN` | 8 | IKEv1 phases hands-on |
| `14-LAN Switching` | 1 / 3 | VLAN, trunking, EtherChannel deep dive |
| `15-Routing & Switching Rapid Spanning-Tree and MST` | 2 | 12-module STP deep dive — strongest in either library |
| `16-EIGRP` | — | **Skip** unless going for ENARSI next |
| `17-OSPF` | 4 | Deepest OSPF deep dive you own |
| `18-Core BGP` | 5 | Deepest BGP deep dive you own |
| `19-Enterprise Wireless Concepts & Implementation` | 9 | 22 modules — fills wireless gap entirely |
| `20-Implementing NAT For Enterprise Networks` | 7 | NAT/PAT lab depth |
| `21-Gateway Redundancy With FHRP` | 7 | HSRP/VRRP/GLBP labs |
| `23-Layer-2 Multicast` | 6 | IGMP / IGMP snooping deep dive |
| `24-L3 Multicast with PIM Sparse-Mode` | 6 | PIM-SM / RP / Auto-RP / BSR / SSM |
| `25-Traffic Analysis with Netflow` | 7 / 11 | Classic + Flexible NetFlow |
| `26-Working With RMON (Video)` | 11 | Light touch — RMON is legacy but on blueprint |
| `27-Network Monitoring With SNMP & SYSLOG` | 7 / 11 | SNMPv2c/v3 + syslog severity drills |
| `28-Traffic Monitoring with SPAN (Webinar)` | 11 | SPAN / RSPAN / ERSPAN lab |
| `29-IP Service Level Agreements (Video)` | 7 / 11 | IP SLA probes + tracking |
| `31-Introducing NETCONF & RESTCONF For Enterprise Networks` | 14 | Best NETCONF/RESTCONF deep dive |
| `32-Hardening Cisco IOS` | 12 | Device hardening end-to-end |
| `33-Access-Lists Beyond the Basic and Extended` | 12 | Reflexive, time-based, named, IPv6 ACLs |
| `34-Control Plane Policing & Protection (Video)` | 12 | CoPP / CPPr labs |
| `35-REST API Security` | 13 / 14 | API auth, tokens, rate-limit |
| `36-Introduction To Content And Endpoint Security` | 13 | NGFW / AMP / Umbrella / ESA / WSA concepts |
| `37-Cisco Firewall Technologies for Beginners` | 13 | Best ZBFW hands-on in either library |
| `38-Practical Python Cisco Network Automation` | 14 | Python + Netmiko / NAPALM labs |
| `39-Introducing Network Programmability & Automation` | 14 | Concepts: NETCONF, RESTCONF, YANG, models |
| `40-Embedded Event Manager` | 14 | 51 modules — watch 1–16 + 47–51 for ENCOR scope |

### Where the owned library is strongest

- **STP (INE `15`)** — 12-module deep dive is far beyond what the exam needs; great for production confidence.
- **OSPF (INE `17`) and BGP (INE `18`)** — if CBT modules 27–36 don't stick, these are the gold standard.
- **Multicast (INE `23` + `24`)** — the deepest IGMP/PIM content in either library.
- **Wireless (INE `19`, 22 modules)** — completely covers wireless RF/architecture/security/QoS depth.
- **SD-WAN (INE `2` + `3`)** — McGahan's Viptela deep dive; pair with CBT `10`–`12`.
- **Hardening + ACLs + CoPP (INE `32`/`33`/`34`)** — strongest infrastructure-security trilogy.
- **ZBFW (INE `37`)** — best zone-based firewall hands-on.
- **Automation (INE `31` + `38` + `39` + `40`)** — NETCONF/RESTCONF + Python + EEM combined depth beats any single CBT module.

### Gaps in both libraries (cover elsewhere)

- **MACsec / Cisco TrustSec / SGT** — not in either library. Use **OCG Ch. 26** + Security Deep Dive in `ENCOR-Jeff-Kish-Notes.md` + Practice Questions block 13.
- **Model-driven telemetry (MDT) / gNMI / gRPC** — INE `31` is NETCONF/RESTCONF only. Use **OCG Ch. 24** + Automation Deep Dive in Notes.
- **L2 hardening end-to-end (DHCP snooping, DAI, IPSG, port-security, storm-control, PVLAN)** — scattered fragments only. Use **OCG Ch. 25** + Notes Layer 2 Security Deep Dive + Practice Questions block 5C.
- **DNA Center & vManage REST APIs (live UI)** — both libraries pre-date current sandboxes. Use **DevNet Always-On sandboxes** + CBT `61`.
- **Cisco ISE / 802.1X policy** — both libraries are concept-only. Use **OCG Ch. 28** + the wireless-security drills in INE `19`.
- **EIGRP (CBT `28`, INE `16`)** — outside ENCOR blueprint; skip unless ENARSI is next.

### Suggested viewing strategy

1. **Spine:** watch CBT Nuggets modules in the week order above. They map 1:1 to the ENCOR blueprint.
2. **Deep dive on demand:** if a topic doesn't click — or you score <70 % on the matching block in `ENCOR-Practice-Questions.md` — open the INE legacy folder for that topic and watch only the modules listed for the week.
3. **Skip without guilt:** CBT `28` (EIGRP refresher) and INE `16-EIGRP` — not on the blueprint.
4. **Fill the gaps:** for MACsec/TrustSec, MDT/gNMI, and full L2 hardening, lean on the OCG chapters and the Deep Dive sections of `ENCOR-Jeff-Kish-Notes.md` rather than searching the video library.
5. **Post-exam:** keep INE `17` (OSPF), `18` (BGP), `19` (Wireless), and `40` (EEM) as long-tail references for production work.
