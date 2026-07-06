# ENCOR 350-401 — Two-Pass Study Plan

**Goal:** Deep working knowledge for production use. Exam-readiness is a side-effect, not the primary driver.  
**Time budget:** ~2 hours/day, irregular. Some days nothing — that's fine, the structure absorbs it.  
**Total duration:** ~24–26 weeks across two passes.  
**Video spine:** CBT Nuggets / Jeff Kish (primary — concepts, big picture, self-check quizzes) + INE legacy CCIE-style courses (secondary — protocol depth and Wireshark-level detail).  
**Labs:** GNS3vault archive (René Molenaar) for all classical topics + hand-written EVE-NG labs for modern topics (VXLAN, LISP, NETCONF, Ansible).  
**Reference:** OCG 2nd edition (Hucaby et al.) — used in Pass 2 for slow re-reads, not as primary source in Pass 1.

---

## How the two passes differ

| | Pass 1 (~14 weeks) | Pass 2 (~10 weeks) |
|---|---|---|
| **Goal** | Build the mental map. See everything once. | Harden what you saw. Fix the gaps. |
| **CBT** | Primary — watch first for concepts and big picture | Only for topics where INE didn't click |
| **INE** | Secondary — watch after CBT for protocol depth | Re-watch only weak modules |
| **Labs** | None. Zero. Resist the urge. | All labs, in order, GNS3vault first |
| **OCG** | Skim only — 1 pass through chapter summary sections | Slow re-read of every chapter you struggled with |
| **Notes** | 3–5 bullets per topic from memory after both videos | Full notes in your own words, "things I got wrong" subsection |
| **Anki** | None | Build deck from Pass 2 notes, review daily |
| **Pace** | Flexible — topic takes 9 days instead of 7? Fine. | More structured — use the weekly shape as a target |

**Why no labs in Pass 1:** Labs demand that you already have a rough mental model of what you're trying to build. Without it, you spend 80% of your lab time confused about *what* to type rather than *why* it works. The confusion is not productive — it's just frustrating. Pass 2 labs will go dramatically faster because you've already seen the topology and the commands. You'll spend your time on the interesting part: why does this break, what does this show command actually tell me.

---

## Daily rhythm (both passes)

You have ~2 hours on a good day, zero on a bad day. Don't fight this — design around it.

**Good day (2 hours):**
- 90 min: CBT first, then INE for the same topic (Pass 1) or lab session (Pass 2)
- 30 min: bullet notes from memory — close the video, write what you remember

**Short day (45–60 min):**
- Watch one CBT module, write 3 bullets. If time allows, follow with the matching INE module.
- Or: Anki review only (Pass 2)

**Zero day:**
- Fine. Don't try to "catch up" the next day by doubling. Just resume where you left off.

**The only rule:** don't skip the 30-min notes pass. Writing from memory — even badly — is worth more than re-watching the same video.

---

## Pre-topic self-check (do this before every Pass 1 topic)

Before opening any video for a new topic, spend 5–10 minutes testing what you already know. This tells you whether to go slow and careful or fast and light — before you commit 90 minutes of video time.

**How:**
1. Open `ENCOR-Practice-Questions.md` and find that week's block.
2. Pick 3–5 questions at random. Answer them cold — no notes, no video, no searching. Just what's already in your head.
3. Check your answers against the file.

**Score and act:**

| Score | What it means | What to do |
|---|---|---|
| 4–5 / 5 correct | You already know this well | Skim CBT at 1.5–2× speed. Skip INE or watch only the parts that surprised you. Redirect the saved time to a weaker topic. |
| 2–3 / 5 correct | Partial knowledge, normal gap | Watch both CBT and INE as planned, normal pace. |
| 0–1 / 5 correct | Real gap | This is exactly what Pass 1 is for. Don't rush it — this topic may need the full 7 days or more. |

**Why this works:** trying to answer first — even when you get it wrong — makes the material stick better once you do watch the video. This is the same effect behind why Anki works, just applied before you learn something instead of after.

**One rule:** be honest with yourself here too. If you vaguely recognize a term but couldn't actually explain it, that's not a correct answer — count it as wrong. The whole point is finding real gaps, not confirming what you want to believe.

---

# PASS 1 — The Map (Weeks 1–14)

**Mode:** CBT as primary. Watch the listed CBT modules first — they give you the concept, the big picture, and a quiz to self-check. Then watch the corresponding INE modules for the same topic — they go deeper into protocol mechanics and packet-level behavior. Skim the OCG chapter summary only (last section of each chapter, usually 1–2 pages). Write 3–5 bullets from memory after both videos.  
**No labs. No Anki. No deep OCG reads.**

---

## Phase 1 — Layer 2 Foundations (Weeks 1–3)

### Week 1 — Switching architecture, packet forwarding, campus design

**CBT (primary — watch first):**
- `01-Explain The Hierarchical Network Model`
- `02-Explain Enterprise Network Design`
- `15-Differentiate Between Switching Mechanisms`

**INE (after CBT — protocol depth):**
- `7-Hardware & Software Switching Mechanisms` — `1 - Cisco Express Forwarding.mp4` (CEF, FIB, adjacency table, process vs CEF)
- `1-Introduction to Enterprise Network Design Principles` — full single module (hierarchical model, 2-tier vs 3-tier, modular enterprise, HA design)
- `14-LAN Switching` — `1 - LAN Switching Introduction` (how a switch works, CAM table, forwarding decisions)

**OCG skim:** Ch. 1 summary + Ch. 22 summary (architecture section only)

**Bullets to write from memory:** How CEF differs from process switching. What the FIB is and where it comes from. The three layers of the campus model and what lives at each.

---

### Week 2 — Spanning Tree Protocol

**CBT (primary — watch first):**
- `23-Fundamentals of STP`
- `24-Customize Spanning Tree Protocol`
- `25-Optimize with Rapid Spanning Tree`
- `26-Configure and Verify MST`

**INE (after CBT — protocol depth):**
- `15-Routing & Switching Rapid Spanning-Tree and MST` — modules 1–12 in order. Cover: 802.1D root election, port roles/states, RSTP convergence, MST regions, IST master, virtual bridge, MST/PVST boundary.
- `14-LAN Switching` — `8 - Spanning-Tree Protocol`, `9 - Optional Spanning-Tree Features` (PortFast, BPDU Guard, Root Guard, Loop Guard, UDLD, Backbone/UplinkFast)

**OCG skim:** Ch. 2, 3, 4 summaries

**Bullets to write:** Root election criteria. The difference between Root Guard and BPDU Guard (placement, what they protect against). Why MST needs a region and what happens at the boundary.

---

### Week 3 — VLANs, trunking, EtherChannel

**CBT (primary — watch first):**
- `21-Troubleshoot Static and Dynamic 802.1q Trunking Protocols`
- `22-Troubleshooting Cisco Etherchannel and LAG Ports`

**INE (after CBT — protocol depth):**
- `14-LAN Switching` — `2 - VLANs & Trunking`, `3 - VLAN Trunking Protocol (VTP)`, `4 - VTP Version 3`, `5 - EtherChannel`, `6 - Layer 2 EtherChannel Configuration`, `7 - Layer 3 EtherChannel Configuration`

**OCG skim:** Ch. 5 summary

**Bullets to write:** DTP modes and when trunks form. VTP modes and the danger of VTP in production. LACP vs PAgP vs static — when each is appropriate. Why a Layer 3 port-channel matters.

---

## Phase 2 — Layer 3: IGP, BGP, Multicast, QoS (Weeks 4–7)

### Week 4 — OSPF

**CBT (primary — watch first):**
- `27-IP Routing Concepts`
- `29-OSPF Foundation Concepts`
- `30-Basic OSPF IPv4 Configuration`
- `31-OSPF Summarization and Filtering`
- `32-Configure and Verify an OSPF NSSA`
- `33-Configure and Verify OSPFv3 with IPv6`

**INE (after CBT — protocol depth):**
- `17-OSPF` — all 21 modules, in order. Key modules: 1 (protocol overview), 3 (adjacency troubleshooting), 4 (areas and LSA types), 5–6 (network types), 8–10 (stub area types), 11–14 (NSSA deep dive), 18–20 (summarization, filtering). Modules 7 (virtual links), 15–17 (authentication) — watch but lighter pass.

**OCG skim:** Ch. 8, 9, 10 summaries

**Bullets to write:** LSA types 1–5 and 7 — what each contains and who generates it. When a totally-stubby area is appropriate vs plain stub. The difference between ABR summarization and ASBR summarization.

---

### Week 5 — BGP

**CBT (primary — watch first):**
- `34-BGP Foundation Concepts`
- `35-Basic BGP IPv4 Configuration`
- `36-Basic BGP IPv6 Configuration`

**INE (after CBT — protocol depth):**
- `18-Core BGP` — all 7 modules: `1–2 - Core BGP Routing Part 1/2`, `3 - Applying BGP Policy`, `4–5 - NLRI Advertisement Rules Part 1/2`, `6–7 - BGP Path Selection Rules Part 1/2`.

**OCG skim:** Ch. 11, 12 summaries

**Note — PBR:** Neither library has a dedicated PBR module. PBR is on the ENCOR blueprint. After finishing the BGP CBT modules, read OCG Ch. 15 §"Policy-Based Routing" (about 8 pages). Write 3 bullets on it. It's a small topic but consistently appears on the exam.

**Bullets to write:** The BGP best-path algorithm — the first 5 steps in order and what each checks. The difference between weight, local-preference, MED, and AS-path prepend in terms of scope (local router / AS / inter-AS). Why iBGP requires full mesh or a route reflector.

---

### Week 6 — Multicast

**CBT (primary — watch first):**
- `44-IPv4 Multicast Fundamentals`

**INE (after CBT — protocol depth):**
- `23-Layer-2 Multicast` — full 22 modules (IGMPv1/2/3, MLD, IGMP snooping, querier election).
- `24-L3 Multicast with PIM Sparse-Mode` — modules covering shared tree, source registration, SPT join, Auto-RP, PIM-BSR, SSM. Skip the deep MSDP/anycast RP modules — beyond ENCOR scope.

**OCG skim:** Ch. 13 summary

**Bullets to write:** Why PIM-SM uses a shared tree first and then switches to SPT (or doesn't). The role of the RP — what happens if it goes down. How IGMP snooping prevents multicast flooding at L2.

---

### Week 7 — FHRP and QoS

**CBT (primary — watch first):**
- `13-Unpacking Cisco QoS Components`
- `14-QoS Marking Strategy Deep Dive`
- `43-Configure and Verify First Hop Redundancy Protocols (FHRP)`

**INE (after CBT — protocol depth):**
- `21-Gateway Redundancy With FHRP` — full module (HSRP, VRRP, GLBP configuration and design)
- `5-Introduction to QoS` — modules 1–12 (QoS tools overview, MQC, classification, marking, NBAR, FIFO/WFQ/CBWFQ/LLQ). Skip modules 13–22 (WTD/WRED internals, policing math) — deeper than ENCOR needs.
- `6-Quality of Service` — watch only the Policing and Shaping modules as a complement.

**OCG skim:** Ch. 14 summary, Ch. 15 §FHRP summary

**Bullets to write:** HSRP vs VRRP vs GLBP — what GLBP does that the others can't. The QoS trust boundary — what it is and where to place it. DSCP EF vs AF — what traffic each is for.

---

## Phase 3 — Services, Overlays, Wireless (Weeks 8–10)

### Week 8 — IP Services: NAT, NTP, SNMP, Syslog, NetFlow, IP SLA

**CBT (primary — watch first):**
- `41-Describe and Configure the Network Time Protocol (NTP)`
- `42-Configure and Verify IPv4 NAT/PAT`
- `45-Diagnose Network Problems with Ping, Traceroute, Debug, SNMP, and Syslog`
- `46-Configure and verify NetFlow and Flexible NetFlow`
- `48-Configure and Verify Cisco IP SLA`

**INE (after CBT — protocol depth):**
- `20-Implementing NAT For Enterprise Networks` — full module
- `27-Network Monitoring With SNMP & SYSLOG` — full module (SNMP v2c/v3 config, syslog severity)
- `25-Traffic Analysis with Netflow` — modules 1–11 (NetFlow versions, original config, Flexible NetFlow records/export, monitoring, sampling)
- `29-IP Service Level Agreements (Video)` — full single module

**OCG skim:** Ch. 15 summary, Ch. 24 §Tools summary

**Bullets to write:** Static NAT vs dynamic NAT vs PAT — what each translates and when to use each. SNMPv3 security levels — authNoPriv vs authPriv. How IP SLA tracking can trigger a route change.

---

### Week 9 — Overlay tunnels: VRF, GRE, IPsec, LISP, VXLAN

**CBT (primary — watch first):**
- `17-Verify Data Path Virtualization Technologies`
- `18-Configure Data Path Virtualization Technologies`
- `19-Describe LISP`
- `20-Describe VXLAN`

**INE (after CBT — protocol depth):**
- `9-VRFs` — `1 - VRFs`, `2 - VRF Lite` (VRF concepts, route leaking, inter-VRF routing)
- `12-IPsec VPNs` — full module (crypto maps, GRE over IPsec, IPsec over DMVPN, VTI, verification)
- `13-IKEv1 IPsec VPN` — modules 1–5 (IKE phases, Phase 1 modes, Phase 2, SA establishment). Skip modules 6–8 unless IPsec lab in Pass 2 feels shaky.
- `11-LISP` — all 5 modules (LISP overview, locator-ID separation, control/data planes, interworking with non-LISP)
- `10-Virtual Extensible LAN (VXLAN) on Nexus NX-OS` — modules 1–7 only (Overview, Terminology, Encapsulation, Basic Workflow, Config Prerequisites, Flood & Learn).

**OCG skim:** Ch. 16 summary, Ch. 23 §LISP/VXLAN summary

**Bullets to write:** What problem VRF-Lite solves vs full MPLS VPN. The two IKE phases — what each negotiates. Why LISP separates locator from identifier — what breaks in traditional routing that LISP fixes.

---

### Week 10 — Wireless

**CBT (primary — watch first):**
- `04-Analyze WLAN Design Principles`
- `37-Describe Physical Wireless Communication`
- `38-Describe Wireless AP Modes and Antenna Types`
- `39-Identify Wireless Joining and Roaming Processes`
- `40-Troubleshoot WLAN Configuration and Wireless Client Connectivity Issues`
- `55-Configure and Verify Wireless Security Features`

**INE (after CBT — protocol depth):**
- `19-Enterprise Wireless Concepts & Implementation` — all 22 modules. Cover: RF measurement, dBm + rule of 10s/3s, 802.11 standards through 11ax, antenna types, CAPWAP, controller discovery and join, redundancy, WLAN roaming, 802.1X with EAP, web auth.

**OCG skim:** Ch. 17–21 summaries (one pass through all five chapter summaries)

**Note on wireless:** This is a read-heavy week — no EVE-NG lab is realistic. In Pass 2, you'll use the Cisco DevNet 9800 Always-On sandbox for one afternoon of show-command practice. For now, just understand the architecture.

**Bullets to write:** CAPWAP split-MAC — what the AP handles locally vs what goes to the controller. L2 roaming vs L3 roaming — what's different. The 802.11 auth sequence: probe → auth → assoc → 802.1X/PSK.

---

## Phase 4 — Architecture, Assurance, Security (Weeks 11–13)

### Week 11 — Enterprise architecture: SD-Access, SD-WAN

**CBT (primary — watch first):**
- `06-Explain the Principles of SD-WAN`
- `07-Identify Cisco SD-WAN Components`
- `08-Explain Cisco SD-WAN Services`
- `09-Explain the Principles of SD-Access`
- `10-Explain SD-Access Fabric Operation`
- `11-Identify SD-Access Components`
- `12-Explain SD-Access Services`

**INE (after CBT — protocol depth):**
- `2-Introduction to Cisco's SD-WAN Solution` — all 9 modules (SDN layers, SD-WAN history, Viptela architecture overview)
- `3-Designing a Cisco SD-WAN Solution` — modules 1–12 (supported hardware, architectural considerations, orchestration/control/management/data planes, overlay bringup, deployment). Modules 13–19 are deeper design — skim unless SD-WAN is a real work need.

**Note — SD-Access:** No dedicated INE SD-Access course exists in the library. CBT modules 09–12 remain the primary source for SDA.

**OCG skim:** Ch. 22, 23 summaries

**Bullets to write:** The four SD-WAN planes — what each plane does and which component owns it. What IS-IS does in an SDA underlay vs LISP+VXLAN in the overlay. What DNA Center provides that traditional NMS doesn't.

---

### Week 12 — Network Assurance

**CBT (primary — watch first):**
- `47-Configure and verify SPAN-RSPAN-ERSPAN`
- `49-Configure Enterprise Campuses with DNA Center Workflows` (Assurance section)
- `50-DNA Center Assurance & Telemetry`

**INE (after CBT — protocol depth):**
- `25-Traffic Analysis with Netflow` — re-watch modules 7–11 (Flexible NetFlow focus — records, exporters, monitors, cache analysis)
- `28-Traffic Monitoring with SPAN (Webinar)` — full single webinar (SPAN/RSPAN/ERSPAN config and limitations)
- `27-Network Monitoring With SNMP & SYSLOG` — re-watch the SNMP v3 modules as a refresher
- `26-Working With RMON (Video)` — single video, light pass (RMON is legacy but exam-eligible)

**OCG skim:** Ch. 24 summary

**Note — MDT/gNMI:** Neither library covers model-driven telemetry. After the CBT modules, read OCG Ch. 24 §"Model-Driven Telemetry" (about 6 pages). Write 3 bullets. It shows up on the exam.

**Bullets to write:** SPAN vs RSPAN vs ERSPAN — what each can do that the others can't. The difference between NetFlow original and Flexible NetFlow (what you define vs what's fixed). What model-driven telemetry solves that SNMP polling doesn't.

---

### Week 13 — Security

**CBT (primary — watch first):**
- `51-Configure and Verify Device Access Control`
- `52-Configure and Verify Access Control Lists`
- `53-Configure and Verify Control Plane Policing (CoPP)`
- `54-Zone-Based Firewall (ZBFW)`
- `56-Next-Gen Security (NGFW, NGIPS, AMP, Umbrella, ESA, WSA)`

**INE (after CBT — protocol depth):**
- `32-Hardening Cisco IOS` — all 9 modules (device architecture, secure management, AAA framework, SNMP/NTP/logging hardening, control-plane security, CoPPr, uRPF)
- `33-Access-Lists Beyond the Basic and Extended` — reflexive ACLs, dynamic ACLs, object groups, time-based ACLs, logging
- `34-Control Plane Policing & Protection (Video)` — full single video
- `37-Cisco Firewall Technologies for Beginners` — full module
- `36-Introduction To Content And Endpoint Security` — full module (NGFW/AMP/Umbrella/ESA/WSA concepts)

**Note — Gaps:** MACsec/TrustSec/SGT and full L2 hardening (DHCP snooping, DAI, IPSG, port-security, storm-control, PVLAN) are not covered by either library. After the INE/CBT pass, read OCG Ch. 25 and Ch. 26 summaries for these topics.

**OCG skim:** Ch. 25, 26, 27, 28 summaries

**Bullets to write:** The AAA framework — authentication vs authorization vs accounting, and where TACACS+ differs from RADIUS in terms of what it encrypts. CoPP placement — where on a router it sits and what it protects. ZBF zone pairs — why self zone matters.

---

## Phase 5 — Automation (Week 14)

### Week 14 — Automation, programmability, Python, EEM

**INE (primary):**
- `39-Introducing Network Programmability & Automation` — full module (concepts: NETCONF, RESTCONF, YANG, data models, management plane evolution)
- `31-Introducing NETCONF & RESTCONF For Enterprise Networks` — full module. Best NETCONF/RESTCONF deep dive in either library.
- `38-Practical Python Cisco Network Automation` — modules 1–8 (Python basics for network engineers, Netmiko, NAPALM — enough for ENCOR scope). Modules 9–15 are post-ENCOR depth.
- `40-Embedded Event Manager` — modules 1–16 + 47–51 (EEM applets, event detectors, action commands, syslog/CLI/timer events). Skip the middle bulk — it's advanced EEM beyond ENCOR.
- `35-REST API Security` — modules 1–4 (API auth, tokens, rate-limiting — exam-relevant)

**CBT (primary — watch first):**
- `57-Network Programmability Foundations`
- `58-JSON / XML / YAML`
- `59-YANG Data Models`
- `60-NETCONF / RESTCONF`
- `61-REST APIs (DNAC, vManage, Meraki, Webex)`
- `62-Python Basics for Network Engineers`
- `63-EEM (Embedded Event Manager)`
- `64-Agent vs. Agentless Orchestration Tools (Ansible/Puppet/Chef/SaltStack)`

**INE (after CBT — protocol depth):**
- `39-Introducing Network Programmability & Automation` — full module (concepts: NETCONF, RESTCONF, YANG, data models, management plane evolution)
- `31-Introducing NETCONF & RESTCONF For Enterprise Networks` — full module
- `38-Practical Python Cisco Network Automation` — modules 1–8 (Python basics for network engineers, Netmiko, NAPALM)
- `40-Embedded Event Manager` — modules 1–16 + 47–51 (EEM applets, event detectors, action commands)
- `35-REST API Security` — modules 1–4 (API auth, tokens, rate-limiting)

**OCG skim:** Ch. 29, 30 summaries

**Bullets to write:** YANG — what it models and who reads it (management system vs device). NETCONF vs RESTCONF — transport, encoding, operations. Why Ansible is called agentless and what that means operationally.

---

## Pass 1 — End of week 14

**Self-check before starting Pass 2:**
Go through each phase topic and ask: *can I explain this to someone in 2 minutes without notes?*
- Yes on 80%+ → start Pass 2
- No on more than 3–4 topics → add a buffer week on those specific topics, then start Pass 2
- Don't add a buffer week across the board — just the weak spots

---

# PASS 2 — The Hardening (Weeks 15–24)

**Mode:** Labs are now the spine. Each topic block follows this pattern:

1. **Re-watch weak INE modules** (the ones where your Pass 1 bullets were thin)
2. **GNS3vault labs** — 2–4 labs per topic, from the `gns3vault-archive/` folder on GitHub
3. **Hand-written EVE-NG lab** — for modern topics GNS3vault doesn't cover (VXLAN, LISP, NETCONF, Ansible)
4. **OCG slow re-read** — full chapter, not just summary
5. **Notes in your own words** — include "Things I got wrong" subsection
6. **Anki cards** — build from notes, review daily

**Lab discipline:** Before looking at `final-configs/`, attempt the task list yourself. Even 40% correct is fine — the attempt is what hardens the syntax. Use `final-configs/` only as a diff, not as a cheat sheet.

---

## Pass 2 — Topic blocks

Each block is one week target. If you're slower, take 10 days. Don't rush.

---

### Block 1 — L2: STP and Guards (Week 15)

**Re-watch if needed:** INE `15` modules 1–6 (root election, port roles, convergence)

**GNS3vault labs** (`gns3vault-archive/Switching/`):
1. `pvst-per-vlan-spanning-tree` — PVST basics, one VLAN per tree
2. `pvrst-per-vlan-rapid-spanning-tree` — RPVST+, convergence verification
3. `mst-multiple-spanning-tree` — MST regions, instance mapping, boundary
4. `spanning-tree-root-guard` — placement, what it blocks
5. `spanning-tree-bpdu-guard` — placement, recovery after violation
6. `spanning-tree-loop-guard` — when it triggers vs Root Guard
7. `spanning-tree-bpdu-filter` — why you usually don't want it in production
8. `udld-unidirectional-link-detection` — fiber link failure detection

**OCG slow re-read:** Ch. 2, 3, 4

**Notes:** Write from memory. Include a table comparing Root Guard / BPDU Guard / Loop Guard / UDLD — placement, trigger, effect, recovery.

---

### Block 2 — L2: VLANs, trunks, EtherChannel (Week 16)

**Re-watch if needed:** INE `14` modules 2–7

**GNS3vault labs** (`gns3vault-archive/Switching/`):
1. `vlans-and-trunks` — basic VLAN/trunk config, native VLAN
2. `vtp-vlan-trunking-protocol` — VTP modes, domain, password
3. `pagp-lacp-etherchannel` — both protocols, static on, negotiation modes
4. `switch-svi-interface-and-routing` — L3 SVI config, inter-VLAN routing
5. `ccnp-switch-lab` — multi-feature capstone lab. Do this last.
6. `dhcp-snooping` — L2 security, rate limiting, trusted ports (also fits Block 8)

**OCG slow re-read:** Ch. 5

---

### Block 3 — OSPF (Week 17)

**Re-watch if needed:** INE `17` modules where your Pass 1 bullets were thin (adjacency troubleshooting, stub areas, summarization are the common gaps)

**GNS3vault labs** (`gns3vault-archive/OSPF/`):
1. `ospf-single-area` — baseline, verify adjacency and routes
2. `ospf-dr-bdr-election` — priority manipulation, show ip ospf interface
3. `ospf-authentication` — plain-text + MD5
4. `ospf-virtual-link` — connecting a non-contiguous area to Area 0
5. `ospf-summarization` — ABR summarization, cost calculation
6. `ospf-stub-area` + `ospf-totally-stubby-area` — verify LSA differences in each
7. `ospf-nssa` — Type 7 LSA, ABR conversion to Type 5
8. `ospf-default-route` — `default-information originate`, always keyword

**OCG slow re-read:** Ch. 8, 9, 10

---

### Block 4 — BGP (Week 18)

**Re-watch if needed:** INE `18` modules 3–7 (policy and path selection — this is where most people's Pass 1 bullets are weakest)

**GNS3vault labs** (`gns3vault-archive/BGP/`):
1. `bgp-basic-configuration` — eBGP session, network advertisement
2. `bgp-ibgp-configuration` — iBGP full mesh, next-hop-self
3. `bgp-attribute-weight` — weight manipulation, local router scope
4. `bgp-attribute-local-preference` — LP, AS-wide scope
5. `bgp-attribute-as-path` — AS-path prepending, influencing inbound traffic
6. `bgp-attribute-med` — MED, scope, deterministic MED
7. `bgp-communities` — standard communities, route-map matching
8. `bgp-route-reflector` — RR and client config, cluster-id

**OCG slow re-read:** Ch. 11, 12

---

### Block 5 — Multicast (Week 19)

**Re-watch if needed:** INE `24` PIM-SM modules (SPT switchover, RP registration — these are the hard parts)

**GNS3vault labs** (`gns3vault-archive/Multicast/`):
1. `pim-sparse-mode` — basic PIM-SM, static RP, verify mroute
2. `auto-rp` — RP announcement, mapping agent
3. `pim-bsr` — BSR candidate and RP candidate
4. `igmp` — IGMP versions, group membership, querier
5. `pim-source-specific-multicast` — SSM, channel model

**OCG slow re-read:** Ch. 13

---

### Block 6 — FHRP + QoS (Week 19, second half or Week 20)

**GNS3vault labs** (`gns3vault-archive/Network Services/` or similar):
1. `hsrp` — priority, preempt, tracking
2. `vrrp` — config differences from HSRP
3. `glbp` — load balancing, AVG/AVF roles

**QoS — no GNS3vault lab covers MQC well.** Use the hand-written EVE-NG lab (Lab NA1 QoS section from the original lab book) for a MQC policy-map exercise. Focus on: class-map match, policy-map queue, service-policy direction. The lab is short — one router, two classes.

**OCG slow re-read:** Ch. 14, Ch. 15 §FHRP

---

### Block 7 — IP Services (Week 20)

**GNS3vault labs** (`gns3vault-archive/Network Services/` and `Network Management/`):
1. `nat-static` + `nat-dynamic-pat` — static, PAT pool, overload
2. `ntp-network-time-protocol` — NTP server, client, stratum
3. `snmpv3-server` — v3 auth+priv, trap config
4. `syslog-server-logging` — severity levels, remote logging
5. `ip-sla` (if available) — ICMP echo probe, tracking, route change trigger. If not in GNS3vault, use EVE-NG: one CSR1000v with ip sla 10, track 1, floating static route.
6. `eem-scripting` + `eem-scripting-event-detector` — applet basics, syslog event, CLI event

**OCG slow re-read:** Ch. 15 §IP Services, Ch. 24 §Tools

---

### Block 8 — Overlays: VRF, GRE, IPsec (Week 21)

**Re-watch if needed:** INE `12` (IPsec VPNs) — crypto map config is dense, worth a second pass

**GNS3vault labs** (`gns3vault-archive/Tunneling/` or similar):
1. `gre-tunnel` — GRE basic, recursive routing problem, verify
2. `gre-over-ipsec` — crypto map + tunnel interface, IKEv1
3. `vrf-lite` — two VRFs, inter-VRF via route leaking

**Hand-written EVE-NG labs** (from original lab book, still needed — GNS3vault is too old for these):
- Lab V3 — *VXLAN Flood-and-Learn* (CSR1000v ×2, NVE interface, VXLAN VNI, verify mac learning)
- Lab A2 — *SD-Access Underlay/Overlay Concept* (IS-IS underlay + LISP+VXLAN overlay, CSR1000v ×3)

**OCG slow re-read:** Ch. 16, Ch. 23 §LISP/VXLAN

---

### Block 9 — Wireless (Week 22)

**No EVE-NG lab — use DevNet instead.**

**Hands-on session (one afternoon, 2–3 hours):**
- Connect to Cisco DevNet Always-On 9800 sandbox: `https://devnetsandbox.cisco.com/`
- Run through these show commands and understand each output:
  - `show wireless summary`
  - `show ap summary`
  - `show wireless client summary`
  - `show wlan summary`
  - `show wireless mobility summary`
  - `show ap dot11 5ghz summary`
- Configure one WLAN: `wlan TEST 1 TEST-SSID`, PSK security, no shutdown

**OCG slow re-read:** Ch. 17, 18, 19, 20, 21 — all five wireless chapters in full. This is the most OCG-heavy week in Pass 2 because there's no lab to carry you.

---

### Block 10 — Network Assurance (Week 22, second half or Week 23)

**GNS3vault labs** (`gns3vault-archive/Network Management/`):
1. `snmpv2-server` + `snmpv3-server` — back to back, compare config
2. `syslog-server-logging` + `system-message-logging` — severity, buffering, remote
3. `kron-task-scheduler` — scheduled config archive

**Hand-written EVE-NG labs:**
- Lab NA2 — *SPAN/RSPAN/ERSPAN* (original lab book — ERSPAN requires CSR1000v)
- Lab NA3 — *Flexible NetFlow + Model-Driven Telemetry* (CSR1000v — MDT via `telemetry ietf subscription`)

**OCG slow re-read:** Ch. 24

---

### Block 11 — Security (Week 23)

**Re-watch if needed:** INE `32` modules 4–7 (AAA config, CoPPr) and INE `37` (ZBFW — this is often where labs reveal gaps)

**GNS3vault labs** (`gns3vault-archive/Security/`):
1. `aaa-authentication` — RADIUS, TACACS+, local fallback
2. `aaa-command-authorization` + `aaa-exec-authorization` — privilege levels
3. `standard-access-list` → `extended-access-list` → `named-access-list` → `time-based-access-list` → `reflexive-access-list` — do all five in sequence
4. `unicast-reverse-path-forwarding-urpf` — strict vs loose mode
5. `control-place-policing` — MQC policy for control-plane
6. `basic-zone-based-firewall` — zone pairs, policy-map inspect, self zone

**GNS3vault labs** (`gns3vault-archive/Switching/`) for L2 hardening:
7. `dhcp-snooping` — trusted/untrusted ports, rate limit
8. Port-security — covered inside `ccnp-switch-lab` from Block 2 (revisit that lab's security section)

**OCG slow re-read:** Ch. 25, 26, 27, 28

---

### Block 12 — Automation (Week 24)

**Re-watch if needed:** INE `31` (NETCONF/RESTCONF) + INE `39` (programmability concepts) — these are dense and a second pass usually reveals things you missed

**Hand-written EVE-NG labs** (GNS3vault too old for these — use original lab book):
- Lab AU1 — *NETCONF/RESTCONF* (CSR1000v, Python requests, `ncclient`, verify with `show netconf-yang sessions`)
- Lab AU2 — *Ansible push* (CSR1000v + vIOS-L3 + vIOS-L2, ios_config module, inventory file, playbook)
- Lab AU3 — *EEM* (any image — applet with syslog trigger, action cli command)

**GNS3vault labs** (`gns3vault-archive/Network Management/`):
- `eem-scripting` + `eem-scripting-event-detector` — back to back with Lab AU3

**OCG slow re-read:** Ch. 29, 30

---

## Pass 2 — Final two weeks (Weeks 25–26)

### Week 25 — Weak-topic sprint

By now you have a clear picture of what didn't stick. This week:

- Pull your "Things I got wrong" sections from every block's notes file. Write them on a single page.
- For each item: if it's CLI — drill it in EVE-NG without config reference. If it's conceptual — re-read the OCG section and rewrite your bullet.
- Run your full Anki deck. Suspend cards you've answered correctly 5+ times. Promote cards you keep getting wrong to daily review.
- Do one GNS3vault lab from whatever domain felt weakest. No starter configs — attempt from blank topology.

### Week 26 — Integration and self-assessment

- Pick 3 random GNS3vault labs from different domains (e.g., one OSPF, one BGP, one Security). Build them from memory — no starter configs, no task list peeking for the first 30 minutes.
- For each lab you attempt: after finishing, write what you would explain differently to a junior engineer — this is your actual knowledge test.
- Read your full notes file start to finish. It should read like something you wrote, not something you copied.
- Optional: Boson ExSim practice exam, timed. If 80%+, you're at strong working knowledge. If below, add a week on the 2–3 lowest-scoring domains.

**No new material this week. Consolidation only.**

---

## Anki strategy (Pass 2 only)

- Create one card per "Things I got wrong" item — these are the highest-value cards.
- Tag cards: `encor::domain::topic` (e.g., `encor::ospf::lsa-types`)
- Review daily, 15–20 minutes maximum. This beats 90-minute weekly sessions significantly.
- Don't add cards for things you already know. Cards are for gaps, not for ego.

---

## What's intentionally different from the original plan

| Original plan | This plan | Why |
|---|---|---|
| CBT as sole spine | CBT first, INE second | CBT gives the big picture and self-check quizzes — ideal for first contact. INE goes Wireshark-deep — ideal once you have the concept framework. |
| Labs every week from the start | Labs only in Pass 2 | Mental model comes first. Labs are dramatically more productive when you know what you're building. |
| 10 h/week rigid schedule | Flexible — topic-based, not calendar-based | Your irregular schedule stops being a problem. If week 4 takes 10 days, nothing breaks. |
| Anki from week 1 | Anki only in Pass 2 | Memorizing something you half-understand is inefficient. Anki works on material you've already processed. |
| Single pass | Two passes | Seeing everything once at concept level, then again at depth, beats trying to go deep on first contact. |

---

## Quick reference: video modules by week

| Pass 1 week | CBT modules (watch first) | INE modules (watch second) |
|---|---|---|
| 1 | `01`, `02`, `15` | `7` (CEF), `1` (design), `14` mod 1 |
| 2 | `23`, `24`, `25`, `26` | `15` mods 1–12, `14` mods 8–9 |
| 3 | `21`, `22` | `14` mods 2–7 |
| 4 | `27`, `29`, `30`, `31`, `32`, `33` | `17` all 21 modules |
| 5 | `34`, `35`, `36` | `18` all 7 modules |
| 6 | `44` | `23` all, `24` PIM-SM modules |
| 7 | `13`, `14`, `43` | `21` (FHRP), `5` mods 1–12, `6` policing/shaping |
| 8 | `41`, `42`, `45`, `46`, `48` | `20` (NAT), `27` (SNMP/syslog), `25` mods 1–11, `29` (IP SLA) |
| 9 | `17`, `18`, `19`, `20` | `9` (VRF), `12` (IPsec), `13` mods 1–5, `11` (LISP), `10` mods 1–7 |
| 10 | `04`, `37`, `38`, `39`, `40`, `55` | `19` all 22 modules |
| 11 | `06`–`12` | `2` (SD-WAN intro), `3` mods 1–12 |
| 12 | `47`, `49`, `50` | `25` mods 7–11, `28` (SPAN), `27` SNMP v3, `26` (RMON) |
| 13 | `51`–`54`, `56` | `32` (hardening), `33` ACL, `34` (CoPP), `37` (ZBFW), `36` (endpoint) |
| 14 | `57`–`64` | `39` (programmability), `31` (NETCONF/RESTCONF), `38` mods 1–8, `40` mods 1–16+47–51, `35` mods 1–4 |
| 9 | `9` (VRF), `12` (IPsec), `13` mods 1–5, `11` (LISP), `10` mods 1–7 |
| 10 | `19` all 22 modules |
| 11 | `2` (SD-WAN intro), `3` mods 1–12 |
| 12 | `25` mods 7–11, `28` (SPAN), `27` SNMP v3, `26` (RMON) |
| 13 | `32` (hardening), `33` advanced ACL, `34` (CoPP), `37` (ZBFW), `36` (endpoint security) |
| 14 | `39` (programmability), `31` (NETCONF/RESTCONF), `38` mods 1–8, `40` mods 1–16+47–51, `35` mods 1–4 |
