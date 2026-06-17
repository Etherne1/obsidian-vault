# Post-ENCOR Study Plan — Advanced Networking, Design & Automation (vendor-light)

**Goal:** harden the skills you built in the ENCOR plan, with an emphasis on **design principles**, **deep protocol understanding**, and **automation that doesn't depend on vendor-specific platforms** (no Cisco DNAC / vManage / Meraki licensing required to practice).

**Why this shape:**
- You're in Russia → realistic day-job kit is **MikroTik, Juniper, Huawei, Eltex, whitebox + SONiC/FRR, Linux routers**. ENARSI-grade protocol depth (BGP, OSPF, MPLS, DMVPN, IPsec, VRF, IPv6) transfers cleanly. Cisco SD-WAN / DNAC / Meraki do not.
- You already own three angles on the same routing content (CBT ENARSI, Pearson route video, INE legacy). Use them as **primary / depth-pass / lab-pass** instead of watching everything once.
- Design (ENSLD) content is the most vendor-neutral knowledge in your library — almost every module teaches a principle, not a Cisco command.
- Automation: NETCONF / RESTCONF / YANG / Python / Git / REST are **industry standards** — Juniper, Nokia, Huawei, Arista all implement them. Cisco-specific automation modules (DNAC, vManage, Meraki, Azure Function) get watched as **architecture references**, not memorized.

**Spine cert (optional, but a useful structure even if you don't sit the exam):** Cisco **CCNP ENARSI 300-410**. The blueprint maps almost 1:1 onto vendor-neutral routing & infrastructure skills.

**Duration:** 16 weeks, ~10–12 h/week (video + lab + reading + Anki).

---

## Your Owned Libraries

| ID | Library | What it is | How I use it |
|---|---|---|---|
| **A** | CBT Nuggets **CCNP ENARSI 300-410** (`videos.txt`, 40 modules) | Spine course for routing/troubleshooting/security/services | **Primary** — watched week-by-week |
| **B** | CBT Nuggets **DEVCOR / ENAUTO 300-435** (`videos-2.txt`, 21 modules) | Git, Python, YANG, NETCONF/RESTCONF, IOS-XE/Meraki/vManage/DNAC automation | **Filtered** — keep vendor-neutral modules, watch Cisco-specific ones as concept overviews |
| **C** | **CCNP Enterprise Design ENSLD 300-420** (`videos-3.txt`, 27 modules) | IPv4/IPv6/IS-IS/EIGRP/OSPF/BGP design, WAN/SD-WAN design, QoS strategy, multicast design, MDT/YANG | **Parallel track** — one design module per week alongside ENARSI |
| **D** | Pearson **CCNP Route video** (`videos-4.txt`, 11 lessons: EIGRP, EIGRPv6, OSPF, OSPFv3, BGP, route manipulation, redistribution, MPLS, DMVPN, infra security, infra services) | Concise route-focused course | **Refresher / second pass** when a CBT topic doesn't click |
| **E** | **INE legacy routing & security deep-dives** (`videos-5.txt`, 27 module folders) | AD/loop-prevention, redistribution, PBR, BFD, EIGRP/OSPF/BGP, IPv6, MPLS/MPLS-VPN, DMVPN, IPsec, IKEv1, ACLs, IPv6 traffic filters, hardening, IPv6 FHS, SNMP, DHCP, IP SLA, NetFlow | **Lab depth** — the strongest hands-on content you own |

> Library D and E both pre-date current platforms but the **protocol theory and CLI mechanics are unchanged**. Treat them as routing canon.

---

## How to read each week

Each week has:
- **Goal** — what you should be able to explain or build by Sunday night.
- **Spine (Library A)** — the ENARSI module(s) for the week.
- **Design (Library C)** — one parallel design module.
- **Deep dive on demand (D / E)** — only if A doesn't click or you want a longer drill.
- **Automation drip (Library B)** — 1–2 short B modules per week so you don't lose momentum on programmability.
- **Labs** — vendor-neutral where possible (EVE-NG with Cisco images you already have, plus MikroTik / FRR / Juniper where it matters).
- **Anki prompts** — 10–20 Q&A; same workflow as the ENCOR plan.
- **Tracker hook** — what to log in your tracker.

---

## Week 1 — VRF-Lite, BFD, and the multi-tenant mindset

**Goal:** explain VRFs as the universal multi-tenant primitive (Cisco, MikroTik, Juniper all do this); use BFD to fail-detect any IGP / BGP in sub-second.

**Spine (A):**
- `4. Configure and Verify VRF-Lite` (modules 19–23: Intro, Overview, IPv4, Address Families, Routing Protocols)
- `5. Bidirectional Forwarding Detection`

**Design (C):** `1. Design an IPv4 and IPv6 Addressing Scheme` — VRF planning starts with an address plan.

**Deep dive (E):** `4-VRF-Lite` and `5-Bi-Directional Forwarding Detection (Video)` if the CBT modules don't go deep enough on route leaking or BFD timers.

**Automation drip (B):** `1. Implement Source Control with Git` + `2. Collaborate with Git` — set up a `network-configs` repo today; you'll commit every lab config from now on.

**Labs (EVE-NG, vendor-light):**
1. Build 3 VRFs on a single CSR1000v — `CUSTOMER_A`, `CUSTOMER_B`, `SHARED_SVC`. Add route leaking via route-target import/export and prove isolation with overlapping `10.0.0.0/24` in A and B.
2. **MikroTik cross-check:** repeat the same scenario on a MikroTik CHR (VRF + route leaking via `/ip route vrf`). This is the test that proves you understand the concept, not the syntax.
3. BFD with OSPF between two CSR1000v's; tear down a link and confirm sub-second convergence in `show ip ospf neighbor detail`.

**Anki (15):** route-distinguisher vs route-target, address-family syntax, why VRF-Lite ≠ MPLS VPN, BFD asynchronous vs demand mode, BFD echo, single-hop vs multi-hop, default BFD timers, which IGPs support BFD natively, what fails first when BFD goes down.

**Tracker:** *VRF-Lite + BFD complete — confident: route leaking; review: BFD multi-hop.*

---

## Week 2 — Route Manipulation, PBR, and the Tools of Control

**Goal:** use prefix-lists, route-maps, distribute-lists, and PBR as the **portable** toolkit for controlling routing on any vendor.

**Spine (A):**
- `1. Tools for Route Manipulation and Control`
- `3. Configure and Verify Policy-Based Routing`

**Design (C):** `11. Explain the Hierarchical Network Model` — refresh the access/distribution/core mental model; design influences where you apply policy.

**Deep dive (D):** `Lesson 6 Route Manipulation and Advanced Routing Concepts` (Pearson) — all 6 sub-modules; this is Pearson's strongest lesson.

**Deep dive (E):** `4-Policy-Based Routing (Video)` for PBR with set-clauses on next-hop / interface / DSCP.

**Automation drip (B):** `5. Interpret Python Scripts` + `6. Python Virtual Environments` — set up `venv` + `requirements.txt` and re-create the simple ping-sweep script from your ENCOR plan.

**Labs:**
1. Six-router topology: control which AS-path / next-hop is used between sites with route-maps. Verify with `show ip route` and `show route-map`.
2. PBR lab: route TCP/80 from a host out a backup link, all other traffic via primary. Verify with `show ip policy` and `traceroute`.
3. **MikroTik cross-check:** the same PBR on MikroTik using `/ip route rule` and mangle marks. You'll appreciate how clean Cisco's route-map syntax is — and how powerful MikroTik's connection-mark chain is.

**Anki (15):** prefix-list `le`/`ge` rules, route-map `match`/`set` order, when to use a distribute-list vs route-map, PBR fast-switching caveats, `set ip next-hop recursive` vs `verify-availability`, why PBR breaks ECMP, MikroTik connection-mark vs Cisco route-map.

---

## Week 3 — Route Redistribution Mastery

**Goal:** build mutual redistribution between IGPs without loops; explain admin-distance and tag-based loop prevention.

**Spine (A):** Re-read `1. Tools for Route Manipulation and Control` with redistribution focus.

**Design (C):** `4. Designing Networks with OSPF` — area design and how it interacts with redistribution.

**Deep dive (D):** `Lesson 7 Route Redistribution` (Pearson) — this is the cleanest redistribution explanation in your libraries.

**Deep dive (E):**
- `1-Preventing Loops With Administrative Distance`
- `2-Implementing Loop Prevention Mechanisms`
- `3-Optimizing IGP Route Redistribution`

**Automation drip (B):** `3. Describe Characteristics of API Styles` + `4. Understand the Challenges of Consuming APIs`.

**Labs:**
1. Classic two-point mutual redistribution between OSPF and EIGRP (or OSPF + IS-IS using FRR / Juniper vMX — both are EVE-NG-friendly). Inject a deliberate loop, then fix it with route tags.
2. Compare AD behavior on Cisco vs MikroTik vs Juniper preference values — make a one-page table for your notes.

**Anki (20):** default AD per protocol per vendor, internal vs external EIGRP AD, OSPFv2 inter-area vs intra-area AD, route tag widths (16-bit OSPF tag vs 32-bit EIGRP tag), why redistribution at two points loops, suppress-map vs unsuppress-map, `metric-type 1` vs `2`.

---

## Week 4 — EIGRP Deep (yes, still useful)

**Goal:** even though EIGRP is Cisco-only, it teaches **DUAL** — and many Russian carriers / ex-Cisco shops still run it on legacy ASR/ISR boxes. Treat as protocol theory + troubleshooting drill.

**Spine (A):**
- `6. Fundamentals of EIGRP`
- `7. EIGRP Address Families`
- `8. EIGRP Neighbor Relationships and Authentication`
- `9. EIGRP Stub Routing`
- `10. Troubleshoot EIGRP`

**Design (C):** `3. Design Networks with EIGRP` — query scope, hub-and-spoke patterns, stub theory.

**Deep dive (D):** `Lesson 1 EIGRP` + `Lesson 2 EIGRPv6`.

**Deep dive (E):** `6-Implementing & Troubleshooting EIGRP` (the deepest EIGRP lab content you own).

**Automation drip (B):** `7. Configuration Management for IOS XE` — read NETCONF/RESTCONF/Ansible/Puppet/Chef comparison; today you commit your first config to your Git repo.

**Labs:**
1. Build named-mode EIGRP across IPv4 + IPv6 address families.
2. Trigger an SIA (Stuck-In-Active) scenario in EVE-NG by removing a route on a remote neighbor; identify with `show ip eigrp topology active` and fix with stub routing.
3. Authentication: SHA-256 key chain (EIGRPv6 requires named-mode and key-chain).

**Anki (15):** DUAL feasibility condition, advertised vs reported distance, why FS < successor AD, named-mode mandatory features, why classic EIGRP uses MD5 but named uses SHA, stub types (`connected`, `summary`, `receive-only`), what triggers SIA.

---

## Week 5 — OSPF Address Families & Authentication

**Goal:** master OSPFv3 with IPv4 + IPv6 address families; configure key-chain authentication across vendors mentally.

**Spine (A):**
- `11. OSPF Address Families`
- `12. Troubleshoot OSPF Authentication for IPv4`
- `13. Troubleshoot OSPF Authentication for IPv6`

**Design (C):** `4. Designing Networks with OSPF` (continued) — ABR placement, prefix suppression, virtual-link avoidance, totally-stubby areas.

**Deep dive (D):** `Lesson 3 OSPF` + `Lesson 4 OSPFv3`.

**Deep dive (E):** `7-Implementing & Troubleshooting OSPF`.

**Automation drip (B):** `8. Understand and Explore Data Models` — first look at YANG; install `pyang` and explore the IETF OSPF model `ietf-ospf.yang`.

**Labs:**
1. Three-area OSPFv3 with one address-family for IPv4 and one for IPv6, key-chain authentication on all adjacencies.
2. Compare OSPFv2 authentication (interface or area mode) vs OSPFv3 (key-chain mandatory) — table it.
3. **FRR cross-check:** same topology on FRR (Linux router) — proves protocol portability.

**Anki (15):** OSPFv3 LSA types vs OSPFv2, address-family syntax, why OSPFv3 needs `ipv6 router id`, prefix-suppression behavior, virtual-link as last resort, key-chain key rotation, MD5 vs SHA1 vs SHA256.

---

## Week 6 — OSPF Network Types, Areas, and Virtual Links

**Goal:** know every OSPF network type cold (point-to-point, broadcast, NBMA, point-to-multipoint, p2mp-nonbroadcast) and when each makes sense.

**Spine (A):**
- `14. Troubleshoot IPv4 OSPF Network Types`
- `15. Troubleshoot IPv6 OSPF Network Types`
- `16. Troubleshoot IPv4 OSPF Areas`
- `17. Troubleshoot IPv6 OSPF Areas`
- `18. Troubleshoot IPv4 OSPF Virtual Links`
- `19. Troubleshoot IPv6 OSPF Virtual Links`

**Design (C):** revisit `4. Designing Networks with OSPF` — area boundary placement, stub/totally-stubby/NSSA decision tree.

**Automation drip (B):** `9. Master YANG Data Models for the Real World` — write your first `pyang`-validated YANG payload to read OSPF neighbors from your CSR1000v over NETCONF (this works on real Juniper too).

**Labs:**
1. NBMA hub-and-spoke OSPF over a Frame-Relay-emulated cloud in EVE-NG — pick the right network type and prove DR/BDR behavior.
2. Insert a discontiguous area-0 deliberately, fix with a virtual link, then re-design to remove the virtual link entirely.
3. NSSA with type-7 → type-5 conversion at the ABR — verify in `show ip ospf database`.

**Anki (20):** which network types elect a DR, hello/dead per type, P2P vs P2MP behavior on a hub-spoke, NSSA P-bit behavior, why virtual-links are anti-pattern, ASBR location vs ABR.

---

## Week 7 — OSPF Path Preference + IS-IS as a sane alternative

**Goal:** finish OSPF; learn IS-IS as a real-world choice for ISP backbones (used by Rostelecom, MTS, ER-Telecom in their cores).

**Spine (A):** `20. OSPF Path Preference`.

**Design (C):** `2. IS-IS Foundations and Design` — this is the only IS-IS content in your library; cover it carefully.

**Automation drip (B):** `10. Automate Cisco IOS-XE Devices with Netconf` — watch as concepts; NETCONF is a standard, the model differs by vendor but the workflow doesn't.

**Labs:**
1. OSPF path preference: prefer E2 → E1 → IA → O on a deliberately ambiguous topology; verify with `show ip route ospf`.
2. **Build a small IS-IS L1/L2 backbone** on FRR + Juniper vMX (Cisco IS-IS too if you have it). Three NETs, two areas, leak L2 → L1 with a route-map.
3. Compare OSPF area-0 constraint with IS-IS's looser hierarchy — write it up in your notes.

**Anki (15):** OSPF E1 vs E2 metric calculation, IS-IS L1 vs L2 vs L1/L2 router types, NET format and area parsing, IS-IS metric styles (narrow/wide), why IS-IS scales better than OSPF in flat backbones, ISP carrier usage of IS-IS.

---

## Week 8 — BGP Beyond Basics: Address Families & Scaling

**Goal:** BGP with multiple address families (IPv4, IPv6, VPNv4 preview), route reflectors, confederations, and the path-selection algorithm cold.

**Spine (A):**
- `21. Break Through BGP Basics with Address Families`
- `22. Solve BGP Scalability Issues`
- `23. Filter Routes in BGP`
- `24. Understand the BGP Best Path Algorithm`

**Design (C):**
- `5. Break Through BGP Basics with Address Families`
- `6. Solve BGP Scalability Issues`
- `7. Understand the BGP Best Path Algorithm`
- `8. Design Networks with BGP`

(Yes, Library A and C overlap here — watch C for design intent, A for troubleshooting drill.)

**Deep dive (D):** `Lesson 5 BGP`.

**Deep dive (E):**
- `9-Core BGP`
- `10-Route BGP Details and Implementation - Part 1`
- `11-Route BGP Details and Implementation - Part 2`
- `12-Route BGP Details and Implementation - Part 3`

**Automation drip (B):** `11. Automate IOS-XE Devices with Restconf` — same concept-level treatment as NETCONF.

**Labs:**
1. Three-AS topology: eBGP between ISPs, iBGP inside one AS with **route reflectors** (not full mesh). Verify cluster-list and originator-id.
2. AS-path filtering with regex (the Pearson `Lesson 6 Route Manipulation / 6.3 RegEx` lesson directly supports this).
3. Local-preference, MED, weight, AS-path-prepend — solve a deliberate sub-optimal path case with each in turn and document which is appropriate for which scenario.

**Anki (20):** BGP path-selection 13-step algorithm (write it out), iBGP synchronization rule and why it's normally disabled, route reflector loop prevention, confederation vs RR, AS-path regex anchors (`^`, `$`, `_`), well-known mandatory vs discretionary attributes, communities (standard, extended, large), `next-hop-self` use case.

---

## Week 9 — BGP Troubleshooting + IPv6 Migration

**Goal:** debug BGP from first principles; finalize an IPv6 migration strategy.

**Spine (A):** `25. Troubleshoot Common BGP Issues`.

**Design (C):** `9. Determine IPv6 Migration Strategies`.

**Deep dive (E):** `13-IPv6 Routing`.

**Automation drip (B):** `12. Other Automation Techniques for Cisco IOS-XE Devices` — quick survey of gNMI, on-box Python, EEM.

**Labs:**
1. Six common BGP failure modes: TCP/179 blocked, eBGP multihop missing, source-interface mismatch on loopbacks, MD5 mismatch, missing `next-hop-self` on RR, route-map deny shadowing an advertisement. Build each, troubleshoot it, write the diagnostic command sequence in your notes.
2. **Dual-stack lab:** OSPFv2/OSPFv3, eBGP IPv4 + IPv6 address families, IPv6 link-local next-hops. Confirm with `show bgp ipv6 unicast`.

**Anki (15):** BGP state machine (Idle → Connect → Active → OpenSent → OpenConfirm → Established), failure that puts you in `Active`, eBGP TTL trick, IPv6 ULA vs GUA, NAT64 / 464XLAT use cases, dual-stack vs translation vs tunneling decision.

---

## Week 10 — MPLS & MPLS L3VPN

**Goal:** understand label switching well enough to deploy or troubleshoot it on any vendor (Cisco LDP, Juniper RSVP-TE, MikroTik LDP all behave the same conceptually).

**Spine (A):**
- `26. Describe MPLS Operations`
- `27. Describe MPLS Layer 3 VPNs`

**Design (C):** no dedicated MPLS module in C, but you'll use `8. Design Networks with BGP` here — MPLS VPN is built on MP-BGP.

**Deep dive (D):** `Lesson 8 Multiprotocol Label Switching (MPLS)`.

**Deep dive (E):**
- `14-Introduction to MPLS`
- `15-Introduction to MPLS VPNs`

**Automation drip (B):** `13. SD-Access Automation with DNA Center Platform` — **watch as architecture only**; skip the DNAC CLI portions. The fabric/overlay concepts transfer to EVPN-VXLAN.

**Labs:**
1. Build a 4-router MPLS core with LDP. Verify LFIB, FIB, and label distribution with `show mpls forwarding-table`, `show mpls ldp neighbor`.
2. Add MPLS L3VPN: two CE sites in `VPN_A`, two CE sites in `VPN_B`, MP-BGP VPNv4 between PEs, RD + RT correctly. Prove route isolation.
3. **Optional:** repeat on Juniper vMX (LDP and MP-BGP behave identically; CLI differs).

**Anki (20):** MPLS label fields, label imposition / swap / disposition / PHP, LDP discovery, RD vs RT, vrf import vs export RT, MP-BGP next-hop behavior, label stack for L3VPN (outer LDP, inner VPN), why MPLS lets you avoid ip-in-ip tunnels.

---

## Week 11 — DMVPN & IPsec (carrier-WAN reality)

**Goal:** deploy DMVPN phase 1/2/3 and IPsec VPNs end-to-end — directly applicable on Cisco, MikroTik (EoIP+IPsec), and Juniper (st0 + IPsec).

**Spine (A):** `28. Configure and Verify DMVPN Networks`.

**Deep dive (D):** `Lesson 9 DMVPN`.

**Deep dive (E):**
- `16-Dynamic Multipoint VPN (DMVPN)`
- `17-IPsec VPNs`
- `18-IKEv1 IPsec VPN`

**Design (C):**
- `16. Compare WAN Connectivity Options`
- `17. Design Site-to-Site VPNs`
- `18. Design Enterprise WAN High Availability`

**Automation drip (B):** `14. Deeper Automation Techniques with DNA Center Platform` — **architecture only**; ignore vendor specifics. Note the intent-based-networking patterns; they'll re-appear in open-source orchestration.

**Labs:**
1. DMVPN Phase 1 hub-and-spoke with NHRP and EIGRP across.
2. Upgrade to Phase 3 (NHRP shortcut + redirect) and observe spoke-to-spoke tunnel formation.
3. IPsec protection of the mGRE tunnels (IKEv2 with PSK, then with certificates if you have a quick CA).
4. **MikroTik cross-check:** IPsec site-to-site between two CHRs (IKEv2, AES-256, SHA-256). Compare the conceptual flow with Cisco.

**Anki (20):** NHRP roles (NHC, NHS), mGRE tunnel-mode, DMVPN phase differences, EIGRP `no split-horizon` on hub, IPsec transform-set, IKE phases 1 + 2, main mode vs aggressive mode, IKEv2 advantages, PFS, DH groups in 2026.

---

## Week 12 — Infrastructure Security

**Goal:** harden any IOS / IOS-XE / IOS-XR / NX-OS box; design parallel CoPP/MPP/control-plane policy on MikroTik / Juniper.

**Spine (A):**
- `29. Troubleshoot Device Security using IOS AAA`
- `30. Troubleshoot Router Security Features`
- `31. Configure and Verify Control Plane Policing (CoPP)`
- `32. Troubleshoot Control Plane Policing`
- `33. Describe IPv6 First Hop Security Features`

**Deep dive (D):** `Lesson 10 Infrastructure Security`.

**Deep dive (E):**
- `19-RouterSwitch Security`
- `20-Access-Lists Beyond the Basic and Extended`
- `21-IPv6 Traffic Filters`
- `22-Hardening Cisco IOS`
- `23-IPv6 First Hop Security vSeminar`

**Design (C):** no direct design module; instead read **OCG ENARSI Ch. 15–17** if you own it.

**Automation drip (B):** `15. Automating with the SD-WAN vManage API` — **architecture only**, again skip the Cisco-licensing parts.

**Labs:**
1. Full AAA stack: TACACS+ for admin login, RADIUS for 802.1X (FreeRADIUS or your existing Tacacs+ box). Use command authorization for two admin tiers.
2. CoPP policy with named classes: management (SSH, NTP, SNMP), routing (OSPF, BGP, EIGRP, LDP), undesirable (ICMP redirects, IP options) → all rate-limited.
3. IPv6 FHS: RA Guard, DHCPv6 Guard, IPv6 Snooping, IPv6 Source Guard on a Layer-2 segment.

**Anki (20):** TACACS+ vs RADIUS (TCP vs UDP, full packet encryption, command authz), AAA method-list precedence, CoPP classification gotchas, IPv6 RA Guard vs DHCPv6 Guard, IPv6 ND inspection, IPv6 source guard prerequisites.

---

## Week 13 — Infrastructure Services & Telemetry

**Goal:** SNMP, syslog, NetFlow, IP SLA, DHCP — the daily-life operational stack.

**Spine (A):**
- `34. Troubleshoot Device Management Protocols`
- `35. Troubleshoot SNMP`
- `36. Troubleshoot Network Problems using Logging`
- `37. Troubleshoot IPv4 and IPv6 DHCP`
- `38. Troubleshoot Network Performance Issues using IP SLA`
- `39. Troubleshoot NetFlow`

**Deep dive (D):** `Lesson 11 Infrastructure Services`.

**Deep dive (E):**
- `24-Understanding SNMP`
- `25-Utilizing IOS DHCP Services`
- `26-IP Service Level Agreements`
- `27-Using & Troubleshooting Netflow`

**Design (C):**
- `21. Select QoS Strategies`
- `22. Design End-to-End QoS Policy`
- `23. Network Management and Telemetry Techniques`

**Automation drip (B):**
- `16. Deeper Automation with the vManage API` — architecture only.
- `17. Develop for the Meraki Dashboard API and Webhooks` — **skip** unless you've decided to learn Meraki for hobby; the patterns are well-covered by the generic REST modules.

**Labs:**
1. SNMPv3 with auth+priv to a Prometheus-snmp-exporter or LibreNMS box (you can run LibreNMS in EVE-NG-adjacent Docker).
2. NetFlow v9 + IPFIX export to ntopng or Akvorado (both run in Docker; both are realistic in a Russian production environment).
3. IP SLA UDP-jitter + ICMP-echo tracked objects driving static-route failover.
4. DHCP relay across VRFs (cross-VRF DHCP is a common interview question).

**Anki (20):** SNMPv2c vs v3, syslog severity 0–7, NetFlow v5 vs v9 vs IPFIX, sampled vs full NetFlow trade-offs, IP SLA reaction triggers, DHCP option 82, DHCP relay agent placement.

---

## Week 14 — DNA Center Assurance lesson (skim) + Design Wrap-up

**Goal:** finish the ENARSI blueprint and complete the design course.

**Spine (A):** `40. Troubleshoot Enterprise Networks with DNA Center Assurance` — **watch once at 1.5×** for awareness; nothing to lab.

**Design (C) — finish remaining modules:**
- `10. Design High Availability in Campus Networks`
- `12. Design Campus L2 Infrastructures`
- `13. Design Multicampus L3 Infrastructures`
- `24. Multicast Routing Foundations and Design`
- `14. Describe SD-Access Architecture` (concepts only — overlay/underlay/control-plane terminology applies directly to EVPN-VXLAN)
- `15. Design an SD-Access Fabric` (concepts only)
- `19. Describe Cisco SD-WAN Architecture and Principles` (concepts only)
- `20. Design Cisco SD-WAN` (concepts only — the overlay routing patterns apply to any SD-WAN: Fortinet, VyOS-based, Juniper SSR, FRR-based open-source SD-WAN)

**Automation drip (B):**
- `18. Deploy a Python Azure Function App with VS Code` — **skip** unless you care about serverless; not transferable to Russian-cloud day-job.
- `19. Implement the Meraki Location Scanning API` — **skip**.
- `20. Deploy the Meraki Captive Portal API` — **skip**.
- `21. Automate Cisco Meraki Security Cameras with MV Sense API` — **skip**.

**Labs:** none new — catch up.

**Anki (10):** SD-Access underlay vs overlay vs fabric edge / fabric border / control-plane node — translate each term to its EVPN-VXLAN equivalent.

---

## Week 15 — Vendor-neutral Automation Pivot

**Goal:** stop watching, start building. By end of week you have a working `network-automation` repo that polls device state from Cisco + MikroTik + (optional) Juniper.

**No new videos.** Instead:

1. **Nornir + NAPALM** — read [Nornir docs](https://nornir.readthedocs.io/) + [NAPALM docs](https://napalm.readthedocs.io/). Both are vendor-neutral.
2. **Ansible network collections** — install `cisco.ios`, `routeros`, `junipernetworks.junos`. Build one playbook that backs up running-configs from all three vendors on a schedule.
3. **NETCONF/RESTCONF with `ncclient` + `requests`** — write two Python scripts that pull OSPF neighbors from a CSR1000v via NETCONF and (separately) via RESTCONF.
4. **gNMI** — read [OpenConfig](https://www.openconfig.net/) intro; install `gnmic`; test against a CSR1000v in EVE-NG and (if available) a Junos VM.
5. **Telemetry stack** — stand up Prometheus + Grafana + a network-exporter (`snmp_exporter` or `gnmic` → Prometheus output). Dashboard interface counters from your EVE-NG topology.
6. **Git discipline** — every commit has a message, every change has a tag, every device has a YAML inventory file.

**Lab outcome to track:** a single `make backup` command in your repo backs up every device you own (virtual or physical). A single `make report` outputs interface error counters from every device.

---

## Week 16 — Capstone & Self-test

**Goal:** prove you can design, build, and operate a small ISP / large-enterprise network end-to-end.

**Capstone topology in EVE-NG:**
- 2 PE routers + 2 P routers (MPLS LDP core)
- 2 CE sites per VRF, 2 VRFs total
- MP-BGP VPNv4 between PEs, OSPF as PE-CE IGP
- One remote office reached over IPsec/DMVPN, terminating into a VRF
- Full AAA (TACACS+ box), CoPP on every router, IPv6 FHS on the access segment
- NetFlow v9 → ntopng; SNMP → LibreNMS; gNMI → Prometheus
- Ansible playbook deploys the whole thing from a Git repo
- Documented in a one-page network diagram + README

**Self-test:**
- Run the **CCNP ENARSI 300-410 practice exam** (if you have the OCG access, the Pearson Test Prep questions are excellent). Target ≥85% on the 200-question full pool before declaring the post-ENCOR plan complete.
- Decide your next direction: **Juniper JNCIS-SP → JNCIP-SP** (vendor-relevant in Russia), **JNCIS-DevOps**, or **Linux-networking deep dive (LFCS/LFCE + FRR/SONiC)**.

---

## Modules I deliberately deprioritized (and why)

| Module | Reason |
|---|---|
| Library B #17–21 (Meraki, Azure Functions) | Vendor lock-in and licensing not realistic in Russia. The REST/Python patterns inside are already covered by B #1–12. |
| Library B #13–16 (DNAC + vManage automation) | Watch the **first** module of each block as architecture orientation; skip the API specifics. The transferable parts (intent model, overlay automation) are covered conceptually. |
| Library A #40 (DNAC Assurance) | Watch once, no lab. Pattern is more important than the product. |
| Library C #14, 15, 19, 20 (SD-Access + SD-WAN design) | Watch as design **patterns** (overlay/underlay/fabric/control-plane separation, transport-independent overlays, application-aware routing). These patterns reappear in every vendor's SD-WAN. |
| EIGRP in Library D / E and Library A #6–10 | Still worth one full pass — it teaches DUAL and is occasionally found in Russian legacy networks — but skip if you've already done ENCOR EIGRP labs. |

---

## Books & Online Material to Buy (realistically obtainable in Russia)

The list is biased toward **vendor-neutral** material; vendor-specific picks are flagged.

### Tier 1 — Buy these first (highest ROI per ruble)

| Item | Why | Approx. availability |
|---|---|---|
| **TCP/IP Illustrated, Vol. 1** (Fall & Stevens, 2nd ed.) | The single best reference on how TCP/IP actually works on the wire. Reads forever. | Russian translations exist (Питер publishing); English available via Litres / Yakaboo / Ozon (used) |
| **Routing TCP/IP, Vol. 1 & 2** (Jeff Doyle) | Universally cited as the best IGP/BGP/EGP/IPv6 conceptual book. Cisco examples, but the protocol theory is vendor-neutral. | New US copies are hard; **used** very findable on Ozon / Avito |
| **BGP** (Iljitsch van Beijnum, O'Reilly) | Vendor-neutral, ISP-pragmatic BGP design. Short, dense, classic. | E-book on O'Reilly / Litres |
| **The Art of Network Architecture** (Russ White & Denise Donohue) | Pure design principles. No CLI. The book every senior network engineer cites. | English e-book; **also a Russian translation by Питер** |
| **Computer Networking Problems and Solutions** (Russ White & Ethan Banks) | Best modern survey of WHY each protocol exists. Vendor-agnostic. | English e-book; Russian translation by ДМК Пресс |

### Tier 2 — Automation & modern infra

| Item | Why |
|---|---|
| **Network Programmability and Automation** (Edelman, Lowe, Oswalt — O'Reilly, 2nd ed. 2023) | The canonical vendor-neutral automation book. Covers Linux, Python, NETCONF/RESTCONF/gNMI, Git, CI/CD for network teams. Russian translation by ДМК Пресс exists for the 1st edition; 2nd is English-only. |
| **Network Automation Cookbook** (Karim Okasha — Packt) | Recipe-style. Ansible, NetMiko, NAPALM. |
| **Hands-On Network Programming with C** OR **Practical Python for DevOps** (Behnam Dayanim et al.) | One for low-level, one for ops scripting. Pick based on your interest. |
| **Mastering Python Networking** (Eric Chou — Packt, 4th ed.) | Same ground as Edelman but more code-heavy. |

### Tier 3 — Protocol & design deep dives

| Item | Why |
|---|---|
| **OSPF and IS-IS: Choosing an IGP for Large-Scale Networks** (Jeff Doyle) | Definitive IGP-design pairing read. |
| **MPLS in the SDN Era** (Antonio Sanchez Monge & Krzysztof Szarkowicz — O'Reilly) | Cisco + Juniper side-by-side. Best MPLS book of the last decade. |
| **EVPN in the Data Center** (Dinesh Dutt — O'Reilly free e-book from NVIDIA) | EVPN-VXLAN is the modern alternative to SD-Access; this is the free, vendor-neutral introduction. |
| **Day One: Inside Junos** + any Juniper Day One book on a topic you'll lab | Juniper publishes them free as PDFs. JNCIS/JNCIP-SP track is **highly relevant in Russia**. |
| **MikroTik MTCNA / MTCRE / MTCINE** official training notes | If you're not already certified — MikroTik is everywhere in Russian SMB / WISP. Russian-language courses widely available. |

### Tier 4 — Video courses (CBT-Nuggets style)

| Source | Best courses for this plan |
|---|---|
| **INE** | Anything by Brian McGahan (MPLS, SD-WAN) or Brian Dennis (IGP / BGP). Subscription model — share with a friend. |
| **Ivan Pepelnjak / ipSpace.net** | The **gold standard** in vendor-neutral design content. *Building Network Automation Solutions*, *EVPN Deep Dive*, *BGP in the Data Center*, *Designing Active-Active Data Centers*. Subscription-based, accepts non-Russian cards via various proxies; one-month subscription per topic is enough. |
| **PacketLife.net + RouterFreak** | Free, vendor-neutral blog content. |
| **Cisco Press Live Lessons** | The Pearson video courses (your library D) are part of this series — high quality, used Pearson redemption codes are findable on Avito / forums. |
| **Juniper Learning Portal** | Free Juniper certification courses (JNCIA / JNCIS) — fully open, no Russian-bank friction. **Highly recommended given Russian market reality.** |
| **NRE Labs** (network reliability engineering, free) | Free interactive labs on Ansible/Salt/StackStorm. Hosted at https://labs.networkreliability.engineering/ |
| **CBT Nuggets** | If you can keep your subscription active: Knox Hutchinson's automation courses, Keith Barker's design courses. |

### Tier 5 — Reference (own these forever)

| Item | Why |
|---|---|
| **RFC 4271** (BGP-4) + RFC 7752 (BGP-LS) + RFC 4760 (Multiprotocol BGP) | Read the actual RFCs at least once. |
| **RFC 2328** (OSPFv2) + **RFC 5340** (OSPFv3) | Same. |
| **ISO 10589** + **RFC 1195** (IS-IS) | Less digestible but worth having. |
| **draft-ietf-grow-bgp-reject** + recent **BGP best practices** drafts | BGP hygiene in 2026 (RPKI, max-prefix, default routes). |

---

## Anki / Spaced-Repetition discipline

Same workflow as your ENCOR plan:
- Add the week's Q&A pack to a single deck `ENCOR-Next` (sub-deck per week if you like).
- Daily review target: 20 minutes max.
- If you skip more than 3 days, **reset the week's new cards** rather than burning out.

---

## Progress tracker hook

Use the same `ENCOR-Progress-Tracker.md` / `.csv` shape you already have — just rename it `Post-ENCOR-Progress-Tracker` and add columns:

| Week | Spine (A) module IDs done | Design (C) module done | Deep-dive ref (D/E) | Labs done (1-N) | Anki added | Confidence (1-5) | Needs review |
|---|---|---|---|---|---|---|---|

Update at end of each week. When confidence ≤ 3 for two weeks in a row, slow down and re-lab.

---

## Final answer to your underlying question

> *"What's actually transferable in Russia?"*

**Transferable (high confidence):** ENARSI routing/troubleshooting, MPLS + L3VPN, DMVPN/IPsec, BGP design, OSPF/IS-IS, IPv6, AAA/TACACS+/RADIUS, NetFlow/IPFIX, SNMPv3, NETCONF/RESTCONF/YANG/gNMI, Python+Nornir+NAPALM, Ansible, FRR, MikroTik, Juniper Junos.

**Not transferable (deprioritize):** Cisco DNAC, Cisco SD-WAN (Viptela), Meraki, anything tied to Cisco Smart Licensing or Smart Account, anything that needs a live cisco.com login to function.

**Best next certification (after this plan):**
- Most-employable single move: **Juniper JNCIS-SP** (free study material, exam reasonably priced, directly applies to Rostelecom-tier carriers).
- Most credential-flexible: **CCNP ENARSI 300-410** (you can sit the exam at a Pearson VUE center in CIS-adjacent countries; the cert is paper-recognizable).
- Most future-proof: **CKA + Linux Foundation Networking Fundamentals** + **NRE Labs automation tracks** — Russia's enterprise market is migrating heavily toward Linux-on-whitebox.
