# Post-ENCOR Labs — EVE-NG (vendor-light)

**Assumed images in EVE-NG (same as your ENCOR setup):**
- Cisco **vIOS L2**, **vIOS L3**, **CSR1000v 17.3.04a**
- **MikroTik CHR** (free, download from mikrotik.com — works on x86 EVE-NG out of the box)
- **FRR on Alpine/Debian Docker** (lightweight, ideal for IS-IS / EVPN drills)
- **Juniper vMX** *(optional — load only if you have the trial image; everything works without it)*

**Conventions:**
- Loopbacks are `Lo0 = X.X.X.X/32` where X is the router number (R1 → 1.1.1.1/32).
- Customer/admin networks use `10.X.0.0/24`. Backbone p2p links use `192.168.0.0/24` `/30` slices.
- IPv6: ULA `fd00:X::/64` per router (X = router number).
- Every lab ends with a **"prove it"** step — a `show` / capture / ping that demonstrates the goal was achieved.
- Every lab ends with a **destroy and rebuild from notes** step so the muscle memory sticks.

---

## Lab 1 — VRF-Lite + BFD (Week 1)

**Topology:**
- 1× CSR1000v (`R1`) acting as a PE-style edge with three VRFs (`CUST_A`, `CUST_B`, `SHARED`)
- 2× vIOS L3 acting as CE routers per VRF
- 1× MikroTik CHR mirror copy of the scenario on the side

**Tasks:**
1. Define VRFs `CUST_A` (RD 65001:1, RT import/export 65001:100), `CUST_B` (RD 65001:2, RT 65001:200), `SHARED` (RD 65001:3, RT export 65001:300, import 65001:100 65001:200).
2. Assign sub-interfaces to each VRF; configure `address-family ipv4 vrf` for OSPF on each.
3. Use overlapping `10.0.0.0/24` in `CUST_A` and `CUST_B` — prove isolation.
4. Leak `SHARED` routes into both customer VRFs via RT import.
5. Enable BFD on the OSPF adjacencies (asynchronous mode, 300 ms × 3).
6. Tear down one link; confirm sub-second convergence in `show ip ospf neighbor detail vrf CUST_A`.
7. **MikroTik cross-check:** repeat steps 1–4 on a CHR (`/ip vrf`, `/routing/ospf/instance`, route-leaking via `/ip route`).

**Prove it:**
- `show vrf detail | i Name|Interfaces`
- `show ip route vrf CUST_A` shows `10.0.0.0/24` from `CUST_A` only; same in `CUST_B`.
- `show bfd neighbors detail` shows `300 ms × 3`, state `Up`.

**Destroy & rebuild:** wipe the config, rebuild from your own notes without looking at the lab guide.

---

## Lab 2 — Route Manipulation & PBR (Week 2)

**Topology:** 6 vIOS L3 in a partial mesh, 3 ASes, eBGP and OSPF mixed.

**Tasks:**
1. Build a prefix-list `BLOCK_RFC1918` matching `10.0.0.0/8 le 32`, `172.16.0.0/12 le 32`, `192.168.0.0/16 le 32`.
2. Route-map `OUT_TO_PEER`: deny `BLOCK_RFC1918`, permit-set local-pref 200 on a specific prefix matched by another prefix-list.
3. Apply outbound to one eBGP neighbor; verify with `show ip bgp neighbors X advertised-routes`.
4. PBR: from a stub host LAN, route **TCP/80** out a backup interface, all else via primary. Use `match ip address` referencing an extended ACL; `set ip next-hop verify-availability` + tracking object on the primary.
5. Tear primary link; confirm PBR fallback path with `traceroute` and `show ip policy`.
6. **MikroTik cross-check:** the same PBR via `/ip firewall mangle` (mark-routing) + `/ip route rule`.

**Prove it:**
- `show route-map` shows match counters incrementing.
- `traceroute` from the LAN to a TCP/80 destination shows the backup path; to a TCP/443 destination shows the primary path.

---

## Lab 3 — Mutual Redistribution Without Loops (Week 3)

**Topology:** 4 vIOS L3 in a square; left edge runs OSPF, right edge runs EIGRP, two middle routers (ASBRs) do mutual redistribution. *Optional:* swap EIGRP for IS-IS on FRR for vendor diversity.

**Tasks:**
1. Build OSPF area 0 on the left, EIGRP AS 100 on the right.
2. On both ASBRs, redistribute mutually with **route tags**: `tag 100` when going OSPF→EIGRP, `tag 200` when going EIGRP→OSPF.
3. Add a deliberate redistribution loop (no filtering). Observe loop / suboptimal path with `show ip route` and `traceroute`.
4. Fix with `redistribute … route-map DENY_RETURNING_TAG` — block any route with the protocol's "own" tag from being re-injected.
5. Verify both directions have correct metric-type, seed-metric, and AD.

**Prove it:**
- `show ip route ospf` on the EIGRP side shows external routes with E2 / tag 200.
- No "ghost" routes in `show ip route eigrp` originating from EIGRP itself via the OSPF path.
- Convergence test: shut a link, verify failover is clean (no microloops).

---

## Lab 4 — EIGRP Deep, Named-Mode (Week 4)

**Topology:** 4 vIOS L3 in a hub-spoke + spoke-spoke triangle.

**Tasks:**
1. Build **named-mode EIGRP** instance `LAB4` for IPv4 + IPv6 address families.
2. Configure SHA-256 key-chain authentication; rotate one key mid-lab and confirm zero downtime.
3. Configure the hub with `eigrp stub connected summary` on the spokes; verify only stub-type queries are sent.
4. Trigger an **SIA scenario**: artificially blackhole a route on a stub by removing it; on the active speaker, observe `show ip eigrp topology active`.
5. Resolve by setting `timers active-time disabled` and using `eigrp stub` properly.

**Prove it:**
- `show eigrp protocols` shows named-mode + SHA-256.
- `show ip eigrp neighbors detail` shows authentication mode.
- During the SIA test, `show ip eigrp topology active` lists the problem; after the fix, no SIA.

---

## Lab 5 — OSPFv3 with Address Families (Week 5)

**Topology:** 3 vIOS L3 in a triangle, dual-stack.

**Tasks:**
1. Configure **OSPFv3 with IPv4 + IPv6 address families** on a single process — single adjacency carries both.
2. Enable key-chain authentication (SHA-256) — mandatory for OSPFv3 in modern IOS.
3. Build a Type 7 → Type 5 NSSA scenario: one router is an ASBR injecting external routes from a static; the other ABR translates type-7 into type-5.
4. **FRR cross-check:** same triangle on FRR; observe identical LSAs.

**Prove it:**
- `show ospfv3 ipv4 neighbor` and `show ospfv3 ipv6 neighbor` both show full adjacency.
- `show ospfv3 ipv4 database nssa-external` on the NSSA ABR; `show ospfv3 ipv4 database external` on the backbone.

---

## Lab 6 — OSPF Network Types, Areas, Virtual Links (Week 6)

**Topology:** 5 vIOS L3 — one acting as an NBMA hub, three as spokes, one as a backbone-disconnected ABR (for the virtual-link drill).

**Tasks:**
1. Build a **point-to-multipoint** OSPF over an NBMA-like topology (sub-interfaces or GRE).
2. Compare with **broadcast** and **point-to-point** modes on the same topology — note DR/BDR election differences.
3. Place an area-3 "behind" area-2 (no direct connection to area 0). Bring it up via a **virtual link** through area-2.
4. Redesign the topology to **remove the virtual link** (add a physical or GRE link between area-3 ABR and area-0).
5. NSSA test: convert area-3 to NSSA; inject an external via `redistribute static subnets`; confirm Type-7 → Type-5 conversion at the area-3 ABR.

**Prove it:**
- `show ip ospf neighbor` shows expected DR/BDR (or no DR) per network type.
- `show ip ospf virtual-links` then **absent** after redesign.
- `show ip ospf database external` on backbone shows the redistributed prefix as type-5.

---

## Lab 7 — IS-IS L1/L2 Backbone (Week 7)

**Topology:** 4 FRR containers + 2 vIOS L3 (or Juniper vMX if you have it). Two areas: `49.0001` (L1) and `49.0002` (L2).

**Tasks:**
1. Configure NETs on all routers (`49.0001.0000.0000.0001.00`, `49.0001.0000.0000.0002.00`, etc.).
2. Set router types: 2 routers `level-1`, 2 routers `level-2`, 2 routers `level-1-2`.
3. Use **wide metrics** everywhere (modern default).
4. Configure SHA-256 hello authentication.
5. Leak selected L2 → L1 routes with a route-policy.
6. Insert a deliberate metric tie; resolve with manual metric tuning.

**Prove it:**
- `show isis neighbors` on each router shows expected adjacencies.
- `show isis database detail` shows TLV-extended IS reachability (wide metrics).
- `show ip route isis` on an L1-only router shows only intra-area + a default toward L1/L2.

---

## Lab 8 — BGP Address Families, RR, and Filtering (Week 8)

**Topology:** 3 ASes — `AS 65010` (left ISP, 2 routers), `AS 65020` (transit core, 3 routers with one route-reflector), `AS 65030` (right ISP, 2 routers).

**Tasks:**
1. eBGP between ASes (multihop where needed); iBGP inside AS 65020 with one **route reflector**; clients reflect to one another.
2. Activate **`address-family ipv4 unicast`** and **`address-family ipv6 unicast`** on every neighbor.
3. AS-path regex filtering: block any path containing `_64500_` (private AS leakage).
4. Communities: tag inbound from one peer with `65020:100`; outbound, deny anything tagged `65020:200`.
5. Implement **outbound route-filtering** (ORF) between two peers and verify the filter is pushed.
6. **Local-pref / MED / weight** lab: produce a sub-optimal path with default values; fix it three different ways and record which is appropriate.

**Prove it:**
- `show bgp ipv4 unicast summary` and `show bgp ipv6 unicast summary` both show all peers Established.
- `show bgp ipv4 unicast neighbors X received-routes` on a client confirms RR cluster-list and originator-id.
- `show bgp regexp _64500_` returns nothing on the receiving side.

---

## Lab 9 — BGP Troubleshooting Gauntlet (Week 9)

**Setup:** start with the Lab 8 topology, **broken** by your friend (or your past self via a saved-but-mangled config).

**Six bugs to find and fix (one each):**
1. TCP/179 dropped by an ACL on a transit router.
2. eBGP multihop missing for a loopback-sourced peering.
3. `update-source Loopback0` set on one side only.
4. MD5 password mismatch between peers.
5. iBGP RR client missing `next-hop-self` for an external route → black hole.
6. Outbound route-map silently denying a prefix that should be advertised.

**Tasks:**
1. For each bug, **first** predict the symptom from the BGP state machine (`Idle / Connect / Active / OpenSent / OpenConfirm / Established`).
2. Identify with the **minimum** set of commands (`show bgp summary`, `show bgp neighbor X`, `debug bgp ipv4 unicast updates`).
3. Document the fix in your notes.
4. After all 6 are fixed: extend the topology to **dual-stack** — add `ipv6 unicast` everywhere, eBGP IPv6 peering, link-local next-hops with `neighbor X update-source GigE0/0`.

**Prove it:** every neighbor reaches `Established`; full IPv4 + IPv6 connectivity confirmed by `ping vrf X X.X.X.X source Loopback0`.

---

## Lab 10 — MPLS L3VPN End-to-End (Week 10)

**Topology:** 4-router MPLS core (P1-P2 in the middle, PE1-PE2 at the edges) + 4 CE routers (2 per VRF — `VPN_RED` and `VPN_BLUE`).

**Tasks:**
1. Underlay: OSPF on all core links; LDP enabled with `mpls ip` on every core interface; LDP router-id = Loopback0.
2. Verify label distribution: `show mpls ldp neighbor`, `show mpls forwarding-table`.
3. Define VRFs on PEs: `VPN_RED` (RD 65000:10, RT 65000:10), `VPN_BLUE` (RD 65000:20, RT 65000:20).
4. PE-CE protocol: OSPF for `VPN_RED`, eBGP for `VPN_BLUE` (cover both).
5. MP-BGP VPNv4 between PE loopbacks: `address-family vpnv4 unicast`; `send-community extended` mandatory.
6. Prove route isolation: overlap `10.0.0.0/24` in both VRFs.
7. **Stretch:** add IPv6 — repeat as `address-family vpnv6 unicast`, 6VPE-style.
8. **Juniper cross-check (optional):** swap one PE for vMX. RSVP-TE vs LDP, but the L3VPN concepts are identical.

**Prove it:**
- `show bgp vpnv4 unicast all summary` — both PE peers Established.
- `show mpls forwarding-table` on a P router shows label-stack imposition for VPN prefixes.
- `traceroute vrf VPN_RED 10.0.0.X` from one CE site shows the MPLS label in the output.

---

## Lab 11 — DMVPN + IPsec (Week 11)

**Topology:** 1 hub + 3 spokes, all CSR1000v. Add 1 MikroTik CHR as a 4th spoke (Phase 1 only — MikroTik DMVPN compatibility is limited).

**Tasks:**
1. Build **DMVPN Phase 1** (hub-and-spoke only): mGRE on hub, p2p GRE on spokes, NHRP NHS on spokes, EIGRP across the tunnel with `no ip split-horizon eigrp`.
2. Upgrade to **Phase 2** — spoke-to-spoke tunnels via NHRP resolution requests.
3. Upgrade to **Phase 3** — NHRP shortcut + redirect; spoke-to-spoke triggered by hub redirect.
4. Apply **IPsec protection** to the mGRE tunnels: IKEv2, PSK first then certificate-based (use a quick CA on an Alpine VM if you have one).
5. Verify spoke-to-spoke direct flow with `show dmvpn detail`, `show crypto session detail`, and a packet capture on the hub's transport interface (should see traffic between spokes go **direct**, not via hub).
6. **MikroTik cross-check:** site-to-site **IPsec** (no DMVPN) between two CHRs — IKEv2, AES-256-GCM, SHA-256, DH group 19. Compare the conceptual flow.

**Prove it:**
- `show ip nhrp` on a spoke shows dynamic entries for other spokes (Phase 2/3).
- `show crypto ikev2 sa detailed` shows the IKEv2 SAs.
- Capture on hub shows tunnel traffic bypassing it in Phase 3.

---

## Lab 12 — Infrastructure Hardening (Week 12)

**Topology:** any topology you have; 1 CSR1000v + 1 vIOS L2 + 1 Alpine VM running FreeRADIUS and tac_plus.

**Tasks:**
1. **AAA stack:** TACACS+ for admin (login + command authorization, two privilege levels); RADIUS for 802.1X on a switchport.
2. Configure **AAA method-lists** with fallback to local.
3. **Control-plane policing (CoPP):** named classes for:
   - `MANAGEMENT` (SSH, NTP, SNMP, syslog)
   - `ROUTING` (OSPF, BGP, EIGRP, LDP)
   - `UNDESIRABLE` (ICMP redirects, IP options, fragments)
   - `CATCH_ALL` (everything else — rate-limit aggressively)
4. **IPv6 First-Hop Security** on the vIOS L2 access segment:
   - **RA Guard** with `device-role host` on host ports
   - **DHCPv6 Guard** with `device-role server` only on the uplink to the DHCPv6 server
   - **IPv6 Snooping** policy applied to the access VLAN
   - **IPv6 Source Guard** on host ports
5. **Hardening checklist:** disable HTTP/HTTPS server, `no ip source-route`, `service password-encryption`, `no service pad`, `login block-for 60 attempts 3 within 30`, `transport input ssh`, banner motd.

**Prove it:**
- Login as one TACACS+ user → can run `show` only; another can configure.
- Send a flood of malformed packets at the management interface → CoPP drops them; counters increment in `show policy-map control-plane`.
- Spoof a rogue RA from a host port → blocked; `show ipv6 snooping events` lists the drop.

---

## Lab 13 — Telemetry & Services Stack (Week 13)

**Topology:** any 4-router topology + an Alpine/Debian VM running **Docker** with Prometheus + Grafana + LibreNMS + ntopng + Akvorado (or just `nfdump`).

**Tasks:**
1. **SNMPv3** with auth+priv (SHA + AES-128) to LibreNMS; add all routers; verify interface graphs.
2. **NetFlow v9** on every router exporting to ntopng on port 2055; add **Flexible NetFlow** with a custom record (src/dst IP, ports, TOS, BGP next-hop).
3. **IP SLA** UDP-jitter probe between two routers; **track** the SLA state; have a static-route conditional on tracker state; tear the path, verify failover.
4. **DHCP relay across VRFs:** DHCPv4 server in VRF `MGMT`, clients in VRF `CUST_A`; configure `ip helper-address vrf MGMT 10.X.X.X global` on the SVI; confirm leases via `show ip dhcp binding`.
5. **gNMI subscription** (stretch): use `gnmic` from the Alpine VM to subscribe to `/interfaces/interface/state/counters` on the CSR1000v; output to Prometheus; build a Grafana dashboard.

**Prove it:**
- LibreNMS dashboard shows traffic graphs from every device.
- ntopng / Akvorado shows top-talkers per AS.
- Pull primary link → IP SLA tracker flaps → static route swaps in `show ip route`.
- Grafana panel shows real-time interface counter from gNMI.

---

## Lab 14 — Skim DNAC Assurance / Finish Design (Week 14)

No new EVE-NG topology. **One** activity:

**Walk the Library C design modules with a notebook:**
- `10. Design High Availability in Campus Networks`
- `12. Design Campus L2 Infrastructures`
- `13. Design Multicampus L3 Infrastructures`
- `24. Multicast Routing Foundations and Design`
- `14./15. SD-Access architecture/fabric` (concepts only)
- `19./20. SD-WAN architecture/design` (concepts only)

For each, draw the **reference architecture** on paper. Then, **for each Cisco-SDA / SD-WAN concept, write its EVPN-VXLAN / open-source equivalent** (e.g., "fabric edge = VTEP", "LISP control-plane node = BGP-EVPN route-reflector", "vSmart = open-source SD-WAN controller like flexiWAN").

This is the most leveraged hour of the week — translating proprietary terminology into vendor-neutral concepts is **the** senior-network-engineer skill.

---

## Lab 15 — Automation Build Week (Week 15)

**Goal:** stand up a real, working `network-automation` Git repo. **No video.**

**Build order:**
1. **Repo structure:**
   ```
   network-automation/
     inventory/
       hosts.yaml          # Nornir host inventory
       groups.yaml         # vendor groups: cisco_iosxe, mikrotik_routeros, juniper_junos
     playbooks/
       backup.yaml         # Ansible
       gather_state.py     # Nornir + NAPALM
     scripts/
       netconf_pull.py     # ncclient
       restconf_pull.py    # requests
       gnmic_subscribe.sh  # gnmic
     telemetry/
       prometheus.yml
       grafana_dashboards/
     Makefile              # `make backup`, `make report`, `make telemetry`
     README.md
   ```
2. **Day 1:** install `python3-venv`, create venv, `pip install nornir nornir-napalm nornir-utils nornir-netmiko ansible netmiko napalm ncclient pygnmi requests pyang`.
3. **Day 2:** `make backup` — Nornir+NAPALM pulls running configs from every device and commits to a `configs/` subdirectory with timestamps.
4. **Day 3:** `make report` — Nornir+NAPALM pulls interface error counters and outputs a Markdown report.
5. **Day 4:** `netconf_pull.py` — pull `/ietf-routing:routing/control-plane-protocols` from a CSR1000v; pretty-print the OSPF neighbor list.
6. **Day 5:** `restconf_pull.py` — same data via RESTCONF + JSON; compare API ergonomics.
7. **Day 6:** `gnmic_subscribe.sh` — subscribe to interface counters; stream to Prometheus; Grafana dashboard.
8. **Day 7:** **Ansible role** that deploys a NEW VRF + OSPF process to a Cisco IOS-XE device and a MikroTik CHR with the same Ansible playbook (vendor-conditional tasks).

**Prove it:**
- `git log` shows ≥ 30 commits with meaningful messages.
- `make backup && make report` runs end-to-end without manual intervention.
- One playbook touches both a Cisco and a MikroTik device successfully.

---

## Lab 16 — Capstone (Week 16)

**The single most important lab of the plan.** This is your portfolio piece.

**Topology (full ISP/large-enterprise reference):**

```
                      [TACACS+/RADIUS/DNS/DHCP/Prometheus/Grafana/LibreNMS]
                                          |
                                       [MGMT vSwitch]
                                          |
        +--------- AS 65000 (your enterprise) ----------+
        |                                               |
       PE1 ============= MPLS Core (LDP) ============= PE2
       /  \                                            /  \
    CE1A   CE1B (VPN_RED)                       CE2A   CE2B (VPN_BLUE)
                                                  |
                                              [DMVPN spoke
                                               over Internet
                                               terminating
                                               in VPN_BLUE on PE2]
```

**Deploy everything by Ansible from your Git repo from Lab 15.**

**Mandatory features (checklist):**
- [ ] OSPF in the MPLS core, LDP, MP-BGP VPNv4 between PEs
- [ ] 2 VRFs (VPN_RED OSPF PE-CE, VPN_BLUE eBGP PE-CE)
- [ ] Overlap `10.0.0.0/24` between VRFs to prove isolation
- [ ] DMVPN Phase 3 from one branch into VPN_BLUE on PE2 (over Internet)
- [ ] IKEv2 with certificates protecting the DMVPN
- [ ] AAA via TACACS+ on every router (two privilege tiers)
- [ ] CoPP on every router (4 classes)
- [ ] IPv6 dual-stack everywhere (BGP IPv6 AF, OSPFv3, IPv6 ACLs, IPv6 FHS on the access segment)
- [ ] NetFlow v9 / IPFIX to ntopng; SNMPv3 to LibreNMS; gNMI to Prometheus
- [ ] IP SLA monitoring critical paths with track-based static routes
- [ ] One-page network diagram (draw.io / Excalidraw / mermaid) in the repo
- [ ] README explaining how to rebuild from scratch in < 30 minutes

**Self-test:**
- Run the **CCNP ENARSI 300-410 practice exam** (Pearson Test Prep, ≥85% on a 200-question full pool).
- Hand the repo to a friend; they should be able to `git clone` + `make all` and rebuild your topology.

**Decision point at the end:** which next certification or skill track? Options laid out in `Post-ENCOR-Study-Plan.md` Week-16 section.

---

## Anki workflow

Same as ENCOR plan — see `Post-ENCOR-Practice-Questions.md` for the per-week Q&A. Convert each to Anki cards (Cloze for command syntax, Basic for concept Q&A). 20 min/day max.

## Tracker workflow

Update `Post-ENCOR-Progress-Tracker.md` or `.csv` after every lab. When confidence ≤ 3 for two weeks in a row, slow down and re-lab.
