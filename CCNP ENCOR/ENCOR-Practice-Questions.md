# ENCOR 350-401 — Practice Questions

**Scope:** 6 ENCOR domains × 3 blocks (A/B/C) × 10–15 scenario-style questions each.
**Origin:** Every question is original and scenario-focused. None are copied from the real exam, exam-dump sites, or the OCG question bank. Use them as drills to test reasoning, not as a substitute for the official Pearson Test Prep questions in the book.
**Format:** Answers appear at the end of each block with detailed explanations including *why each wrong option is wrong*, not just why the right one is right.

**Domain index**

1. [Architecture](#1-architecture)
2. [Virtualization](#2-virtualization)
3. [Infrastructure](#3-infrastructure)
4. [Network Assurance](#4-network-assurance)
5. [Security](#5-security)
6. [Automation & Programmability](#6-automation--programmability)

---

# 1. Architecture

## Block 1A — Campus design & hardware redundancy (12 questions)

**1A.1** A regional bank's campus has two access switches, two distribution switches, and two core switches. The architect collapses the design so distribution and core are one layer. What is this design called, and what is the most common trade-off?
- A. Two-tier collapsed core; trades scalability for lower cost and simplicity.
- B. Spine-and-leaf; trades east-west bandwidth for fewer hops.
- C. Three-tier hierarchical; trades cost for resiliency.
- D. SD-Access fabric; trades manual config for controller dependency.

**1A.2** A network architect places StackWise Virtual between two Catalyst 9500s in the distribution layer. From the perspective of access switches connected with LACP, what does the SVL pair appear as?
- A. Two independent neighbors running VRRP.
- B. One logical neighbor — both physical chassis share the same LACP system ID.
- C. Two neighbors elected via HSRP active/standby.
- D. One logical neighbor only if MEC is disabled.

**1A.3** Which dual-active detection (DAD) mechanism uses the StackWise Virtual Link itself plus an additional physical link with PAgP+ enhancements?
- A. Fast Hello DAD
- B. ePAgP DAD
- C. BFD DAD
- D. UDLD DAD

**1A.4** Which traffic profile most strongly justifies a three-tier design over a two-tier collapsed-core design?
- A. East-west between two access closets in a small office.
- B. A large campus with many distribution blocks where the core must remain switching-only.
- C. A branch site with one distribution and four access switches.
- D. A data center with leaf-spine topology already in place.

**1A.5** An engineer wants to ensure that if one supervisor on a Catalyst 9600 fails, the other takes over without losing forwarding state. Which feature provides hitless switchover with zero packet loss for established flows?
- A. SSO with NSF
- B. HSRP preempt
- C. BFD echo
- D. Graceful Restart on BGP only

**1A.6** A campus is being designed for an enterprise where the distribution and access switches must remain Layer 2 below the distribution because of a legacy application. What is the most important consideration?
- A. Place HSRP/VRRP at the distribution layer for first-hop redundancy.
- B. Place HSRP/VRRP at the access layer.
- C. Run OSPF on the access switches.
- D. Run BGP between access and distribution.

**1A.7** Which statement most accurately describes the *core* layer's responsibility in a three-tier campus design?
- A. Apply ACLs and policy enforcement.
- B. Terminate user VLANs and provide first-hop gateway.
- C. Provide high-speed, low-latency switching with minimal processing.
- D. Aggregate access switches and run STP root.

**1A.8** A network designer is choosing between SSO+NSF and graceful BGP restart for control-plane resiliency on a router with redundant supervisors. Which statement is true?
- A. SSO+NSF replaces the need for graceful restart on routing protocols.
- B. Graceful restart is a peer-signaled extension that complements SSO+NSF — both are usually needed.
- C. Graceful restart only applies to OSPF, not BGP.
- D. SSO+NSF requires both supervisors to be active simultaneously.

**1A.9** In a campus design, you observe that the spanning-tree root for VLAN 10 is an access-layer switch. What is the most likely problem?
- A. A new switch with a lower (better) bridge priority was added without hardening.
- B. Root Guard was misconfigured on the distribution uplinks.
- C. RPVST+ is not enabled.
- D. The access switch is functioning correctly; this is by design.

**1A.10** Which campus block is most appropriate to apply BPDU Guard?
- A. All trunk ports between core and distribution.
- B. Distribution uplinks toward the WAN edge.
- C. Edge access ports configured with PortFast.
- D. Server-facing trunks toward the data center.

**1A.11** A new Catalyst 9300 stack of 4 members has the following stack priorities: SW1=1, SW2=5, SW3=10, SW4=1. After a reboot, which switch becomes Active?
- A. SW1 (first in the stack)
- B. SW3 (highest priority)
- C. SW4 (lowest member number tie-breaker)
- D. None — the priorities are invalid.

**1A.12** Which feature on Catalyst 9000 platforms allows a TCAM region to be reallocated between IPv4 routes, IPv6 routes, ACLs, and MAC entries?
- A. CEF
- B. SDM templates
- C. SVI carving
- D. ASIC pooling

### Answers — Block 1A

**1A.1 → A.** Collapsing distribution and core into one tier yields a *two-tier (collapsed core)* design — cheaper and simpler, but the converged box must handle both transit and aggregation, limiting scale. Spine-leaf is a DC topology, not a campus reduction; SD-Access is an overlay architecture, not a tier reduction.

**1A.2 → B.** StackWise Virtual presents two chassis as one logical device with a single control plane and a shared LACP system ID, so a downstream switch sees one neighbor when running MEC (Multi-chassis EtherChannel). VRRP/HSRP are unrelated — SVL is hardware-level virtualization, not FHRP. MEC is the *whole point* of SVL, so disabling it (D) breaks the design.

**1A.3 → B.** *ePAgP DAD* extends PAgP to detect a dual-active situation. *Fast Hello DAD* uses a separate dedicated direct link sending hellos. *BFD DAD* uses BFD between the chassis. UDLD is a fiber unidirectional-link detection feature, not a DAD method.

**1A.4 → B.** Three-tier wins when distribution blocks multiply: the core stays pure transit, providing predictable any-to-any switching between many distributions. Small offices, branches, and DC leaf-spine all fit different patterns.

**1A.5 → A.** Stateful Switchover (SSO) keeps Layer 2 and forwarding state synced between supervisors; Nonstop Forwarding (NSF) keeps the data plane forwarding using the FIB while the new control plane re-converges with neighbors using Graceful Restart. HSRP preempt is host-redundancy, not chassis. BFD echo is detection, not failover.

**1A.6 → A.** When access is L2, the L3 boundary is at distribution, so FHRP (HSRP/VRRP/GLBP) lives there. Running HSRP at access layer (B) wouldn't make sense without L3 on access. Running OSPF/BGP at access (C/D) is a different design entirely (routed access).

**1A.7 → C.** The core's job is fast, dumb forwarding — keep it simple, no policy enforcement, no user-VLAN termination, no STP root. Policy (A) and user-VLAN termination (B) belong at distribution. STP root (D) also belongs at distribution.

**1A.8 → B.** SSO+NSF needs Graceful Restart support on the routing protocol so the *neighbor* helps preserve the adjacency during the switchover by maintaining state. GR is supported for OSPF, BGP, IS-IS, and EIGRP, so C is wrong. Both supervisors aren't active simultaneously under SSO (D) — one is active, one is standby.

**1A.9 → A.** An access switch becoming root almost always means someone introduced a switch with a numerically lower bridge priority and Root Guard wasn't applied on distribution downlinks to prevent it. Root Guard misconfiguration (B) usually shows as `root-inconsistent` blocks, not a successful takeover.

**1A.10 → C.** BPDU Guard belongs on *edge access ports with PortFast* — places where you never expect a BPDU. Putting it on trunks between switches (A, B, D) would shut down legitimate inter-switch links the first time STP runs.

**1A.11 → B.** Stack Active election: highest priority wins; lowest MAC address as tie-breaker. SW3's priority of 10 beats everyone. Lowest member number / first member (A, C) aren't election criteria.

**1A.12 → B.** SDM templates carve TCAM regions on Catalyst platforms — `sdm prefer` lets you bias toward more routes, more ACLs, or more MAC entries. CEF (A) is forwarding, not TCAM partitioning.

---

## Block 1B — SD-Access & SD-WAN architecture (12 questions)

**1B.1** In a Cisco SD-Access fabric, which node performs the LISP Map-Server / Map-Resolver function?
- A. Fabric Edge node
- B. Fabric Border node
- C. Fabric Control Plane node
- D. Extended node

**1B.2** SD-Access uses VXLAN as the data-plane encapsulation. What additional field does the SD-Access VXLAN header carry beyond the standard VXLAN-GPO header?
- A. MPLS label stack
- B. Group Policy ID (SGT) and VRF ID (VN)
- C. CAPWAP tunnel ID
- D. GRE key field

**1B.3** Which protocol is Cisco's recommended underlay IGP for SD-Access, and why?
- A. OSPF — fastest convergence
- B. EIGRP — proprietary tuning
- C. IS-IS — CLNS-based, no IP needed for control, scales well
- D. BGP — policy flexibility

**1B.4** In Catalyst SD-WAN, which plane is responsible for authenticating new devices joining the overlay and assisting with NAT traversal?
- A. vManage
- B. vBond
- C. vSmart
- D. cEdge

**1B.5** OMP (Overlay Management Protocol) advertises three primary route types. Which is responsible for representing a transport-side public/private address pair and color?
- A. vRoute
- B. TLOC
- C. Service route
- D. Multicast route

**1B.6** A Catalyst SD-WAN administrator configures two transports — MPLS (color `mpls`) and Internet (color `biz-internet`). For a single cEdge with two TLOCs, how many BFD sessions per remote cEdge will it establish in a full-mesh topology with the remote also having two TLOCs?
- A. 1
- B. 2
- C. 3
- D. 4

**1B.7** Which component pushes templates and centralized policy down to cEdges in Catalyst SD-WAN?
- A. vBond
- B. vSmart
- C. vManage
- D. DNA Center

**1B.8** A network engineer migrating a 1000-site WAN from DMVPN to Catalyst SD-WAN asks which protocol replaces NHRP in the new architecture for spoke-to-spoke reachability.
- A. mGRE with NHRP still required
- B. OMP — vSmart advertises TLOCs, allowing direct IPsec tunnels between sites
- C. BGP-LU
- D. LISP

**1B.9** In SD-Access, what carries the security policy that an SGT (Scalable Group Tag) represents from one fabric edge to another?
- A. The VXLAN header's Group Policy ID field
- B. A separate MPLS label
- C. The CAPWAP control channel
- D. The 802.1Q outer tag

**1B.10** A Fabric Edge boots up and registers its EID prefixes. What happens next?
- A. The Edge floods the prefix to all other Edges directly.
- B. The Edge registers the prefix with the Control Plane Node (Map-Server), which holds the EID-to-RLOC mapping.
- C. The Edge sends a BGP UPDATE to the Border.
- D. The Edge sends an OSPF Type-5 LSA into the fabric.

**1B.11** What is the role of DNA Center (Catalyst Center) in an SD-Access deployment?
- A. It is the data-plane device and must be inline.
- B. It is the controller responsible for automation (provisioning, software image management), policy (with ISE), and assurance.
- C. It is a reporting tool only.
- D. It replaces ISE for identity.

**1B.12** Which is a *limitation* of Catalyst SD-WAN compared to traditional WAN routing?
- A. It cannot use the Internet as transport.
- B. It requires complete reliance on the centralized controllers for some control-plane functions (vSmart for policy/routing distribution).
- C. It cannot run BGP toward LAN.
- D. It does not support IPsec.

### Answers — Block 1B

**1B.1 → C.** The Fabric Control Plane node runs the LISP Map-Server / Map-Resolver. Edges register their EID-to-RLOC mappings to it; Edges query it to forward to remote EIDs. Borders connect the fabric to the outside world; Extended nodes are L2 extensions for IoT.

**1B.2 → B.** SD-Access uses **VXLAN-GPO (Group Policy Option)** which extends standard VXLAN with **Group Policy ID (SGT)** and **VN (VRF) ID** fields, enabling micro/macro segmentation within the fabric. MPLS labels, CAPWAP IDs, and GRE keys are different encapsulations.

**1B.3 → C.** Cisco officially recommends **IS-IS** for SD-Access underlay because it's CLNS-based (no IP needed to form adjacency, useful during automated provisioning), uses TLVs for extensibility, and scales well. OSPF works but is not Cisco's recommended default.

**1B.4 → B.** **vBond** is the orchestrator — it sits on a public-reachable IP, authenticates new devices (via certificates), and facilitates NAT discovery and traversal. vManage is management. vSmart is the control-plane that runs OMP. cEdges are data plane.

**1B.5 → B.** **TLOC (Transport Locator)** routes carry the system-IP + color + encap, advertising "I can be reached here via this transport." vRoutes advertise prefixes (with associated TLOC). Service routes advertise service insertion points.

**1B.6 → D.** Each TLOC on the local side establishes a BFD session with each TLOC on the remote side: 2 × 2 = 4 sessions. (You can restrict via TLOC color restrictions, but the default full-mesh produces 4.)

**1B.7 → C.** **vManage** is where templates live and from which policies are activated. vSmart enforces control-plane policy at the OMP level, but vManage is the GUI/API origin. vBond and DNA Center are unrelated to template push in SD-WAN.

**1B.8 → B.** OMP replaces NHRP. vSmart learns all TLOCs and advertises them; any cEdge can build direct IPsec tunnels site-to-site without an NHRP-like resolution step. mGRE+NHRP (A) is DMVPN, not SD-WAN.

**1B.9 → A.** The SGT travels in the **VXLAN-GPO Group Policy ID field** between fabric edges, allowing the destination edge to enforce SGACL based on source group. MPLS, CAPWAP, and 802.1Q are different mechanisms.

**1B.10 → B.** Edges register EID prefixes (and clients dynamically) to the Map-Server (Control Plane Node). Other Edges resolve EID→RLOC on demand from the Map-Resolver. Direct flooding (A) doesn't happen in LISP. BGP/OSPF (C, D) aren't the SD-Access overlay control plane.

**1B.11 → B.** DNA Center / Catalyst Center is the controller. It automates (PnP, image upgrades, fabric provisioning), integrates with ISE for policy, and provides assurance/AI analytics. It's *not* inline (A), not just reporting (C), and doesn't replace ISE (D) — they work together.

**1B.12 → B.** SD-WAN's design relies on centralized controllers (vSmart in particular) for routing distribution and policy; loss of controller connectivity in extreme outage scenarios is a real architectural concern, although data-plane forwarding continues. It can use Internet (A is wrong), supports BGP service-side (C wrong), and uses IPsec (D wrong).

---

## Block 1C — Wireless deployment architecture (10 questions)

**1C.1** A small office has 5 APs and no dedicated WLC. Which Cisco deployment lets one of the APs act as the controller?
- A. Cisco DNA Spaces
- B. Embedded Wireless Controller on Catalyst 9300
- C. Catalyst 9800-CL
- D. Mobility Express (deprecated for new deployments, used on legacy Aironet)

**1C.2** Centralized WLC deployment uses CAPWAP. Which UDP ports does CAPWAP use for control and data?
- A. 5246 control / 5247 data
- B. 16666 control / 4789 data
- C. 8443 control / 443 data
- D. 5060 control / 5061 data

**1C.3** In a FlexConnect deployment, an AP loses connectivity to the WLC. Which statement most accurately describes what happens to existing wireless clients?
- A. They are immediately deauthenticated.
- B. They continue with local switching for traffic already permitted; new associations may fail unless FlexConnect Local Authentication is configured.
- C. They roam automatically to a neighboring controller.
- D. They lose all connectivity until the WLC returns.

**1C.4** Which AP mode is used for dedicated wireless intrusion detection without serving any clients?
- A. Local
- B. FlexConnect
- C. Monitor
- D. Sniffer

**1C.5** Which AP mode captures packets and forwards them to a remote analyzer (e.g., Wireshark) for protocol analysis?
- A. Monitor
- B. Sniffer
- C. SE-Connect
- D. Rogue Detector

**1C.6** In intra-controller roaming, what happens when a client moves between two APs registered to the same WLC, both anchored to the same VLAN?
- A. The WLC tunnels client data through a mobility anchor.
- B. The client gets a new IP via DHCP.
- C. The WLC updates its client database with the new AP; no IP change, no mobility tunnel needed.
- D. The client is deauthenticated and must re-associate.

**1C.7** A client roams from AP1 (WLC-A, VLAN 10) to AP2 (WLC-B, VLAN 20). Which roaming type is this, and what mechanism preserves the client's IP?
- A. Intra-controller L2; no tunneling needed.
- B. Inter-controller L3; mobility tunnel anchors the client's traffic back to WLC-A.
- C. Inter-controller L2; both VLANs must match.
- D. Roaming is not supported across VLANs.

**1C.8** Which Wi-Fi standard introduced OFDMA and is referred to as Wi-Fi 6?
- A. 802.11ac (Wi-Fi 5)
- B. 802.11ax
- C. 802.11n
- D. 802.11be (Wi-Fi 7)

**1C.9** A wireless designer wants APs to be reachable from the WLC across an L3 network. Which AP-to-WLC discovery method is most resilient and recommended in production?
- A. Broadcast on AP's subnet
- B. DHCP Option 43 with WLC management IPs + DNS resolution of `CISCO-CAPWAP-CONTROLLER.<domain>`
- C. Static IP hardcoded on each AP
- D. mDNS service discovery

**1C.10** What is the primary purpose of Cisco's CleanAir technology on supported APs?
- A. Encrypted control-plane channel between AP and WLC.
- B. Real-time RF spectrum analysis to detect and classify non-Wi-Fi interferers.
- C. Air-gap management for IoT devices.
- D. Hardware-accelerated WPA3 cryptography.

### Answers — Block 1C

**1C.1 → B.** **Embedded Wireless Controller (EWC)** runs on Catalyst 9300 (or 9100 series APs in some cases), turning a switch (or one AP) into a small-site controller without a dedicated 9800 box. DNA Spaces (A) is location/analytics. 9800-CL (C) is a virtual WLC — separate VM, not embedded in an AP. Mobility Express (D) was the Aironet-era equivalent but is end-of-life.

**1C.2 → A.** CAPWAP uses **UDP/5246** for control and **UDP/5247** for data, both with optional DTLS (mandatory on control). The other ports listed belong to other protocols (4789 = VXLAN; 8443 = web admin; 5060/5061 = SIP).

**1C.3 → B.** In FlexConnect, when the AP loses WLC connectivity, *locally switched* WLANs continue to forward client traffic. Existing clients keep going; new associations require **FlexConnect Local Authentication** (e.g., local EAP) to succeed without the WLC.

**1C.4 → C.** **Monitor mode** — AP doesn't serve clients, only listens for WIPS / rogue detection / location. **Sniffer mode** is similar but for packet capture toward an analyzer (next question).

**1C.5 → B.** **Sniffer mode** captures 802.11 frames and forwards them to a defined IP (your laptop running Wireshark with airopeek/peekremote decoder). SE-Connect (C) is for Spectrum Expert/CleanAir integration with the AP.

**1C.6 → C.** Intra-controller, same VLAN → it's basically just an L2 client move; the WLC simply updates its client→AP mapping. No mobility tunnel, no IP change.

**1C.7 → B.** Different WLCs and different VLANs = **inter-controller L3 roaming**. The original WLC (WLC-A) becomes the *anchor*; the new WLC (WLC-B) is the *foreign*. A mobility tunnel between them keeps the client's IP intact by anchoring data back through WLC-A.

**1C.8 → B.** **802.11ax** = Wi-Fi 6 (and 6E for 6 GHz extension), introducing OFDMA, MU-MIMO uplink+downlink, BSS coloring, target wake time. 802.11be is Wi-Fi 7.

**1C.9 → B.** **DHCP Option 43** carries WLC management IPs to the AP via the DHCP server, combined with DNS lookup of `CISCO-CAPWAP-CONTROLLER.<domain>` as a fallback. Broadcast (A) only works on the same subnet. Static IPs (C) don't scale. mDNS (D) is for service discovery, not AP-to-WLC bootstrap.

**1C.10 → B.** **CleanAir** uses purpose-built RF ASICs on supported APs to do real-time spectrum analysis and classify interferers (microwave ovens, Bluetooth, jammers, video bridges, etc.) — beyond what client adapters can see.

---

# 2. Virtualization

## Block 2A — Hypervisors, VRFs, and overlay basics (12 questions)

**2A.1** What is the primary architectural difference between a Type-1 and a Type-2 hypervisor?
- A. Type-1 runs on bare metal; Type-2 runs on a host OS.
- B. Type-1 supports only Windows guests.
- C. Type-2 is faster because it bypasses the kernel.
- D. There is no functional difference.

**2A.2** A Cisco engineer creates a VRF named `RED` on a CSR1000v and assigns Gi2 to it. Which command moves the interface into the VRF without erasing its existing IPv4 address?
- A. `interface Gi2 / vrf forwarding RED`
- B. `interface Gi2 / ip vrf forwarding RED` (note: removing and re-applying IP is required on this platform)
- C. `interface Gi2 / vrf member RED`
- D. The interface cannot be moved while an IP is assigned.

**2A.3** Two VRFs (`CUST-A` and `CUST-B`) on the same PE both have overlapping 10.0.0.0/24 subnets. What design pattern keeps them logically separate but allows controlled leaking between them?
- A. Apply the same RD to both — duplicate routes drop.
- B. Use different RDs and selectively import each other's RTs.
- C. Static routes between VRFs.
- D. Use VRF-Lite with NAT only.

**2A.4** In an MP-BGP VPNv4 advertisement, what is prepended to the IPv4 prefix to make it globally unique?
- A. The Route Target (RT)
- B. The Route Distinguisher (RD)
- C. The Site of Origin (SoO)
- D. The AS path

**2A.5** A GRE tunnel between two CSR1000v devices is up but ping across it fails. The underlay routing is fine. Which is the most likely cause?
- A. Tunnel keepalive is enabled.
- B. Recursive routing — the tunnel destination is being routed through the tunnel itself.
- C. GRE doesn't support IPv4 over IPv4.
- D. MTU mismatch on the underlay.

**2A.6** What is the GRE overhead added to each encapsulated IP packet?
- A. 8 bytes
- B. 20 bytes
- C. 24 bytes (4 GRE + 20 outer IP)
- D. 50 bytes

**2A.7** In a DMVPN Phase 3 design, what is the primary improvement over Phase 2?
- A. Spoke-to-spoke tunnels become possible.
- B. NHRP redirect messages from the hub let spokes establish direct tunnels using a more scalable routing model (e.g., summarization at the hub).
- C. mGRE is replaced by p2p GRE.
- D. IPsec becomes optional.

**2A.8** Which IPsec mode is most commonly paired with GRE to avoid double encapsulation of the IP header?
- A. Tunnel mode
- B. Transport mode
- C. Aggressive mode
- D. Main mode

**2A.9** In LISP terminology, what is the EID?
- A. The provider-allocated, routable locator address.
- B. The endpoint identifier — the host's IP at the source/destination, separate from the underlay locator.
- C. The encryption ID used by IPsec on the underlay.
- D. The MAC address of the host.

**2A.10** Which LISP component is queried by an ITR to resolve an EID into an RLOC?
- A. Map-Server
- B. Map-Resolver
- C. ETR
- D. PETR

**2A.11** A VXLAN frame is sent from VTEP1 (1.1.1.1) to VTEP2 (2.2.2.2) carrying a tenant frame on VNI 10100. Which statement is true about the outer header?
- A. Outer SrcIP = 2.2.2.2, DstIP = 1.1.1.1, UDP/4789.
- B. Outer SrcIP = 1.1.1.1, DstIP = 2.2.2.2, UDP/4789.
- C. Outer is MPLS-encapsulated with VNI as label.
- D. Outer uses GRE with VNI as the GRE key.

**2A.12** Which VXLAN deployment uses BGP EVPN to advertise MAC/IP bindings instead of relying on data-plane flood-and-learn?
- A. BGP EVPN VXLAN
- B. Multicast-mode VXLAN
- C. LISP-only VXLAN
- D. OTV

### Answers — Block 2A

**2A.1 → A.** Type-1 (bare metal): ESXi, KVM, Hyper-V, Xen. Type-2 (hosted on a regular OS): VMware Workstation, VirtualBox. Type-1 is generally faster because there's no host OS layer.

**2A.2 → B.** On IOS-XE, applying `vrf forwarding` removes the interface's existing IPv4 address (IOS warns "Interface ... IP address removed due to enabling VRF"). You must re-apply the IP after moving into the VRF. The newer syntax is `vrf forwarding NAME` (without `ip`), but the behavior is the same — config has to be re-pasted. (D) is wrong because you *can* move the interface, just with this caveat.

**2A.3 → B.** Different RDs make the prefixes unique in MP-BGP; selective import/export of RTs gives surgical control over which routes leak between VRFs. Same RD (A) defeats the purpose. Static routes (C) don't work cleanly between VRFs without `ip route vrf` plumbing. NAT alone (D) is a workaround, not a real solution.

**2A.4 → B.** **RD (Route Distinguisher)** is prepended to make a globally unique 64+32 = 96-bit VPNv4 prefix. **RT (Route Target)** is a BGP extended community that controls import/export. **SoO** is for loop prevention.

**2A.5 → B.** **Recursive routing** is the classic GRE failure mode: a route to the tunnel destination ends up pointing back through the tunnel, causing the tunnel to bounce up/down. The router will usually log "%TUN-5-RECURDOWN". MTU issues (D) are common but typically allow ping with small packets while breaking TCP.

**2A.6 → C.** **24 bytes total**: 4-byte GRE header + 20-byte new outer IP header. Useful for path-MTU calculations.

**2A.7 → B.** DMVPN Phase 3 introduces **NHRP redirects from the hub**, so the hub can summarize routes and still inform spokes that a more direct path is available, allowing spoke-to-spoke tunnels and route summarization. Phase 2 already supports spoke-to-spoke (A), but requires full reachability info on each spoke (no summarization).

**2A.8 → B.** **Transport mode** keeps the original IP header (the GRE encapsulation already added a new outer header), avoiding the double-outer-header overhead of tunnel mode. Tunnel mode would add yet another IP header.

**2A.9 → B.** **EID (Endpoint Identifier)** = the host's IP/MAC in the user's address space. **RLOC (Routing Locator)** = the underlay-routable address used to reach the EID's site.

**2A.10 → B.** ITR queries the **Map-Resolver** for EID→RLOC mappings. **Map-Server** is where ETRs register their EID prefixes. Often Map-Server and Map-Resolver run on the same box.

**2A.11 → B.** The outer header sources from the originating VTEP to the destination VTEP, UDP/4789. Always SrcIP=local VTEP, DstIP=remote VTEP. (A) is reversed.

**2A.12 → A.** **BGP EVPN VXLAN** uses MP-BGP L2VPN EVPN AFI/SAFI to advertise Type-2 (MAC/IP), Type-3 (IMET for BUM), Type-5 (IP prefix), etc. Multicast-mode VXLAN (B) is flood-and-learn. OTV (D) is a different DCI overlay.

---

## Block 2B — DMVPN, IPsec, VXLAN deep (12 questions)

**2B.1** Which IKEv2 component negotiates the symmetric keys used for the actual data encryption?
- A. Phase 1 (IKE_SA_INIT)
- B. Phase 2 (CREATE_CHILD_SA)
- C. AUTH exchange
- D. NAT-D payload

**2B.2** An engineer configures IKEv1 in aggressive mode for an IPsec VPN. Which is true?
- A. Aggressive mode is preferred because it uses 3 messages instead of 6.
- B. Aggressive mode reveals identities in clear text and is generally discouraged.
- C. Aggressive mode replaces ESP with AH.
- D. Aggressive mode disables Phase 2.

**2B.3** Which IPsec protocol provides confidentiality (encryption) and is the most commonly used today?
- A. ESP (protocol 50)
- B. AH (protocol 51)
- C. ISAKMP (UDP 500)
- D. IKEv2 (UDP 4500)

**2B.4** In a DMVPN hub-and-spoke deployment, what role does NHRP play?
- A. NHRP encrypts the tunnel.
- B. NHRP allows the hub to map a spoke's tunnel-IP (the inside) to its NBMA address (the outside) so others can build dynamic tunnels.
- C. NHRP runs OSPF over GRE.
- D. NHRP replaces BGP between hub and spokes.

**2B.5** A multicast-mode VXLAN deployment uses which control mechanism to learn which VTEP owns which MAC?
- A. BGP EVPN Type-2 routes.
- B. Data-plane flood-and-learn — BUM traffic is sent to a multicast group; remote VTEPs see the source MAC and source VTEP, populating their MAC table.
- C. LISP Map-Server registrations.
- D. Static MAC mappings only.

**2B.6** What field in the VXLAN header distinguishes overlay tenants/segments?
- A. The 24-bit VNI (VXLAN Network Identifier).
- B. The outer VLAN tag.
- C. The TCP source port.
- D. The DSCP value.

**2B.7** Which destination UDP port does VXLAN use, and why was this port chosen over a fixed port for source?
- A. UDP/4789 dest; source is randomized so ECMP hashing in the underlay distributes flows.
- B. UDP/4789 source and dest, fixed.
- C. UDP/5247, randomized source.
- D. UDP/8472, fixed source.

**2B.8** In BGP EVPN VXLAN, which route type carries individual MAC/IP host advertisements?
- A. Type 1 (Ethernet Auto-Discovery)
- B. Type 2 (MAC/IP Advertisement)
- C. Type 3 (Inclusive Multicast Ethernet Tag)
- D. Type 5 (IP Prefix)

**2B.9** Which BGP EVPN route type advertises a VTEP's interest in a given VNI's BUM traffic (used to build the multicast/ingress-replication tree)?
- A. Type 1
- B. Type 2
- C. Type 3
- D. Type 4

**2B.10** A network team wants to avoid using multicast in the VXLAN underlay. Which BGP EVPN mechanism replaces multicast for BUM replication?
- A. Anycast routing
- B. Ingress replication (head-end replication) — the sending VTEP unicasts a copy to each remote VTEP advertised via Type-3 routes
- C. PIM-SSM only
- D. MSDP between fabrics

**2B.11** An engineer configures `tunnel mode ipsec ipv4` on a VTI (Virtual Tunnel Interface) between two CSR1000v sites. Which is true about a VTI compared to a classic crypto-map?
- A. VTI is route-based: any traffic routed into the tunnel interface is encrypted; no crypto ACL needed.
- B. VTI requires a crypto ACL just like crypto-map.
- C. VTI doesn't support dynamic routing protocols.
- D. VTI is the same as a GRE tunnel.

**2B.12** Which feature allows multiple VRFs on a single device to share a common physical interface and IP, with each VRF maintaining its own routing table?
- A. VRF-Lite (with subinterfaces or shared interface as inside)
- B. SD-Access overlay
- C. MPLS L3VPN
- D. EVPN

### Answers — Block 2B

**2B.1 → B.** IKEv2 Phase 2 (CREATE_CHILD_SA) negotiates the IPsec SA, including the symmetric keys derived from the Phase 1 SKEYID. Phase 1 (IKE_SA_INIT) sets up the IKE SA itself.

**2B.2 → B.** Aggressive mode is fewer messages but transmits identities in clear text before authentication is complete, and is vulnerable to offline dictionary attacks on PSKs. It's discouraged; use main mode or IKEv2.

**2B.3 → A.** **ESP (IP protocol 50)** provides confidentiality + authentication + integrity. AH provides only auth/integrity (no encryption) and is rarely used. ISAKMP/IKEv2 are key exchange, not the actual data encryption protocol.

**2B.4 → B.** **NHRP (Next Hop Resolution Protocol)** maps overlay tunnel-IPs to underlay NBMA addresses. Spokes register with the hub (NHS); when one spoke wants to talk to another, it resolves through the hub.

**2B.5 → B.** Multicast-mode VXLAN is **flood-and-learn**: BUM (broadcast/unknown unicast/multicast) traffic is sent to a multicast group in the underlay; remote VTEPs source-learn from arriving frames. BGP EVPN (A) is the *opposite* approach.

**2B.6 → A.** The **24-bit VNI** in the VXLAN header isolates up to ~16M segments. Outer VLAN tags are part of the underlay, not the overlay tenant identifier.

**2B.7 → A.** Destination UDP/4789 (IANA-assigned). The source UDP port is *randomized* (typically a hash of inner flow tuple) precisely so that ECMP load-balancing in the underlay can distribute flows across paths — fixed source would collapse all VXLAN traffic onto one underlay path.

**2B.8 → B.** **Type 2 (MAC/IP Advertisement)** is the workhorse — advertises a MAC, optionally with an IP, for ARP suppression and host mobility. Type 1 is auto-discovery for multi-homing. Type 5 is for L3 IP prefix advertisement.

**2B.9 → C.** **Type 3 (Inclusive Multicast Ethernet Tag, IMET)** is how each VTEP signals which VNIs it's interested in for BUM, building the replication list (for ingress replication) or multicast group join (for multicast-mode).

**2B.10 → B.** **Ingress replication** (a.k.a. head-end replication): the sending VTEP makes N unicast copies, one per remote VTEP listed in the Type-3 advertisements. No underlay multicast required, at the cost of bandwidth on the sender.

**2B.11 → A.** **VTI (Virtual Tunnel Interface)** is route-based — anything routed into the tunnel interface gets encrypted, simplifying routing-protocol support and avoiding the brittle crypto-ACL approach of classic crypto-maps. It is *not* the same as GRE — GRE is a separate encapsulation that you can combine with IPsec.

**2B.12 → A.** **VRF-Lite** keeps multiple routing tables on one device. You can use subinterfaces (802.1Q tagging) or assign physical interfaces to different VRFs. MPLS L3VPN (C) and EVPN (D) are full multi-PE overlays, more than just local virtualization.

---

## Block 2C — Network function virtualization & containers (10 questions)

**2C.1** Which Cisco virtual router platform is most commonly used to deploy a CSR in EVE-NG and in production on KVM/ESXi for branch/cloud use cases?
- A. Catalyst 9800-CL
- B. CSR1000v / Catalyst 8000v
- C. NX-OSv 9000
- D. ASAv

**2C.2** A network engineer wants to host multiple routing instances on a single x86 server, each running a different IOS-XE image version, for lab testing. Which technology is most appropriate?
- A. Containers (Docker)
- B. Type-1 hypervisor (KVM/ESXi) with multiple CSR1000v VMs
- C. Bare-metal install
- D. Routing process IDs

**2C.3** Which container orchestration platform is built into Cisco IOS-XE for hosting third-party applications on devices like Catalyst 9300?
- A. Kubernetes
- B. Application Hosting (Docker-based) on IOX/Guest Shell
- C. OpenStack
- D. Docker Swarm

**2C.4** What is the Guest Shell on IOS-XE used for?
- A. A privileged exec mode for routing changes.
- B. A Linux container (CentOS-based) running on the device for hosting Python scripts, custom tools, and integration with the device's CLI via `dohost`.
- C. A wireless guest network management interface.
- D. A read-only diagnostic shell.

**2C.5** Which is *not* a valid reason to deploy NFV (Network Function Virtualization)?
- A. Run multiple network services (firewall, router, WLC) as VMs on commodity x86.
- B. Reduce hardware-specific procurement and accelerate deployment.
- C. Achieve guaranteed line-rate forwarding equivalent to dedicated ASICs in all cases.
- D. Test services in lab environments before production rollout.

**2C.6** What Cisco platform is purpose-built to run multiple virtualized network functions (router, firewall, WAN optimization) on a single x86 appliance at branch sites?
- A. Catalyst 9300
- B. ENCS (Enterprise Network Compute System) running NFVIS
- C. ASR 9000
- D. UCS C-series with VMware ESXi only

**2C.7** A virtual switch (vSwitch) inside a hypervisor handles VM-to-VM traffic. Which feature is typical of a vSwitch but absent on a physical access switch?
- A. STP participation by default.
- B. Promiscuous mode and direct integration with hypervisor APIs for VM mobility (vMotion).
- C. PoE delivery to VMs.
- D. LACP support.

**2C.8** What is the role of SR-IOV in virtualization?
- A. It encrypts inter-VM traffic.
- B. It allows a physical NIC to present multiple virtual functions (VFs) directly to VMs, bypassing the hypervisor vSwitch for line-rate performance.
- C. It runs BGP between hypervisors.
- D. It replaces SDN controllers.

**2C.9** Why is DPDK (Data Plane Development Kit) often used in virtual routers like CSR1000v / Cat8000v?
- A. To enable encryption.
- B. To bypass the kernel network stack and process packets in userspace for higher throughput.
- C. To provide GUI management.
- D. To replace BGP with a Cisco-proprietary protocol.

**2C.10** An engineer deploys a CSR1000v on a public cloud (AWS) to act as a VPN gateway to on-prem. Which limitation should they specifically watch for?
- A. CSR1000v doesn't support IPsec in cloud environments.
- B. Throughput is licensed (e.g., 100 Mbps, 1 Gbps, 10 Gbps) and constrained by the chosen license tier and VM instance type.
- C. CSR1000v cannot run BGP in AWS.
- D. CSR1000v doesn't support multiple network interfaces.

### Answers — Block 2C

**2C.1 → B.** CSR1000v and its successor Catalyst 8000v are the standard Cisco virtual router platforms for KVM, ESXi, AWS, Azure, GCP. The 9800-CL is the *virtual WLC*. NX-OSv 9000 is for Nexus, not enterprise WAN. ASAv is the virtual ASA.

**2C.2 → B.** A Type-1 hypervisor lets you run multiple isolated CSR VMs, each with its own IOS-XE image. Containers can't run a full IOS-XE kernel. Bare metal would only run one image at a time.

**2C.3 → B.** **IOx Application Hosting** (Docker-based) is built into IOS-XE on Catalyst 9000 (9300 in particular) and on routers like the IR series. **Guest Shell** is one IOx app — a CentOS-based shell.

**2C.4 → B.** Guest Shell is a CentOS Linux container running on the device, with Python, network tools, and the `dohost` command to push CLI back into IOS-XE — used for on-box automation and custom scripting.

**2C.5 → C.** NFV does *not* guarantee ASIC-level line-rate forwarding in all cases; that's actually a *limitation* — virtualized data planes can struggle with very high packet rates compared to purpose-built ASICs. All other options are valid NFV benefits.

**2C.6 → B.** **ENCS with NFVIS** (Enterprise Network Compute System running Cisco's NFV Infrastructure Software) is purpose-built for branch NFV — host multiple VNFs (vEdge, vASA, etc.) on one box. ASR 9000 is service-provider hardware. UCS C-series can do it but isn't branch-purpose-built.

**2C.7 → B.** vSwitches don't run STP by default (they have built-in loop-prevention rules and assume VM uplinks are protected), and they integrate deeply with hypervisor APIs to support live migration. PoE (C) is physical-only. LACP (D) is common to both.

**2C.8 → B.** **SR-IOV (Single-Root I/O Virtualization)** exposes a PCIe device as multiple VFs, each assignable directly to a VM, bypassing the hypervisor's vSwitch for near-line-rate performance — at the cost of losing some vSwitch features (like easy vMotion).

**2C.9 → B.** **DPDK** uses kernel bypass + poll-mode drivers in userspace to dramatically increase packet throughput on virtualized routers and switches. It's why a CSR1000v can push multi-gigabit throughput.

**2C.10 → B.** Cisco virtual routers in cloud are licensed by throughput (AX/SEC tiers, smart licensing). Even if you size a giant VM, the license caps throughput. BGP, IPsec, multiple ENIs are all supported.

---

# 3. Infrastructure

## Block 3A — Layer 2 (STP, VLANs, EtherChannel) (15 questions)

**3A.1** A new access switch was added to a network running RPVST+. After it powered on, multiple VLAN trees re-converged briefly. Which mechanism would prevent this new switch from ever becoming the root bridge?
- A. BPDU Guard on access ports.
- B. Root Guard on the trunk ports of all *other* switches facing this new switch (or globally lowering the legit root's priority well below it).
- C. Loop Guard on all trunks.
- D. UDLD aggressive on uplinks.

**3A.2** What is the default RSTP path cost for a 10 Gbps interface using the long-cost method?
- A. 2
- B. 200
- C. 2,000
- D. 20,000

**3A.3** Two switches both have STP root priority of 4096 for VLAN 10, but neither was intentionally configured as primary. How is the root chosen?
- A. Lower MAC address wins.
- B. Higher MAC address wins.
- C. Lower IP wins.
- D. STP fails to converge.

**3A.4** Which RSTP port role is the standby for the Root Port?
- A. Designated
- B. Alternate
- C. Backup
- D. Blocking

**3A.5** A trunk between SW1 and SW2 carries VLANs 10, 20, 30. The native VLAN on SW1 is VLAN 1, on SW2 is VLAN 99. What is the most immediate symptom?
- A. STP topology change every 2 seconds, CDP "Native VLAN mismatch" warnings, and untagged traffic crossing the wrong VLAN.
- B. Trunk goes down.
- C. All VLAN traffic is silently dropped.
- D. No symptom — RSTP handles it.

**3A.6** A network admin wants the access ports to bypass the listening/learning STP states for end devices. Which feature should they enable, and which pairing is recommended for safety?
- A. PortFast + BPDU Guard.
- B. PortFast + Loop Guard.
- C. UplinkFast + BPDU Filter.
- D. PortFast trunk + Root Guard.

**3A.7** MST requires which three identical configuration items for switches to be in the same region?
- A. Region name, revision number, VLAN-to-instance mapping.
- B. Region name, IP address, VLAN-to-instance mapping.
- C. Domain name, MD5 password, VLAN-to-instance mapping.
- D. Revision number, IST ID, switch priority.

**3A.8** Two switches negotiate EtherChannel via LACP but only 1 of 4 links bundles successfully. The other 3 show `(s)` suspended. Which is the most likely cause?
- A. LACP is incompatible with the IOS version.
- B. Mismatched speed/duplex, VLAN allowed list, or trunk mode among the candidate links.
- C. PAgP is enabled instead of LACP.
- D. Channel-group number differs on both sides (must match).

**3A.9** True or false: the channel-group ID *must* match between the two ends of an EtherChannel.
- A. True — the channel-group ID is exchanged via LACPDU.
- B. False — the channel-group ID is locally significant; only mode/protocol/parameters must align.
- C. True — but only for static `on` mode.
- D. False — the channel-group ID is only used for PAgP, not LACP.

**3A.10** A trunk shows `negotiating` indefinitely. Both sides are configured as `switchport mode dynamic auto`. What's the fix?
- A. Set at least one side to `dynamic desirable` or `trunk` to actively negotiate.
- B. Enable DTP globally.
- C. Disable VTP.
- D. Set both sides to `access` and back to `auto`.

**3A.11** What is the difference between RPVST+ and MST in terms of CPU and convergence behavior on a switch with 1000 VLANs?
- A. RPVST+ runs one STP instance per VLAN (1000 instances), high CPU; MST groups VLANs into instances (e.g., 3-4 instances), much lower CPU.
- B. MST runs per-VLAN; RPVST+ groups.
- C. They are identical in CPU usage.
- D. RPVST+ converges slower than 802.1D.

**3A.12** Which command on a trunk allows only VLANs 10, 20, 30 to traverse, ignoring all others?
- A. `switchport trunk allowed vlan 10,20,30`
- B. `switchport trunk allowed vlan add 10,20,30`
- C. `switchport trunk vlan 10,20,30`
- D. `switchport access vlan 10,20,30`

**3A.13** A loop forms between two switches because a fiber went unidirectional (TX failed on one strand). Which feature detects this and shuts the port?
- A. STP alone — it sees the link as up/up and converges.
- B. UDLD in aggressive mode.
- C. BPDU Guard.
- D. CDP exclusively.

**3A.14** Which VTP version is required for extended VLAN (1006–4094) propagation?
- A. VTP v1
- B. VTP v2
- C. VTP v3
- D. VTP transparent only

**3A.15** Which protocol is best to use between two switches and a downstream firewall that does *not* support PAgP?
- A. LACP
- B. PAgP
- C. Static `on`
- D. Any of the above

### Answers — Block 3A

**3A.1 → B.** **Root Guard** on the *legitimate* root's downstream trunks (or any trunks facing untrusted switches) puts a port in `root-inconsistent` state if it ever receives a superior BPDU. This is the proper defense against an unintentional new switch becoming root. BPDU Guard (A) is for access ports with PortFast, not trunks. Loop Guard (C) defends against unidirectional links, not root takeover.

**3A.2 → C.** Long-cost (802.1t) values: 10 Mbps = 2,000,000; 100 Mbps = 200,000; 1 Gbps = 20,000; **10 Gbps = 2,000**; 100 Gbps = 200.

**3A.3 → A.** With identical priority, **lower MAC address wins** the bridge-ID comparison.

**3A.4 → B.** **Alternate** is the next-best root path on the same switch. **Backup** is a redundant designated port on the same switch (rare — same-segment redundancy).

**3A.5 → A.** Native VLAN mismatch leaks untagged traffic between two different VLANs and triggers STP TC + CDP warnings. The trunk stays up. RSTP doesn't auto-correct (D) — it just complains.

**3A.6 → A.** **PortFast + BPDU Guard** is the canonical pair for edge access ports: skip listening/learning so endpoints come up fast, but instantly err-disable if a BPDU ever arrives (someone plugged a switch in).

**3A.7 → A.** MST region match requires identical region **name**, **revision number**, and **VLAN-to-instance mapping**. Any difference means they're in different regions.

**3A.8 → B.** Bundle suspension on individual member links is almost always due to parameter mismatch — speed, duplex, MTU, allowed VLANs, or trunk mode. The bundle *would* form if it were a protocol incompatibility — instead, the LACPDUs likely succeed but the bundling step rejects mismatched links.

**3A.9 → B.** Channel-group ID is **locally significant**; it's just the local switch's identifier for the Port-channel interface. LACP system-ID/key matter; channel-group number doesn't.

**3A.10 → A.** Two `dynamic auto` ends will both wait for the other — neither initiates DTP. At least one must be `dynamic desirable` (initiates) or hardcoded `trunk`.

**3A.11 → A.** RPVST+ keeps one STP instance per VLAN — at 1000 VLANs, this is huge CPU and memory. MST collapses many VLANs into a handful of instances, dramatically reducing overhead.

**3A.12 → A.** `allowed vlan 10,20,30` replaces the list. `allowed vlan add ...` (B) appends to the existing list. (C) and (D) aren't valid trunk commands in this form.

**3A.13 → B.** **UDLD aggressive** sends hellos and err-disables when no response comes back — detects unidirectional fiber. STP alone (A) might miss it because the link looks up. CDP (D) helps but doesn't err-disable.

**3A.14 → C.** **VTP v3** supports extended VLANs (1006–4094); v1/v2 only support 1–1005.

**3A.15 → A.** **LACP** is the IEEE 802.3ad standard supported by virtually all vendors. PAgP is Cisco-proprietary. Static `on` works but skips negotiation, which is fragile (no detection of misconfig).

---

## Block 3B — IGPs (OSPF, EIGRP) (15 questions)

**3B.1** Two OSPF routers stay stuck in `EXSTART` state. What is the most likely cause?
- A. MTU mismatch on the interfaces.
- B. Different OSPF process IDs.
- C. Mismatched area types.
- D. Different hello/dead timers.

**3B.2** Which OSPF LSA type is generated by an ASBR when redistributing external routes into the OSPF domain?
- A. Type 1
- B. Type 3
- C. Type 5
- D. Type 7

**3B.3** Which LSA type is generated *inside* a NSSA when redistributing external routes locally?
- A. Type 3
- B. Type 4
- C. Type 5
- D. Type 7

**3B.4** When OSPF Type-7 LSAs leave the NSSA at the ABR, what happens?
- A. They are translated into Type-5 LSAs.
- B. They are flooded as Type-7 throughout the OSPF domain.
- C. They are dropped.
- D. They are translated into Type-3 LSAs.

**3B.5** What is the OSPF reference bandwidth's default value, and why is changing it often necessary?
- A. 100 Mbps — modern interfaces all show cost = 1, hiding their relative capacity.
- B. 1 Gbps — modern interfaces show varying costs.
- C. 10 Gbps — default since IOS-XE 17.
- D. 100 Kbps — only legacy serial relevance.

**3B.6** Which OSPF network type elects a DR/BDR and has a default hello interval of 10 seconds?
- A. Broadcast.
- B. Point-to-point.
- C. NBMA (also DR/BDR, but hello = 30s by default — distinct from broadcast).
- D. Point-to-multipoint.

**3B.7** Two OSPF routers on a multi-access broadcast LAN both have priority 0. What happens to DR election?
- A. The router with the highest router-ID becomes DR.
- B. Both refuse to be DR/BDR; no DR is elected — adjacencies remain 2-WAY among them, FULL only with the actual DR/BDR.
- C. They fall back to point-to-point operation.
- D. OSPF fails to start.

**3B.8** Which OSPF area type allows external routes to enter via local ASBR but blocks Type-5 from other parts of the OSPF domain?
- A. Stub
- B. Totally Stubby
- C. NSSA
- D. Totally NSSA

**3B.9** An ABR configured with `area 1 range 10.1.0.0 255.255.0.0` does what?
- A. Summarizes inter-area Type-3 LSAs leaving area 1 into a single 10.1.0.0/16.
- B. Filters Type-5 externals.
- C. Sets a stub area.
- D. Configures virtual link.

**3B.10** Which OSPF feature is used to prevent more-specific external routes from being installed when a summary is in place?
- A. `discard-route external`
- B. `not-advertise` keyword on `area X range`
- C. `auto-cost reference-bandwidth`
- D. `passive-interface default`

**3B.11** A router running EIGRP has a successor with metric 1000 and a feasible successor with metric 1500. The reported distance of the feasible successor is 800. Does it satisfy the feasibility condition?
- A. Yes — RD (800) < FD (1000), feasibility condition met.
- B. No — RD (800) > FD (1000), feasibility condition not met.
- C. No — feasible successors require equal metrics.
- D. Yes — but only because RD < FD by less than 1.

**3B.12** Which EIGRP K-value pairs are used by default, and what does this mean?
- A. K1=K3=1, others=0 — bandwidth and delay influence the metric.
- B. K1–K5 all = 1 — all metrics used.
- C. K2=K4=1 — load and reliability used.
- D. K-values not used by default.

**3B.13** Why must EIGRP K-values match between neighbors?
- A. They're a tiebreaker in the route-selection algorithm.
- B. Neighbors with mismatched K-values won't form an adjacency.
- C. They encrypt the EIGRP packets.
- D. They're cosmetic only.

**3B.14** What is the default OSPF dead timer relative to the hello timer?
- A. 2× hello
- B. 3× hello
- C. 4× hello
- D. Equal to hello

**3B.15** Which OSPF feature lets you authenticate adjacencies using SHA HMAC instead of plain text or MD5?
- A. OSPF Authentication Trailer (RFC 5709) — per-interface or per-area `ip ospf authentication key-chain`.
- B. OSPF doesn't support SHA.
- C. Only IS-IS supports SHA HMAC.
- D. Built-in default since IOS 12.

### Answers — Block 3B

**3B.1 → A.** **ExStart stuck = MTU mismatch.** Routers fail to exchange DBDs because the larger packet doesn't fit on one side. Other causes (process ID, hello/dead, area type) typically prevent the *neighbor* from forming at all, not specifically ExStart.

**3B.2 → C.** **Type 5 (External LSA)** is generated by an ASBR for redistributed routes. Type 4 (ASBR-summary) tells everyone where the ASBR is.

**3B.3 → D.** **Type 7** is the NSSA External — used inside an NSSA where Type-5 isn't allowed. The NSSA ABR translates Type-7 to Type-5 when forwarding into other areas.

**3B.4 → A.** **Translated to Type 5** at the NSSA ABR. (D) Type-3 is for inter-area summaries of intra-area routes, not externals.

**3B.5 → A.** Default reference bandwidth is **100 Mbps**, so anything ≥100 Mbps gets cost = 1. Today's 10G/40G/100G interfaces all show cost 1, breaking shortest-path semantics. Bump it to at least 10 Gbps (`auto-cost reference-bandwidth 10000`).

**3B.6 → A.** **Broadcast** OSPF type: DR/BDR election, hello = 10 s, dead = 40 s. NBMA is also DR/BDR but with hello = 30 s.

**3B.7 → B.** Priority 0 means "ineligible." If *both* routers have priority 0, no DR/BDR is elected between them — they stay 2-WAY (which on a multi-access broadcast LAN is unusual but valid). In a real DR/BDR election, other routers with priority > 0 would win.

**3B.8 → C.** **NSSA** blocks Type-5 from the rest of the OSPF domain entering it, but allows local Type-7 (externals from a local ASBR) to be generated.

**3B.9 → A.** `area X range` at an ABR summarizes Type-3 LSAs leaving area X into a single summary. To do the same for *externals* at an ASBR, use `summary-address`.

**3B.10 → B.** The `not-advertise` keyword on `area X range A.B.C.D MASK not-advertise` suppresses the summary entirely (used to filter). For preventing routing loops with summarization, IOS automatically installs a discard route to Null0 at the summarizing router.

**3B.11 → A.** Feasibility condition: **RD of FS < FD of successor**. 800 < 1000, condition met. FS metric (1500) only matters for installation priority once it becomes successor.

**3B.12 → A.** Default EIGRP K-values are **K1=1, K3=1, others=0**, meaning bandwidth and delay (not load/reliability/MTU) factor into the composite metric.

**3B.13 → B.** **K-value mismatch = no neighbor.** EIGRP enforces matching K-values for adjacency formation, since otherwise neighbors would compute incompatible metrics.

**3B.14 → C.** OSPF dead timer defaults to **4× hello** (10s hello → 40s dead on broadcast; 30s hello → 120s dead on NBMA).

**3B.15 → A.** OSPF supports **HMAC-SHA1/256** via the OSPF Authentication Trailer (RFC 5709), configured via `ip ospf authentication key-chain CHAIN-NAME` with appropriate `cryptographic-algorithm hmac-sha-256` in the key chain.

---

## Block 3C — BGP, FHRP, multicast (12 questions)

**3C.1** Two BGP neighbors are stuck in `Active` state. Which is *not* a plausible cause?
- A. TCP/179 blocked between them by an ACL.
- B. Wrong neighbor IP on one side.
- C. Missing `ebgp-multihop` when peering loopback-to-loopback over multiple hops.
- D. AS number matches — eBGP needs different AS numbers.

**3C.2** Which BGP attribute is used to influence outbound traffic from your AS, and is iBGP-only (not sent across eBGP)?
- A. Weight
- B. Local Preference
- C. AS-path
- D. MED

**3C.3** Which BGP attribute is Cisco-proprietary and locally significant on a single router?
- A. Weight
- B. Local Preference
- C. AS-path
- D. Origin

**3C.4** Compare the BGP best-path algorithm: between two paths with equal Weight, equal Local Preference, equal AS-path length, equal Origin, equal MED, both eBGP — which tiebreaker comes next?
- A. Lowest IGP metric to next-hop.
- B. Oldest path (most stable).
- C. Lowest router-ID.
- D. Lowest neighbor IP.

**3C.5** A network engineer configures `next-hop-self` on an iBGP peer-group. What problem does this solve?
- A. Ensures that prefixes learned from an eBGP neighbor are re-advertised to iBGP peers with the local router's IP as next-hop, eliminating the need for iBGP peers to have a route to the eBGP next-hop.
- B. Encrypts BGP UPDATE messages.
- C. Reduces BGP table memory.
- D. Replaces route reflection.

**3C.6** Route reflectors break iBGP's split-horizon. Which BGP attribute does the RR add to prevent loops among reflected routes?
- A. ORIGINATOR_ID and CLUSTER_LIST
- B. AS-PATH
- C. NEXT_HOP
- D. LOCAL_PREFERENCE

**3C.7** Which HSRP version supports group numbers up to 4095 and uses IPv6?
- A. HSRPv1
- B. HSRPv2
- C. VRRPv2 only
- D. GLBP

**3C.8** A switch is configured with `standby 10 ip 192.168.10.1` and `standby 10 priority 110`. Its peer has default priority. Which router will be active by default, and what enables predictable preemption after a reload?
- A. The higher-priority router is active; `standby 10 preempt` enables preemption.
- B. The lower-priority is active; `standby preempt-delay` enables preemption.
- C. Routers alternate randomly without preempt.
- D. The first to boot wins; preemption is automatic.

**3C.9** Which is true about VRRP vs HSRP?
- A. VRRP is Cisco-only; HSRP is standard.
- B. VRRP preempts by default; HSRP does not.
- C. VRRP supports IPv6 but not IPv4.
- D. HSRP uses multicast 224.0.0.18; VRRP uses 224.0.0.2.

**3C.10** PIM-SM uses an RP for initial group-tree setup. Which feature switches from the shared tree (*,G) to the shortest-path tree (S,G) on the last-hop router?
- A. RP fallback
- B. SPT switchover (default threshold = 0 — switch on first packet)
- C. PIM Dense Mode
- D. MSDP

**3C.11** Which multicast feature lets two RPs in different domains exchange information about active sources?
- A. Anycast RP
- B. MSDP (Multicast Source Discovery Protocol)
- C. Auto-RP
- D. BSR

**3C.12** A multicast designer needs to ensure the RP is highly available without a single point of failure. Which technique provides this within a single PIM domain?
- A. Static RP only
- B. Anycast RP with MSDP between multiple RPs sharing the same IP
- C. BGP-LU
- D. PIM-DM as fallback

### Answers — Block 3C

**3C.1 → D.** AS number matching/mismatching: eBGP requires *different* AS numbers (so D is correctly identifying eBGP needs different AS — meaning matching AS is a problem, but the answer asks which is *not* a plausible cause for being stuck in Active). Re-reading: matching AS for *eBGP* would prevent the session from forming at all, so it *is* a possible cause; but `Active` typically indicates TCP can't establish. Most precisely: stuck-in-Active means *we tried to open TCP/179 and failed*. So (A), (B), (C) are all classic TCP-can't-establish causes; (D) "AS number matches — eBGP needs different AS" is the *odd one* because matching AS for eBGP would actually let the TCP succeed but the OPEN message would be rejected (BGP Notification), not stuck Active. → **D is not a typical cause of stuck-in-Active.**

**3C.2 → B.** **Local Preference** is the AS-wide outbound preference, exchanged only among iBGP peers (not eBGP). Weight (A) is Cisco-proprietary and local-only. AS-path (C) and MED (D) cross eBGP.

**3C.3 → A.** **Weight** is Cisco-proprietary, locally significant, highest wins. It's not advertised to any neighbor.

**3C.4 → A.** After the standard tiebreakers (W, LP, AS-path, Origin, MED, eBGP-over-iBGP), the next is **lowest IGP metric to the BGP next-hop**, followed by oldest eBGP path, lowest router-ID, lowest neighbor IP.

**3C.5 → A.** **`next-hop-self`** rewrites the BGP next-hop to the local router's IP when re-advertising eBGP-learned prefixes to iBGP peers, sparing iBGP peers from needing reachability to external next-hops via IGP.

**3C.6 → A.** **ORIGINATOR_ID** (the original announcer's router-ID) and **CLUSTER_LIST** (the list of RR cluster IDs traversed) are added by RRs to detect reflection loops.

**3C.7 → B.** **HSRPv2** supports groups 0–4095, includes IPv6, uses multicast 224.0.0.102, virtual MAC 0000.0c9f.fxxx. HSRPv1 was IPv4 only, groups 0–255.

**3C.8 → A.** Higher priority wins (110 > 100). **`standby 10 preempt`** is required for the higher-priority router to take over when it reappears after a reload — by default HSRP does *not* preempt.

**3C.9 → B.** **VRRP preempts by default**; HSRP does not. VRRP uses multicast 224.0.0.18, HSRP uses 224.0.0.2 (v1) or 224.0.0.102 (v2). VRRP is RFC standard; HSRP is Cisco.

**3C.10 → B.** **SPT switchover** on the last-hop router moves traffic from the (*,G) shared tree (rooted at RP) to a more efficient (S,G) source-specific tree. Default threshold is `0`, meaning switch after the first packet. Tunable with `ip pim spt-threshold`.

**3C.11 → B.** **MSDP** is the inter-domain multicast source discovery protocol — RPs in different PIM-SM domains exchange Source-Active (SA) messages so receivers in one domain can find sources in another.

**3C.12 → B.** **Anycast RP with MSDP**: multiple RPs share the same IP via anycast routing; MSDP between them ensures each knows about the others' sources. PIM picks the nearest RP by unicast routing. If one fails, traffic re-converges to the next.

---

# 4. Network Assurance

## Block 4A — SPAN/NetFlow/SLA (12 questions)

**4A.1** A network engineer needs to mirror traffic from VLAN 10 on switch SW1 to a packet analyzer plugged into Gi0/5 on the *same* switch. Which monitor session type fits?
- A. SPAN
- B. RSPAN
- C. ERSPAN
- D. Encapsulated SPAN

**4A.2** A SPAN destination port — which is *false* about it?
- A. It doesn't participate in STP.
- B. It can also be a regular access port carrying production traffic.
- C. It receives mirrored copies only.
- D. Some platforms allow it to also receive inbound traffic if `ingress` is enabled.

**4A.3** A switch needs to mirror traffic across multiple switches to a remote analyzer using a dedicated VLAN. Which feature?
- A. SPAN
- B. RSPAN with `remote-span` VLAN configured on all transit switches
- C. ERSPAN
- D. PCAP

**4A.4** Which SPAN variant encapsulates mirrored traffic in GRE and delivers it across L3 boundaries?
- A. SPAN
- B. RSPAN
- C. ERSPAN
- D. Cloud-SPAN

**4A.5** A Flexible NetFlow record is missing critical info — only source and destination IPs are captured. Which `match` statement is most likely missing for full 5-tuple flow info?
- A. `match transport source-port` and `match transport destination-port` (+ `match ipv4 protocol`).
- B. `match interface input`.
- C. `match flow direction`.
- D. `match application name`.

**4A.6** What is the role of the `collect` keyword in a flow record vs `match`?
- A. `match` defines the flow key (what makes a flow unique); `collect` adds non-key data (counters, timestamps, etc.).
- B. `match` collects bytes; `collect` filters.
- C. They are interchangeable.
- D. `collect` defines key fields.

**4A.7** Why is NetFlow v9 generally preferred over v5?
- A. v9 is template-based, supports IPv6, MPLS labels, and Flexible NetFlow extensions; v5 is fixed-field IPv4-only.
- B. v9 is faster but less detailed.
- C. v5 supports IPv6; v9 doesn't.
- D. v9 uses TCP; v5 uses UDP.

**4A.8** An engineer configures an IP SLA probe to check reachability of 8.8.8.8 every 10 seconds and ties it to a track object that conditions a default route. What is this design called?
- A. PBR
- B. Object Tracking with conditional static route — failover when upstream becomes unreachable
- C. NHRP
- D. SLA-based BGP origination

**4A.9** Which IP SLA operation type measures jitter, one-way delay, and packet loss between two routers (both running IP SLA)?
- A. ICMP-echo
- B. UDP-jitter (with responder enabled on the far side)
- C. DNS lookup
- D. HTTP

**4A.10** Why is `ip sla responder` required for accurate jitter measurement?
- A. The responder runs control-plane priority to timestamp packets in/out, allowing the source to compute round-trip and one-way delay accurately.
- B. Without it, ICMP echoes are blocked.
- C. It encrypts the IP SLA packets.
- D. It's not required.

**4A.11** Model-Driven Telemetry — which Cisco-supported encoding is binary and optimized for bandwidth?
- A. JSON-IETF
- B. XML
- C. kvGPB (Google Protocol Buffers, key-value)
- D. CSV

**4A.12** Which transport does gRPC-based telemetry use, and what's its advantage over traditional SNMP polling?
- A. HTTP/2 over TCP — push-based subscriptions, far lower overhead and near-real-time updates compared to SNMP polling.
- B. UDP only — fire and forget.
- C. SNMPv3 underneath.
- D. SMTP for delivery.

### Answers — Block 4A

**4A.1 → A.** Local **SPAN** — source and destination on the same switch.

**4A.2 → B.** A SPAN destination port **cannot also carry production traffic** by default — it's dedicated to receiving mirrored traffic and doesn't run STP/CDP/LACP normally. Some platforms allow `ingress` reception for response traffic (D), but that's a specific opt-in.

**4A.3 → B.** **RSPAN** uses a `remote-span` VLAN trunked across all transit switches.

**4A.4 → C.** **ERSPAN** encapsulates mirrored traffic in GRE for L3 transit. Useful for sending mirror traffic to a centralized analyzer across a routed network.

**4A.5 → A.** Full 5-tuple = source IP, dest IP, source port, dest port, protocol. Missing the transport ports + protocol means flows are only IP-pair granular.

**4A.6 → A.** **`match`** = the key (defines flow uniqueness). **`collect`** = non-key data accumulated for that flow (byte counts, packet counts, timestamps, TCP flags, etc).

**4A.7 → A.** v9 is template-based (sender pushes template, then data records), enabling IPv6, MPLS, IPv4 multicast, MAC, and Flexible NetFlow custom fields. v5 is fixed IPv4 only.

**4A.8 → B.** Classic **Object Tracking + IP SLA + static route** pattern: track 1 monitors SLA 1 reachability; the default route is conditioned on `track 1`. If 8.8.8.8 fails, route is withdrawn — failover.

**4A.9 → B.** **UDP-jitter** measures jitter, one-way delay (if clocks are synced), and packet loss. Requires `ip sla responder` on the far side. Voice quality (MOS) can also be estimated.

**4A.10 → A.** The responder uses control-plane timestamps and high-priority handling to give accurate timing measurements at both endpoints, enabling one-way delay calculation.

**4A.11 → C.** **kvGPB** (key-value Google Protocol Buffers) is the most compact, binary, gRPC-friendly encoding for Cisco MDT. JSON-IETF and XML are human-readable but larger.

**4A.12 → A.** gRPC uses **HTTP/2 over TCP**, push-based subscriptions. Big advantages over SNMP polling: subscribe once and receive periodic or on-change updates, far less overhead per metric.

---

## Block 4B — DNA Center, Assurance, and troubleshooting (12 questions)

**4B.1** Which Cisco platform provides AI-driven assurance, automated path tracing, and machine reasoning across enterprise networks?
- A. Cisco DNA Center (Catalyst Center) Assurance
- B. ASDM
- C. CSM
- D. ISE

**4B.2** What is the Machine Reasoning Engine (MRE) in DNA Center used for?
- A. Encrypting management traffic
- B. Guided root-cause analysis using knowledge-base inference on syslog/SNMP/telemetry data
- C. Replacing OSPF with ML-based routing
- D. Configuring SD-Access fabrics

**4B.3** An engineer needs to find which switch port a misbehaving host is connected to and identify the path it takes through the network. Which DNA Center feature most directly helps?
- A. SWIM
- B. Path Trace
- C. SD-Access provisioning
- D. Network Hierarchy

**4B.4** Health scores in DNA Center Assurance are computed from multiple inputs. Which of these is *not* typically one of them?
- A. Telemetry (interface stats, CPU, memory)
- B. Syslog severity
- C. Client onboarding success rates (wireless)
- D. Routing protocol AS-paths

**4B.5** A network admin observes intermittent packet drops on a Catalyst 9300 uplink. Which on-box command shows interface ASIC-level drop counters?
- A. `show interfaces Gi1/0/1 counters errors`
- B. `show platform hardware fed switch active fwd-asic drops`
- C. `show ip interface brief`
- D. `show running-config`

**4B.6** A user reports slow web access. Which sequence of troubleshooting steps best follows a layered approach?
- A. Random debug commands until something matches.
- B. Layer 1 (physical link), Layer 2 (STP/VLAN), Layer 3 (routing/ARP), Layer 4+ (firewall/NAT/DNS), application.
- C. Always start with packet capture.
- D. Reload the router.

**4B.7** A user reports inability to reach 192.168.50.10 from their workstation 192.168.10.5. Pings to the default gateway succeed; pings to a known-working external host succeed; pings to 192.168.50.10 fail. Where should you look first?
- A. The workstation's NIC.
- B. The remote network — either the destination host itself is down/unreachable, or there's a firewall/ACL blocking that specific path.
- C. The local DHCP server.
- D. DNS resolution.

**4B.8** Which command on a Cisco router or switch shows the current ARP table, including incomplete entries?
- A. `show ip arp`
- B. `show mac address-table`
- C. `show interface arp`
- D. `show cef arp`

**4B.9** What does the `debug ip ospf adj` command output?
- A. OSPF DR/BDR election + LSA flooding details
- B. OSPF adjacency state transitions (Init, 2-Way, ExStart, etc.)
- C. OSPF SPF calculation steps
- D. OSPF route installation events

**4B.10** A user reports DNS works but HTTPS to a specific site fails. Pings to that site's IP work. Where is the problem most likely?
- A. Layer 3 routing.
- B. TCP/443 — firewall, proxy, TLS handshake issue, certificate problem.
- C. DNS.
- D. ARP cache poisoning.

**4B.11** What does `show ip route 10.1.1.1` show that `show ip route 10.1.1.0` may not?
- A. The longest-matching route for the specific host 10.1.1.1, including next-hop and outgoing interface details.
- B. The MAC address of the next hop.
- C. The full RIB.
- D. Nothing different.

**4B.12** Which IOS-XE command captures a brief snapshot of all interfaces' status, IPs, and protocol state in one screen?
- A. `show running-config`
- B. `show ip interface brief`
- C. `show interfaces description`
- D. `show vlan brief`

### Answers — Block 4B

**4B.1 → A.** **DNA Center / Catalyst Center Assurance** is Cisco's AI-driven assurance platform. ASDM/CSM are firewall management. ISE is identity.

**4B.2 → B.** **MRE** uses a knowledge base to guide root-cause analysis — given symptoms (logs, telemetry), it walks through diagnostic questions to narrow down probable cause, similar to expert systems.

**4B.3 → B.** **Path Trace** in DNA Center traces packets through the network hop-by-hop, identifying ACLs and policy decisions at each device. **SWIM** is Software Image Management.

**4B.4 → D.** Health scores draw from telemetry, syslog, client experience, throughput, etc. **Routing protocol AS-paths** are operational data but don't typically feed health scores directly.

**4B.5 → B.** `show platform hardware fed switch active fwd-asic drops` (or similar variants) exposes ASIC-level drop counters on Catalyst 9000. `show interfaces ... counters errors` (A) is also useful but at a higher level.

**4B.6 → B.** Layered troubleshooting (bottom-up or top-down) is the disciplined approach. Random commands waste time; capture-first ignores context.

**4B.7 → B.** Local connectivity works (gateway, external host) but a *specific* remote host fails — almost certainly a remote-side issue (destination down, route missing on that side, firewall/ACL blocking that pair).

**4B.8 → A.** **`show ip arp`** lists the ARP table including Incomplete entries (resolution failed). `show mac address-table` is the L2 forwarding DB.

**4B.9 → B.** `debug ip ospf adj` shows adjacency state changes — useful for diagnosing stuck-in-INIT, ExStart, etc.

**4B.10 → B.** DNS works (ping by name resolves) and L3 works (ping succeeds). Failure on HTTPS but not ICMP usually means TCP/443 is being blocked, intercepted (proxy), or there's a TLS-layer issue (cert mismatch, SNI block).

**4B.11 → A.** `show ip route <specific IP>` performs longest-match lookup and shows the exact route + next-hop + interface for that specific destination. Useful for "what would this router do with a packet to X?"

**4B.12 → B.** **`show ip interface brief`** is the bread-and-butter "everything in one screen" command — interface name, IP, status, protocol.

---

## Block 4C — SNMP, Syslog, NTP (10 questions)

**4C.1** SNMPv3's `authPriv` security level requires which two services?
- A. Authentication (HMAC-SHA/MD5) and Privacy (AES/DES encryption)
- B. Authentication only
- C. Authentication and group membership
- D. Privacy only

**4C.2** Difference between SNMP traps and informs?
- A. Traps are reliable; informs are not.
- B. Informs use acknowledgment (the receiver ACKs); traps are fire-and-forget.
- C. Traps use TCP; informs use UDP.
- D. They are identical.

**4C.3** A syslog message has severity "Notification". Which level number is that?
- A. 3
- B. 4
- C. 5
- D. 6

**4C.4** A network admin wants to log everything from severity 0 (Emergency) through severity 4 (Warning) but suppress Notifications and below. Which command set does this?
- A. `logging trap warnings`
- B. `logging trap notifications`
- C. `logging trap debugging`
- D. `logging buffered notifications`

**4C.5** A device shows stratum 16 for an NTP server. What does that mean?
- A. The server is highly accurate.
- B. The server is unsynchronized (stratum 16 = "unsynchronized" sentinel).
- C. There are 16 reference clocks.
- D. NTP is disabled.

**4C.6** Why is `ntp source LoopbackX` recommended in NTP configurations?
- A. Encrypts NTP packets.
- B. Sources NTP requests/responses from a stable loopback IP that won't change with interface failures, simplifying ACLs and authentication.
- C. Required by RFC.
- D. Reduces stratum number.

**4C.7** What is PTP (Precision Time Protocol, IEEE 1588) used for that NTP isn't?
- A. Sub-microsecond synchronization, often required by financial trading, broadcast video, industrial automation.
- B. Authentication of time sources.
- C. Higher stratum than NTP.
- D. PTP is just newer NTP.

**4C.8** Which NTP role disseminates time to other devices in an enterprise's hierarchy after receiving from upstream public servers?
- A. Stratum 0
- B. Stratum 1
- C. NTP server (configured as a server for downstream clients, often the local stratum 2/3 enterprise time host)
- D. Broadcast slave

**4C.9** Which command on IOS-XE encrypts SNMPv3 user passwords in the running-config?
- A. `service password-encryption`
- B. `snmp-server user U G v3 auth sha PASS priv aes 128 PASS` (the user passwords are automatically stored encrypted/hashed)
- C. They cannot be encrypted.
- D. `key config-key password-encrypt`

**4C.10** An engineer wants to send logging output to a remote syslog server while suppressing console output. Which combination works?
- A. `logging console disable` + `logging host 192.0.2.50` + `logging trap informational`
- B. `no logging console` is sufficient; remote logging is automatic.
- C. `logging trap` alone enables console suppression.
- D. Cisco doesn't support disabling console logging.

### Answers — Block 4C

**4C.1 → A.** **authPriv = Authentication (HMAC) + Privacy (encryption)**. The three SNMPv3 levels are `noAuthNoPriv`, `authNoPriv`, `authPriv`.

**4C.2 → B.** **Informs are acknowledged** (receiver ACKs back to sender), giving SNMP something resembling reliability. **Traps are fire-and-forget UDP**. Both use UDP.

**4C.3 → C.** Severity 5 = **Notification**. The full list: 0 Emergency, 1 Alert, 2 Critical, 3 Error, 4 Warning, 5 Notification, 6 Informational, 7 Debugging.

**4C.4 → A.** **`logging trap warnings`** logs levels 0–4 (Emergency through Warning). `logging trap notifications` would also include level 5. Lower-severity-number = more severe; the keyword *includes* that severity and everything more severe.

**4C.5 → B.** Stratum **16 = unsynchronized**. Stratum 0 is the reference clock; stratum 1 is directly attached; each subsequent layer increments by 1, up to a max of 15 (16 is the "I'm not synced" marker).

**4C.6 → B.** **Loopback sourcing** prevents NTP packets from sourcing different IPs as physical interfaces flap, simplifying ACLs and authentication on upstream NTP servers.

**4C.7 → A.** **PTP** uses hardware timestamping and a master/boundary/transparent clock hierarchy to achieve **sub-microsecond** precision. Critical for HFT, broadcast, telecom synchronization, industrial control.

**4C.8 → C.** An enterprise typically runs internal NTP servers (stratum 2 or 3) that get time from public stratum-1 sources and serve it down to internal clients. Stratum 0 is the reference clock itself.

**4C.9 → B.** SNMPv3 user auth/priv passwords are stored as **localized keys** (already hashed against the engine ID) — not plaintext. `service password-encryption` (A) is for type-7 weak encryption of other passwords, not SNMPv3.

**4C.10 → A.** Disable console (`logging console disable` or `no logging console`), define remote host, set severity. All three work together.

---

# 5. Security

## Block 5A — AAA, RADIUS, TACACS+ (12 questions)

**5A.1** Which AAA protocol uses TCP/49 and encrypts the entire packet payload?
- A. RADIUS
- B. TACACS+
- C. DIAMETER
- D. Kerberos

**5A.2** A network admin wants per-command authorization for network engineers (i.e., authorize each CLI command individually). Which protocol fits best?
- A. RADIUS
- B. TACACS+
- C. LDAP only
- D. SAML

**5A.3** Why is RADIUS more commonly used for 802.1X end-user authentication than TACACS+?
- A. RADIUS supports EAP natively and was designed for network access policies.
- B. RADIUS uses TCP, more reliable.
- C. TACACS+ is end-of-life.
- D. RADIUS encrypts more than TACACS+.

**5A.4** A router has the following config: `aaa authentication login VTY group RAD local`. The RADIUS server is unreachable. What happens on SSH login?
- A. Login is denied immediately.
- B. Login falls back to the local user database (because of the `local` keyword after `group RAD`).
- C. Login waits forever.
- D. Login succeeds with no password.

**5A.5** Which RADIUS attribute carries the username?
- A. User-Name (attribute 1)
- B. User-Password (attribute 2)
- C. NAS-Identifier
- D. Service-Type

**5A.6** A network admin wants to limit a user to read-only access on a router. Which approach is most common with TACACS+?
- A. AAA exec authorization returning a privilege level (e.g., 1) and a per-command authorization profile permitting only `show` commands.
- B. Local username with no privilege.
- C. Use ACLs on VTY only.
- D. Disable enable secret.

**5A.7** Cisco TrustSec uses SGTs. Where are SGTs typically assigned to user sessions?
- A. At the destination switch.
- B. At ISE during 802.1X authentication, based on user identity, posture, and other attributes.
- C. Hardcoded in DHCP.
- D. By DNS lookup.

**5A.8** What does an SGACL enforce, and where?
- A. A policy matrix based on (Source SGT, Destination SGT) tuples — usually enforced at the egress switch (destination side) where the destination SGT is known.
- B. Per-port ACLs only.
- C. URL filtering.
- D. WLAN-only policies.

**5A.9** Which 802.1X EAP method requires both a server certificate AND a client certificate?
- A. EAP-TLS
- B. PEAP-MSCHAPv2
- C. EAP-FAST with PAC
- D. EAP-MD5

**5A.10** A user fails 802.1X authentication and the port should fall back to MAB. Which configuration order is correct on the switch port?
- A. `authentication order dot1x mab` + `authentication priority dot1x mab` + `mab` + `dot1x pae authenticator`.
- B. `mab` only.
- C. `authentication open` only.
- D. There's no fallback in 802.1X.

**5A.11** What is the purpose of `authentication open` on a switchport?
- A. Disables 802.1X entirely.
- B. Allows traffic before authentication completes (monitor mode), useful during deployment of 802.1X to observe failures without blocking users.
- C. Authenticates by IP only.
- D. Opens a backdoor for admins.

**5A.12** A guest user connects to a captive portal and authenticates via web. Which AAA flow is invoked?
- A. WebAuth (with RADIUS auth on the back end, often via ISE Guest Portal).
- B. 802.1X.
- C. MAB.
- D. TACACS+ command authorization.

### Answers — Block 5A

**5A.1 → B.** **TACACS+** uses TCP/49 and encrypts the entire payload. RADIUS encrypts only the password attribute, on UDP/1812+1813.

**5A.2 → B.** **TACACS+** separates A/A/A — per-command authorization is one of its main strengths. RADIUS technically can but isn't as cleanly designed for it.

**5A.3 → A.** **RADIUS supports EAP** natively and was built for network access (NAS) policies. 802.1X carries EAP, RADIUS transports it to/from the auth server.

**5A.4 → B.** **Fallback to local** when RADIUS unreachable — the `local` keyword after the group is the fallback method. This is the canonical "RADIUS first, local backup" pattern. Without the `local`, login would fail.

**5A.5 → A.** **User-Name (attribute 1)** carries the username in RADIUS messages.

**5A.6 → A.** TACACS+ AAA exec authorization returns a privilege level + per-command authorization. Combined with a profile permitting only `show *` commands, this enforces read-only.

**5A.7 → B.** **ISE assigns SGTs** based on identity + posture + AD group during 802.1X authentication. The SGT travels in the VXLAN-GPO header (in SDA) or via inline tagging in classic networks.

**5A.8 → A.** **SGACLs** enforce a (Source SGT × Destination SGT) policy matrix, usually applied at the **egress switch** (destination side) where it knows the destination SGT.

**5A.9 → A.** **EAP-TLS** requires both server cert and client cert — strongest 802.1X method, common for managed devices. PEAP and EAP-FAST require only server cert; client authenticates with credentials inside the tunnel.

**5A.10 → A.** Define both methods, set order and priority, enable MAB and dot1x on the port.

**5A.11 → B.** **`authentication open`** is "monitor mode" — let traffic flow while 802.1X auth completes (or fails) in parallel. Useful to deploy 802.1X without breaking users while you debug supplicants. Should be removed for enforcement.

**5A.12 → A.** **WebAuth** (Captive Portal) is the typical guest flow — back end is RADIUS to ISE Guest Portal or similar.

---

## Block 5B — Layer 2 hardening & device security (12 questions)

**5B.1** A switch port should accept DHCP server responses only from a known uplink. Which feature, applied to the uplink trunk, enables this?
- A. `ip dhcp snooping trust` on the uplink.
- B. `ip arp inspection trust` on the uplink.
- C. `ip verify source`.
- D. Port-security MAC list.

**5B.2** DHCP Snooping's binding table — what does it contain?
- A. (MAC, IP, VLAN, Interface, Lease Time) tuples for each successfully snooped DHCP lease.
- B. ARP cache.
- C. MAC address table.
- D. Routing table.

**5B.3** Dynamic ARP Inspection (DAI) relies on which other feature for its source-of-truth?
- A. DHCP Snooping binding table (or static ARP ACLs).
- B. CDP.
- C. STP topology.
- D. Routing table.

**5B.4** What does IP Source Guard (IPSG) do that DAI doesn't?
- A. Drops ARP packets with spoofed source.
- B. Drops IP packets whose source IP doesn't match the snooping binding table for that port.
- C. Encrypts DHCP traffic.
- D. Blocks DHCP requests.

**5B.5** Port-security violation modes — which logs and counts violations but does *not* err-disable the port?
- A. shutdown
- B. restrict
- C. protect
- D. None — all err-disable

**5B.6** Which port-security violation mode silently drops violating traffic with no logging or counter increment?
- A. shutdown
- B. restrict
- C. protect
- D. None — all log

**5B.7** Which feature on Catalyst switches limits unicast/multicast/broadcast traffic rate and can shut a port if exceeded?
- A. Storm Control
- B. CoPP
- C. uRPF
- D. Port-security

**5B.8** Control Plane Policing (CoPP) — where is the policy attached?
- A. `control-plane` subconfig with `service-policy input <name>`.
- B. Each interface.
- C. VLAN database.
- D. Routing process.

**5B.9** uRPF strict mode — what does it check on every received packet?
- A. The source IP must be reachable via the exact incoming interface.
- B. The destination IP exists.
- C. The TTL is > 1.
- D. The MAC matches the ARP table.

**5B.10** uRPF loose mode is more permissive — what does it check?
- A. The source IP must have *any* route in the FIB (any interface, not just the incoming one).
- B. The destination IP must match a default route.
- C. The MAC is in the snooping table.
- D. Nothing — loose mode is a no-op.

**5B.11** A private VLAN has port types: promiscuous, isolated, and community. Which port type can communicate with all other ports in the secondary VLANs?
- A. Promiscuous
- B. Isolated
- C. Community
- D. Trunk

**5B.12** A Catalyst switch is configured with `ip ssh version 2` and `transport input ssh`. A user attempts to telnet — what happens?
- A. Telnet connection refused (only SSH transport accepted).
- B. Telnet succeeds but with a warning.
- C. SSH falls back to telnet.
- D. The port shuts down.

### Answers — Block 5B

**5B.1 → A.** **`ip dhcp snooping trust`** on the uplink marks it as trusted to send DHCP server messages. By default, all ports are untrusted (servers blocked); uplinks toward legitimate DHCP servers must be explicitly trusted.

**5B.2 → A.** Snooping binding table: **(MAC, IP, VLAN, Interface, Lease Time)** populated from observed DHCPACKs on untrusted ports.

**5B.3 → A.** DAI uses the **DHCP Snooping binding table** as its truth: for each ARP, check that the (MAC, IP, VLAN, port) matches a binding. Static ARP ACLs are an alternative for non-DHCP hosts.

**5B.4 → B.** **IP Source Guard** validates the source IP (and optionally MAC) of *IP traffic* against the snooping binding — DAI validates only ARP. IPSG catches IP spoofing where DAI catches ARP spoofing.

**5B.5 → B.** **Restrict** drops violating traffic, increments a counter, and generates a violation notification (syslog/SNMP) but doesn't err-disable.

**5B.6 → C.** **Protect** silently drops, no log, no counter. Rarely used because you lose visibility.

**5B.7 → A.** **Storm Control** rate-limits broadcast/multicast/unknown unicast traffic and can shut the port (with `storm-control action shutdown`) if exceeded.

**5B.8 → A.** CoPP attaches under the special **`control-plane`** logical interface with `service-policy input <name>`. The policy classifies traffic destined to the RP and police/permits/drops.

**5B.9 → A.** **uRPF strict**: source IP must be reachable via the **same interface** the packet arrived on. Strict mode breaks asymmetric routing.

**5B.10 → A.** **uRPF loose**: source IP must have any route in the FIB. Lets asymmetric flows pass. Common at SP edge.

**5B.11 → A.** **Promiscuous** ports talk to everyone (used by gateways/firewalls in the primary VLAN). **Isolated** ports talk only to promiscuous. **Community** ports talk to other community ports in the same community + promiscuous.

**5B.12 → A.** **Telnet refused** when `transport input ssh` excludes telnet. Only SSH is accepted. Best practice.

---

## Block 5C — ZBFW, MACsec, NGFW, Cisco security portfolio (11 questions)

**5C.1** ZBFW (Zone-Based Firewall) — what is the default policy between two zones with no zone-pair defined?
- A. Permit all
- B. Implicit deny (drop)
- C. Inspect TCP only
- D. Configurable global default

**5C.2** Which ZBFW class-map type inspects Layer 7 application info (e.g., HTTP, SIP)?
- A. `class-map type inspect`
- B. `class-map type inspect http`
- C. `class-map match-any`
- D. `class-map type qos`

**5C.3** MACsec (IEEE 802.1AE) — at which OSI layer does it operate?
- A. Layer 2, hop-by-hop encryption.
- B. Layer 3.
- C. Layer 4.
- D. Layer 7.

**5C.4** Which protocol distributes MACsec session keys between two devices?
- A. MKA (MACsec Key Agreement, part of 802.1X-2010).
- B. IKEv2.
- C. RADIUS only.
- D. SNMPv3.

**5C.5** A network engineer uses Cisco TrustSec end-to-end. After ISE authenticates a user as `Employee` and assigns SGT 10, how does the destination switch know which SGT applies?
- A. The SGT travels inline in the Cisco Meta Data (CMD) header in the Ethernet frame, or in the VXLAN-GPO header in SD-Access.
- B. The destination switch queries ISE on every packet.
- C. DNS lookup.
- D. SGT is in the DSCP field.

**5C.6** Cisco Umbrella's primary security layer is which?
- A. DNS-layer filtering and SIG (Secure Internet Gateway) — block malicious domains and apply policy at DNS resolution time.
- B. Endpoint AV.
- C. WAN optimization.
- D. SD-WAN orchestration.

**5C.7** Cisco Stealthwatch / Secure Network Analytics' core capability?
- A. Encrypted traffic analysis using NetFlow and ML to detect anomalous behavior without decryption.
- B. SSL VPN gateway.
- C. NAC.
- D. WLAN controller.

**5C.8** Cisco Firepower / Secure Firewall NGFW provides which capabilities beyond a stateful firewall?
- A. NGIPS, application-layer awareness, URL filtering, AMP file inspection.
- B. Routing only.
- C. WAN optimization.
- D. Wireless LAN control.

**5C.9** Cisco AMP (Advanced Malware Protection, now part of Secure Endpoint) does what?
- A. File-trajectory analysis, retrospective detection, malware sandboxing, cloud reputation lookups.
- B. Replaces antivirus signature-only matching with sandboxing + cloud intelligence.
- C. Both A and B.
- D. WLAN guest portal.

**5C.10** Cisco WSA (Web Security Appliance) / Secure Web Appliance is primarily what?
- A. HTTP/HTTPS proxy with URL filtering, malware scanning, and acceptable-use policy.
- B. Email gateway.
- C. Wireless WIPS.
- D. SDN controller.

**5C.11** Cisco ESA (Email Security Appliance) / Secure Email is primarily what?
- A. Mail gateway with anti-spam, anti-phishing, DMARC/SPF/DKIM enforcement, AMP file inspection.
- B. SDN controller.
- C. NGFW for L7.
- D. Captive portal.

### Answers — Block 5C

**5C.1 → B.** **Implicit deny** between zones unless a zone-pair with policy is defined. (Within a zone, traffic flows freely. Across zones with no zone-pair, all dropped.)

**5C.2 → B.** Protocol-specific inspection class-maps like `class-map type inspect http NAME` allow L7 inspection (HTTP method, URL patterns). The generic `class-map type inspect` is L4-aware.

**5C.3 → A.** **MACsec = Layer 2, hop-by-hop** encryption between directly-connected devices (switch-to-switch or host-to-switch). Each hop decrypts and re-encrypts.

**5C.4 → A.** **MKA** is the IEEE 802.1X-2010 key distribution protocol for MACsec. Uses EAPoL frames to negotiate the SAK (Secure Association Key).

**5C.5 → A.** The **SGT travels inline** — either in the Cisco Meta Data (CMD) header inserted into the Ethernet frame (classic networks) or in the VXLAN-GPO header (SD-Access). Switches read the SGT and apply SGACL at egress.

**5C.6 → A.** Umbrella started as OpenDNS — DNS-layer threat blocking. Now extended to full SIG (proxy, CASB, DLP, RBI) but DNS-layer is the foundation.

**5C.7 → A.** **Stealthwatch / Secure Network Analytics** consumes NetFlow + ETA (Encrypted Traffic Analytics) to detect anomalous flows using ML, without needing decryption.

**5C.8 → A.** NGFW = stateful firewall + NGIPS + L7 app awareness + URL filtering + AMP for files + identity awareness.

**5C.9 → C.** Both A and B. AMP/Secure Endpoint does file trajectory, retrospective detection (re-evaluating files days later as new intel arrives), sandboxing (Threat Grid), cloud reputation.

**5C.10 → A.** WSA = web proxy with security features. HTTP/HTTPS inspection, URL filtering, malware scanning, acceptable-use enforcement.

**5C.11 → A.** ESA = email security gateway. Anti-spam, anti-phishing, DMARC/SPF/DKIM, AMP file inspection on attachments, URL rewriting.

---

# 6. Automation & Programmability

## Block 6A — Data formats & APIs (12 questions)

**6A.1** Which data format is most commonly used in REST API payloads for modern Cisco platforms?
- A. JSON
- B. XML
- C. CSV
- D. YAML

**6A.2** Which data format is used by NETCONF for its payloads?
- A. JSON
- B. XML
- C. YAML
- D. Protobuf

**6A.3** Which is *not* a valid JSON data type?
- A. string
- B. number
- C. comment
- D. boolean

**6A.4** YAML is most commonly seen in which Cisco-relevant context?
- A. Ansible playbooks and inventories.
- B. NETCONF payloads.
- C. REST API responses.
- D. SNMP MIBs.

**6A.5** A REST API call uses HTTP method `PUT`. What does it most commonly mean?
- A. Create new resource only.
- B. Replace (or create) the resource at the URL with the provided representation.
- C. Append to resource.
- D. Delete resource.

**6A.6** Which HTTP method partially updates a resource?
- A. GET
- B. POST
- C. PATCH
- D. PUT

**6A.7** A REST call returns HTTP 401. What does this mean?
- A. Bad request (malformed).
- B. Unauthorized — auth missing or invalid.
- C. Forbidden — authenticated but not permitted.
- D. Not found.

**6A.8** A REST call returns HTTP 403. What does this mean?
- A. Bad request.
- B. Unauthorized.
- C. Forbidden — authenticated but not permitted for this resource.
- D. Service unavailable.

**6A.9** Cisco DNA Center / Catalyst Center API authentication flow?
- A. POST /auth/token with Basic credentials → returns X-Auth-Token used on subsequent calls.
- B. SSH-based.
- C. SNMPv3.
- D. SAML only.

**6A.10** Which authentication style does RESTCONF on IOS-XE most commonly use over HTTPS?
- A. HTTP Basic Auth (or NACM-integrated auth) — username:password over TLS.
- B. SSH keys.
- C. SNMP community.
- D. RADIUS direct.

**6A.11** When parsing JSON in Python, which standard library function loads a JSON string into a Python dict?
- A. `json.loads(string)`
- B. `json.dump(file)`
- C. `xml.parse(string)`
- D. `yaml.load(string)`

**6A.12** Which tool is best for interactively testing REST API calls during development?
- A. Postman or curl
- B. SNMPwalk
- C. tcpdump only
- D. SSH

### Answers — Block 6A

**6A.1 → A.** **JSON** is the dominant REST payload format (and the default for DNA Center, RESTCONF on IOS-XE, etc.).

**6A.2 → B.** **NETCONF uses XML** for both RPC operations and configuration payloads.

**6A.3 → C.** JSON does *not* support comments. Valid types: string, number, boolean, null, array, object.

**6A.4 → A.** **Ansible playbooks and inventory** are YAML. Cisco engineers most often see YAML in this context.

**6A.5 → B.** **PUT** = replace (or create) the entire resource at the URL. Idempotent.

**6A.6 → C.** **PATCH** = partial update. PUT replaces entirely.

**6A.7 → B.** **401 Unauthorized** = no/invalid auth.

**6A.8 → C.** **403 Forbidden** = authenticated but lacks permission.

**6A.9 → A.** DNA Center: POST `/dna/system/api/v1/auth/token` with Basic auth → returns a Bearer token used in `X-Auth-Token` header on subsequent calls.

**6A.10 → A.** RESTCONF over HTTPS with **HTTP Basic** (username:password) is the most common style. AAA-integrated auth is also supported (NACM).

**6A.11 → A.** **`json.loads(string)`** parses a JSON string to a Python dict. `json.dump(obj, file)` writes the opposite direction.

**6A.12 → A.** **Postman** (GUI) and **curl** (CLI) are the standard REST testing tools.

---

## Block 6B — NETCONF / RESTCONF / YANG (12 questions)

**6B.1** Which transport and port does NETCONF use by default on IOS-XE?
- A. SSH on TCP/830
- B. HTTPS on TCP/443
- C. Telnet on TCP/23
- D. UDP/161

**6B.2** Which NETCONF datastore allows configuration changes to be validated before being applied to running?
- A. running
- B. startup
- C. candidate (with confirmed-commit support)
- D. ephemeral

**6B.3** Which NETCONF operation reads only the configuration data (not operational state) from a datastore?
- A. `get`
- B. `get-config`
- C. `edit-config`
- D. `copy-config`

**6B.4** Which NETCONF operation reads both config and operational state?
- A. `get`
- B. `get-config`
- C. `edit-config`
- D. `lock`

**6B.5** Default NETCONF `edit-config` operation when none is specified explicitly?
- A. replace
- B. merge
- C. delete
- D. create

**6B.6** YANG — what's the primary purpose?
- A. A modeling language for defining the structure of configuration and operational data, used by NETCONF/RESTCONF/gNMI.
- B. A scripting language for routers.
- C. A replacement for OSPF.
- D. A debugging tool.

**6B.7** Which YANG construct represents a list of objects with a key for uniqueness?
- A. `container`
- B. `leaf`
- C. `list`
- D. `grouping`

**6B.8** Which YANG construct represents a single value, like an IP address field?
- A. `container`
- B. `leaf`
- C. `list`
- D. `module`

**6B.9** Which is a *Cisco-native* YANG model that mirrors IOS-XE CLI structure closely?
- A. `Cisco-IOS-XE-native`
- B. `openconfig-interfaces`
- C. `ietf-interfaces`
- D. `ieee802-dot1q-bridge`

**6B.10** OpenConfig YANG models are designed for what?
- A. Vendor-neutral, operator-driven data models for cross-vendor compatibility.
- B. Cisco-only models.
- C. Replacing JSON.
- D. Replacing SSH.

**6B.11** Which Cisco IOS-XE command enables NETCONF?
- A. `netconf-yang`
- B. `restconf`
- C. `ip http server`
- D. `feature netconf`

**6B.12** A Python developer uses `ncclient` to push config to a CSR1000v. Which connection function signature is typical?
- A. `manager.connect(host=..., port=830, username=..., password=..., hostkey_verify=False, device_params={'name': 'iosxe'})`
- B. `requests.get(url, auth=...)`
- C. `paramiko.SSHClient().connect(...)`
- D. `snmp.session(...)`

### Answers — Block 6B

**6B.1 → A.** **SSH on TCP/830** is NETCONF default.

**6B.2 → C.** **Candidate datastore** lets you stage changes, validate, then commit. IOS-XE supports it with `netconf-yang feature candidate-datastore`.

**6B.3 → B.** **`get-config`** reads only config from a specified datastore. `get` reads both config and operational state.

**6B.4 → A.** **`get`** = config + operational state.

**6B.5 → B.** **Default is merge** — `edit-config` without an explicit `operation` attribute merges the new XML into the existing config.

**6B.6 → A.** **YANG** = "Yet Another Next Generation" — a data modeling language defined in RFC 6020/7950. It describes config/operational data structure for NETCONF, RESTCONF, gNMI.

**6B.7 → C.** **`list`** is a YANG construct for a list of entries with key(s).

**6B.8 → B.** **`leaf`** = single value.

**6B.9 → A.** **`Cisco-IOS-XE-native`** mirrors the CLI structure of IOS-XE. The others are vendor-neutral / IETF / IEEE standards.

**6B.10 → A.** **OpenConfig** is operator-driven (Google, Facebook, AT&T, etc.) — vendor-neutral models for the things operators actually want to manage uniformly.

**6B.11 → A.** **`netconf-yang`** in global config.

**6B.12 → A.** `ncclient.manager.connect(...)` is the standard call. `device_params={'name': 'iosxe'}` helps the library handle vendor-specific quirks.

---

## Block 6C — Ansible, EEM, telemetry, DNA-C automation (10 questions)

**6C.1** Ansible is "agentless" — what does this mean for Cisco devices?
- A. No software needs to be installed on the device; Ansible uses SSH (network_cli) or NETCONF/RESTCONF to push config.
- B. Devices run Python agents.
- C. Ansible installs a Cisco-specific daemon.
- D. Ansible uses SNMP polling.

**6C.2** Which Ansible inventory variable specifies the connection type for a Cisco IOS device?
- A. `ansible_connection=network_cli` (with `ansible_network_os=cisco.ios.ios`).
- B. `ansible_connection=ssh` only.
- C. `ansible_become=true`.
- D. `ansible_python_interpreter=/usr/bin/python3`.

**6C.3** What does "idempotent" mean in Ansible automation?
- A. Running the same playbook multiple times produces the same end state without unnecessary changes after the first run.
- B. Playbook is read-only.
- C. Playbook always changes something.
- D. Playbook runs in parallel only.

**6C.4** A network admin wants a router to run a series of show commands whenever an interface goes down. Which Cisco feature does this?
- A. EEM applet with `event syslog pattern` matching the LINK-DOWN message.
- B. SNMP only.
- C. NetFlow.
- D. RADIUS.

**6C.5** Which EEM event detector fires on a recurring schedule (e.g., every minute)?
- A. `event timer`
- B. `event syslog`
- C. `event snmp`
- D. `event none`

**6C.6** What is the difference between EEM applets and EEM scripts?
- A. Applets are CLI-based with limited logic; scripts use Tcl for full programmability.
- B. Applets are faster.
- C. Scripts only run on Nexus.
- D. They are identical.

**6C.7** Which encoding is most efficient for model-driven telemetry over gRPC?
- A. JSON
- B. XML
- C. kvGPB (key-value Google Protocol Buffers)
- D. CSV

**6C.8** A telemetry subscription pushes data every 10 seconds regardless of value change. Which subscription type is this?
- A. periodic
- B. on-change
- C. polled
- D. manual

**6C.9** Cisco DNA Center / Catalyst Center API uses which authentication paradigm?
- A. Bearer token obtained via /auth/token POST with Basic auth, then included in `X-Auth-Token` header.
- B. SNMPv3.
- C. SSH.
- D. RADIUS.

**6C.10** A developer wants to push a network intent to DNA Center via API. Which is the typical workflow?
- A. Authenticate → use the appropriate Intent API endpoint (e.g., `/dna/intent/api/v1/network-device`) with JSON payload → poll for task completion via taskId.
- B. Direct SSH to all devices.
- C. SNMP set.
- D. Send Ansible playbook to DNAC.

### Answers — Block 6C

**6C.1 → A.** **Ansible agentless = no daemon on the device.** It uses standard SSH (network_cli) or NETCONF/RESTCONF/HTTP APIs from the Ansible control node.

**6C.2 → A.** **`ansible_connection=network_cli`** + **`ansible_network_os=cisco.ios.ios`** is the modern path. (Older `ios` connection plugin is deprecated.)

**6C.3 → A.** **Idempotent** = same result regardless of how many times you run it. After the first successful run, subsequent runs should be `changed=0`.

**6C.4 → A.** **EEM applet** with `event syslog pattern "%LINK-3-UPDOWN.*GigabitEthernet0/1.*down"` triggers actions when that message is logged.

**6C.5 → A.** **`event timer`** — supports `cron`, `countdown`, `absolute`, `watchdog` variants.

**6C.6 → A.** **Applets** = CLI-defined, action steps in `action 1.0 ... 2.0 ...` style, limited control flow. **Scripts** = Tcl, full programmability. Applets are easier; scripts handle complex logic.

**6C.7 → C.** **kvGPB** — binary, compact, designed for high-volume streaming.

**6C.8 → A.** **Periodic** subscription = push every N intervals. **On-change** = push only when a value changes.

**6C.9 → A.** DNA Center / Catalyst Center: POST /auth/token with HTTP Basic → receive Bearer token → use it as `X-Auth-Token` header.

**6C.10 → A.** Authenticate, call Intent API endpoint with JSON, poll task ID for completion. Many Intent API calls are asynchronous and return a task ID.

---

## How to use this question bank

- After finishing a week of study, do the corresponding block(s) **closed-book**, then check answers.
- Mark misses in your `ENCOR-Progress-Tracker.md` under "Open gaps."
- A week before the exam, re-do all blocks back-to-back as a timed dry run.
- These are scenario practice. They complement (do not replace) the official Pearson Test Prep questions in the OCG.
