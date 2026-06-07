# ENCOR 350-401 — 14-Week Study Plan

**Spine course:** CBT Nuggets — *CCNP Enterprise: Implementing and Operating Cisco Enterprise Network Core Technologies (350-401 ENCOR)* with Jeff Kish (and co-instructors Knox Hutchinson / Keith Barker on select skills).
**Parallel video reference:** INE — *Enterprise CORE Exam: 350-401 ENCOR v1.2 Learning Path* (33 courses, ~87 h video, instructors Keith Bogart / Rohit Pardasani / Brian McGahan). [Learning path on INE](https://my.ine.com/Networking/learning-paths/00836fff-00c8-49b0-a54b-b34071c15c48/enterprise-core-exam-350-401-encor-v12).
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

- **CBT Nuggets section / videos** — I match by **skill section name + Nugget title**, not by index, because CBT Nuggets re-orders videos when refreshing courses. When you load the course, search for the skill name (e.g. "Spanning Tree Protocol") and watch the nuggets in order.
- **INE (parallel)** — the equivalent course(s) in the INE v1.2 Learning Path. Numbering matches the master table at the end of this document ("INE Course Reference"). INE is structured by **topic depth + instructor**, not by exam domain, so several weeks pull from non-adjacent INE courses — use this column rather than the INE section order.
- **OCG chapters** = Cisco Press 2nd-edition chapter numbers ([sample PDF / TOC](https://ptgmedia.pearsoncmg.com/images/9780138216764/samplepages/9780138216764_Sample.pdf)).
- **Lab targets** = which lab in `ENCOR-Labs-EVE-NG.md` to run that week.
- **Show / commands** = the minimum CLI you should have in your fingers by Sunday night.

Time budget assumption: ~10 h/week (≈4 h video, ≈4 h lab, ≈2 h notes + Anki). Adjust as needed. INE courses are typically 2–3× longer than the matching CBT Nuggets skill (especially BGP at 25 h and OSPF at 17 h) — if you only have the weekly budget, watch CBT Nuggets first as the spine and use INE for **targeted deep dives** on topics that didn't click (or where you scored poorly on the matching Practice Questions block).

---

## Phase 1 — Foundations & Layer 2 (Weeks 1–3)

### Week 1 — Course intro, packet forwarding, switching architecture

- **CBT Nuggets:** *Welcome / About the Course* + *Packet Forwarding* skill (CEF, process vs. fast vs. CEF switching, TCAM/CAM, SDM templates, Stackwise / VSS / StackWise Virtual overview).
- **INE:** Course **1 — Enterprise Network Architecture Principles** (2 h 40 m, Bogart) for the campus 2-tier/3-tier + hardware-redundancy story; pull the CEF / TCAM / SDM lessons out of Course **3 — Enterprise & Campus LAN Switching Fundamentals** (9 h 15 m, Bogart) — only the early modules this week, the rest land in Week 3. Optional: Course **2 — Cloud Service Models & Connectivity** (2 h 23 m) if you want the cloud connectivity material front-loaded.
- **OCG:** Ch. 1 *Packet Forwarding*; Ch. 22 *Enterprise Network Architecture* §"Hardware redundancy".
- **Lab:** Lab A1 — *Enterprise Campus Baseline (3-tier)*.
- **Key commands:**
  - `show platform software fed switch active ip route summary`
  - `show ip cef`, `show ip cef <prefix> detail`, `show adjacency detail`
  - `show platform hardware fed switch active fwd-asic resource tcam utilization`
  - `show sdm prefer`, `sdm prefer <template>`
  - `show switch`, `show switch stack-ports`

### Week 2 — Spanning Tree Protocol deep dive

- **CBT Nuggets:** *Spanning Tree Protocol* skill — STP fundamentals, PVST+/RPVST+/MST, port roles & states, BPDU Guard / Root Guard / Loop Guard / BPDU Filter, UDLD.
- **INE:** Course **4 — Rapid Spanning-Tree and MST** (6 h 23 m, Bogart) for RSTP/MST mechanics, then Course **5 — Securing the Spanning-Tree** (4 h 16 m, Bogart) for BPDU Guard / Root Guard / Loop Guard / BPDU Filter / UDLD. Skim only — if INE Course 4 covers classic STP at the start, watch that block; otherwise jump straight to RSTP since CBT Nuggets already covered legacy STP basics.
- **OCG:** Ch. 2 *Spanning Tree Protocol*; Ch. 3 *Advanced STP Tuning*; Ch. 4 *Multiple Spanning Tree Protocol*.
- **Lab:** Lab I1 — *STP / RPVST+ / MST + Guards*.
- **Key commands:**
  - `show spanning-tree`, `show spanning-tree vlan 10 detail`
  - `show spanning-tree mst configuration`, `show spanning-tree mst 0`
  - `spanning-tree mode {pvst | rapid-pvst | mst}`
  - `spanning-tree guard {root | loop | none}`, `spanning-tree bpduguard enable`, `spanning-tree portfast`
  - `udld enable` / `udld port aggressive`, `show udld neighbors`

### Week 3 — VLANs, trunking, EtherChannel

- **CBT Nuggets:** *VLANs and Trunking* skill + *EtherChannel* skill — access/trunk, native VLAN, DTP, VTP v1/2/3, voice VLAN, Layer 2 vs Layer 3 EtherChannel, PAgP/LACP/static, load-balance algorithms, MLAG concepts.
- **INE:** Remainder of Course **3 — Enterprise & Campus LAN Switching Fundamentals** (9 h 15 m, Bogart) — the back half of this course is exactly the VLAN/trunk/VTP/EtherChannel material. This is the densest single course in the early plan; budget the full 4 h video slot here (or split across Mon+Tue).
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

- **CBT Nuggets:** *OSPF* skill — neighbor states, network types, areas (normal/stub/totally stubby/NSSA), LSAs 1–7, summarization, virtual links, OSPFv3 (address-families).
- **INE:** Course **7 — Implementing & Troubleshooting OSPF** (17 h 9 m, Bogart). This is **way more than one week's budget** — prioritize: (1) neighbor states & network types, (2) LSAs 1–5 + 7, (3) area types (stub/totally stubby/NSSA/totally NSSA), (4) summarization & filtering, (5) OSPFv3 address-families. Skip or skim virtual links and authentication unless you want depth. Note: INE also has Course **6 — Deploying EIGRP in Enterprise Networks** (10 h 44 m, Pardasani) which CBT Nuggets de-emphasizes — EIGRP is **not on the ENCOR blueprint**, so unless you want it for ENARSI later, **skip Course 6**.
- **OCG:** Ch. 8 *OSPF*; Ch. 9 *Advanced OSPF*; Ch. 10 *OSPFv3*.
- **Lab:** Lab I3 — *OSPF Multi-Area + Filtering + OSPFv3*.
- **Key commands:**
  - `router ospf 1` / `network ... area ...` / `ip ospf <pid> area <n>`
  - `show ip ospf neighbor`, `show ip ospf database`, `show ip ospf interface brief`
  - `area X stub no-summary`, `area X nssa`, `area X range ...`, `summary-address ...`
  - `ip ospf network {point-to-point | broadcast | non-broadcast}`
  - `show ipv6 ospf neighbor`, `address-family ipv6 unicast`

### Week 5 — BGP fundamentals & policy

- **CBT Nuggets:** *BGP* skill — eBGP/iBGP, neighbor states, best-path algorithm, attributes (Weight/LocPref/AS-path/Origin/MED), route-maps, communities, route reflection (intro).
- **INE:** Course **8 — BGP for the Enterprise** (25 h 35 m, Bogart) — the longest course in the path. For ENCOR scope, target: (1) eBGP vs iBGP behavior + neighbor states, (2) best-path algorithm step-by-step, (3) Weight / LOCAL_PREF / AS-path / MED with route-map manipulation, (4) communities (standard + extended) + filtering, (5) route reflectors (concept only). **Skip** confederations deep dive, BGP+MPLS L3VPN, and most multiprotocol BGP — those are ENARSI/SP territory. Pair with Course **9 — Essentials of Policy-Based Routing** (1 h 6 m, Bogart) if you have time — PBR shows up on the blueprint and is a quick win.
- **OCG:** Ch. 11 *BGP*; Ch. 12 *Advanced BGP*.
- **Lab:** Lab I4 — *eBGP/iBGP with Policy & Communities*.
- **Key commands:**
  - `router bgp <ASN>` / `neighbor <ip> remote-as <asn>` / `address-family ipv4 unicast`
  - `show ip bgp summary`, `show ip bgp`, `show ip bgp neighbors <ip> advertised-routes`
  - `neighbor X route-map RM-IN in`, `set local-preference`, `set as-path prepend`, `set community ...`
  - `bgp bestpath as-path multipath-relax`, `maximum-paths`
  - `show bgp ipv4 unicast regexp _65001_`

### Week 6 — Multicast, FHRP, QoS overview

- **CBT Nuggets:** *Multicast* + *First-Hop Redundancy* + *QoS* skills — IGMPv2/v3, PIM-SM, RPF check, SSM/Bidir, MSDP intro; HSRP/VRRP/GLBP; classification, marking, queueing, MQC.
- **INE:** Three courses from non-adjacent sections converge here — Course **10 — Understanding IP Multicast Routing** (12 h 23 m, Bogart, target the PIM-SM + RPF + IGMP modules; skip MSDP/Anycast-RP if short on time), Course **22 — Gateway Redundancy With FHRP** (5 h 8 m, Bogart — HSRP/VRRP/GLBP with tracking), and Course **19 — Understanding Enterprise Network Quality of Service (QoS)** (2 h 53 m, McGahan — MQC, classification/marking, policing vs shaping, LLQ/CBWFQ). This is the heaviest video week of the plan; split: Mon = QoS (short), Tue = FHRP, Wed/Thu = Multicast modules.
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

- **CBT Nuggets:** *IP Services* skill — NAT/PAT, NTP, PTP intro, SNMPv2c/v3, syslog, IP SLA, Object Tracking, NetFlow / Flexible NetFlow.
- **INE:** Five short courses, all map cleanly here — Course **21 — Essentials of NAT** (2 h 20 m, Bogart), Course **20 — Time Synchronization with NTP** (4 h 19 m, Bogart — covers PTP too), Course **23 — Network Monitoring With SNMP & SYSLOG** (3 h 6 m, Bogart), Course **26 — Measuring Network Performance with IP SLA** (3 h 43 m, Bogart). NetFlow gets its own dedicated INE course (**24 — Traffic Analysis with Netflow**, 6 h 10 m, Bogart) — watch the first half this week (flow record/exporter/monitor concepts), defer Flexible NetFlow deep dive to Week 11 alongside ERSPAN labs.
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

- **CBT Nuggets:** *Tunnels and Overlays* skill — GRE, mGRE/DMVPN intro, IPsec site-to-site, LISP control/data plane, VXLAN (EVPN intro).
- **INE:** Course **12 — IP Traffic Tunneling from GRE to IPsec** (10 h 35 m, Bogart) — the full GRE → GRE-over-IPsec → IPsec site-to-site progression; this is the best single resource in either course library for the tunneling pillar. **INE has no dedicated LISP or VXLAN course** — LISP/VXLAN concepts are folded into Course **18 — Understanding SD-Access** (3 h 13 m, McGahan), which you'll watch in Week 10. For this week, pull the **LISP intro + VXLAN intro modules out of Course 18 early** so you have the data-plane vocabulary for Lab V3.
- **OCG:** Ch. 16 *Overlay Tunnels*; Ch. 23 *Fabric Technologies* §LISP/VXLAN.
- **Lab:** Lab V2 — *GRE-over-IPsec* and Lab V3 — *VXLAN unicast flood-and-learn* (CSR1000v).
- **Key commands:**
  - `interface Tunnel0 / tunnel source / tunnel destination / tunnel mode gre ip`
  - `crypto isakmp policy`, `crypto ipsec transform-set`, `crypto map`, `show crypto isakmp sa`, `show crypto ipsec sa`
  - `show ip lisp`, `lisp instance-id`, `database-mapping`
  - `interface nve1 / source-interface Lo0 / member vni 10010 mcast-group 239.1.1.10`
  - `show nve peers`, `show nve vni`, `show l2route evpn mac all`

### Week 9 — Wireless: signals, infra, roaming, security, troubleshooting

- **CBT Nuggets:** *Wireless* skill block (≈5 sub-skills) — RF fundamentals, AP modes (local/FlexConnect/sniffer/bridge), WLC deployment (centralized/Embedded WLC on Catalyst 9800), CAPWAP, roaming (intra/inter-controller, mobility groups), wireless security (WPA2/3, 802.1X, PSK, OWE), Cisco DNA Spaces / location intro, troubleshooting tools.
- **INE:** **No dedicated wireless course exists** in the v1.2 path — this is the biggest gap in INE's coverage versus CBT Nuggets. Workarounds: (1) Use **Jeff Kish's wireless skill as the primary source this week** — it's the best wireless content across both libraries; (2) supplement with the Wireless Deep Dive section in `ENCOR-Jeff-Kish-Notes.md` (CAPWAP discovery order, AP modes, 802.11r/k/v, Wi-Fi 6); (3) for hands-on, use the [Cisco DevNet Catalyst 9800 Always-On sandbox](https://devnetsandbox.cisco.com/) as planned. If you previously had access to INE's legacy ENCOR v1 path (pre-April 2026), the old "Wireless" course modules are still excellent — check your INE library archive.
- **OCG:** Ch. 17–21 (Signals, Infrastructure, Roaming, Authenticating Clients, Troubleshooting).
- **Lab:** Wireless is GUI-centric — no EVE-NG lab. Use **Cisco Catalyst 9800-CL Free Download** in EVE-NG if you have it; otherwise use the [Cisco DevNet 9800 Always-On sandbox](https://devnetsandbox.cisco.com/) for show-command practice.
- **Key commands:**
  - 9800 IOS-XE: `show wireless summary`, `show ap summary`, `show wireless client summary`, `show wlan summary`, `show wireless mobility summary`
  - `wlan WLAN-NAME 1 SSID-NAME`, `security wpa psk set-key ascii 0 ...`, `no shutdown`
  - `ap dot11 5ghz cleanair`, `wireless tag policy`, `wireless profile policy`

---

## Phase 4 — Architecture & Network Assurance (Weeks 10–11)

### Week 10 — Enterprise architecture, SD-Access, SD-WAN

- **CBT Nuggets:** *Enterprise Network Design* + *SD-Access* + *SD-WAN* skills — 2-tier vs 3-tier, on-prem/cloud/hybrid, SD-Access components (DNA Center, ISE, fabric edge/border/control), SD-WAN components (vManage/vSmart/vBond/vEdge or Catalyst SD-WAN cEdge), OMP, TLOC. Also Cisco Wireless architecture choices (autonomous/centralized/Mobility Express/embedded).
- **INE:** Course **17 — Understanding Cisco Software Defined WAN (SD-WAN)** (5 h 23 m, McGahan) + Course **18 — Understanding Cisco Software Defined Access (SD-Access)** (3 h 13 m, McGahan). Both are McGahan's strongest material in the path — he co-wrote the original Viptela SD-WAN deep dives. INE explicitly aligns to the v1.2 blueprint, so SD-WAN terminology here is the current **Catalyst SD-WAN** wording (cEdge/vEdge, OMP route types). Re-watch the architecture intro modules of Course **1** (Bogart) if you didn't cover them in Week 1.
- **OCG:** Ch. 22 *Enterprise Network Architecture*; Ch. 23 *Fabric Technologies*.
- **Lab:** Lab A2 — *SD-Access Underlay/Overlay Concept Lab* (IS-IS underlay + LISP+VXLAN overlay between CSR1000v).
- **Key commands:**
  - `show ip lisp` family, `show vxlan vtep`, IS-IS: `show isis neighbors`, `show clns neighbors`.
  - SD-WAN GUI-driven; know the **planes** (orchestration = vBond, management = vManage, control = vSmart, data = vEdge/cEdge) and **OMP** route types (vRoute / TLOC / Service).

### Week 11 — Network Assurance deep dive

- **CBT Nuggets:** *Network Assurance* skill — diagnostic tools (`ping/traceroute/debug` discipline), SPAN/RSPAN/ERSPAN, IPSLA, NetFlow/Flexible NetFlow, syslog levels, SNMP traps vs informs, model-driven telemetry, Cisco DNA Center Assurance (concepts).
- **INE:** Course **25 — Traffic Mirroring with SPAN/RSPAN/ERSPAN** (3 h 4 m, Bogart) + the **back half of Course 24 — Traffic Analysis with Netflow** (6 h 10 m total, you watched the first half in Week 7) for Flexible NetFlow. Re-watch IP SLA from Course **26** (3 h 43 m) if your Week 7 IP SLA lab was shaky. **Model-driven telemetry is not deeply covered** in the INE v1.2 path — use CBT Nuggets + OCG Ch. 24 + the Automation Deep Dive in `ENCOR-Jeff-Kish-Notes.md` for the MDT/gNMI material.
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

- **CBT Nuggets:** *Infrastructure Security* + *Device Access Control* skills — AAA (TACACS+/RADIUS), Privilege levels, line/console/VTY hardening, SSH v2, CoPP, MPP, uRPF, ACLs (standard/extended/named/time-based), DHCP snooping, Dynamic ARP Inspection, IP Source Guard, port-security, storm-control, PVLANs.
- **INE:** Course **14 — Understanding Cisco Device Access Control** (2 h 6 m, Pardasani — AAA, TACACS+/RADIUS, privilege levels, SSH, line hardening), Course **15 — Access-Lists: Beyond the Basic and Extended** (6 h 59 m, Bogart — the big ACL deep dive, including time-based and reflexive), Course **16 — Understanding Control-Plane Protection** (1 h 23 m, Pardasani — CoPP/MPP). The L2 hardening pillar (DHCP snooping / DAI / IPSG / port-security / storm-control / PVLAN) is **lightly covered in INE** — most of it lives inside Course **5 — Securing the Spanning-Tree** (you watched in Week 2) and scattered modules in Course **3**. CBT Nuggets is stronger on L2 hardening; lean on the *Infrastructure Security* skill there.
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

- **CBT Nuggets:** *Network Security* skill — Zone-Based Firewall, IPsec VPN, MACsec (downlink/uplink), Cisco TrustSec / SGT/SGACL concepts, 802.1X / MAB / Web-auth, Cisco Umbrella / Stealthwatch / NGFW / NGIPS / WSA / ESA overview.
- **INE:** Course **13 — Enterprise Network Security Principles** (3 h 44 m, Pardasani) is the right primary — it covers the security portfolio (Umbrella, Stealthwatch, NGFW/NGIPS, WSA, ESA), TrustSec/SGT concepts, 802.1X/MAB, and the policy framework. **ZBFW and MACsec are not first-class topics in the v1.2 INE path** — CBT Nuggets *Network Security* skill is the better source for both; use INE Course 13 for the portfolio-level material and CBT for the hands-on ZBFW configuration walkthrough. IPsec was already covered in Week 8 via INE Course 12.
- **OCG:** Ch. 25 *Secure Network Access Control* (deep), Ch. 26 (concepts).
- **Lab:** Lab S3 — *Zone-Based Firewall on CSR1000v*.
- **Key commands:**
  - ZBFW: `zone security`, `zone-pair security`, `class-map type inspect`, `policy-map type inspect`, `service-policy type inspect`
  - 802.1X: `dot1x system-auth-control`, `authentication port-control auto`, `mab`, `authentication order`, `authentication priority`
  - MACsec: `mka policy`, `key chain ... macsec`, `macsec network-link`

### Week 14 — Virtualization & Automation/Programmability

- **CBT Nuggets:** *Virtualization* skill (hypervisors, VRF-lite, GRE/VXLAN tunneling for virt, LISP+VXLAN intro) + *Automation/Programmability* skill (data formats JSON/XML/YAML, REST APIs, NETCONF/RESTCONF, YANG, Python `requests`/`ncclient`, Ansible, Cisco DNA Center API, Puppet/Chef overview, EEM applets, Cisco SDN intro).
- **INE:** This is the **strongest week for INE** — the automation section is a v1.2 rewrite and beats CBT Nuggets on depth. Watch: Course **11 — Virtualization Fundamentals** (2 h 31 m, Bogart — hypervisors, VRF, virtual switching), then the full automation block: Course **27 — Network Automation Foundation** (12 h 38 m, Pardasani — data formats, REST, NETCONF/RESTCONF, YANG), Course **30 — Using APIs** (4 h 15 m, Pardasani — DNA Center, vManage, Meraki APIs), Course **28 — Fundamentals of Network Automation Using Python** (10 h 22 m, Pardasani — `requests`, `ncclient`, `netmiko`), Course **29 — Embedded Event Manager (EEM)** (7 h 3 m, Pardasani — the most thorough EEM treatment of any ENCOR course), Course **31 — Understanding Orchestration Tools** (49 m, Pardasani — Ansible/Puppet/Chef compare). That's ~37 h total — you cannot watch all of it in one week; instead, **use it as a 2–3 week post-exam reference** and for week 14 focus on Courses 11 + 27 + 29 (the EEM applet maps directly to Lab AU3).
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

## INE Course Reference (v1.2 Learning Path, full course list)

Use the **course number** below to find the exact course on INE — the weekly notes above point back to these numbers. Pulled from the [INE ENCOR v1.2 learning path page](https://my.ine.com/Networking/learning-paths/00836fff-00c8-49b0-a54b-b34071c15c48/enterprise-core-exam-350-401-encor-v12) (33 courses, ~87 h video, refreshed April 2026 per the [INE launch announcement](https://ine.com/newsroom/ine-launches-fully-revamped-cisco-ccnp-enterprise-encor-v12-learning-path-for-certification-success)).

| # | Course | INE Section | Duration | Instructor | Week in this plan |
|---|---|---|---|---|---|
| 1 | Enterprise Network Architecture Principles | Foundations | 2 h 40 m | Bogart | Week 1 (+ refresh in W10) |
| 2 | Cloud Service Models & Connectivity | Foundations | 2 h 23 m | Bogart | Week 1 (optional) |
| 3 | Enterprise & Campus LAN Switching Fundamentals | Layer 2 | 9 h 15 m | Bogart | Weeks 1 + 3 |
| 4 | Rapid Spanning-Tree and MST | Layer 2 | 6 h 23 m | Bogart | Week 2 |
| 5 | Securing the Spanning-Tree | Layer 2 | 4 h 16 m | Bogart | Weeks 2 + 12 |
| 6 | Deploying EIGRP in Enterprise Networks | Layer 3 | 10 h 44 m | Pardasani | **Skip** — not on ENCOR blueprint |
| 7 | Implementing & Troubleshooting OSPF | Layer 3 | 17 h 9 m | Bogart | Week 4 |
| 8 | BGP for the Enterprise | Layer 3 | 25 h 35 m | Bogart | Week 5 |
| 9 | Essentials of Policy-Based Routing | Layer 3 | 1 h 6 m | Bogart | Week 5 (quick win) |
| 10 | Understanding IP Multicast Routing | Layer 3 | 12 h 23 m | Bogart | Week 6 |
| 11 | Virtualization Fundamentals | Virtualization | 2 h 31 m | Bogart | Week 14 |
| 12 | IP Traffic Tunneling from GRE to IPsec | Virtualization | 10 h 35 m | Bogart | Week 8 |
| 13 | Enterprise Network Security Principles | Security | 3 h 44 m | Pardasani | Week 13 |
| 14 | Understanding Cisco Device Access Control | Security | 2 h 6 m | Pardasani | Week 12 |
| 15 | Access-Lists: Beyond the Basic and Extended | Security | 6 h 59 m | Bogart | Week 12 |
| 16 | Understanding Control-Plane Protection | Security | 1 h 23 m | Pardasani | Week 12 |
| 17 | Understanding Cisco Software Defined WAN (SD-WAN) | SD Arch | 5 h 23 m | McGahan | Week 10 |
| 18 | Understanding Cisco Software Defined Access (SD-Access) | SD Arch | 3 h 13 m | McGahan | Week 10 (+ LISP/VXLAN preview in W8) |
| 19 | Understanding Enterprise Network Quality of Service | Services | 2 h 53 m | McGahan | Week 6 |
| 20 | Time Synchronization with NTP | Services | 4 h 19 m | Bogart | Week 7 |
| 21 | Essentials of Network Address Translation (NAT) | Services | 2 h 20 m | Bogart | Week 7 |
| 22 | Gateway Redundancy With FHRP | Services | 5 h 8 m | Bogart | Week 6 |
| 23 | Network Monitoring With SNMP & SYSLOG | Assurance | 3 h 6 m | Bogart | Week 7 |
| 24 | Traffic Analysis with Netflow | Assurance | 6 h 10 m | Bogart | Weeks 7 + 11 |
| 25 | Traffic Mirroring with SPAN/RSPAN/ERSPAN | Assurance | 3 h 4 m | Bogart | Week 11 |
| 26 | Measuring Network Performance with IP SLA | Assurance | 3 h 43 m | Bogart | Week 7 (+ W11 refresh) |
| 27 | Network Automation Foundation | Automation | 12 h 38 m | Pardasani | Week 14 |
| 28 | Fundamentals of Network Automation Using Python | Automation | 10 h 22 m | Pardasani | Week 14 (or post-exam) |
| 29 | Embedded Event Manager (EEM) | Automation | 7 h 3 m | Pardasani | Week 14 |
| 30 | Using APIs | Automation | 4 h 15 m | Pardasani | Week 14 (or post-exam) |
| 31 | Understanding Orchestration Tools | Automation | 49 m | Pardasani | Week 14 (quick win) |
| 32 | CCNP Enterprise — ENCOR Practice Labs | Final Prep | 15 h 13 m | Bogart | Final mile |
| 33 | ENCOR Final Exam Preparation | Final Prep | — | — | Final mile |

### Where INE is stronger than CBT Nuggets

- **Automation block (Courses 27–31)** — ~35 h of focused programmability content rewritten for v1.2. Far deeper Python, EEM, and DNA Center / vManage / Meraki API treatment than the single CBT *Automation* skill.
- **BGP (Course 8, 25 h)** — if BGP didn't click from CBT Nuggets, INE's Bogart course is the single best ENCOR-aligned BGP resource available.
- **OSPF (Course 7, 17 h)** — same story; the troubleshooting modules are particularly good.
- **SD-WAN (Course 17)** — McGahan literally wrote books on Viptela; pick this over the CBT SD-WAN nuggets if you have to choose one.
- **Practice Labs (Course 32, 15 h)** — dedicated guided lab walkthroughs that complement (not replace) your EVE-NG labs.

### Where INE has gaps you'll need to cover elsewhere

- **Wireless** — no dedicated course in v1.2. Use CBT Nuggets *Wireless* skill + Wireless Deep Dive in `ENCOR-Jeff-Kish-Notes.md` + DevNet 9800 sandbox.
- **L2 hardening (DHCP snooping, DAI, IPSG, port-security, PVLAN, storm-control)** — scattered across Courses 3 and 5 rather than in one place. CBT *Infrastructure Security* covers this end-to-end.
- **ZBFW & MACsec** — not first-class topics in INE v1.2. Use CBT *Network Security* skill.
- **Model-driven telemetry (MDT) / gNMI** — light coverage. Use CBT + OCG Ch. 24 + Automation Deep Dive in Notes.
- **EIGRP (Course 6)** — outside ENCOR blueprint; safe to skip unless prepping for ENARSI next.

### Suggested viewing strategy

1. **Spine:** CBT Nuggets / Jeff Kish, watched week-by-week as scheduled. This stays the primary source for breadth, pacing, and exam alignment.
2. **Deep dive on demand:** when you hit a topic that doesn't click — or you score <70% on the matching block in `ENCOR-Practice-Questions.md` — jump to the INE course referenced for that week and watch only the relevant modules.
3. **Post-exam:** keep INE Courses 8 (BGP), 27–31 (Automation), and 32 (Practice Labs) as a long-tail library for production work and the next certification.
