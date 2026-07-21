# ENCOR 350-401 — EXPRESS Plan (v2)

**⚠️ EXPRESS/compressed version — accelerated job-readiness only.**
**Full version:** `ENCOR-Study-Plan-TwoPass.md` (24–26 weeks, both passes, all domains).
**Tracker:** `ENCOR-Progress-EXPRESS.md` (single-file, module-level checkboxes + confidence scores).

**Excluded entirely:** SD-WAN, SD-Access, Wireless, Multicast, FHRP, QoS, IP Services, Network Assurance, Automation.
**Full depth:** Architecture/Design, OSPF, BGP, Security.
**Brief/overview only:** VXLAN.
**Video spine:** same as main plan — CBT Nuggets first (concepts), INE second (protocol depth). Labs from GNS3vault archive where available; hands-on build for Architecture where no GNS3vault lab exists.
**Time budget:** ~35h theory (CBT+INE+OCG) + ~19h labs = ~54h.

**Lab platform note (applies throughout):** EVE-NG with vIOS-L2, vIOS-L3, CSR1000v — single-RP virtual images. Any task requiring dual-supervisor hardware (live NSF/SSO switchover, true VSS/StackWise stacking, MACsec) is NOT achievable here. Where this applies, the task below is explicitly marked read-only/concept-only — don't spend time trying to force a live demo the platform can't support.

---

## Block 1 — Architecture & Design (~5h theory + 3h hands-on)

**CBT (primary — watch first):**
- `01-Explain The Hierarchical Network Model`
- `02-Explain Enterprise Network Design`
- `03-Describe High-Availability Techniques`
- `05-Differentiate Between On-Premises and Cloud Deployments`

**INE (after CBT):**
- `1-Introduction to Enterprise Network Design Principles` — full single module

**Note:** NSF/SSO vs StackWise vs VSS is CBT module `03` + your own notes — you've already got this solid, treat as review only.

**OCG skim:** Ch. 1 summary

**Hands-on (no GNS3vault lab exists for this topic — build these yourself):**

1. **3-tier campus baseline** — build from scratch, no starter config (2h). Just the topology: access/distribution/core tiers, redundant links between tiers. No IP addressing, no protocol config yet — that comes in later blocks/labs.

2. **NSF/SSO + VSS/StackWise — read, don't type (1h).**

   **Platform reality check:** on CSR1000v/vIOS-L3 (IOS-XE), `mode sso`, `main-cpu`, `switch virtual domain`, and `nsf enable` are typically NOT exposed — these require classic IOS on chassis hardware with slot-based supervisors, which this lab kit doesn't have. Don't burn time trying to force them. If curious, a 10-min check of `show license feature` or trying a newer C8000v image *might* unlock `mode sso`/`main-cpu` on some IOS-XE builds — optional, not required.

   **What you CAN actually type** on your core-tier pair (e.g. `core-1`/`core-2`): `redundancy` → `application` (redundancy groups for app-level HA, e.g. NAT stateful failover — a different mechanism than SSO) and `redundancy` → `maintenance-mode` (pulls a device out of active redundancy participation for planned work). Type both, read the `?` help output — this is your one genuine hands-on rep for this task.

   **Command reference (read, since you can't type these on this platform):**

   | Command | What it does |
   |---|---|
   | `redundancy` | Enters redundancy configuration mode — umbrella container for HA settings. |
   | `mode sso` | Sets Stateful Switchover — standby RP mirrors active RP's state continuously, so failover is near-instant with no session loss. |
   | `main-cpu` | Submode for primary RP behavior, e.g. `auto-sync running-config` (keeps standby's config identical to active's). |
   | `nsf enable` | Non-Stop Forwarding — data plane keeps forwarding using the last-known routing table during an RP switchover while routing protocols reconverge. |
   | `switch virtual domain <id>` | Groups two physical switches into one logical VSS pair, sharing a domain ID. |
   | `switch virtual` | Confirms virtual-switch mode is active after domain config. |
   | `switch 1/2 priority` | Sets which chassis wins the VSS active-member election. |

   **Bullets to write from this table, not from typing:** What failure does NSF/SSO solve that a plain reboot doesn't? Difference between RPR, SSO, and VSS in terms of what state survives a switchover.

**Bullets to write:** Three layers of the campus model. NSF/SSO vs StackWise vs VSS — failure scenario each solves. On-prem vs cloud trade-offs (control, latency, cost, elasticity).

---

## Block 2 — OSPF (~8h theory + 4h lab)

**CBT (primary — watch first):**
- `29-OSPF Foundation Concepts`
- `30-Basic OSPF IPv4 Configuration`
- `31-OSPF Summarization and Filtering`
- `32-Configure and Verify an OSPF NSSA`
- `33-Configure and Verify OSPFv3 with IPv6`

**INE (after CBT — prioritized subset, do NOT watch all 21 modules):**
- `17-OSPF` — modules 1 (overview), 3 (adjacency troubleshooting), 4 (areas + LSA types), 5–6 (network types), 8–10 (stub area types), 11–14 (NSSA), 18–20 (summarization/filtering).
- Skip: module 7 (virtual links), 15–17 (authentication) — skim only if time allows.

**OCG skim:** Ch. 8, 9, 10 summaries

**GNS3vault labs** (`gns3vault-archive/OSPF/`):
1. `ospf-single-area` — baseline adjacency
2. `ospf-dr-bdr-election` — priority manipulation
3. `ospf-summarization` — ABR summarization
4. `ospf-stub-area` + `ospf-totally-stubby-area` — LSA differences
5. `ospf-nssa` — Type 7 → Type 5 conversion

**Troubleshooting drill (added for EXPRESS):** reuse the topology from labs 1–5 above (don't rebuild) — intentionally break neighbor states (MTU mismatch, area mismatch, network type mismatch), fix from symptoms only using `show ip ospf neighbor` / `show ip ospf interface` output, no starter config, no task list open while diagnosing.

**Bullets to write:** LSA types 1–5 and 7 — what each contains, who generates it. Stub vs totally-stubby vs NSSA — what each blocks. ABR vs ASBR summarization difference.

---

## Block 3 — BGP (~10h theory + 6h lab)

**CBT (primary — watch first):**
- `34-BGP Foundation Concepts`
- `35-Basic BGP IPv4 Configuration`
- `36-Basic BGP IPv6 Configuration`

**INE (after CBT):**
- `18-Core BGP` — all 7 modules: `1–2 - Core BGP Routing Part 1/2`, `3 - Applying BGP Policy`, `4–5 - NLRI Advertisement Rules Part 1/2`, `6–7 - BGP Path Selection Rules Part 1/2`.

**OCG skim:** Ch. 11, 12 summaries

**GNS3vault labs** (`gns3vault-archive/BGP/`):
1. `bgp-basic-configuration` — eBGP session, network advertisement
2. `bgp-ibgp-configuration` — iBGP full mesh, next-hop-self
3. `bgp-attribute-weight` — weight, local scope
4. `bgp-attribute-local-preference` — LP, AS-wide scope
5. `bgp-attribute-as-path` — AS-path prepending
6. `bgp-attribute-med` — MED
7. `bgp-communities` — standard communities, route-map matching
8. `bgp-route-reflector` — RR/client config

**Troubleshooting drill (added for EXPRESS):** reuse the topology from labs 1–8 above (don't rebuild) — break peering (AS mismatch, missing network, route-map denying), diagnose from `show ip bgp` / `show ip bgp neighbors` output only, no config file peeking.

**Bullets to write:** BGP best-path algorithm — first 5 steps in order. Weight vs local-preference vs MED vs AS-path — scope of each (local router / AS / inter-AS). Why iBGP needs full mesh or a route reflector.

---

## Block 4 — Security (~8h theory + 6h lab)

**CBT (primary — watch first):**
- `51-Configure and Verify Device Access Control`
- `52-Configure and Verify Access Control Lists`
- `53-Configure and Verify Control Plane Policing (CoPP)`
- `54-Zone-Based Firewall (ZBFW)`

**INE (after CBT):**
- `32-Hardening Cisco IOS` — full module (AAA framework, control-plane security, CoPPr, uRPF)
- `33-Access-Lists Beyond the Basic and Extended` — reflexive ACLs, object groups, time-based ACLs
- `34-Control Plane Policing & Protection (Video)` — full single video
- `37-Cisco Firewall Technologies for Beginners` — full module (ZBFW)

**Note — Gaps (per main plan):** MACsec/TrustSec/SGT and full L2 hardening (DHCP snooping, DAI, port-security) are not covered by CBT/INE — and MACsec specifically is NOT supported on EVE-NG virtual switches at all, so don't attempt to configure it. Read OCG Ch. 27, 28 summaries only, one paragraph each — concept-level knowledge, no lab.

**OCG skim:** Ch. 25, 26 summaries + Ch. 27, 28 summaries (light pass)

**GNS3vault labs** (`gns3vault-archive/Security/`):
1. `aaa-authentication` — RADIUS, TACACS+, local fallback (leverage your real work experience here)
2. `standard-access-list` → `extended-access-list` → `named-access-list` — core three, skip reflexive/time-based unless time allows
3. `control-place-policing` — MQC policy for control-plane
4. `basic-zone-based-firewall` — zone pairs, self zone, default-deny

**Bullets to write:** AAA — authentication/authorization/accounting, TACACS+ vs RADIUS (what each encrypts). CoPP placement — what it protects. ZBFW zone pairs — why self zone matters.

---

## Block 5 — VXLAN brief (~2h theory, no lab)

**CBT (primary — watch first):**
- `20-Describe VXLAN`

**INE (after CBT — brief subset only):**
- `10-Virtual Extensible LAN (VXLAN) on Nexus NX-OS` — modules 1–3 only (Overview, Terminology, Encapsulation). Skip modules 4–7 (workflow/prerequisites/flood-and-learn) — EXPRESS is talking-points depth, not lab depth.

**OCG skim:** Ch. 23 §VXLAN summary

**Bullets to write:** What VXLAN solves (4096 VLAN limit, L2 over L3). VNI + UDP encapsulation concept. Why it needs a control plane (BGP EVPN, one sentence).

---

## Lab summary (~19h)

| Block | Labs | Hours |
|---|---|---|
| Architecture | 3-tier baseline build (2h) + NSF/SSO/VSS read-and-explain, `application`/`maintenance-mode` typed (1h) | 3h |
| OSPF | 5 GNS3vault labs + troubleshooting drill (same topology) | 4h |
| BGP | 8 GNS3vault labs + troubleshooting drill (same topology) | 6h |
| Security | 4 GNS3vault labs | 6h |
| VXLAN | none — theory only | 0h |

---

## Self-check before calling this "done"

- [ ] Explain 3-tier model + NSF/SSO/StackWise/VSS without notes.
- [ ] Explain OSPF LSA types 1/2/3/5/7 and area types from memory.
- [ ] Explain full BGP best-path order with a route-map example.
- [ ] Explain AAA (TACACS+ vs RADIUS), CoPP, and ZBFW zone logic from memory.
- [ ] Explain what VXLAN solves and why it needs BGP EVPN, in 2–3 sentences.
- [ ] Completed all GNS3vault labs above with no starter configs on second attempt.

---

## What got cut vs the full Two-Pass plan

- SD-WAN, SD-Access (Week 2) — skipped per request.
- Wireless (Week 9) — skipped per request.
- QoS, device virtualization, VRF/GRE/IPsec/LISP (Weeks 3–4, except VXLAN) — cut for time.
- Multicast, FHRP, IP Services (Weeks 10) — cut for time.
- Network Assurance (Week 11) — cut for time.
- Automation (Week 14) — cut for time; lean on real Oxidized/scripting experience if asked in interview.
- STP, VLANs/trunking/EtherChannel (Weeks 5–6) — cut, since you're already strong here from work experience; revisit only if a specific gap surfaces.
