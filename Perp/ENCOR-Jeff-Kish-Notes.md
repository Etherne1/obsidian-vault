# ENCOR 350-401 — Notes & Recall (Jeff Kish CBT Nuggets + OCG)

**How to use this file**

- One section per week. Top half = bullet notes you fill/expand as you study. Bottom half = 10–20 scenario-style Q&A prompts ready to paste into Anki (front / back separated by `;` — Anki "Basic" type with semicolon delimiter, or split on `→`).
- I've **seeded** each week with the core facts so you have a working baseline; refine in your own words as you go (active recall > passive copying).
- Citations to the OCG mean the 2nd edition (Hucaby et al., ISBN 978-0-13-821676-4).

---

## Week 1 — Packet Forwarding & Switching Architecture

### Notes

- **Forwarding paths:**
  - *Process switching* — CPU per-packet, slowest, only for control-plane traffic or punted frames.
  - *Fast switching* — first packet creates a cache entry, subsequent packets bypass routing table. Deprecated on modern IOS-XE.
  - *CEF* — pre-built FIB + adjacency tables, hardware-friendly, multi-pathing, default everywhere.
- **CEF tables:**
  - **FIB** — mirror of RIB optimized for longest-match.
  - **Adjacency table** — pre-resolved L2 next-hop rewrite info.
- **TCAM / CAM:**
  - CAM (binary) for MAC table lookups (exact match).
  - TCAM (ternary, supports masks) for ACL, QoS, FIB longest-prefix.
- **SDM templates** control how TCAM is partitioned (more routes vs more ACLs vs more MAC entries).
- **Stack vs StackWise Virtual vs VSS:**
  - *StackWise (1–9)* — physical stack cable, one control plane, common on 9300.
  - *StackWise Virtual* — two chassis appear as one (9500/9600), SVL link + dual active detection.
  - *VSS* — legacy 6500/4500 equivalent.

### Anki prompts (Week 1)

1. Which forwarding mechanism builds the FIB and adjacency tables from the RIB and ARP cache? → CEF (Cisco Express Forwarding).
2. What is stored in the CEF adjacency table that is NOT in the FIB? → The pre-built Layer-2 rewrite (next-hop MAC and outgoing encap).
3. Which memory type supports masked lookups suitable for ACLs and FIB longest-prefix match? → TCAM (Ternary CAM).
4. Which switch feature carves TCAM regions among routing, ACL, MAC, and multicast? → SDM templates.
5. On a Cat9500 SVL pair, what link bundles the two chassis into one logical switch? → The Stackwise Virtual Link (SVL).
6. Name the failure-detection mechanism that prevents both StackWise-Virtual chassis becoming Active. → Dual Active Detection (DAD) — over fast-hello / ePAgP / BFD.
7. Which forwarding path is used for the very first ICMP echo when CEF is disabled and fast-switching is enabled? → Process switching (CPU), then fast-cached for subsequent packets.
8. What CLI verifies the next-hop adjacency CEF has installed for a given prefix? → `show ip cef <prefix> detail`.
9. What does `show platform hardware fed switch active fwd-asic resource tcam utilization` reveal? → Per-feature TCAM utilization on Catalyst 9k.
10. Name three reasons a router process-switches a packet otherwise eligible for CEF. → IP options set, fragmentation required, TTL=1 generating ICMP, log-input ACL match, no FIB entry.
11. Which command shows whether CEF is enabled globally and per-interface? → `show ip cef summary` and `show cef interface`.
12. In a stack, which member runs the routing protocol and forwards control-plane traffic? → The Active switch (one per stack, elected).

---

## Week 2 — Spanning Tree Protocol

### Notes

- **Port states (802.1D):** Disabled → Blocking → Listening → Learning → Forwarding.
- **802.1w RSTP roles:** Root, Designated, Alternate, Backup. States simplified to Discarding / Learning / Forwarding.
- **PVST+** — per-VLAN STP, Cisco. **RPVST+** — per-VLAN RSTP, default modern Cisco IOS.
- **MST (802.1s)** — multiple VLANs into instances (MSTIs). All switches in same MST region must share name, revision, and VLAN-to-instance map.
- **Guards:**
  - *BPDU Guard* — err-disables port that receives ANY BPDU (PortFast edge ports).
  - *Root Guard* — port goes root-inconsistent if it receives a superior BPDU.
  - *Loop Guard* — port goes loop-inconsistent if BPDUs stop being received on a non-designated port.
  - *BPDU Filter* — drops BPDUs entirely (dangerous — disables STP on that port).
- **UDLD** — bi-directional fiber link check, *normal* logs, *aggressive* err-disables.
- **Path cost** — depends on cost mode (short vs long); RPVST+ uses 802.1t long-style by default.

### Anki prompts (Week 2)

1. Which STP variant assigns one spanning tree per VLAN with rapid convergence? → RPVST+ (Rapid PVST+).
2. What three attributes must match exactly on all switches in the same MST region? → Region name, revision number, VLAN-to-instance mapping.
3. A port receives a superior BPDU and gets put into root-inconsistent state. Which guard? → Root Guard.
4. Which guard err-disables an access port that receives any BPDU? → BPDU Guard.
5. What is the default RSTP cost of a 1-Gbps link using long path cost? → 20,000.
6. Compare BPDU Guard vs BPDU Filter. → Guard shuts the port on BPDU receipt; Filter silently drops BPDUs, disabling STP on that port.
7. UDLD aggressive vs normal — what does aggressive add? → Aggressive err-disables the port after repeated unanswered hellos; normal only logs.
8. Which RSTP port role is the next-best replacement for a Root Port? → Alternate.
9. Which RSTP port role offers a backup path to a segment from the same switch? → Backup.
10. What is the result of running `spanning-tree portfast` on a trunk-mode port? → No effect (ignored unless you use `portfast trunk`).
11. In MST, which instance carries VLANs that are not explicitly mapped to any user instance? → MSTI 0 (IST — Internal Spanning Tree).
12. A network admin sees a port stuck in `loop-inconsistent`. Which feature triggered it and why? → Loop Guard — stopped hearing BPDUs on a non-designated port, suspects unidirectional link.
13. Which feature is recommended on point-to-point fiber uplinks for unidirectional link detection? → UDLD aggressive (or single-strand detection).
14. What command sets a switch as primary root for VLAN 10? → `spanning-tree vlan 10 root primary` (sets priority 24576).
15. After enabling RPVST+, which STP timers convergence-tune the protocol? → Hello (2s), Forward Delay (15s), Max Age (20s) — RSTP also adds proposal/agreement handshake removing reliance on timers.

---

## Week 3 — VLANs, Trunks, EtherChannel

### Notes

- **Trunk encapsulation:** 802.1Q (industry standard, 4-byte tag, native VLAN untagged) vs ISL (Cisco legacy, removed from modern code).
- **DTP modes:** dynamic auto / dynamic desirable / trunk / access / nonegotiate. Best practice: hardcode mode and `switchport nonegotiate`.
- **Native VLAN mismatch** = CDP/STP warnings + potential VLAN hopping. Always tag native or set to unused.
- **VTP** v1/v2 share VLAN DB cluster-wide (risky), v3 adds primary server concept and supports extended VLANs (1006–4094) and PVLAN/MST propagation. Use *transparent* in production.
- **Voice VLAN** — adds tagged voice + untagged data on same access port via CDP/LLDP-MED.
- **EtherChannel modes:**
  - LACP (`active`/`passive`) — IEEE 802.3ad.
  - PAgP (`desirable`/`auto`) — Cisco proprietary.
  - `on` — static, no negotiation.
- **Load balance:** default src-dst-MAC (L2) or src-dst-IP (L3). 9000-series adds src-dst-IP-port and adaptive load-balance.
- **L3 EtherChannel:** `no switchport` then bring up `interface Port-channel X` and assign IP.

### Anki prompts (Week 3)

1. What is the trunk-mode pair that will NOT form a trunk? → Two switches both in `dynamic auto`.
2. Why is leaving the native VLAN as VLAN 1 untagged risky? → VLAN-hopping via double-tagging (802.1Q-in-Q injection attack).
3. Which VTP version is required to propagate extended VLAN range (1006–4094)? → VTP v3.
4. Which LACP modes form a bundle? → active/active, active/passive. (Not passive/passive.)
5. Which PAgP modes form a bundle? → desirable/desirable, desirable/auto. (Not auto/auto.)
6. What command shows real-time traffic distribution across Po1 members? → `show etherchannel port-channel` + `show interface port-channel 1 etherchannel`.
7. Set Layer-3 port-channel — what's the order of operations? → `no switchport` on members FIRST, then `channel-group X mode active`, then config Po interface IP.
8. Voice VLAN — how does the phone know which tag to use? → CDP or LLDP-MED carries the voice VLAN ID to the phone.
9. EtherChannel min-links — what does `port-channel min-links 2` do? → Po stays down until at least 2 members are bundled.
10. Why might `show etherchannel summary` show flag `s` (suspended)? → Misconfiguration: speed/duplex/VLAN/mode mismatch among bundle members.
11. Default Catalyst 9k EtherChannel hash algorithm? → src-dst-mixed-ip-port (varies by platform — check `show etherchannel load-balance`).
12. Difference between LACP `active` and `passive`? → Active initiates LACPDUs; passive only responds.
13. Which VLAN range is `extended`, requiring VTP v3 or transparent mode? → 1006–4094.
14. Best practice for an inter-switch trunk where you never want fallback to access mode? → `switchport mode trunk` + `switchport nonegotiate`.

---

## Week 4 — OSPFv2 / OSPFv3

### Notes

- **Neighbor states:** Down → Attempt → Init → 2-Way → ExStart → Exchange → Loading → Full.
- **Network types:** broadcast (DR/BDR), point-to-point, NBMA, point-to-multipoint, P2MP non-broadcast, loopback.
- **LSAs:**
  - Type 1 Router (intra-area)
  - Type 2 Network (DR for broadcast/NBMA)
  - Type 3 Summary (ABR — inter-area routes)
  - Type 4 ASBR-summary (location of ASBR)
  - Type 5 External (ASBR redistribution — E1 metric grows, E2 flat)
  - Type 7 NSSA External (NSSA only; ABR converts to Type 5 when leaving area)
- **Stub variants:**
  - *Stub* — no Type 5; default injected.
  - *Totally stubby* — no Type 3 or 5; only default.
  - *NSSA* — Type 5 blocked but Type 7 allowed.
  - *Totally NSSA* — also blocks Type 3.
- **OSPFv3** — runs over IPv6, but supports IPv4 + IPv6 address-families since RFC 5838. Router-id still 32-bit, must be configured manually if no IPv4.

### Anki prompts (Week 4)

1. In which OSPF state are routers exchanging DBDs to decide master/slave? → ExStart (then Exchange for actual DBDs).
2. Which LSA type traverses the entire OSPF domain except stub/NSSA areas? → Type 5 (External).
3. ABR running NSSA Area 1 to backbone Area 0 — what happens to Type 7 LSAs at the ABR? → Translated to Type 5 toward Area 0.
4. Why might two routers stay stuck in ExStart? → MTU mismatch on the interfaces.
5. Default OSPF metric formula? → Cost = reference-bandwidth / interface-bandwidth (default ref-bw 100 Mbps).
6. Command to change ref-bw to 100 Gbps? → `auto-cost reference-bandwidth 100000`.
7. Two OSPF routers on a broadcast LAN — which one(s) form Full with all others? → DR and BDR; DROTHERS form Full only with DR/BDR, 2-Way with each other.
8. What's the LSA type that advertises an ASBR's location to the rest of the domain? → Type 4 ASBR-summary.
9. How is the OSPFv3 router-id chosen when no IPv4 address exists? → Must be manually configured via `router-id A.B.C.D`.
10. Which area type allows ASBR redistribution but not Type 5 LSAs from elsewhere? → NSSA.
11. What command summarizes inter-area routes at an ABR? → `area X range A.B.C.D mask`.
12. What command summarizes external routes at an ASBR? → `summary-address A.B.C.D mask`.
13. What is the default OSPF network type on a serial point-to-point interface? → point-to-point.
14. Filter a specific external prefix from being installed locally on R1? → `distribute-list prefix DENY-X in` under `router ospf 1`.
15. A new neighbor never reaches Full and `show ip ospf neighbor` shows it stuck at Loading. Likely cause? → LSDB synchronization stuck — usually duplicate router-id or corrupted LSA.

---

## Week 5 — BGP

### Notes

- **eBGP vs iBGP** — eBGP between AS, AD 20; iBGP within AS, AD 200. iBGP must be full mesh OR use route reflectors / confederations.
- **Best-path algorithm (mnemonic: W-LLAOMNIIIIN):**
  1. Weight (highest)
  2. Local Preference (highest)
  3. Locally originated (network/aggregate/redistribute)
  4. AS-path (shortest)
  5. Origin (IGP < EGP < incomplete)
  6. MED (lowest, same neighbor AS by default)
  7. eBGP > iBGP
  8. IGP metric to next-hop (lowest)
  9. Oldest eBGP path (stability)
  10. Lowest router-id
  11. Lowest neighbor IP
- **Communities:** well-known (NO_EXPORT, NO_ADVERTISE, LOCAL_AS, INTERNET) + private 32-bit. Must `send-community` per neighbor.
- **Route reflectors:** cluster-id, RR + clients; non-clients still need full iBGP mesh among themselves.
- **Filters apply in order:** route-map > prefix-list > distribute-list > filter-list (AS-path).

### Anki prompts (Week 5)

1. AD of eBGP vs iBGP? → 20 vs 200.
2. Order the top 4 best-path tie-breakers. → Weight, Local-Pref, Locally-originated, AS-path length.
3. What does NO_EXPORT do? → Prevents the prefix from being advertised outside the local AS.
4. Why does iBGP require full mesh by default? → Because iBGP-learned routes are NOT re-advertised to other iBGP peers (split-horizon rule).
5. How do route reflectors break the iBGP full-mesh rule? → They reflect routes from clients to other clients and to non-clients.
6. Which attribute is exchanged ONLY between iBGP peers and never crosses an eBGP boundary? → LOCAL_PREF. (AS_PATH and MED can cross eBGP; MED by default goes one AS hop only.)
7. What command lets multipath BGP install paths with equal AS-path length but different next-hop AS? → `bgp bestpath as-path multipath-relax` (+ `maximum-paths`).
8. The OPEN message exchange happens in which BGP state? → OpenSent → OpenConfirm → Established.
9. A peer is stuck in Active state. Common reasons? → TCP/179 unreachable, ACL blocking, wrong neighbor IP, eBGP multihop missing, update-source mismatch.
10. Difference between `network` statement in BGP vs OSPF? → BGP `network` advertises a specific prefix that must exist in the routing table; OSPF `network` enables OSPF on matching interfaces.
11. What attribute is best for influencing inbound traffic to a multi-homed AS? → AS-path prepending (least surgical), more granular: communities to upstream.
12. What attribute is best for influencing outbound traffic from a multi-homed AS? → Local Preference.
13. Which BGP attribute is non-transitive — only sent to direct eBGP neighbor? → MED (Multi-Exit Discriminator), though by config it can be sent further.
14. Confederation vs RR? → Confederation splits AS into sub-ASes (member ASes), RR keeps single AS but reflects routes.
15. What is a BGP peer-group used for? → Apply common policy and outbound update generation efficiency to many neighbors.
16. Why is `next-hop-self` often used on RR or iBGP edge? → To ensure the iBGP-learned eBGP next-hop is reachable via IGP for all iBGP peers.

---

## Week 6 — Multicast, FHRP, QoS

### Notes

- **Multicast addressing:** 224.0.0.0/4. 224.0.0.0/24 link-local (not forwarded). 232.0.0.0/8 SSM. 239.0.0.0/8 admin-scoped (private).
- **L2/L3 mapping:** copy 23 LSBs of IP into MAC 01-00-5e-... → 5-bit overlap (32-to-1 ambiguity).
- **IGMP:** v2 hosts join; v3 adds source-specific (S,G) filtering. Querier election lowest IP.
- **PIM-SM:** RPT (*,G) via RP, then SPT switch-over per (S,G). RP discovery via static / Auto-RP / BSR.
- **RPF check:** validates incoming interface against unicast route back to source; failure = drop.
- **FHRP:**
  - *HSRP* — Cisco. v1: groups 0–255, MAC 0000.0c07.acXX. v2: 0–4095, MAC 0000.0c9f.fXXX. Active/Standby.
  - *VRRP* — RFC, MAC 0000.5e00.01XX, no Standby state (Master/Backup), preempt on by default.
  - *GLBP* — Cisco, single VIP with multiple active forwarders (load-balanced via AVG/AVF).
- **QoS:** Classification (NBAR2, ACL) → Marking (DSCP/CoS) → Queueing (CBWFQ, LLQ) → Shaping/Policing → Drop (WRED).

### Anki prompts (Week 6)

1. Which multicast range is for Source-Specific Multicast? → 232.0.0.0/8.
2. Why is there 32-to-1 overlap between multicast IPs and MACs? → Only 23 of the 28 host bits map to the MAC.
3. PIM-SM (*,G) tree is rooted where? → At the RP (Rendezvous Point).
4. RPF check — what is checked? → That the incoming interface matches the unicast route back to the source.
5. HSRP virtual MAC OUI for v1? → 0000.0c07.acXX (XX = group).
6. Default HSRP priority and how to make a router Active? → 100; set higher priority + `preempt`.
7. VRRP master/backup vs HSRP active/standby — key difference? → VRRP preempts by default; HSRP does not.
8. GLBP — what does AVG do? → Active Virtual Gateway responds to ARP for the VIP and hands out one of up-to-4 AVF MACs.
9. Difference between PIM-SM and PIM-DM? → SM uses pull/explicit join via RP; DM floods then prunes (rarely used).
10. SPT switchover threshold? → On the last-hop router; default switches after first packet (`ip pim spt-threshold 0`).
11. Trust boundary — where should QoS markings be trusted? → As close to the edge / endpoint as you control (IP phone, AP).
12. LLQ vs CBWFQ? → LLQ adds a priority queue (strict, policed) on top of CBWFQ.
13. WRED purpose? → Probabilistic drop to avoid TCP global synchronization.
14. DSCP value for voice (EF)? → 46.
15. DSCP for signaling (CS3)? → 24.
16. Policing vs shaping? → Policing drops/remarks excess; shaping buffers and delays excess.
17. Which IGMP message reports source-specific group membership? → IGMPv3 membership report with source filter (INCLUDE/EXCLUDE).

---

## Week 7 — IP Services

### Notes

- **NAT terms:** Inside Local / Inside Global / Outside Local / Outside Global.
- **PAT** = NAT overload, ports differentiate flows.
- **NTP:** stratum 0 (atomic), stratum 16 = unsynced. Symmetric vs server/client vs broadcast. `ntp authenticate` required for security.
- **PTP (1588v2):** sub-microsecond accuracy via hardware timestamping; relevant in financial / industrial nets.
- **SNMP v2c vs v3:** v3 adds noAuthNoPriv / authNoPriv / authPriv (HMAC-SHA + AES-128/256).
- **Syslog severity levels:** 0 Emerg, 1 Alert, 2 Crit, 3 Err, 4 Warn, 5 Notice, 6 Info, 7 Debug.
- **IP SLA** — synthetic probes (ICMP, UDP-jitter, HTTP, DNS, DHCP, PathTrace). Pair with Object Tracking for HSRP / static route conditional behavior.
- **NetFlow** — v5 fixed fields, v9 template-based, IPFIX (RFC 7011) standard. **Flexible NetFlow** = user-defined record/exporter/monitor.

### Anki prompts (Week 7)

1. Define Inside Global. → The IP a host on the inside is seen as from the outside (post-NAT).
2. Difference between NAT and PAT? → PAT also translates ports, allowing many-to-one mapping.
3. What stratum is an unsynchronized NTP source? → 16.
4. SNMPv3 authPriv requires which two services? → Authentication (HMAC-SHA/MD5) and Privacy (AES/DES).
5. Syslog severity for "Notification"? → 5.
6. Which IP SLA type measures voice quality (one-way jitter / MOS estimate)? → UDP jitter.
7. IP SLA + track + static route — typical use case? → Conditional default route — drop static if upstream becomes unreachable.
8. NetFlow v9 advantage over v5? → Template-based fields, IPv6 support, MPLS labels, supports newer fields.
9. Flexible NetFlow building blocks? → flow record (match/collect), flow exporter (destination), flow monitor (record+exporter applied to interface).
10. Command to enable PTP boundary clock? → `ptp clock boundary domain X` + per-interface `ptp enable`.
11. Why is `ntp source LoopbackX` recommended? → Sources NTP packets from a stable loopback IP regardless of egress interface.
12. NAT order of operations inside-to-outside? → Routing → NAT translation → encrypt (if VPN). (Outside-to-inside: decrypt → un-NAT → route.)
13. Which DSCP-aware marking can SNMPv3 packets carry? → Whatever the `snmp-server` tos / control-plane marking is set to (default CS6 on management).
14. Difference between SNMP traps and informs? → Informs are acknowledged (reliable); traps are fire-and-forget.

---

## Week 8 — Tunnels & Overlays

### Notes

- **GRE** — IP protocol 47. Adds 24-byte header (4 GRE + 20 IP). Multi-protocol, no encryption.
- **mGRE / DMVPN** — single tunnel, multipoint, NHRP resolves spoke addresses.
- **IPsec** — IKEv1 (Main/Aggressive) vs IKEv2 (single 4-message exchange, simpler). ESP (proto 50) for encryption + auth; AH (proto 51) auth only, rarely used.
- **Transport vs Tunnel mode IPsec:** transport keeps original IP header (used with GRE-over-IPsec); tunnel adds a new outer IP.
- **LISP** — separates *EID* (endpoint id) from *RLOC* (routing locator). Map-Server / Map-Resolver / ITR / ETR roles. Map-cache built on demand.
- **VXLAN** — MAC-in-UDP, UDP/4789, 24-bit VNI (16M segments). Control plane = flood-and-learn (multicast) OR BGP EVPN (VTEP advertises MAC/IP).

### Anki prompts (Week 8)

1. GRE IP protocol number? → 47.
2. Why use GRE-over-IPsec in transport mode? → To avoid double-encapsulating the IP header (saves 20 bytes per packet).
3. IKEv2 vs IKEv1 — how many messages? → IKEv2 = 4 in initial exchange; IKEv1 main = 6, aggressive = 3.
4. DMVPN phase 3 advantage? → Spoke-to-spoke shortcuts via NHRP redirect, scalable hub.
5. LISP role that maintains EID-to-RLOC mappings? → Map-Server.
6. LISP role that handles encapsulation at egress site? → ETR (Egress Tunnel Router).
7. VXLAN VNI bit-length and max segments? → 24 bits, ~16.7M.
8. VXLAN destination UDP port? → 4789.
9. Control plane for "flood-and-learn" VXLAN? → Multicast (one group per VNI typically) + data-plane learning.
10. Control plane for EVPN VXLAN? → MP-BGP carrying L2VPN EVPN AFI/SAFI route types (Type 2 = MAC/IP, Type 3 = IMET, etc).
11. Outer / inner IP of a VXLAN frame from VTEP1 to VTEP2? → Outer: VTEP1 → VTEP2; Inner: original host frame including tenant MAC + IP.
12. Why is MTU 1554 (or higher with EVPN) recommended on underlay? → Account for VXLAN/IPsec encapsulation overhead.
13. Which IPsec mode hides the original source/destination IP? → Tunnel mode.

---

## Week 9 — Wireless

### Notes

- **RF basics:** 2.4 GHz (3 non-overlapping channels 1/6/11 @ 20 MHz), 5 GHz (many channels, DFS bands), 6 GHz (Wi-Fi 6E).
- **Watt math:** dBm = 10·log10(mW). 0 dBm = 1 mW. +3 dB = ×2 power, +10 dB = ×10.
- **AP modes:** Local (default, CAPWAP to WLC), FlexConnect (local switching when WAN down), Sniffer, Monitor, Bridge (mesh), SE-Connect (spectrum), Rogue Detector.
- **WLC deployment:** Centralized (5520/9800 appliance), Embedded (9800 on Catalyst 9300), Cloud (9800-CL), Mobility Express (Aironet — deprecated).
- **CAPWAP:** UDP 5246 (control), UDP 5247 (data). DTLS optional on data, mandatory on control.
- **Roaming:** intra-controller (fastest), inter-controller L2 (same VLAN), inter-controller L3 (mobility tunneling). 802.11r/k/v assist client decisions.
- **Security:** WPA2-PSK (Pre-shared), WPA2-Enterprise (802.1X), WPA3-SAE (Simultaneous Auth of Equals — replaces 4-way handshake), OWE (Opportunistic Wireless Encryption — open with encryption).

### Anki prompts (Week 9)

1. CAPWAP control vs data UDP ports? → 5246 control / 5247 data.
2. Non-overlapping 2.4 GHz channels at 20 MHz width in North America? → 1, 6, 11.
3. AP mode that switches client traffic locally when WAN is down? → FlexConnect (formerly H-REAP).
4. WPA3 personal authentication mechanism? → SAE (Simultaneous Authentication of Equals).
5. dBm value for 100 mW? → 20 dBm.
6. dBm value for 1 W (1000 mW)? → 30 dBm.
7. Which 802.11 amendment introduced fast BSS transition for roaming? → 802.11r.
8. Wi-Fi 6 (802.11ax) headline features? → OFDMA, MU-MIMO uplink+downlink, BSS coloring, target wake time.
9. Roaming type when client moves between APs on different VLANs anchored to same WLC? → Intra-controller L3 (mobility tunneling within one WLC).
10. AP mode that doesn't serve clients and only monitors RF for WIPS? → Monitor mode.
11. Default authentication for OWE? → None (open) — encryption via Diffie-Hellman key exchange.
12. Which standard combines 802.11k/v/r for client-assisted roaming? → 802.11r/k/v together (sometimes called "K-V-R").
13. Catalyst 9800-CL is which deployment? → Cloud / virtual WLC running on KVM, ESXi, AWS, etc.

---

## Week 10 — Architecture, SD-Access, SD-WAN

### Notes

- **2-tier (collapsed core)** for small campus; **3-tier** access/dist/core for large.
- **SD-Access:**
  - *DNA Center* — controller / GUI / Assurance.
  - *ISE* — identity, SGT assignment.
  - *Fabric Edge* — first-hop, encapsulates into VXLAN.
  - *Fabric Border* — egress to outside (Internet/WAN).
  - *Fabric Control Plane (LISP map-server)* — keeps EID-to-RLOC mappings.
  - Underlay typically IS-IS (Cisco's recommendation), overlay VXLAN + LISP + Cisco TrustSec SGT.
- **SD-WAN (Catalyst SD-WAN / Viptela):**
  - *vBond* — orchestration, public-reachable, authenticates devices.
  - *vManage* — management & policy.
  - *vSmart* — control plane, runs OMP.
  - *cEdge / vEdge* — data plane.
  - **OMP route types:** vRoute (prefix), TLOC (transport locator), Service (VAS).
  - **Tunnels** — IPsec or GRE over any underlay (MPLS, Internet, LTE).

### Anki prompts (Week 10)

1. SD-Access component that runs LISP Map-Server? → Fabric Control Plane node.
2. SD-Access overlay encapsulation? → VXLAN (with VNI + Group Policy ID for SGT).
3. SD-WAN plane that authenticates new devices and helps with NAT traversal? → vBond (orchestration plane).
4. SD-WAN plane running OMP and distributing policy? → vSmart (control plane).
5. OMP route type carrying transport location info? → TLOC route.
6. Why use IS-IS as the SD-Access underlay? → CLNS-based (no IP needed for control), fewer LSPs than OSPF for large fabrics, vendor-friendly.
7. Cisco TrustSec SGT — where assigned? → At authentication time by ISE based on identity.
8. Cisco's two-box collapsed-core variant where two physical switches act as one? → StackWise Virtual (or VSS on legacy).
9. Catalyst SD-WAN's name before Cisco acquired Viptela? → Viptela / vEdge.
10. cEdge vs vEdge — main difference? → cEdge runs IOS-XE; vEdge runs Viptela OS.
11. SD-Access policy plane for segmentation? → Cisco TrustSec SGTs + SGACL (Group-Based Access Control).
12. DNA Center's role in SD-Access? → Centralized provisioning, automation, assurance, and policy admin.

---

## Week 11 — Network Assurance

### Notes

- **SPAN local, RSPAN across switches via dedicated VLAN, ERSPAN encapsulated in GRE across L3 boundaries.**
- **Model-Driven Telemetry** — push-based, replaces SNMP polling. Encodings: kvGPB (gRPC), JSON-IETF, XML. Subscription types: periodic, on-change.
- **gNMI / gNOI** — gRPC-based config + telemetry.
- **DNA Center Assurance** — health scores, AI Endpoint Analytics, MRE (Machine Reasoning Engine).

### Anki prompts (Week 11)

1. SPAN session destination port restrictions? → No L2/L3 protocols processed on it; it only outputs mirrored frames.
2. RSPAN VLAN special configuration? → `vlan X / remote-span`.
3. ERSPAN transport protocol? → GRE (proto 47) across IP.
4. Model-Driven Telemetry encoding optimized for size? → kvGPB (Google Protocol Buffers key-value).
5. Telemetry subscription mode that pushes when value changes? → on-change (vs periodic).
6. gNMI is built on which RPC framework? → gRPC.
7. SNMP polling vs telemetry push — main scalability gain? → Streaming telemetry reduces polling overhead and provides near-real-time updates.
8. Which DNA-C component derives root cause from logs? → Machine Reasoning Engine (MRE) under Assurance.

---

## Week 12 — Infrastructure & Device Security

### Notes

- **AAA = Authentication / Authorization / Accounting.**
- **TACACS+** TCP/49, encrypts entire payload, separates A/A/A. **RADIUS** UDP/1812 auth + 1813 acct, encrypts only password, combines authn+authz.
- **CoPP (Control-Plane Policing)** — QoS-style policy applied to packets bound for the route processor.
- **MPP (Management Plane Protection)** — restricts which interfaces accept SSH/SNMP/etc.
- **uRPF** strict (input interface must match RPF) vs loose (just any route).
- **DHCP snooping** — DB of MAC/IP/port from DHCPACK; trusted ports = uplinks. **DAI** validates ARP against snooping DB. **IPSG** drops traffic with src IP not in binding table.
- **Port-security** — limit MACs per port, violation actions: protect / restrict / shutdown.

### Anki prompts (Week 12)

1. TACACS+ vs RADIUS transport? → TCP/49 vs UDP/1812+1813.
2. Which protocol encrypts the entire AAA payload? → TACACS+.
3. Why prefer TACACS+ for device admin and RADIUS for end-user 802.1X? → TACACS+ separates A/A/A (per-command authorization), RADIUS handles network-access policy and accounting at scale.
4. CoPP applied with which command? → `control-plane / service-policy input <name>`.
5. uRPF strict mode requirement? → Source must be reachable via the exact incoming interface.
6. DAI relies on which other feature for binding info? → DHCP snooping binding table (or static ARP ACLs).
7. Port-security violation action that drops traffic but doesn't err-disable? → `restrict` (logs + counts).
8. SSH version required for ENCOR best practice? → SSHv2 only (`ip ssh version 2`).
9. Privilege level for read-only show access? → typically 1 (default exec) or custom level 5/7 with `privilege exec level X command`.
10. Storm-control action options? → trap, shutdown (and just log via threshold).
11. PVLAN port types? → Promiscuous, isolated, community.
12. CoPP policy class for routing protocols should be? → permit with a higher rate; example: OSPF/BGP/EIGRP traffic to RP.
13. What command shows DHCP snooping bindings? → `show ip dhcp snooping binding`.

---

## Week 13 — Network Security: ZBFW, MACsec, TrustSec, 802.1X

### Notes

- **Zone-Based Firewall (ZBFW):** zones contain interfaces, zone-pairs define direction, policy-maps inspect.
- **MACsec (802.1AE):** L2 hop-by-hop encryption, AES-GCM-128/256. **MKA (802.1X-2010)** distributes keys.
- **Cisco TrustSec:** SGT tag in CMD field (Cisco Meta Data) or inline in VXLAN GBP. SGACL enforces policy by SGT pair.
- **802.1X:** Supplicant ↔ Authenticator (switch) ↔ Authentication Server (RADIUS/ISE). EAP methods: EAP-TLS (cert), PEAP, EAP-FAST.
- **MAB (MAC Authentication Bypass)** for non-802.1X devices.
- **Web-Auth** as fallback portal.

### Anki prompts (Week 13)

1. ZBFW — what defines direction of traffic flow? → A `zone-pair security` (source zone, destination zone).
2. Default ZBFW policy between two zones with no zone-pair? → Implicit deny — all dropped.
3. MACsec layer of operation? → Layer 2, hop-by-hop.
4. MKA protocol number / EtherType? → EAPoL (0x888E), with MKA-specific PDU.
5. Where in a frame is the SGT carried inline? → Cisco Meta Data (CMD) field added to the Ethernet frame (proto type 0x8909).
6. 802.1X EAP method using mutual cert authentication? → EAP-TLS.
7. PEAP vs EAP-FAST distinguishing trait? → PEAP needs server cert; EAP-FAST uses PAC (Protected Access Credential).
8. MAB and 802.1X — which is tried first by default? → 802.1X (timeout) → MAB fallback (`authentication order dot1x mab`).
9. Cisco ISE's role in TrustSec? → Authentication server, SGT classification, policy push, SGACL distribution.
10. Catalyst NAD command that disables 802.1X re-auth timer? → `authentication timer reauthenticate server` to use RADIUS-pushed value, or explicit `authentication timer reauthenticate <s>`.
11. ZBFW class-map type? → `class-map type inspect match-any/match-all NAME`.
12. Cisco Umbrella's main function? → DNS-layer security / cloud-delivered SIG.

---

## Week 14 — Virtualization & Automation/Programmability

### Notes

- **Hypervisors:** Type-1 (bare metal — ESXi, KVM, Hyper-V) vs Type-2 (hosted — VMware Workstation).
- **VRF-Lite:** local-only VRF (no MPLS). MPLS L3VPN adds MP-BGP VPNv4.
- **Data formats:** JSON (most common in REST), XML (NETCONF), YAML (Ansible). YANG = data modeling language for NETCONF/RESTCONF/gNMI.
- **NETCONF** — XML over SSH (port 830). Datastores: running, candidate, startup. Operations: get, get-config, edit-config, copy-config, lock, unlock, close-session.
- **RESTCONF** — HTTP(S), JSON or XML payload, simpler than NETCONF, no candidate-by-default but supported on IOS-XE 17.x.
- **Tools:** Ansible (agentless, YAML playbooks, push), Puppet/Chef (agent-based, pull), Terraform (declarative infra-as-code).
- **EEM** — Event Manager applets (CLI) or scripts (Tcl). Triggers: syslog pattern, counter, snmp, timer, cli command.
- **Cisco DNA Center API** — REST with Bearer-token auth, used to programmatically push intent.

### Anki prompts (Week 14)

1. NETCONF transport and port? → SSH, 830.
2. NETCONF operation that merges config into running? → `edit-config` with `merge` operation (default).
3. Which datastore allows multi-step config before commit? → `candidate` (with `feature candidate-datastore` enabled).
4. RESTCONF default media type for JSON? → `application/yang-data+json`.
5. YANG building block for hierarchical config? → `container` and `list`.
6. Ansible "agentless" — what mechanism does it use to push to Cisco? → SSH (network_cli) or HTTPS (NETCONF/RESTCONF) — no agent on device.
7. Difference between Puppet/Chef and Ansible deployment model? → Puppet/Chef = agent-based pull; Ansible = agentless push.
8. EEM event detector that watches for log strings? → `event syslog pattern "..."`.
9. EEM event detector that runs on a schedule? → `event timer cron / countdown / absolute`.
10. Cisco DNA Center authentication flow? → POST `/dna/system/api/v1/auth/token` with Basic auth → returns `X-Auth-Token` for subsequent calls.
11. Which Python lib is most common for NETCONF? → `ncclient`.
12. Which Python lib is most common for REST? → `requests`.
13. Idempotent — define it in automation context. → A task that, when run multiple times, produces the same end state with no additional changes after the first successful run.
14. YANG-push subscription pushing on data change is which type? → on-change subscription.
15. Difference between gNMI and NETCONF? → gNMI uses gRPC + Protobuf, faster and binary; NETCONF uses SSH + XML.

---

## Final-week review checklist

- [ ] All weeks marked "confident" in `ENCOR-Progress-Tracker.csv`.
- [ ] Re-run every lab marked "Needs review".
- [ ] Read OCG Ch. 30 *Final Preparation* and the Pearson Test Prep "Practice Exam Mode" once at exam pacing (90 questions / 105 min).
- [ ] Skim OCG Ch. 31 *ENCOR 350-401 Exam Updates* for any v1.1→v1.2 deltas.
- [ ] Confirm exam center / online proctoring + ID + quiet room ≥ 48 hours before exam date.

Good luck — when you finish a week, report back as: *"Week N done, videos N1–N2 watched, labs X/Y/Z complete, notes section drafted, confident on A/B, needs review on C"* and I'll update the tracker.
