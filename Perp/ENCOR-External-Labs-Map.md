# External Labs Map — NetworkLessons / containerlab

**Source:** [networklessons/labs](https://github.com/networklessons/labs) — containerlab labs by René Molenaar (NetworkLessons.com).

**Why this file exists:** The hand-written labs in `ENCOR-Labs-EVE-NG.md` are good for modern IOS-XE features (NETCONF, RESTCONF, EEM, telemetry, VXLAN, LISP) but I have made placement mistakes in classic L2/L3 protocol labs (Lab I1 took multiple iterations). For OSPF / BGP / EIGRP / IS-IS / MPLS / HSRP / VXLAN, this external repo gives you **pre-validated, modern, vendor-correct topologies**. We use them for the protocol fundamentals and reserve hand-written labs for the modern/automation gap.

**Validation status:** The NetworkLessons repo is actively maintained (2024+), uses real Cisco IOL 17.12.01 (IOS-XE codebase, same as your CSR1000v 17.3.04a — almost identical CLI), and is built by René Molenaar, who has been writing CCIE labs for ~15 years. Treat configs in this repo as ground truth; if my hand-written lab and the external lab disagree, trust the external one.

---

## Platform translation

| External lab uses | Your image | Notes |
|---|---|---|
| `cisco_iol:17.12.01` (Cisco IOS-XE on Linux) | **CSR1000v 17.3.04a** | Same OS family. Configs paste in nearly verbatim. Differences: `Ethernet0/1` → `GigabitEthernet1` on CSR; some `show platform` output differs (harmless for exam). |
| `arista_ceos`, `frr`, `fortinet`, `vyos` | **Skip** for ENCOR | Use only if you want a vendor-comparison side lab; not required for the exam. |
| Cisco NX-OS containers (VXLAN labs) | **Adapt** to CSR1000v | NX-OS VXLAN syntax differs from IOS-XE. I'll rewrite these to IOS-XE for you when you reach them. |
| Cisco ASA | **Skip** for ENCOR | ASA is CCNP Security territory, not ENCOR. |
| ONOS, OpenDaylight | **Skip** | Conceptually interesting but outside ENCOR blueprint. |

**How to use a containerlab lab without containerlab:**
1. Open the lab's `*.clab.yml` to see the topology (nodes + links).
2. Open each node's `startup-config.cfg.partial` — that's the Cisco config to paste into your CSR1000v on EVE-NG.
3. Wire your EVE-NG nodes to match the YAML `links:` section.
4. Paste each config into the matching node, then verify with the lab's `show` commands (usually documented in the matching NetworkLessons.com article — the lab folder name maps to an article URL like `https://networklessons.com/cisco/ccnp-encor/<lab-name>`).

---

## Lab-by-lab mapping to your ENCOR study plan

Below: every Cisco/router-flavored lab in the external repo, mapped to the ENCOR week and what it teaches. Status: **USE** = drop-in for your study plan / **ADAPT** = small CLI tweaks needed (I'll do them on demand) / **SKIP** = not in ENCOR scope.

### Week 1–2 — L2 (STP / VLAN / EtherChannel)

| External lab | Status | Notes |
|---|---|---|
| *(none — the external repo has no STP/VLAN/EtherChannel labs)* | — | **Gap.** Use Jeremy's IT Lab (free YouTube, CCNA-level but topology-correct), Cisco Packet Tracer ENCOR sample labs, or my hand-written Lab I1 (now Validated). |

**Verdict for L2:** External repo doesn't cover. Keep hand-written Lab I1 (Validated) + Lab I2 (I'll validate when you reach it).

---

### Week 3 — OSPF basics

| External lab | Status | Maps to objective |
|---|---|---|
| [ospf-basic-configuration](https://github.com/networklessons/labs/tree/main/containerlab/labs/ospf/cisco/ospf-basic-configuration) | **USE** | 3-router triangle, single area, MD5 auth. Replaces my Lab I3 Phase 1. |
| [ospf-three-routers-triangle](https://github.com/networklessons/labs/tree/main/containerlab/labs/ospf/cisco/ospf-three-routers-triangle) | **USE** | Same topology, simpler config. Good for first day. |
| [ospf-dr-bdr-two-routers](https://github.com/networklessons/labs/tree/main/containerlab/labs/ospf/cisco/ospf-dr-bdr-two-routers) | **USE** | DR/BDR election mechanics. |
| [ospf-dr-bdr-three-routers](https://github.com/networklessons/labs/tree/main/containerlab/labs/ospf/cisco/ospf-dr-bdr-three-routers) | **USE** | DR/BDR with 3 routers — see how election handles ties. |
| [ospf-md5-authentication](https://github.com/networklessons/labs/tree/main/containerlab/labs/ospf/cisco/ospf-md5-authentication) | **USE** | OSPF authentication. |
| [ospf-six-routers-in-a-row](https://github.com/networklessons/labs/tree/main/containerlab/labs/ospf/cisco/ospf-six-routers-in-a-row) | **USE** | Reference bandwidth, cost, path selection. |
| [ospf-four-routers-square](https://github.com/networklessons/labs/tree/main/containerlab/labs/ospf/cisco/ospf-four-routers-square) | **USE** | ECMP and tiebreakers. |

### Week 4 — OSPF advanced

| External lab | Status | Maps to objective |
|---|---|---|
| [ospf-area-0-1-2](https://github.com/networklessons/labs/tree/main/containerlab/labs/ospf/cisco/ospf-area-0-1-2) | **USE** | Multi-area OSPF (ABR behavior). |
| [ospf-area-0-1-2-external](https://github.com/networklessons/labs/tree/main/containerlab/labs/ospf/cisco/ospf-area-0-1-2-external) | **USE** | LSA type 5 (external) propagation. |
| [ospf-area-0-1-2-external-nssa](https://github.com/networklessons/labs/tree/main/containerlab/labs/ospf/cisco/ospf-area-0-1-2-external-nssa) | **USE** | NSSA / LSA type 7 — exam favorite. |
| [ospf-virtual-link](https://github.com/networklessons/labs/tree/main/containerlab/labs/ospf/cisco/ospf-virtual-link) | **USE** | Virtual links — uncommon but exam-eligible. |
| [ospf-network-type-broadcast](https://github.com/networklessons/labs/tree/main/containerlab/labs/ospf/cisco/ospf-network-type-broadcast) | **USE** | All 6 network type labs (p2p, p2mp, p2mp-nb, broadcast, non-broadcast, loopback) — exam tests these constantly. |
| [ospf-network-type-p2p](https://github.com/networklessons/labs/tree/main/containerlab/labs/ospf/cisco/ospf-network-type-p2p) | **USE** | Same. |
| [ospf-network-type-non-broadcast](https://github.com/networklessons/labs/tree/main/containerlab/labs/ospf/cisco/ospf-network-type-non-broadcast) | **USE** | Same. |
| [ospf-network-type-p2mp](https://github.com/networklessons/labs/tree/main/containerlab/labs/ospf/cisco/ospf-network-type-p2mp) | **USE** | Same. |
| [ospf-network-type-p2mp-non-broadcast](https://github.com/networklessons/labs/tree/main/containerlab/labs/ospf/cisco/ospf-network-type-p2mp-non-broadcast) | **USE** | Same. |
| [ospf-network-type-loopback](https://github.com/networklessons/labs/tree/main/containerlab/labs/ospf/cisco/ospf-network-type-loopback) | **USE** | Same. |
| [ospf-vrf-three-routers](https://github.com/networklessons/labs/tree/main/containerlab/labs/ospf/cisco/ospf-vrf-three-routers) | **USE** | OSPF inside a VRF — pairs with VRF-Lite lab (Lab V1). |
| [ospf-lsa-type-10-mpls-te](https://github.com/networklessons/labs/tree/main/containerlab/labs/ospf/cisco/ospf-lsa-type-10-mpls-te) | **SKIP** | MPLS-TE is Post-ENCOR territory. |

**Verdict for OSPF:** External repo replaces my Lab I3 entirely. ~12 high-quality OSPF labs, covers blueprint completely.

---

### Week 5 — BGP

| External lab | Status | Maps to objective |
|---|---|---|
| [ebgp-two-routers](https://github.com/networklessons/labs/tree/main/containerlab/labs/bgp/cisco/ebgp-two-routers) | **USE** | First BGP lab — eBGP basics. |
| [bgp-ebgp-two-routers](https://github.com/networklessons/labs/tree/main/containerlab/labs/bgp/cisco/bgp-ebgp-two-routers) | **USE** | Duplicate of above with slight variation; do one. |
| [ibgp-two-routers](https://github.com/networklessons/labs/tree/main/containerlab/labs/bgp/cisco/ibgp-two-routers) | **USE** | iBGP, next-hop-self requirement. |
| [bgp-ebgp-multihop-loopbacks](https://github.com/networklessons/labs/tree/main/containerlab/labs/bgp/cisco/bgp-ebgp-multihop-loopbacks) | **USE** | eBGP multihop + loopback peering — exam favorite. |
| [bgp-ebgp-multihop-not-directly-connected](https://github.com/networklessons/labs/tree/main/containerlab/labs/bgp/cisco/bgp-ebgp-multihop-not-directly-connected) | **USE** | Same concept, second scenario. |
| [bgp-as-path-prepend](https://github.com/networklessons/labs/tree/main/containerlab/labs/bgp/cisco/bgp-as-path-prepend) | **USE** | AS-path manipulation — best-path selection mechanics. |
| [bgp-route-reflector](https://github.com/networklessons/labs/tree/main/containerlab/labs/bgp/cisco/bgp-route-reflector) | **USE** | RR — scales iBGP, exam topic. |

**Verdict for BGP:** External repo replaces my Lab I4 entirely. 6 solid BGP labs. Gaps in this repo: no community/local-pref labs — for those use my Lab I4 Phase 2 (which I'll validate when you reach it) or the corresponding NetworkLessons.com articles which cover them in the OCG-equivalent depth.

---

### Week 6 — EIGRP + IS-IS

| External lab | Status | Maps to objective |
|---|---|---|
| [eigrp-authentication-hmac-sha-256](https://github.com/networklessons/labs/tree/main/containerlab/labs/eigrp/eigrp-authentication-hmac-sha-256) | **USE** | Only EIGRP lab in the repo. ENCOR has EIGRP at survey-level only, so this single lab + OCG reading is enough. |
| [is-is-single-area-two-routers](https://github.com/networklessons/labs/tree/main/containerlab/labs/isis/cisco/is-is-single-area-two-routers) | **USE** | IS-IS basics. ENCOR doesn't test IS-IS, but seeing it once helps the "compare IGPs" exam questions. |

**Verdict:** Light coverage matches light ENCOR weight on these.

---

### Week 7 — First-hop redundancy (HSRP / VRRP)

| External lab | Status | Maps to objective |
|---|---|---|
| [hsrp-basic-ip-only](https://github.com/networklessons/labs/tree/main/containerlab/labs/ip-services/hsrp-basic-ip-only) | **USE** | Simplest HSRP. Start here. |
| [hsrp-basic-single-group](https://github.com/networklessons/labs/tree/main/containerlab/labs/ip-services/hsrp-basic-single-group) | **USE** | Single-group HSRP with virtual IP. |
| [hsrp-preemption](https://github.com/networklessons/labs/tree/main/containerlab/labs/ip-services/hsrp-preemption) | **USE** | Preemption — exam topic. |
| [hsrp-timers](https://github.com/networklessons/labs/tree/main/containerlab/labs/ip-services/hsrp-timers) | **USE** | Hello/hold timers. |
| [hsrp-authentication-md5-key-string](https://github.com/networklessons/labs/tree/main/containerlab/labs/ip-services/hsrp-authentication-md5-key-string) | **USE** | HSRP authentication. |
| [hsrp-authentication-md5-keychain](https://github.com/networklessons/labs/tree/main/containerlab/labs/ip-services/hsrp-authentication-md5-keychain) | **USE** | Authentication with keychain. |
| [hsrp-authentication-plain-text](https://github.com/networklessons/labs/tree/main/containerlab/labs/ip-services/hsrp-authentication-plain-text) | **USE** | (Why you shouldn't use it — comparison.) |
| [hsrp-object-tracking-interface](https://github.com/networklessons/labs/tree/main/containerlab/labs/ip-services/hsrp-object-tracking-interface) | **USE** | Interface tracking — exam favorite. |
| [hsrp-object-tracking-ip-sla](https://github.com/networklessons/labs/tree/main/containerlab/labs/ip-services/hsrp-object-tracking-ip-sla) | **USE** | IP SLA tracking — exam favorite. |
| [hsrp-bfd-peering](https://github.com/networklessons/labs/tree/main/containerlab/labs/ip-services/hsrp-bfd-peering) | **USE** | HSRP + BFD for sub-second failover. |
| [hsrp-ipv6](https://github.com/networklessons/labs/tree/main/containerlab/labs/ip-services/hsrp-ipv6) | **USE** | HSRPv2 on IPv6. |

**Verdict for HSRP:** External repo replaces my Lab I5 entirely. 11 deep HSRP labs. No VRRP labs in the external repo — VRRP is similar enough to HSRP that one OCG reading + one quick hand-typed VRRP config covers it.

---

### Week 8 — Multicast

| External lab | Status | Maps to objective |
|---|---|---|
| *(no native multicast lab)* | — | **Gap.** Keep hand-written Lab I6 (PIM-SM with static RP) — when you reach it, ask me to validate it. |

---

### Week 9 — Wireless

| External lab | Status | Maps to objective |
|---|---|---|
| *(no wireless lab)* | — | Use Cisco DevNet Always-On 9800 sandbox. No emulation option. |

---

### Week 10 — Overlay tunnels & VPNs (VRF-Lite, GRE, IPsec, DMVPN)

| External lab | Status | Maps to objective |
|---|---|---|
| [ospf-vrf-three-routers](https://github.com/networklessons/labs/tree/main/containerlab/labs/ospf/cisco/ospf-vrf-three-routers) | **USE** | OSPF in a VRF. Closest external lab to VRF-Lite. Combine with my Lab V1 for inter-VRF routing. |
| [mpls-ldp-two-routers](https://github.com/networklessons/labs/tree/main/containerlab/labs/mpls/mpls-ldp-two-routers) | **USE** | MPLS LDP basics. Not heavy on ENCOR but referenced. |
| [mpls-vpn-pe-ce-ospf](https://github.com/networklessons/labs/tree/main/containerlab/labs/mpls/mpls-vpn-pe-ce-ospf) | **POST-ENCOR** | MPLS L3VPN is Post-ENCOR territory, but worth peeking ahead. |
| *(no GRE-over-IPsec lab)* | — | Keep my Lab V2 (will validate when you reach it). |

---

### Week 11–12 — VXLAN / SD-Access

| External lab | Status | Maps to objective |
|---|---|---|
| [vxlan-underlay-ospf-31-overlay-false](https://github.com/networklessons/labs/tree/main/containerlab/labs/vxlan/cisco/vxlan-underlay-ospf-31-overlay-false) | **ADAPT** | Cisco NX-OS — I'll rewrite to IOS-XE for CSR1000v when you reach it. Same idea as my Lab V3. |
| [vxlan-bgp-evpn-l2-vni](https://github.com/networklessons/labs/tree/main/containerlab/labs/vxlan/cisco/vxlan-bgp-evpn-l2-vni) | **POST-ENCOR** | EVPN is overkill for ENCOR. Save for Post-ENCOR. |
| [vxlan-bgp-evpn-l3-vni](https://github.com/networklessons/labs/tree/main/containerlab/labs/vxlan/cisco/vxlan-bgp-evpn-l3-vni) | **POST-ENCOR** | Same. |
| All other `vxlan-*` labs (~15) | **POST-ENCOR** | Wide variety of underlays — perfect for Post-ENCOR weeks 8–10. |

**Verdict:** ENCOR-level VXLAN is light (concept + one lab). The big VXLAN library here is a goldmine for Post-ENCOR.

---

### Week 13 — Security (AAA, port-security, CoPP, ZBF, MACsec)

| External lab | Status | Notes |
|---|---|---|
| *(no AAA/port-sec/CoPP labs)* | — | **Gap.** Keep my Labs S1 / S2 / S3 (will validate when you reach them). |
| [asa-single](https://github.com/networklessons/labs/tree/main/containerlab/labs/cisco/asa/asa-single), [asa-failover](https://github.com/networklessons/labs/tree/main/containerlab/labs/cisco/asa/asa-failover) | **SKIP** | ASA is CCNP Security, not ENCOR. |

---

### Week 14 — Automation & telemetry

| External lab | Status | Notes |
|---|---|---|
| [sdn-onos](https://github.com/networklessons/labs/tree/main/containerlab/labs/sdn/onos) | **SKIP** | ONOS / OpenDaylight are not in ENCOR blueprint. Conceptually interesting but not exam-relevant. |
| [sdn-opendaylight](https://github.com/networklessons/labs/tree/main/containerlab/labs/sdn/opendaylight) | **SKIP** | Same. |
| *(no NETCONF/RESTCONF/EEM/Ansible labs)* | — | Keep my Labs AU1 / AU2 / AU3 (automation labs are my strongest territory — they're single-image, single-feature, no topology subtleties for me to mis-pattern). |

---

### Bonus / not on a specific week

| External lab | Status | Notes |
|---|---|---|
| [3tier-test](https://github.com/networklessons/labs/tree/main/containerlab/labs/3-tier-test) | **USE later** | A 3-tier campus topology you can use as scaffolding for any week. |
| [dhcp-relay-ipv4](https://github.com/networklessons/labs/tree/main/containerlab/labs/dhcp/dhcp-relay-ipv4) | **USE** | DHCP relay — small but exam-relevant. Slot it into Week 7 (services). |
| [vpc-three-switches](https://github.com/networklessons/labs/tree/main/containerlab/labs/vpc/vpc-three-switches) | **POST-ENCOR** | NX-OS vPC = StackWise/VSS equivalent — Post-ENCOR data-center. |
| [topologies/r1-r2-r3](https://github.com/networklessons/labs/tree/main/containerlab/labs/topologies/r1-r2-r3) | **USE** | Bare 3-router topology. Use as a starting template for any custom lab. |
| `ospf/arista`, `ospf/frr`, `ospf/fortinet`, `vxlan/arista`, `vxlan/frr`, `vxlan/vyos` | **SKIP for ENCOR** | Single-vendor for ENCOR. Save these for Post-ENCOR. |

---

## How to actually run one external lab on your EVE-NG

Take `ospf-basic-configuration` as the example:

1. Open the lab folder: https://github.com/networklessons/labs/tree/main/containerlab/labs/ospf/cisco/ospf-basic-configuration
2. Read `ospf-basic-configuration.clab.yml` — gives you the topology (R1, R2, R3) and links (R1 Et0/1 ↔ R2 Et0/1; R1 Et0/2 ↔ R3 Et0/1; R2 Et0/2 ↔ R3 Et0/2).
3. In EVE-NG, drop 3 CSR1000v nodes, wire them in a triangle. Map IOL `Ethernet0/1` → CSR `GigabitEthernet1`, `Ethernet0/2` → `GigabitEthernet2`. Keep the same logical wiring.
4. For each router, open `clab-ospf-basic-configuration/R<N>/startup-config.cfg.partial`, copy the config block, paste into your CSR (only fix needed: rename `Ethernet0/X` to `GigabitEthernetX`).
5. Verify with the standard OSPF show commands (covered in the matching NetworkLessons.com article).

**If anything fails, paste the `show` output here and I'll diagnose** — this is the workflow I'm most reliable at: reading real device output against a known-good reference.

---

## Updated per-week guidance

| Week | Topic | Primary lab source | Hand-written labs to keep |
|---|---|---|---|
| 1 | L2 baseline | Hand-written A1 | A1 (validate when you reach it) |
| 2 | STP / EtherChannel | Hand-written I1 / I2 | I1 (Validated), I2 (will validate) |
| 3 | OSPF basics | **External** (5 labs above) | — |
| 4 | OSPF advanced | **External** (8 labs above) | — |
| 5 | BGP | **External** (6 labs above) | I4 Phase 2 for community/local-pref (will validate) |
| 6 | EIGRP + IS-IS | **External** (2 labs above) | — |
| 7 | HSRP / VRRP / DHCP relay | **External** (11 HSRP + DHCP relay) | Small hand-written VRRP snippet |
| 8 | Multicast | Hand-written I6 (will validate) | I6 |
| 9 | Wireless | DevNet sandbox | — |
| 10 | Tunnels / VRF / MPLS | **External** (VRF + MPLS LDP) + my V1, V2 (validate) | V1, V2 |
| 11–12 | VXLAN / SD-Access | **External** (1 ADAPT lab) + my A2, V3 (validate) | A2, V3 |
| 13 | Security | Hand-written S1, S2, S3 (validate) | S1, S2, S3 |
| 14 | Automation | Hand-written AU1, AU2, AU3 | AU1, AU2, AU3 |

**Net result:** ~40% of your lab work moves to pre-validated external labs (OSPF, BGP, HSRP, MPLS basics, VXLAN underlay). ~60% stays hand-written, focused on areas where my error rate is lower (single-image, single-feature, no topology subtleties).

---

## Open question for you

The external HSRP labs cover 11 scenarios — that's deeper than the ENCOR exam needs. Do you want to:
- (a) do all 11 for completeness (Post-ENCOR-quality depth), or
- (b) pick the 4 most exam-relevant (basic, preemption, object-tracking-interface, object-tracking-ip-sla) and skip the auth/timer variants

My recommendation: (b) for the exam, then revisit (a) for Post-ENCOR if you want to specialize in carrier/enterprise FHRP. Tell me which and I'll prune the week 7 list accordingly.
