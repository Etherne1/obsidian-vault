# Post-ENCOR Study Plan — Advanced Networking, Design & Automation

**Goal:** deep working knowledge for production use in a mixed-vendor environment. ENARSI exam readiness is a side-effect, not the driver.

**Time budget:** ~2 hours/day, irregular. Same reality as the ENCOR plan — some days nothing, that's fine.

**Total duration:** ~28–32 weeks across two passes.

**Video spine:** INE legacy (Library E) as primary for routing/security depth. CBT ENARSI (Library A) as secondary for exam angle and troubleshooting flow. Design (Library C) as a parallel once-per-topic track. Automation (Library B) filtered — vendor-neutral modules only.

**Labs:** GNS3vault archive for all classical topics. Hand-written EVE-NG labs for DMVPN, IS-IS, BFD, OSPFv3 AFs, named-mode EIGRP, IPv6 FHS, telemetry, automation, and the capstone. See `Post-ENCOR-Labs-EVE-NG.md` for the full lab book.

**Context on the libraries:**

| ID | Library | Role in this plan |
|---|---|---|
| **A** | CBT Nuggets CCNP ENARSI 300-410 (40 modules) | Secondary — watched after INE for exam angle and troubleshooting flow |
| **B** | CBT Nuggets DEVCOR/ENAUTO 300-435 (21 modules) | Filtered — vendor-neutral modules only; Cisco-specific ones are architecture-only overviews |
| **C** | CCNP Enterprise Design ENSLD 300-420 (27 modules) | Parallel track — one module per topic alongside the routing content |
| **D** | Pearson CCNP Route video (11 lessons) | Tertiary refresher when A or E doesn't click on a specific topic |
| **E** | INE legacy routing & security deep-dives (27 module folders) | **Primary** — the deepest protocol content you own; watched first |

**Why INE first:** the INE courses are CCIE-caliber depth. They explain *why* protocols behave the way they do. CBT is exam-angle — it tells you *what* to configure. Getting depth before the exam angle means CBT becomes consolidation rather than introduction. You retain more from the second pass.

---

## How the two passes differ

| | Pass 1 (~16 weeks) | Pass 2 (~12 weeks) |
|---|---|---|
| **Goal** | Build the mental map. See everything once. | Harden with labs, notes, and Anki. |
| **INE (E)** | Primary — watch in full | Re-watch only weak modules |
| **CBT (A)** | Secondary — watch after INE | Only for topics where INE didn't click |
| **Design (C)** | One module per topic — light pass | Re-watch design modules for weak topics |
| **Library D** | Only when A + E both don't click | — |
| **Labs** | None | All labs from the lab book, GNS3vault first |
| **OCG/notes** | 3–5 bullets per INE module from memory | Full notes, "things I got wrong" subsection |
| **Anki** | None | Build from notes, review daily |
| **MikroTik cross-checks** | None | After each relevant lab |

**Daily rhythm (both passes):**

- Good day (2h): 90 min INE/CBT video (Pass 1) or lab session (Pass 2) + 30 min bullets from memory.
- Short day (45–60 min): one INE module, 3 bullets.
- Zero day: fine. Don't double up the next day. Resume where you left off.
- The one rule: always write bullets after watching. Closing the video and writing from memory is worth more than re-watching.

---

# PASS 1 — The Map (Weeks 1–16)

No labs. No Anki. No deep OCG reads. INE first → CBT second → one design module → 5 bullets from memory.

If a topic takes 10 days instead of 7, extend it. Nothing breaks.

---

## Phase 1 — Routing Foundations (Weeks 1–4)

### Week 1 — VRF-Lite, BFD, route manipulation foundations

**INE (E — primary):**
- `4-VRF-Lite` — full module (VRF concept, RD/RT mechanics, route leaking, address families)
- `5-Bi-Directional Forwarding Detection (Video)` — full module (BFD asynchronous vs demand, timers, IGP/BGP integration)

**CBT (A — after INE):**
- `4. Configure and Verify VRF-Lite` (modules 19–23)
- `5. Bidirectional Forwarding Detection`

**Design (C):** `1. Design an IPv4 and IPv6 Addressing Scheme` — VRF planning starts with an address plan.

**Automation drip (B):** `1. Implement Source Control with Git` + `2. Collaborate with Git` — set up a `network-configs` repo. You'll commit every lab config from here on.

**Bullets to write:** RD vs RT — what each does and why they're separate. VRF-Lite vs MPLS VPN — where they differ. BFD asynchronous mode — what it replaces and why it's faster.

---

### Week 2 — Route manipulation and PBR

**INE (E — primary):**
- `4-Policy-Based Routing (Video)` — full module (set-clauses, next-hop, interface, DSCP, verify-availability)

**CBT (A — after INE):**
- `1. Tools for Route Manipulation and Control`
- `3. Configure and Verify Policy-Based Routing`

**Library D (if A + E don't click):** `Lesson 6 Route Manipulation and Advanced Routing Concepts` — Pearson's strongest lesson on this topic.

**Design (C):** `11. Explain the Hierarchical Network Model` — where policy tools belong in the hierarchy.

**Automation drip (B):** `5. Interpret Python Scripts` + `6. Python Virtual Environments` — set up venv + requirements.txt.

**Bullets to write:** prefix-list `le`/`ge` semantics. Route-map `match`/`set` evaluation order. When PBR breaks ECMP and why. `set ip next-hop verify-availability` — what it prevents.

---

### Week 3 — Route redistribution

**INE (E — primary):**
- `1-Preventing Loops With Administrative Distance` — full module
- `2-Implementing Loop Prevention Mechanisms` — full module
- `3-Optimizing IGP Route Redistribution` — full module

**CBT (A — after INE):** `1. Tools for Route Manipulation and Control` (redistribution focus — re-watch the redistribution sections).

**Library D (if needed):** `Lesson 7 Route Redistribution` — Pearson's cleanest redistribution explanation.

**Design (C):** `4. Designing Networks with OSPF` — ABR placement and how it interacts with redistribution.

**Automation drip (B):** `3. Describe Characteristics of API Styles` + `4. Understand the Challenges of Consuming APIs`.

**Bullets to write:** why two-point redistribution loops. Route tag widths (16-bit OSPF vs 32-bit EIGRP). Suppress-map vs unsuppress-map. Default AD per protocol — Cisco, MikroTik, Juniper comparison.

---

### Week 4 — EIGRP deep

**INE (E — primary):**
- `6-Implementing & Troubleshooting EIGRP` — full module (the deepest EIGRP content you own: DUAL, feasibility condition, SIA, named-mode, stub routing, SHA-256)

**CBT (A — after INE):**
- `6. Fundamentals of EIGRP`
- `7. EIGRP Address Families`
- `8. EIGRP Neighbor Relationships and Authentication`
- `9. EIGRP Stub Routing`
- `10. Troubleshoot EIGRP`

**Library D (if needed):** `Lesson 1 EIGRP` + `Lesson 2 EIGRPv6`.

**Design (C):** `3. Design Networks with EIGRP` — query scope, hub-and-spoke patterns, stub theory.

**Automation drip (B):** `7. Configuration Management for IOS XE` — NETCONF/RESTCONF/Ansible comparison; commit your first config to Git today.

**Bullets to write:** DUAL feasibility condition — FD vs AD in plain language. Why FS < successor AD. Named-mode mandatory features (SHA-256 requires it). What triggers SIA and how stub routing prevents it.

---

## Phase 2 — OSPF and IS-IS (Weeks 5–7)

### Week 5 — OSPFv3 and authentication

**INE (E — primary):**
- `7-Implementing & Troubleshooting OSPF` — full module (the deepest OSPF content you own: OSPFv3 AF config, key-chain authentication, adjacency troubleshooting, NSSA, summarization)

**CBT (A — after INE):**
- `11. OSPF Address Families`
- `12. Troubleshoot OSPF Authentication for IPv4`
- `13. Troubleshoot OSPF Authentication for IPv6`

**Library D (if needed):** `Lesson 3 OSPF` + `Lesson 4 OSPFv3`.

**Design (C):** `4. Designing Networks with OSPF` (Part 1) — ABR placement, prefix suppression, totally-stubby areas.

**Automation drip (B):** `8. Understand and Explore Data Models` — first look at YANG. Install `pyang`.

**Bullets to write:** OSPFv3 LSA types vs OSPFv2 — what changed and why. Why OSPFv3 requires `ipv6 router id`. Key-chain rotation — what happens to the adjacency during a key roll.

---

### Week 6 — OSPF network types, areas, virtual links

**INE (E — primary):** Re-watch relevant sections of `7-Implementing & Troubleshooting OSPF` — specifically the network types and area types modules. This is a second pass on the same INE course, focused on a different slice.

**CBT (A — after INE):**
- `14. Troubleshoot IPv4 OSPF Network Types`
- `15. Troubleshoot IPv6 OSPF Network Types`
- `16. Troubleshoot IPv4 OSPF Areas`
- `17. Troubleshoot IPv6 OSPF Areas`
- `18. Troubleshoot IPv4 OSPF Virtual Links`
- `19. Troubleshoot IPv6 OSPF Virtual Links`

**Design (C):** `4. Designing Networks with OSPF` (Part 2) — virtual-link avoidance, area boundary placement, decision tree for stub/NSSA.

**Automation drip (B):** `9. Master YANG Data Models for the Real World` — write your first pyang-validated YANG payload for OSPF neighbors over NETCONF.

**Bullets to write:** which network types elect a DR. Hello/dead timers per type. P2MP vs NBMA on a hub-and-spoke — which one you'd choose and why. Why virtual links are an anti-pattern.

---

### Week 7 — OSPF path preference + IS-IS

**INE (E — primary):** No dedicated IS-IS module in the library. For OSPF path preference, re-watch the relevant section of `7-Implementing & Troubleshooting OSPF`.

**CBT (A — after INE):** `20. OSPF Path Preference`.

**Design (C):** `2. IS-IS Foundations and Design` — the only IS-IS content in your library; cover it carefully. This is the primary source for IS-IS this week since INE doesn't have it.

**Automation drip (B):** `10. Automate Cisco IOS-XE Devices with Netconf` — concepts only; the workflow transfers to any vendor.

**Bullets to write:** OSPF path preference order (O > O IA > E1 > E2 > N1 > N2). IS-IS L1 vs L2 vs L1/L2 router types. NET format parsing — what each field means. Why IS-IS has no area-0 constraint.

---

## Phase 3 — BGP Deep (Weeks 8–9)

### Week 8 — BGP address families, RR, filtering, path selection

**INE (E — primary):**
- `9-Core BGP` — full module
- `10-Route BGP Details and Implementation - Part 1` — full module
- `11-Route BGP Details and Implementation - Part 2` — full module
- `12-Route BGP Details and Implementation - Part 3` — full module

These four modules are the deepest BGP content you own. Watch in order.

**CBT (A — after INE):**
- `21. Break Through BGP Basics with Address Families`
- `22. Solve BGP Scalability Issues`
- `23. Filter Routes in BGP`
- `24. Understand the BGP Best Path Algorithm`

**Design (C):**
- `5. Break Through BGP Basics with Address Families`
- `6. Solve BGP Scalability Issues`
- `7. Understand the BGP Best Path Algorithm`
- `8. Design Networks with BGP`

Note: Library A and C overlap deliberately here. Watch C for design intent, A for troubleshooting drill. They're short enough that both are manageable in one week given the INE foundation.

**Library D (if needed):** `Lesson 5 BGP`.

**Automation drip (B):** `11. Automate IOS-XE Devices with Restconf` — concepts; same workflow as NETCONF.

**Bullets to write:** BGP best-path algorithm — first 7 steps in order. RR loop prevention (cluster-list + originator-id). Confederation vs RR — when you'd choose each. AS-path regex anchors (`^`, `$`, `_`).

---

### Week 9 — BGP troubleshooting + IPv6 migration

**INE (E — primary):**
- `13-IPv6 Routing` — full module (dual-stack, OSPFv2/v3, eBGP IPv4 + IPv6 AFs, link-local next-hops, migration strategies)

**CBT (A — after INE):**
- `25. Troubleshoot Common BGP Issues`

**Design (C):** `9. Determine IPv6 Migration Strategies`.

**Automation drip (B):** `12. Other Automation Techniques for Cisco IOS-XE Devices` — survey of gNMI, on-box Python, EEM.

**Bullets to write:** BGP state machine — what failure puts you in `Active` vs `Idle`. eBGP TTL trick. The six most common BGP failure modes — predict the state for each. IPv6 ULA vs GUA — when to use each.

---

## Phase 4 — MPLS and WAN (Weeks 10–11)

### Week 10 — MPLS and L3VPN

**INE (E — primary):**
- `14-Introduction to MPLS` — full module (label fields, imposition/swap/disposition/PHP, LDP discovery)
- `15-Introduction to MPLS VPNs` — full module (VRF, RD/RT, MP-BGP VPNv4, label stack, PE-CE protocols)

**CBT (A — after INE):**
- `26. Describe MPLS Operations`
- `27. Describe MPLS Layer 3 VPNs`

**Library D (if needed):** `Lesson 8 Multiprotocol Label Switching (MPLS)`.

**Design (C):** `8. Design Networks with BGP` (re-use from Week 8 with MPLS VPN lens — MP-BGP is the control plane for L3VPN).

**Automation drip (B):** `13. SD-Access Automation with DNA Center Platform` — architecture only. Note the fabric/overlay patterns — they reappear in EVPN-VXLAN.

**Bullets to write:** PHP — what it is and why it exists. RD vs RT — one sentence each. MP-BGP next-hop behavior in L3VPN — why the PE sets it to itself. Why MPLS lets you avoid IP-in-IP tunnels.

---

### Week 11 — DMVPN and IPsec

**INE (E — primary):**
- `16-Dynamic Multipoint VPN (DMVPN)` — full module (Phase 1/2/3 mechanics, NHRP roles, mGRE, EIGRP hub no-split-horizon)
- `17-IPsec VPNs` — full module (IKE phases, transform-set, crypto map, VTI, GRE over IPsec)
- `18-IKEv1 IPsec VPN` — modules 1–5 (IKE phase 1 modes, phase 2, SA establishment). Skip modules 6–8 unless DMVPN lab reveals gaps.

**CBT (A — after INE):** `28. Configure and Verify DMVPN Networks`.

**Library D (if needed):** `Lesson 9 DMVPN`.

**Design (C):**
- `16. Compare WAN Connectivity Options`
- `17. Design Site-to-Site VPNs`
- `18. Design Enterprise WAN High Availability`

**Automation drip (B):** `14. Deeper Automation Techniques with DNA Center Platform` — architecture only; note the intent-based-networking patterns.

**Bullets to write:** DMVPN phases — what changes between each. NHRP NHS vs NHC roles. Why `no ip split-horizon eigrp` is mandatory on the DMVPN hub. IKEv2 advantages over IKEv1 (fewer message exchanges, built-in NAT-T, EAP).

---

## Phase 5 — Security and Services (Weeks 12–13)

### Week 12 — Infrastructure security

**INE (E — primary):**
- `19-RouterSwitch Security` — full module
- `20-Access-Lists Beyond the Basic and Extended` — full module (reflexive, dynamic, object groups, time-based, logging)
- `21-IPv6 Traffic Filters` — full module
- `22-Hardening Cisco IOS` — full module (device architecture, AAA framework, SNMP/NTP/logging hardening, control-plane security, CoPPr, uRPF)
- `23-IPv6 First Hop Security vSeminar` — full module (RA Guard, DHCPv6 Guard, IPv6 Snooping, Source Guard)

**CBT (A — after INE):**
- `29. Troubleshoot Device Security using IOS AAA`
- `30. Troubleshoot Router Security Features`
- `31. Configure and Verify Control Plane Policing (CoPP)`
- `32. Troubleshoot Control Plane Policing`
- `33. Describe IPv6 First Hop Security Features`

**Library D (if needed):** `Lesson 10 Infrastructure Security`.

**Design (C):** No direct design module this week — use the time for a slow read of the CoPP and hardening sections in your OCG if you have it.

**Automation drip (B):** `15. Automating with the SD-WAN vManage API` — architecture only.

**Bullets to write:** TACACS+ vs RADIUS — TCP vs UDP, what TACACS+ fully encrypts that RADIUS doesn't, command authorization difference. CoPP placement — where it sits and what it protects vs MPP. IPv6 RA Guard vs DHCPv6 Guard — what each blocks.

---

### Week 13 — Infrastructure services and telemetry

**INE (E — primary):**
- `24-Understanding SNMP` — full module (v2c vs v3, auth+priv, MIB walk, traps vs informs)
- `25-Utilizing IOS DHCP Services` — full module (DHCP server, relay, option 82, cross-VRF relay)
- `26-IP Service Level Agreements` — full module (probe types, reaction, tracking, route failover)
- `27-Using & Troubleshooting Netflow` — full module (v5 vs v9 vs IPFIX, sampled vs full, Flexible NetFlow)

**CBT (A — after INE):**
- `34. Troubleshoot Device Management Protocols`
- `35. Troubleshoot SNMP`
- `36. Troubleshoot Network Problems using Logging`
- `37. Troubleshoot IPv4 and IPv6 DHCP`
- `38. Troubleshoot Network Performance Issues using IP SLA`
- `39. Troubleshoot NetFlow`

**Library D (if needed):** `Lesson 11 Infrastructure Services`.

**Design (C):**
- `21. Select QoS Strategies`
- `22. Design End-to-End QoS Policy`
- `23. Network Management and Telemetry Techniques`

**Automation drip (B):** `16. Deeper Automation with the vManage API` — architecture only. Skip B `17–21` (Meraki, Azure Functions) entirely.

**Bullets to write:** SNMPv3 security levels — noAuthNoPriv vs authNoPriv vs authPriv. IP SLA reaction types — what each triggers. NetFlow v5 vs v9 vs IPFIX — key differences. DHCP option 82 — what it adds and why a relay inserts it.

---

## Phase 6 — Design Wrap-up and Automation (Weeks 14–16)

### Week 14 — Design wrap-up + DNA Center awareness

**INE (E):** No new INE content this week.

**CBT (A):** `40. Troubleshoot Enterprise Networks with DNA Center Assurance` — watch once at 1.5×. Awareness only, no lab.

**Design (C) — finish remaining modules:**
- `10. Design High Availability in Campus Networks`
- `12. Design Campus L2 Infrastructures`
- `13. Design Multicampus L3 Infrastructures`
- `24. Multicast Routing Foundations and Design`
- `14. Describe SD-Access Architecture` — concepts only; translate each term to its EVPN-VXLAN equivalent as you watch
- `15. Design an SD-Access Fabric` — same
- `19. Describe Cisco SD-WAN Architecture and Principles` — concepts only
- `20. Design Cisco SD-WAN` — overlay routing patterns transfer to any SD-WAN vendor

**Skip:** Library B modules 18–21 (Azure Functions, Meraki) — not transferable to day-job reality.

**Bullets to write:** SD-Access fabric edge = ? in EVPN-VXLAN. LISP control-plane node = ? in BGP-EVPN. What SD-WAN's transport-independent overlay means in practice (not Cisco-specific language).

---

### Week 15 — Automation

**INE (E — primary):** No dedicated automation module in the INE library. This week is CBT + Library B primary.

**CBT ENAUTO (B — primary this week):**
- `1. Implement Source Control with Git` + `2. Collaborate with Git` (if not done in Week 1)
- `5. Interpret Python Scripts`
- `6. Python Virtual Environments`
- `7. Configuration Management for IOS XE`
- `8. Understand and Explore Data Models`
- `9. Master YANG Data Models for the Real World`
- `10. Automate Cisco IOS-XE Devices with Netconf`
- `11. Automate IOS-XE Devices with Restconf`
- `12. Other Automation Techniques for Cisco IOS-XE Devices`

**Bullets to write:** YANG — what it models and who reads it. NETCONF vs RESTCONF — transport, encoding, operations. Why Ansible is agentless and what that means in practice. gNMI vs SNMP — what each pushes/polls and at what granularity.

---

### Week 16 — Pass 1 review

No new content. Self-check before starting Pass 2:

Go through each phase topic and ask: *can I explain this in 2 minutes without notes?*
- 80%+ clear → start Pass 2
- Gaps in more than 3–4 topics → one extra week on those specific topics only

Write a one-page summary of your biggest weak spots. These become the first targets in Pass 2 re-watches.

---

# PASS 2 — The Hardening (Weeks 17–28)

**Mode:** labs are now the spine. Each block follows this pattern:

1. **Re-watch weak INE modules** — the ones where your Pass 1 bullets were thin
2. **GNS3vault labs** from `Post-ENCOR-Labs-EVE-NG.md` — pre-validated, task-listed, YouTube walkthroughs
3. **Hand-written EVE-NG lab** for topics GNS3vault doesn't cover
4. **Full notes in your own words** — include "Things I got wrong" subsection
5. **Anki cards** from notes — review daily, 15–20 min max

**Lab discipline:** attempt the GNS3vault task list before opening `final-configs/`. Diff your config against the solution after — the diff is what you learn from.

---

## Pass 2 blocks (one block = one week target; take 10 days if needed)

### Block 1 — VRF-Lite + BFD (Week 17)

**Re-watch if thin:** INE `4-VRF-Lite`.

**Labs:**
- GNS3vault: `vrf-lite`, `vrf-routing`
- Hand-written: BFD on OSPF + BGP (no GNS3vault coverage), MikroTik VRF cross-check

See `Post-ENCOR-Labs-EVE-NG.md` Lab 1.

---

### Block 2 — Route manipulation + PBR (Week 17, second half / Week 18)

**Re-watch if thin:** INE `4-Policy-Based Routing`.

**Labs:**
- GNS3vault: `policy-based-routing`, `bgp-filtering-extended-access-list`, `bgp-as-path-access-list`
- Hand-written: multi-AS route-map extension, MikroTik PBR cross-check

See `Post-ENCOR-Labs-EVE-NG.md` Lab 2.

---

### Block 3 — Redistribution (Week 18)

**Re-watch if thin:** INE `1`, `2`, `3` (loop prevention + redistribution series).

**Labs:**
- GNS3vault: `ospf-single-area` + `eigrp-basic` as building blocks
- Hand-written: full redistribution with loop prevention (no GNS3vault coverage for mutual redistribution)

See `Post-ENCOR-Labs-EVE-NG.md` Lab 3.

---

### Block 4 — EIGRP deep (Week 19)

**Re-watch if thin:** INE `6-Implementing & Troubleshooting EIGRP` SIA section.

**Labs:**
- GNS3vault: `eigrp-basic`, `eigrp-authentication`, `eigrp-summarization`
- Hand-written: named-mode extension (SHA-256, AF, SIA scenario)

See `Post-ENCOR-Labs-EVE-NG.md` Lab 4.

---

### Block 5 — OSPFv3 + authentication (Week 20)

**Re-watch if thin:** INE `7-Implementing & Troubleshooting OSPF` AF and authentication sections.

**Labs:**
- GNS3vault: `ospf-authentication`, `ospf-nssa-not-so-stubby-area`, `ospf-virtual-link`
- Hand-written: OSPFv3 address-family lab + FRR cross-check

See `Post-ENCOR-Labs-EVE-NG.md` Lab 5.

---

### Block 6 — OSPF areas and network types (Week 21)

**Re-watch if thin:** INE `7` network-type sections.

**Labs:**
- GNS3vault: `ospf-stub-area`, `ospf-totally-stub`, `ospf-nssa-not-so-stubby-area`, `ospf-totally-nssa`, `ospf-virtual-link`, `ospf-lsa-type-3-summarization`, `ospf-lsa-type-5-summarization`
- Hand-written: modern network-type extension (no Frame Relay)

See `Post-ENCOR-Labs-EVE-NG.md` Lab 6.

---

### Block 7 — IS-IS (Week 22)

**Re-watch if thin:** Library C `2. IS-IS Foundations and Design` (this is the primary IS-IS source).

**Labs:**
- Hand-written entirely (no GNS3vault IS-IS): FRR Docker containers + vIOS-L3

See `Post-ENCOR-Labs-EVE-NG.md` Lab 7.

---

### Block 8 — BGP deep (Week 23)

**Re-watch if thin:** INE `9–12` (whichever modules your Pass 1 bullets were thinnest on — path selection and communities are the most common gaps).

**Labs:**
- GNS3vault: `bgp-basic`, `ibgp-internal-bgp`, `bgp-next-hop-self`, `bgp-route-reflectors`, `bgp-attribute-local-preference`, `bgp-attribute-as-path`, `bgp-attribute-med`, `bgp-attribute-weight`, `bgp-communities`, `bgp-communities-no-export`, `bgp-as-path-access-list`, `bgp-peer-group`
- Hand-written: IPv6 AF extension

See `Post-ENCOR-Labs-EVE-NG.md` Lab 8.

---

### Block 9 — BGP troubleshooting (Week 24)

**Labs:**
- GNS3vault: `bgp-ebgp-multihop`, `bgp-update-source`, `bgp-md5-authentication`
- Hand-written: 6-bug gauntlet + dual-stack extension

See `Post-ENCOR-Labs-EVE-NG.md` Lab 9.

---

### Block 10 — MPLS L3VPN (Week 25)

**Re-watch if thin:** INE `14` (MPLS mechanics) and `15` (VPN label stack).

**Labs:**
- GNS3vault: `mpls-ldp`, `vrf-lite`, `basic-mpls-vpn`, `mpls-vpn-pe-ce-using-ospf`
- Hand-written: eBGP PE-CE extension, 6VPE stretch, optional Juniper cross-check

See `Post-ENCOR-Labs-EVE-NG.md` Lab 10.

---

### Block 11 — DMVPN + IPsec (Week 26)

**Re-watch if thin:** INE `16` (DMVPN) and `17` (IPsec — specifically Phase 2 and SA establishment).

**Labs:**
- GNS3vault warm-up: `gre-tunnel-basic`, `gre-over-ipsec`, `site-to-site-ipsec-vpn`
- Hand-written: full DMVPN Phase 1→2→3 + IKEv2 protection + MikroTik IPsec cross-check

See `Post-ENCOR-Labs-EVE-NG.md` Lab 11.

---

### Block 12 — Security (Week 27)

**Re-watch if thin:** INE `22-Hardening Cisco IOS` (CoPP sections) and `23-IPv6 First Hop Security`.

**Labs:**
- GNS3vault: `aaa-authentication`, `aaa-command-authorization`, `aaa-exec-authorization`, `standard-access-list`, `extended-access-list`, `reflexive-access-list`, `unicast-reverse-path-forwarding-urpf`, `control-place-policing`, `basic-zone-based-firewall`
- Hand-written: IPv6 FHS (no GNS3vault coverage), production CoPP 4-class extension

See `Post-ENCOR-Labs-EVE-NG.md` Lab 12.

---

### Block 13 — Telemetry and services (Week 27, second half / Week 28)

**Re-watch if thin:** INE `24–27` (whichever was thinnest — SNMP v3 config and Flexible NetFlow are the most common gaps).

**Labs:**
- GNS3vault: `snmpv2-server`, `snmpv3-server`, `syslog-server-logging`, `system-message-logging`, `ntp-network-time-protocol`, `ip-service-level-agreement-sla`, `kron-task-scheduler`
- Hand-written: LibreNMS/ntopng/Prometheus stack, gNMI stretch, cross-VRF DHCP relay

See `Post-ENCOR-Labs-EVE-NG.md` Lab 13.

---

### Block 14 — Design translation (Week 28)

No labs. Structured notebook activity — translate every Cisco-proprietary SD-Access/SD-WAN concept to its vendor-neutral equivalent. See `Post-ENCOR-Labs-EVE-NG.md` Lab 14 for the full exercise.

---

### Block 15 — Automation build (Week 29)

Full week building the `network-automation` Git repo. No video — all hands on keyboard.

See `Post-ENCOR-Labs-EVE-NG.md` Lab 15 for the day-by-day build order.

---

### Block 16 — Capstone (Week 30)

Full ISP/enterprise reference topology deployed by Ansible. The portfolio piece.

See `Post-ENCOR-Labs-EVE-NG.md` Lab 16.

---

## Books (same list as original, kept for reference)

### Tier 1 — Buy these first

| Book | Why |
|---|---|
| **TCP/IP Illustrated Vol. 1** (Fall & Stevens) | Best single reference on how TCP/IP works on the wire. Russian translations available via Питер / Ozon. |
| **Routing TCP/IP Vol. 1 & 2** (Jeff Doyle) | The definitive IGP/BGP conceptual reference. Protocol theory is vendor-neutral. |
| **BGP** (Iljitsch van Beijnum, O'Reilly) | Vendor-neutral, ISP-pragmatic BGP design. Short, dense, worth reading twice. |
| **The Art of Network Architecture** (Russ White & Denise Donohue) | Pure design principles. No CLI. Russian translation by Питер. |
| **Computer Networking Problems and Solutions** (Russ White & Ethan Banks) | Why each protocol exists. Vendor-agnostic. Russian translation by ДМК Пресс. |

### Tier 2 — Automation

| Book | Why |
|---|---|
| **Network Programmability and Automation** (Edelman, Lowe, Oswalt — O'Reilly 2nd ed. 2023) | Canonical vendor-neutral automation book. Python, NETCONF/RESTCONF/gNMI, Git, CI/CD. |
| **Network Automation Cookbook** (Karim Okasha — Packt) | Recipe-style Ansible/NetMiko/NAPALM. |
| **Mastering Python Networking** (Eric Chou — Packt 4th ed.) | Same ground, more code-heavy. |

### Tier 3 — Protocol depth

| Book | Why |
|---|---|
| **MPLS in the SDN Era** (Sanchez Monge & Szarkowicz — O'Reilly) | Cisco + Juniper side-by-side. Best MPLS book of the last decade. |
| **EVPN in the Data Center** (Dinesh Dutt — free PDF from NVIDIA) | Vendor-neutral EVPN-VXLAN introduction. The modern alternative to SD-Access. |
| **Juniper Day One books** (free PDFs from Juniper) | JNCIS-SP track is directly relevant in the Russian/CIS market. |
| **OSPF and IS-IS: Choosing an IGP** (Jeff Doyle) | Definitive IGP design comparison. |

### Tier 4 — Video and online

| Source | What to watch |
|---|---|
| **Ivan Pepelnjak / ipSpace.net** | Gold standard vendor-neutral design. BGP in the Data Center, EVPN Deep Dive, Building Network Automation Solutions. One-month subscription per topic is enough. |
| **Juniper Learning Portal** | Free JNCIA/JNCIS courses. No Russian-bank friction. Highly recommended given Russian market reality. |
| **NRE Labs** (https://labs.networkreliability.engineering/) | Free interactive Ansible/Salt labs. |

---

## What's transferable in Russia (unchanged from original)

**High confidence:** ENARSI routing/troubleshooting, MPLS + L3VPN, DMVPN/IPsec, BGP design, OSPF/IS-IS, IPv6, AAA/TACACS+/RADIUS, NetFlow/IPFIX, SNMPv3, NETCONF/RESTCONF/YANG/gNMI, Python + Nornir + NAPALM, Ansible, FRR, MikroTik, Juniper Junos.

**Deprioritize:** Cisco DNAC, Cisco SD-WAN (Viptela), Meraki, anything tied to Cisco Smart Licensing.

**Best next cert after this plan:**
- Most-employable single move: **Juniper JNCIS-SP** (free study material, exam accessible in CIS-adjacent countries, directly applies to carrier-grade routing).
- Most credential-flexible: **CCNP ENARSI 300-410** (sit at a Pearson VUE center in CIS-adjacent countries).
- Most future-proof: **CKA + Linux Foundation Networking Fundamentals** + FRR/SONiC — Russia's enterprise market is migrating to Linux-on-whitebox.
