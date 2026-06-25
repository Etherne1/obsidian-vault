# Post-ENCOR Labs — EVE-NG (vendor-light)

**Lab source philosophy:** use pre-validated GNS3vault labs (René Molenaar) wherever the topic exists in the archive. Hand-written labs only for topics GNS3vault is too old to cover: DMVPN, IS-IS, BFD, OSPFv3 address-families, named-mode EIGRP, IPv6 FHS, gNMI/telemetry, MikroTik cross-checks, automation, and the capstone.

**GNS3vault repo:** https://github.com/networklessons/labs/tree/main/gns3vault-archive/

**How to use GNS3vault labs:**
1. Open the lab's `.md` file — it contains the scenario, numbered task list, and a YouTube walkthrough link.
2. Build the topology in EVE-NG matching the `.png` diagram.
3. Apply each router's `startup-configs/RN.txt` to get the same starting point.
4. Work through the task list. Don't open `final-configs/` until you've attempted everything.
5. Verify with the YouTube walkthrough. Only then diff your config against `final-configs/`.

**Platform translation:** GNS3vault uses IOS 12.4/15.x on 3640/3725/7200. Your CSR1000v 17.3.04a is upward-compatible for all topics below. Rename `Serial0/0` → `GigabitEthernet1`, `FastEthernet0/0` → `GigabitEthernet1`. L2 features use vIOS-L2 instead of the NM-16ESW switch module.

**Assumed images in EVE-NG:**
- Cisco **vIOS-L2**, **vIOS-L3**, **CSR1000v 17.3.04a**
- **MikroTik CHR** (free from mikrotik.com — boots on x86 EVE-NG)
- **FRR on Alpine/Debian Docker** (for IS-IS and vendor-neutral drills)
- **Juniper vMX** *(optional — everything works without it)*

**Conventions:**
- Loopback0 = `X.X.X.X/32` where X = router number (R1 → 1.1.1.1/32)
- Customer networks: `10.X.0.0/24`. Backbone p2p links: `192.168.0.X/30` slices.
- IPv6: ULA `fd00:X::/64` per router.
- Every lab ends with a **prove it** step and a **destroy and rebuild from notes** step.

---

## Lab 1 — VRF-Lite + BFD (Week 1)

**Coverage split:** VRF-Lite → GNS3vault. BFD → hand-written (not in GNS3vault).

### Part A — VRF-Lite (GNS3vault)

**GNS3vault labs** (`gns3vault-archive/MPLS/`):

| Lab | What to do |
|---|---|
| [vrf-lite](https://github.com/networklessons/labs/tree/main/gns3vault-archive/MPLS/vrf-lite) | Start here. VRF creation, RD, interface assignment, routing separation. |
| [vrf-routing](https://github.com/networklessons/labs/tree/main/gns3vault-archive/MPLS/vrf-routing) | Inter-VRF routing via route leaking. Prove overlapping `10.0.0.0/24` is isolated. |

After both labs work, extend the topology yourself: add a `SHARED` VRF and leak specific routes into both customer VRFs using RT import. This is the creative step — no task list, just your notes.

**MikroTik cross-check (hand-written):** repeat the `vrf-lite` scenario on a CHR (`/ip vrf`, `/routing/ospf/instance`, route-leaking via `/ip route`). Target: same VRF isolation, same overlapping prefix, proven with `/ip route print vrf=CUST_A`.

### Part B — BFD (hand-written)

GNS3vault has no BFD lab. Build this on two CSR1000v routers:

**Tasks:**
1. OSPF adjacency between R1 and R2 on `Gi0/1` (area 0, point-to-point).
2. Enable BFD: `bfd interval 300 min_rx 300 multiplier 3` on both interfaces.
3. Enable BFD under OSPF: `bfd all-interfaces`.
4. Verify: `show bfd neighbors detail` — confirm asynchronous mode, state Up, echo enabled.
5. Simulate failure: `shutdown` the link. Measure convergence time against OSPF-only dead-interval (40 s default) vs BFD (0.9 s). Write the delta in your notes.
6. Repeat for a BGP session: `neighbor X fall-over bfd`. Tear the session, confirm faster detection.

**Prove it:**
- `show bfd neighbors detail` shows `300 ms × 3`, state `Up`.
- After link failure: `show ip ospf neighbor` shows neighbor gone within 1 second.

---

## Lab 2 — Route Manipulation & PBR (Week 2)

**GNS3vault labs** (`gns3vault-archive/Network Services/` and `gns3vault-archive/BGP/`):

| Lab | What to do |
|---|---|
| [policy-based-routing](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Network%20Services/policy-based-routing) | Core PBR lab — route-map `match ip address`, `set ip next-hop verify-availability`. |
| [bgp-filtering-extended-access-list](https://github.com/networklessons/labs/tree/main/gns3vault-archive/BGP/bgp-filtering-extended-access-list) | ACL-based BGP filtering — builds the prefix-list mental model. |
| [bgp-as-path-access-list](https://github.com/networklessons/labs/tree/main/gns3vault-archive/BGP/bgp-as-path-access-list) | AS-path regex filtering — directly exam-relevant. |

After the GNS3vault labs, do this **extension** on your own topology (no task list — build from knowledge):

- Six-router partial mesh, 3 ASes, eBGP between them. Apply a route-map outbound that: denies RFC1918 (`prefix-list BLOCK_RFC1918`), sets local-pref 200 on a specific prefix. Verify with `show ip bgp neighbors X advertised-routes`.
- PBR: from a stub LAN, route TCP/80 out a backup link, all other traffic via primary. Add `track 1` on the primary — PBR falls back when the primary is down.

**MikroTik cross-check:** same PBR on a CHR via `/ip firewall mangle` (mark-routing) + `/ip route rule`. Compare the conceptual flow in your notes — note where Cisco and MikroTik differ in *where* the policy sits.

---

## Lab 3 — Route Redistribution (Week 3)

**GNS3vault does not have a dedicated redistribution lab** but the building blocks are there. Use this sequence:

| Lab | Purpose |
|---|---|
| [ospf-single-area](https://github.com/networklessons/labs/tree/main/gns3vault-archive/OSPF/ospf-single-area) | Baseline OSPF — use as left side of the redistribution topology. |
| [eigrp-basic](https://github.com/networklessons/labs/tree/main/gns3vault-archive/EIGRP/eigrp-basic) | Baseline EIGRP — use as right side. |

Then hand-build the redistribution scenario:

**Topology:** 4 vIOS-L3 in a square. Left edge OSPF area 0, right edge EIGRP AS 100. Two middle routers are the ASBRs doing mutual redistribution.

**Tasks:**
1. Redistribute OSPF into EIGRP and EIGRP into OSPF on both ASBRs — no filtering first. Observe the routing table. Identify the loop or suboptimal path with `traceroute`.
2. Fix with route tags: `tag 100` when redistributing OSPF→EIGRP, `tag 200` when redistributing EIGRP→OSPF. Add `route-map DENY_RETURNING_TAG` on each ASBR to block re-injection.
3. Verify correct metric-type (`metric-type 1` vs `2`) and seed metrics.
4. **Optional IS-IS variant:** swap EIGRP for IS-IS on FRR containers — proves the technique is vendor-neutral. The tag mechanism is identical.

**Prove it:**
- `show ip route ospf` on the EIGRP side shows E2 routes with tag 200.
- No "ghost" EIGRP routes originating via the OSPF path in `show ip route eigrp`.
- Shut a redistribution link — no microloop during convergence.

**Admin-distance comparison table (write in your notes):**

| Protocol | Cisco AD | MikroTik distance | Juniper preference |
|---|---|---|---|
| Connected | 0 | 0 | 0 |
| Static | 1 | 1 | 5 |
| eBGP | 20 | 20 | 170 |
| OSPF intra | 110 | 110 | 10 |
| IS-IS L1 | 115 | 115 | 15 |
| EIGRP internal | 90 | — | — |
| iBGP | 200 | 200 | 170 |

---

## Lab 4 — EIGRP Deep: Named-Mode (Week 4)

**GNS3vault labs** (`gns3vault-archive/EIGRP/`) — start with these three before the named-mode extension:

| Lab | What to do |
|---|---|
| [eigrp-basic](https://github.com/networklessons/labs/tree/main/gns3vault-archive/EIGRP/eigrp-basic) | Classic-mode EIGRP baseline, DUAL verification. |
| [eigrp-authentication](https://github.com/networklessons/labs/tree/main/gns3vault-archive/EIGRP/eigrp-authentication) | MD5 key-chain — foundation before SHA upgrade. |
| [eigrp-summarization](https://github.com/networklessons/labs/tree/main/gns3vault-archive/EIGRP/eigrp-summarization) | Summarization, auto-summary disable, discard route. |

**Named-mode extension (hand-written):** GNS3vault uses classic-mode EIGRP. Rebuild the `eigrp-basic` topology in named-mode:

**Tasks:**
1. Convert to named-mode: `router eigrp LAB4` / `address-family ipv4 unicast autonomous-system 100` / `address-family ipv6 unicast autonomous-system 100`. Both AFs on a single adjacency.
2. Replace MD5 authentication with SHA-256 key-chain (requires named-mode). Rotate one key mid-lab — confirm zero downtime.
3. Configure stub routing on spokes: `eigrp stub connected summary`. Verify the hub receives only stub-type queries.
4. **SIA scenario:** on a stub spoke, remove a route from the RIB manually with a `null0` static. On the active querying router, observe `show ip eigrp topology active`. Fix with proper stub configuration.

**Prove it:**
- `show eigrp protocols` shows named-mode, AF IPv4 + IPv6.
- `show ip eigrp neighbors detail` shows SHA-256 auth mode.
- `show ip eigrp topology active` is clean after the fix.

---

## Lab 5 — OSPFv3 with Address Families (Week 5)

**GNS3vault labs** (`gns3vault-archive/OSPF/`) — complete these first as the OSPFv2 foundation:

| Lab | What to do |
|---|---|
| [ospf-authentication](https://github.com/networklessons/labs/tree/main/gns3vault-archive/OSPF/ospf-authentication) | Authentication mechanics — key-chain concept. |
| [ospf-nssa-not-so-stubby-area](https://github.com/networklessons/labs/tree/main/gns3vault-archive/OSPF/ospf-nssa-not-so-stubby-area) | NSSA Type-7/Type-5 conversion — needed in the OSPFv3 lab too. |
| [ospf-virtual-link](https://github.com/networklessons/labs/tree/main/gns3vault-archive/OSPF/ospf-virtual-link) | Virtual link mechanics — you'll recreate the problem in OSPFv3 too. |

**OSPFv3 address-family lab (hand-written):** GNS3vault has no OSPFv3 AF lab.

**Topology:** 3 CSR1000v in a triangle, dual-stack (`10.X.0.0/24` + `fd00:X::/64`).

**Tasks:**
1. Single OSPFv3 process with two AFs: `address-family ipv4 unicast` and `address-family ipv6 unicast`. One adjacency carries both.
2. SHA-256 key-chain authentication (mandatory for OSPFv3 in modern IOS — no plain-text option).
3. Build a NSSA on one router: ASBR injects an external static; ABR translates Type-7 → Type-5. Verify with `show ospfv3 ipv4 database nssa-external` vs `show ospfv3 ipv4 database external`.
4. **FRR cross-check:** same triangle on FRR Docker containers. Confirm identical LSA behavior — write a one-paragraph diff in your notes on any CLI differences.

**Prove it:**
- `show ospfv3 ipv4 neighbor` and `show ospfv3 ipv6 neighbor` both show Full.
- Both AFs show correct routes in `show ip route ospf` and `show ipv6 route ospf`.

---

## Lab 6 — OSPF Network Types, Areas, Virtual Links (Week 6)

**GNS3vault labs** (`gns3vault-archive/OSPF/`) — these cover the full week:

| Lab | What to do |
|---|---|
| [ospf-stub-area](https://github.com/networklessons/labs/tree/main/gns3vault-archive/OSPF/ospf-stub-area) | Stub area — LSA type 5 filtered. |
| [ospf-totally-stub](https://github.com/networklessons/labs/tree/main/gns3vault-archive/OSPF/ospf-totally-stub) | Totally stubby — type 3 + 5 filtered. |
| [ospf-nssa-not-so-stubby-area](https://github.com/networklessons/labs/tree/main/gns3vault-archive/OSPF/ospf-nssa-not-so-stubby-area) | NSSA — Type-7 to Type-5 conversion. |
| [ospf-totally-nssa](https://github.com/networklessons/labs/tree/main/gns3vault-archive/OSPF/ospf-totally-nssa) | Totally NSSA — all filtering combined. |
| [ospf-virtual-link](https://github.com/networklessons/labs/tree/main/gns3vault-archive/OSPF/ospf-virtual-link) | Virtual link — connect discontiguous area-0. |
| [ospf-lsa-type-3-summarization](https://github.com/networklessons/labs/tree/main/gns3vault-archive/OSPF/ospf-lsa-type-3-summarization) | ABR summarization. |
| [ospf-lsa-type-5-summarization](https://github.com/networklessons/labs/tree/main/gns3vault-archive/OSPF/ospf-lsa-type-5-summarization) | ASBR summarization. |

**Network types extension (hand-written):** GNS3vault Frame-Relay network-type labs are obsolete. Build this instead on vIOS-L3:

**Topology:** 1 hub + 3 spokes using sub-interfaces to simulate NBMA. Apply each network type in sequence and note the DR/BDR behavior:

| Network type | DR elected? | Hello/dead | When to use |
|---|---|---|---|
| broadcast | yes | 10/40 | Ethernet |
| non-broadcast | yes | 30/120 | legacy NBMA |
| point-to-multipoint | no | 30/120 | hub-spoke, no DR needed |
| point-to-point | no | 10/40 | any p2p link |

Then redesign the virtual-link topology from `ospf-virtual-link` to **remove** the virtual link entirely — add a physical or GRE link between the disconnected ABR and area-0. Write in your notes why virtual links are an anti-pattern in production.

---

## Lab 7 — IS-IS L1/L2 Backbone (Week 7)

**GNS3vault coverage:** minimal. The archive has no usable IS-IS labs for modern IOS. Use FRR containers instead — IS-IS is identical conceptually across vendors.

**Topology:** 4 FRR Docker containers + 2 vIOS-L3. Two areas: `49.0001` (L1 only) and `49.0002` (backbone L2). Two L1/L2 routers bridge the areas.

**Tasks:**
1. Configure NETs on all routers (`49.0001.0000.0000.0001.00` etc.). Enable `router isis` on FRR (`net 49.0001.0000.0000.0001.00`), `router isis` on IOS.
2. Set router types: two routers `is-type level-1`, two `is-type level-2-only`, two `is-type level-1-2`.
3. Use **wide metrics** everywhere: `metric-style wide` on IOS, `metric-style wide` on FRR.
4. SHA-256 hello authentication: `authentication key-chain IS-IS-AUTH` on IOS; `isis authentication mode md5` on FRR (FRR supports MD5 only — note the gap).
5. Leak selected L2 → L1 routes with a route-map on the L1/L2 routers.
6. Insert a deliberate metric tie; resolve with manual metric tuning.

**Prove it:**
- `show isis neighbors` shows expected L1/L2 adjacencies.
- `show isis database detail` shows TLV-extended IS reachability (wide metrics active).
- An L1-only router's `show ip route isis` shows only intra-area routes + a default toward the L1/L2 router.

**Notes to write:** compare IS-IS area model vs OSPF area model — specifically why IS-IS has no Area-0 constraint and why that matters for large flat backbones.

---

## Lab 8 — BGP Address Families, RR, and Filtering (Week 8)

**GNS3vault labs** (`gns3vault-archive/BGP/`) — this is where the archive is richest. Do these in order:

| Lab | What to do |
|---|---|
| [bgp-basic](https://github.com/networklessons/labs/tree/main/gns3vault-archive/BGP/bgp-basic) | Baseline — session, network advertisement. |
| [ibgp-internal-bgp](https://github.com/networklessons/labs/tree/main/gns3vault-archive/BGP/ibgp-internal-bgp) | iBGP full mesh, next-hop behavior. |
| [bgp-next-hop-self](https://github.com/networklessons/labs/tree/main/gns3vault-archive/BGP/bgp-next-hop-self) | `next-hop-self` — why it's needed on iBGP. |
| [bgp-route-reflectors](https://github.com/networklessons/labs/tree/main/gns3vault-archive/BGP/bgp-route-reflectors) | RR + client config. Verify cluster-list and originator-id. |
| [bgp-attribute-local-preference](https://github.com/networklessons/labs/tree/main/gns3vault-archive/BGP/bgp-attribute-local-preference) | LOCAL_PREF — AS-wide scope. |
| [bgp-attribute-as-path](https://github.com/networklessons/labs/tree/main/gns3vault-archive/BGP/bgp-attribute-as-path) | AS-path prepending — influence inbound. |
| [bgp-attribute-med](https://github.com/networklessons/labs/tree/main/gns3vault-archive/BGP/bgp-attribute-med) | MED — inter-AS metric. |
| [bgp-attribute-weight](https://github.com/networklessons/labs/tree/main/gns3vault-archive/BGP/bgp-attribute-weight) | Weight — local-router only. |
| [bgp-communities](https://github.com/networklessons/labs/tree/main/gns3vault-archive/BGP/bgp-communities) | Standard communities. |
| [bgp-communities-no-export](https://github.com/networklessons/labs/tree/main/gns3vault-archive/BGP/bgp-communities-no-export) | `no-export` well-known community. |
| [bgp-as-path-access-list](https://github.com/networklessons/labs/tree/main/gns3vault-archive/BGP/bgp-as-path-access-list) | AS-path regex — block private AS leakage. |
| [bgp-peer-group](https://github.com/networklessons/labs/tree/main/gns3vault-archive/BGP/bgp-peer-group) | Peer groups — operational efficiency. |

**IPv6 AF extension (hand-written):** GNS3vault BGP labs are IPv4 only. After the labs above, extend your topology:

Add `address-family ipv6 unicast` to every peer. Use link-local next-hops where applicable (`neighbor X update-source GigE0/1`). Confirm with `show bgp ipv6 unicast summary`.

---

## Lab 9 — BGP Troubleshooting Gauntlet (Week 9)

**GNS3vault labs** — use these as a foundation, then build the gauntlet yourself:

| Lab | Purpose |
|---|---|
| [bgp-ebgp-multihop](https://github.com/networklessons/labs/tree/main/gns3vault-archive/BGP/bgp-ebgp-multihop) | Covers the multihop failure mode. |
| [bgp-update-source](https://github.com/networklessons/labs/tree/main/gns3vault-archive/BGP/bgp-update-source) | Covers the update-source mismatch failure mode. |
| [bgp-md5-authentication](https://github.com/networklessons/labs/tree/main/gns3vault-archive/BGP/bgp-md5-authentication) | Covers the MD5 mismatch failure mode. |

**Gauntlet (hand-written extension):** take the Lab 8 topology and break it six ways, one at a time. For each bug, predict the BGP state machine state first, then diagnose:

1. TCP/179 blocked by an ACL on a transit router → session stuck in `Active`.
2. eBGP multihop missing for loopback-sourced peering → `Idle`.
3. `update-source Loopback0` set on one side only → `Active`.
4. MD5 password mismatch → `Active` (TCP connects, Open rejected).
5. iBGP RR client missing `next-hop-self` → session `Established` but routes black-holed.
6. Outbound route-map silently denying a prefix → session up, prefix missing from peer's table.

For each: write the minimum diagnostic command sequence in your notes. These become your BGP troubleshooting runbook.

---

## Lab 10 — MPLS L3VPN End-to-End (Week 10)

**GNS3vault labs** (`gns3vault-archive/MPLS/`) — the archive has solid MPLS coverage:

| Lab | What to do |
|---|---|
| [mpls-ldp](https://github.com/networklessons/labs/tree/main/gns3vault-archive/MPLS/mpls-ldp) | LDP basics — label distribution, LFIB, FIB. Verify with `show mpls ldp neighbor` and `show mpls forwarding-table`. |
| [vrf-lite](https://github.com/networklessons/labs/tree/main/gns3vault-archive/MPLS/vrf-lite) | VRF review if needed — RD/RT mechanics. |
| [basic-mpls-vpn](https://github.com/networklessons/labs/tree/main/gns3vault-archive/MPLS/basic-mpls-vpn) | Core L3VPN lab — PE-CE with static routes, MP-BGP VPNv4, RT import/export. |
| [mpls-vpn-pe-ce-using-ospf](https://github.com/networklessons/labs/tree/main/gns3vault-archive/MPLS/mpls-vpn-pe-ce-using-ospf) | PE-CE using OSPF — important for the capstone. |

**Extensions (hand-written):** after the GNS3vault labs:

1. Add a second VRF (`VPN_BLUE`) with eBGP as PE-CE protocol instead of OSPF. This gives you both PE-CE options in one topology — important for the Week 16 capstone.
2. Prove route isolation: overlap `10.0.0.0/24` in both VRFs. Verify `show bgp vpnv4 unicast all` shows distinct RDs.
3. **Stretch — 6VPE:** add `address-family vpnv6 unicast` to both PEs. Configure IPv6 CEs. Verify `show bgp vpnv6 unicast all`.
4. **Juniper cross-check (optional):** swap one PE for vMX. CLI differs; RSVP-TE vs LDP differs; L3VPN concept is identical. Write a one-page CLI comparison in your notes.

**Prove it:**
- `show bgp vpnv4 unicast all summary` — both PE peers Established.
- `show mpls forwarding-table` on a P router shows label stacks.
- `traceroute vrf VPN_RED 10.0.0.X` from a CE shows MPLS labels in output.

---

## Lab 11 — DMVPN + IPsec (Week 11)

**GNS3vault coverage:** the archive has GRE and IPsec labs but no DMVPN. Use the GNS3vault IPsec labs as a warm-up, then build DMVPN from scratch.

**GNS3vault warm-up** (`gns3vault-archive/Tunneling/`):

| Lab | What to do |
|---|---|
| [gre-tunnel-basic](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Tunneling/gre-tunnel-basic) | GRE recap — mGRE is an extension of this. |
| [gre-over-ipsec](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Tunneling/gre-over-ipsec) | GRE+IPsec mechanics — the template for DMVPN phase 1. |
| [site-to-site-ipsec-vpn](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Tunneling/site-to-site-ipsec-vpn) | Plain IPsec — verify IKEv1 SA establishment before upgrading to IKEv2. |

**DMVPN hand-written lab:**

**Topology:** 1 hub CSR1000v + 3 spoke CSR1000v. Add 1 MikroTik CHR as a 4th spoke for Phase 1 only (MikroTik DMVPN interop is IKEv1/Phase-1 only; document the limitation).

**Tasks:**
1. **Phase 1** — hub-and-spoke only. Hub: `tunnel mode gre multipoint`, NHRP NHS config. Spokes: `tunnel destination` = hub, NHRP NHS = hub. EIGRP across tunnel with `no ip split-horizon eigrp` on hub.
2. **Phase 2** — enable NHRP redirect on hub, NHRP shortcut on spokes. Verify spoke-to-spoke tunnel formation with `show dmvpn detail`.
3. **Phase 3** — NHRP shortcut + redirect on hub. Spoke-to-spoke triggered by hub redirect. Confirm traffic bypasses hub: capture on hub's transport interface during a spoke-to-spoke ping — hub should see only the NHRP redirect, not the data traffic.
4. **IPsec protection:** IKEv2 + PSK over the mGRE tunnels. Later, replace PSK with certificates (Alpine VM as CA if available).
5. **MikroTik cross-check:** site-to-site IPsec between two CHRs — IKEv2, AES-256-GCM, SHA-256, DH group 19. No DMVPN — just the IPsec part. Compare the conceptual IKE phase flow with your Cisco notes.

**Prove it:**
- `show ip nhrp` on a spoke shows dynamic entries for other spokes (Phase 2/3).
- `show crypto ikev2 sa detailed` shows active IKEv2 SAs.
- Packet capture on hub's WAN interface: spoke-to-spoke traffic absent after Phase 3 shortcut established.

---

## Lab 12 — Infrastructure Security (Week 12)

**GNS3vault labs** (`gns3vault-archive/Security/`) — the archive is strong here:

| Lab | What to do |
|---|---|
| [aaa-authentication](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Security/aaa-authentication) | RADIUS + TACACS+ auth, local fallback. |
| [aaa-command-authorization](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Security/aaa-command-authorization) | Command authorization — two privilege tiers. |
| [aaa-exec-authorization](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Security/aaa-exec-authorization) | EXEC authorization. |
| [standard-access-list](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Security/standard-access-list) | ACL refresher. |
| [extended-access-list](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Security/extended-access-list) | Extended ACL. |
| [reflexive-access-list](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Security/reflexive-access-list) | Reflexive ACL — stateful inspection precursor. |
| [unicast-reverse-path-forwarding-urpf](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Security/unicast-reverse-path-forwarding-urpf) | uRPF strict vs loose mode. |
| [control-place-policing](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Security/control-place-policing) | CoPP — MQC for the control plane. |
| [basic-zone-based-firewall](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Security/basic-zone-based-firewall) | ZBF — zone pairs, policy-map inspect, self zone. |

**IPv6 FHS extension (hand-written):** GNS3vault has no IPv6 FHS lab. Build on vIOS-L2 access segment:

**Tasks:**
1. **RA Guard:** `ipv6 nd raguard policy HOST` / `device-role host` on access ports. Send a rogue RA from a host port — blocked. Confirm with `show ipv6 snooping events`.
2. **DHCPv6 Guard:** `ipv6 dhcp guard policy SERVER` / `device-role server` only on the uplink to the DHCPv6 server. Rogue DHCPv6 offer from a host port — dropped.
3. **IPv6 Snooping:** `ipv6 snooping policy SNOOP` applied to the access VLAN. Verify binding table with `show ipv6 neighbors binding`.
4. **IPv6 Source Guard:** `ipv6 source-guard policy SGP` on host ports. Host with spoofed IPv6 source — dropped.

**CoPP extension:** after the GNS3vault CoPP lab, build a production-grade 4-class policy on a CSR1000v:

```
class MANAGEMENT  → SSH, SNMP, NTP, TACACS+ — police 64 kbps
class ROUTING     → OSPF, BGP, EIGRP, LDP hello — police 256 kbps
class UNDESIRABLE → ICMP redirects, IP options, fragments — police 8 kbps
class default     → everything else — police 1 Mbps
```

Verify: send a flood of malformed packets at the management interface — `show policy-map control-plane` shows drops incrementing in the right class.

---

## Lab 13 — Telemetry & Services Stack (Week 13)

**GNS3vault labs** (`gns3vault-archive/Network Management/`) — good coverage of the classical stack:

| Lab | What to do |
|---|---|
| [snmpv2-server](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Network%20Management/snmpv2-server) + [snmpv3-server](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Network%20Management/snmpv3-server) | Both back-to-back. Compare config complexity. |
| [syslog-server-logging](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Network%20Management/syslog-server-logging) + [system-message-logging](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Network%20Management/system-message-logging) | Remote + local logging, severity levels. |
| [ntp-network-time-protocol](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Network%20Management/ntp-network-time-protocol) | NTP server + client, stratum, authentication. |
| [ip-service-level-agreement-sla](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Network%20Services/ip-service-level-agreement-sla) | IP SLA probes. |
| [kron-task-scheduler](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Network%20Management/kron-task-scheduler) | Scheduled config tasks — production-relevant. |

**Modern telemetry extensions (hand-written):** GNS3vault is too old for these. Build on a CSR1000v + Docker VM:

1. **SNMPv3 → LibreNMS:** point LibreNMS (running in Docker) at your CSR1000v. Add all routers. Verify interface graphs appear.
2. **NetFlow v9 → ntopng:** `flow exporter` on the CSR, export to ntopng on port 2055. Add Flexible NetFlow with a custom record (src/dst IP, ports, TOS, BGP next-hop). View top-talkers per AS.
3. **IP SLA + track + failover:** UDP-jitter probe between two routers. `track 1 ip sla 1 reachability`. Floating static conditional on track state. Pull the primary link — verify static route switches within the SLA reaction time.
4. **DHCP relay across VRFs:** DHCPv4 server in VRF `MGMT`, clients in VRF `CUST_A`. `ip helper-address vrf MGMT X.X.X.X global` on the SVI. Confirm leases: `show ip dhcp binding`.
5. **gNMI subscription (stretch):** install `gnmic` on the Docker VM. Subscribe to `/interfaces/interface/state/counters` on the CSR1000v. Pipe output to Prometheus. Build a Grafana panel showing real-time interface counters.

**Prove it:**
- LibreNMS shows traffic graphs from every device.
- ntopng shows top-talkers per AS from NetFlow.
- Pull primary link → IP SLA tracker flaps → `show ip route` shows standby static activated.
- Grafana panel shows live counter increments from gNMI.

---

## Lab 14 — Design Concepts (Week 14)

No new EVE-NG topology. **One structured activity:**

Walk the Library C design modules with a notebook. For each module, draw the reference architecture on paper. Then, for every Cisco-proprietary concept, write its vendor-neutral / open-source equivalent:

| Cisco concept | Vendor-neutral equivalent |
|---|---|
| SD-Access fabric edge | VTEP (EVPN-VXLAN) |
| LISP control-plane node | BGP-EVPN route-reflector |
| DNA Center | OpenConfig + Prometheus + NetBox |
| vSmart (SD-WAN) | Open-source SD-WAN controller (flexiWAN, VyOS-based) |
| DMVPN NHS | WireGuard hub, strongSwan IKEv2 server |
| MP-BGP VPNv4 | Same — it's an IETF standard, all vendors implement it |

This translation exercise is more valuable than any CLI lab this week. Senior engineers think in concepts, not vendor menus.

---

## Lab 15 — Automation Build Week (Week 15)

No video. The entire week is building a working `network-automation` Git repo.

**Repo structure:**
```
network-automation/
  inventory/
    hosts.yaml          # Nornir host inventory
    groups.yaml         # cisco_iosxe, mikrotik_routeros, juniper_junos
  playbooks/
    backup.yaml         # Ansible
    gather_state.py     # Nornir + NAPALM
  scripts/
    netconf_pull.py     # ncclient — pull OSPF neighbors
    restconf_pull.py    # requests — same data via REST
    gnmic_subscribe.sh  # gnmic → Prometheus
  telemetry/
    prometheus.yml
    grafana_dashboards/
  Makefile              # make backup, make report, make telemetry
  README.md
```

**Day-by-day build order:**

| Day | Task |
|---|---|
| 1 | `python3 -m venv .venv`, install: `nornir nornir-napalm nornir-utils nornir-netmiko ansible netmiko napalm ncclient requests pyang gnmic`. First `git commit`. |
| 2 | `make backup` — Nornir+NAPALM pulls running configs from every device. Commits to `configs/` with timestamp. |
| 3 | `make report` — Nornir+NAPALM pulls interface error counters. Outputs a Markdown table. |
| 4 | `netconf_pull.py` — pull `/ietf-routing:routing/control-plane-protocols` from CSR1000v via NETCONF. Pretty-print OSPF neighbor list. |
| 5 | `restconf_pull.py` — same data via RESTCONF + JSON. Write a paragraph in your notes comparing the two API ergonomics. |
| 6 | `gnmic_subscribe.sh` — subscribe to interface counters. Stream to Prometheus. Grafana dashboard. |
| 7 | Ansible role: deploy a new VRF + OSPF process to a Cisco IOS-XE device **and** a MikroTik CHR from the same playbook (vendor-conditional tasks). |

**Prove it:**
- `git log` shows ≥ 30 commits with meaningful messages.
- `make backup && make report` runs end-to-end without manual intervention.
- One playbook successfully touches both a Cisco and a MikroTik device.

---

## Lab 16 — Capstone (Week 16)

**The portfolio piece.** Everything from Labs 1–15 in one topology, deployed by Ansible from your Lab 15 repo.

**Topology:**

```
              [TACACS+ / Prometheus / Grafana / LibreNMS / ntopng]
                                    |
                                [MGMT vSwitch]
                                    |
     +------- AS 65000 (your ISP/enterprise) -------+
     |                                               |
    PE1 ============ MPLS Core (LDP) ============= PE2
    / \                                             / \
CE1A  CE1B (VPN_RED)                         CE2A  CE2B (VPN_BLUE)
                                               |
                                          [DMVPN Phase 3 spoke
                                           over simulated Internet
                                           terminating in VPN_BLUE]
```

**Mandatory checklist:**

- [ ] OSPF in MPLS core, LDP, MP-BGP VPNv4 between PEs
- [ ] VPN_RED: OSPF PE-CE. VPN_BLUE: eBGP PE-CE.
- [ ] Overlapping `10.0.0.0/24` between VRFs — isolation proven
- [ ] DMVPN Phase 3 from one branch into VPN_BLUE on PE2, IKEv2 protection
- [ ] AAA via TACACS+ on every router, two privilege tiers
- [ ] CoPP 4-class policy on every router
- [ ] IPv6 dual-stack everywhere: BGP IPv6 AF, OSPFv3, IPv6 ACLs, IPv6 FHS on access segment
- [ ] NetFlow v9 → ntopng; SNMPv3 → LibreNMS; gNMI → Prometheus
- [ ] IP SLA monitoring critical paths with track-based floating statics
- [ ] Ansible playbook deploys the whole topology from the Git repo
- [ ] One-page network diagram (draw.io / Excalidraw / mermaid) committed to the repo
- [ ] README: explains how to rebuild from scratch in under 30 minutes

**Self-test:**
- Run the CCNP ENARSI 300-410 practice exam (Pearson Test Prep). Target ≥ 85% on the 200-question pool.
- Hand the repo to someone else. They `git clone` + `make all` and rebuild your topology without asking you anything.

---

## GNS3vault coverage summary

| Post-ENCOR topic | GNS3vault coverage | Hand-written supplement |
|---|---|---|
| VRF-Lite | Full (`vrf-lite`, `vrf-routing`) | Route-leaking extension, MikroTik cross-check |
| BFD | None | Full hand-written |
| Route manipulation / PBR | Partial (`policy-based-routing`, BGP filtering) | Multi-AS extension, MikroTik cross-check |
| Redistribution | Indirect (OSPF + EIGRP baselines) | Full redistribution + loop-prevention hand-written |
| EIGRP classic | Full (23 labs in archive) | Named-mode extension hand-written |
| OSPFv3 AFs | None | Full hand-written |
| OSPF areas / network types | Full (12+ labs) | Modern network-type extension (no FR) |
| IS-IS | None (containerlab topology only) | Full hand-written on FRR |
| BGP deep | Excellent (50+ labs) | IPv6 AF extension hand-written |
| BGP troubleshooting | Partial (3 failure-mode labs) | Gauntlet hand-written |
| MPLS L3VPN | Good (`mpls-ldp`, `basic-mpls-vpn`, PE-CE OSPF) | eBGP PE-CE + 6VPE extension, Juniper cross-check |
| DMVPN | None | Full hand-written (GRE/IPsec warm-up from archive) |
| Infrastructure security | Excellent (AAA, ACL, CoPP, ZBF) | IPv6 FHS extension, production CoPP extension |
| SNMP / NTP / syslog / IP SLA | Full | — |
| Modern telemetry (gNMI / Grafana) | None | Full hand-written |
| Automation repo | None | Full hand-written |
| Capstone | None | Full hand-written |
