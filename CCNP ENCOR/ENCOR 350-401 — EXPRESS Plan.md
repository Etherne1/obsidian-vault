# ENCOR 350-401 — EXPRESS Plan

**⚠️ EXPRESS/compressed version — accelerated job-readiness only.**  
**Full version:** `ENCOR-Study-Plan-TwoPass.md` (24–26 weeks, both passes, all domains).  
**Tracker:** `ENCOR-Progress-EXPRESS.md` (single-file, module-level checkboxes + confidence scores).

**Excluded entirely:** SD-WAN, SD-Access, Wireless, Multicast, FHRP, QoS, IP Services, Network Assurance, Automation.  
**Full depth:** Architecture/Design, OSPF, BGP, Security.  
**Brief/overview only:** VXLAN.  
**Video spine:** same as main plan — CBT Nuggets first (concepts), INE second (protocol depth). Labs from GNS3vault archive where available; hands-on build for Architecture where no GNS3vault lab exists.  
**Time budget:** ~35h theory (CBT+INE+OCG) + ~20h labs = ~55h.

---

## Block 1 — Architecture & Design (~5h theory + 4h hands-on)

**CBT (primary — watch first):**

- `01-Explain The Hierarchical Network Model`
    
- `02-Explain Enterprise Network Design`
    
- `03-Describe High-Availability Techniques`
    
- `05-Differentiate Between On-Premises and Cloud Deployments`
    

**INE (after CBT):**

- `1-Introduction to Enterprise Network Design Principles` — full single module
    

**Note:** NSF/SSO vs StackWise vs VSS is CBT module `03` + your own notes — you've already got this solid, treat as review only (~1h).

**OCG skim:** Ch. 1 summary

**Hands-on (no GNS3vault lab exists for this topic — build these yourself):**

1. 3-tier campus baseline — build from scratch, no starter config (2h)
    
2. NSF/SSO failover demo, or VSS/StackWise config syntax review if hardware/platform doesn't support live failover (2h)
    

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
    

**Troubleshooting drill (added for EXPRESS):** intentionally break neighbor states (MTU mismatch, area mismatch, network type mismatch), fix from symptoms only — no starter config.

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
    

**Troubleshooting drill (added for EXPRESS):** break peering (AS mismatch, missing network, route-map denying), diagnose from `show ip bgp` output only.

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
    

**Note — Gaps (per main plan):** MACsec/TrustSec/SGT and full L2 hardening (DHCP snooping, DAI, port-security) are not covered by CBT/INE — read OCG Ch. 27, 28 summaries only, one paragraph each, no lab.

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

## Lab summary (~20h)

|Block|Labs|Hours|
|---|---|---|
|Architecture|3-tier baseline build + NSF/SSO/StackWise/VSS review|4h|
|OSPF|5 GNS3vault labs + troubleshooting drill|4h|
|BGP|8 GNS3vault labs + troubleshooting drill|6h|
|Security|4 GNS3vault labs|6h|
|VXLAN|none — theory only|0h|

---

## Self-check before calling this "done"

- Explain 3-tier model + NSF/SSO/StackWise/VSS without notes.
    
- Explain OSPF LSA types 1/2/3/5/7 and area types from memory.
    
- Explain full BGP best-path order with a route-map example.
    
- Explain AAA (TACACS+ vs RADIUS), CoPP, and ZBFW zone logic from memory.
    
- Explain what VXLAN solves and why it needs BGP EVPN, in 2–3 sentences.
    
- Completed all GNS3vault labs above with no starter configs on second attempt.
    

---

## What got cut vs the full Two-Pass plan

- SD-WAN, SD-Access (Week 2) — skipped per request.
    
- Wireless (Week 9) — skipped per request.
    
- QoS, device virtualization, VRF/GRE/IPsec/LISP (Weeks 3–4, except VXLAN) — cut for time.
    
- Multicast, FHRP, IP Services (Weeks 10) — cut for time.
    
- Network Assurance (Week 11) — cut for time.
    
- Automation (Week 14) — cut for time; lean on real Oxidized/scripting experience if asked in interview.
    
- STP, VLANs/trunking/EtherChannel (Weeks 5–6) — cut, since you're already strong here from work experience; revisit only if a specific gap surfaces.