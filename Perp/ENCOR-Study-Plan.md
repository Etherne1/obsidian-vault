# ENCOR 350-401 — 14-Week Study Plan

**Spine course:** CBT Nuggets — *CCNP Enterprise: Implementing and Operating Cisco Enterprise Network Core Technologies (350-401 ENCOR)* with Jeff Kish (and co-instructors Knox Hutchinson / Keith Barker on select skills).
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
- **OCG chapters** = Cisco Press 2nd-edition chapter numbers ([sample PDF / TOC](https://ptgmedia.pearsoncmg.com/images/9780138216764/samplepages/9780138216764_Sample.pdf)).
- **Lab targets** = which lab in `ENCOR-Labs-EVE-NG.md` to run that week.
- **Show / commands** = the minimum CLI you should have in your fingers by Sunday night.

Time budget assumption: ~10 h/week (≈4 h video, ≈4 h lab, ≈2 h notes + Anki). Adjust as needed.

---

## Phase 1 — Foundations & Layer 2 (Weeks 1–3)

### Week 1 — Course intro, packet forwarding, switching architecture

- **CBT Nuggets:** *Welcome / About the Course* + *Packet Forwarding* skill (CEF, process vs. fast vs. CEF switching, TCAM/CAM, SDM templates, Stackwise / VSS / StackWise Virtual overview).
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
- **OCG:** Ch. 22 *Enterprise Network Architecture*; Ch. 23 *Fabric Technologies*.
- **Lab:** Lab A2 — *SD-Access Underlay/Overlay Concept Lab* (IS-IS underlay + LISP+VXLAN overlay between CSR1000v).
- **Key commands:**
  - `show ip lisp` family, `show vxlan vtep`, IS-IS: `show isis neighbors`, `show clns neighbors`.
  - SD-WAN GUI-driven; know the **planes** (orchestration = vBond, management = vManage, control = vSmart, data = vEdge/cEdge) and **OMP** route types (vRoute / TLOC / Service).

### Week 11 — Network Assurance deep dive

- **CBT Nuggets:** *Network Assurance* skill — diagnostic tools (`ping/traceroute/debug` discipline), SPAN/RSPAN/ERSPAN, IPSLA, NetFlow/Flexible NetFlow, syslog levels, SNMP traps vs informs, model-driven telemetry, Cisco DNA Center Assurance (concepts).
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
- **OCG:** Ch. 25 *Secure Network Access Control* (deep), Ch. 26 (concepts).
- **Lab:** Lab S3 — *Zone-Based Firewall on CSR1000v*.
- **Key commands:**
  - ZBFW: `zone security`, `zone-pair security`, `class-map type inspect`, `policy-map type inspect`, `service-policy type inspect`
  - 802.1X: `dot1x system-auth-control`, `authentication port-control auto`, `mab`, `authentication order`, `authentication priority`
  - MACsec: `mka policy`, `key chain ... macsec`, `macsec network-link`

### Week 14 — Virtualization & Automation/Programmability

- **CBT Nuggets:** *Virtualization* skill (hypervisors, VRF-lite, GRE/VXLAN tunneling for virt, LISP+VXLAN intro) + *Automation/Programmability* skill (data formats JSON/XML/YAML, REST APIs, NETCONF/RESTCONF, YANG, Python `requests`/`ncclient`, Ansible, Cisco DNA Center API, Puppet/Chef overview, EEM applets, Cisco SDN intro).
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
