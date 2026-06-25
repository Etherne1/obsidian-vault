# External Labs Map — NetworkLessons / GNS3vault Archive

**Source:** [networklessons/labs](https://github.com/networklessons/labs) — combined repo containing two collections:

1. **`gns3vault-archive/`** — René Molenaar's classic GNS3vault labs, retired from gns3vault.com and archived here. **Each lab has a full scenario, goal/task list, topology diagram, starter configs, final solution configs, and a YouTube video walkthrough.** ~291 labs.
2. **`containerlab/labs/`** — Modern containerlab topologies with startup configs but **no description, no task list, no goals**. ~78 labs.

**Correction from my earlier mapping:** I pointed you at containerlab first, which is what most people will see when they look at the repo, but those are *topology files only* — no learning material. The **`gns3vault-archive/` is the actually useful collection** for self-study. Use that.

**Validation status:** GNS3vault labs were written by René Molenaar for the gns3vault.com community over ~10 years (2008–2018). They have been used by thousands of CCNP/CCIE candidates. Treat them as ground truth. Configs use older IOS versions (12.4 / 15.x) but the protocol mechanics are identical to what your CSR1000v 17.3.04a runs.

---

## How each GNS3vault lab is structured

Open any lab folder, e.g. [OSPF / ospf-authentication](https://github.com/networklessons/labs/tree/main/gns3vault-archive/OSPF/ospf-authentication). You will see:

| File | What it is |
|---|---|
| `<lab-name>.md` | **The lab document.** Scenario (story setup), Goal (numbered task list), Additional information, IOS image used, YouTube link. **This is what you were looking for.** |
| `topology-<lab-name>.png` | Topology diagram. |
| `<Lab Name>.gns3` | Native GNS3 project file (you can ignore — for users running GNS3). |
| `startup-configs/` | Starter configs per router (interfaces, IPs preconfigured — you do the protocol work). |
| `final-configs/` | The solution. Open *only* after attempting the lab. |
| `containerlab/` | Containerlab version of the topology. Ignore for EVE-NG. |

**Lab workflow:**
1. Read `<lab-name>.md` — understand the scenario and the task list.
2. Build the topology in EVE-NG matching the PNG.
3. Apply each router's `startup-configs/R<N>.txt` to give yourself the same starting point.
4. Do the tasks listed in the goal section. Don't look at final-configs.
5. Verify with the YouTube video walkthrough — the link in the .md file.
6. Only if stuck, peek at `final-configs/R<N>.txt`.

---

## Platform translation

GNS3vault labs assume Cisco 3640/3725/7200 routers with IOS 12.4 or 15.x. Your CSR1000v 17.3.04a is newer but the CLI is upward-compatible for all ENCOR-relevant features. Specifically:

| GNS3vault uses | Your image | Difference |
|---|---|---|
| `c3640-jk9s-mz` / `c3725-adventerprisek9-mz` / `c7200-adventerprisek9-mz` | **CSR1000v 17.3.04a** | Newer IOS-XE. All OSPF/BGP/EIGRP/MPLS commands identical. Some legacy commands removed (mostly Frame Relay, ATM). |
| `Serial0/0`, `FastEthernet0/0` | `GigabitEthernet1`, `GigabitEthernet2` | Cosmetic — rename interfaces. |
| L2 switching features (VLAN, STP, trunks) | **vIOS-L2** | GNS3vault used a 3640 with NM-16ESW switch module. Use vIOS-L2 instead. |

**Frame Relay labs:** GNS3vault has many "OSPF over Frame Relay" labs. Frame Relay is **removed from CCNA/CCNP since 2016** — skip these. The concept they teach (OSPF network types) is covered by other labs in the same OSPF folder.

---

## Lab-by-lab mapping to your ENCOR study plan

Status legend: **USE** = drop-in / **ADAPT** = needs minor CLI/topology tweak / **SKIP** = not in ENCOR scope or obsolete (Frame Relay, RIP, etc.) / **BONUS** = beyond ENCOR but worth doing if you have time.

### Week 1–2 — L2 (STP / VLAN / EtherChannel / Trunks)

From `gns3vault-archive/Switching/`:

| Lab | Status | Notes |
|---|---|---|
| [vlans-and-trunks](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Switching/vlans-and-trunks) | **USE** | Start here. |
| [vtp-vlan-trunking-protocol](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Switching/vtp-vlan-trunking-protocol) | **USE** | ENCOR has light VTP coverage but worth doing. |
| [pagp-lacp-etherchannel](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Switching/pagp-lacp-etherchannel) | **USE** | Both protocols in one lab. Replaces my Lab I2. |
| [pvst-per-vlan-spanning-tree](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Switching/pvst-per-vlan-spanning-tree) | **USE** | PVST basics. |
| [pvrst-per-vlan-rapid-spanning-tree](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Switching/pvrst-per-vlan-rapid-spanning-tree) | **USE** | RPVST+ — exam-relevant. Replaces my Lab I1 Phase 1. |
| [mst-multiple-spanning-tree](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Switching/mst-multiple-spanning-tree) | **USE** | MST — replaces my Lab I1 Phase 2. |
| [spanning-tree-root-guard](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Switching/spanning-tree-root-guard) | **USE** | Dedicated Root Guard lab. **Do this one. After my Lab I1 errors, this is exactly the validation you need.** |
| [spanning-tree-bpdu-guard](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Switching/spanning-tree-bpdu-guard) | **USE** | Dedicated BPDU Guard lab. |
| [spanning-tree-loop-guard](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Switching/spanning-tree-loop-guard) | **USE** | Loop Guard. |
| [spanning-tree-bpdu-filter](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Switching/spanning-tree-bpdu-filter) | **USE** | BPDU Filter (and why you usually don't want it). |
| [udld-unidirectional-link-detection](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Switching/udld-unidirectional-link-detection) | **USE** | UDLD — exam-relevant. |
| [dhcp-snooping](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Switching/dhcp-snooping) | **USE** | DHCP snooping. Slot into Week 13 (security) too. |
| [private-vlan](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Switching/private-vlan) | **BONUS** | Private VLAN — niche but exam-eligible. |
| [vacl-vlan-access-list](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Switching/vacl-vlan-access-list) | **BONUS** | VACL. |
| [flex-links-backup-interface](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Switching/flex-links-backup-interface) | **SKIP** | Flex links are deprecated. |
| [switch-svi-interface-and-routing](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Switching/switch-svi-interface-and-routing) | **USE** | SVI L3 — fundamental. |
| [spanning-tree-for-ccna](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Switching/spanning-tree-for-ccna) | **SKIP** | CCNA-level recap. Skip unless you want a warmup. |
| [ccnp-switch-lab](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Switching/ccnp-switch-lab) | **USE** | Multi-feature integrated lab. Good week-2 capstone. |

**Verdict:** External repo fully replaces my Labs I1 and I2. Do all 4 spanning-tree guard labs in sequence — they will give you the correct placement intuition that I failed to deliver.

---

### Week 3 — OSPF basics

From `gns3vault-archive/OSPF/`:

| Lab | Status | Notes |
|---|---|---|
| [ospf-single-area](https://github.com/networklessons/labs/tree/main/gns3vault-archive/OSPF/ospf-single-area) | **USE** | Start here. |
| [ospf-dr-bdr-election](https://github.com/networklessons/labs/tree/main/gns3vault-archive/OSPF/ospf-dr-bdr-election) | **USE** | DR/BDR mechanics. |
| [ospf-authentication](https://github.com/networklessons/labs/tree/main/gns3vault-archive/OSPF/ospf-authentication) | **USE** | Plain + MD5 auth + virtual link auth. |
| [ospf-md5-authentication-rotating-key](https://github.com/networklessons/labs/tree/main/gns3vault-archive/OSPF/ospf-md5-authentication-rotating-key) | **USE** | Key rotation — exam-relevant. |
| [ospf-auto-cost-reference-bandwidth](https://github.com/networklessons/labs/tree/main/gns3vault-archive/OSPF/ospf-auto-cost-reference-bandwidth) | **USE** | Cost calculation. |
| [ospf-per-neighbor-cost](https://github.com/networklessons/labs/tree/main/gns3vault-archive/OSPF/ospf-per-neighbor-cost) | **USE** | Per-neighbor cost manipulation. |
| [ospf-intermediate](https://github.com/networklessons/labs/tree/main/gns3vault-archive/OSPF/ospf-intermediate) | **USE** | Multi-feature integration. |

### Week 4 — OSPF advanced

| Lab | Status | Notes |
|---|---|---|
| [ospf-stub-area](https://github.com/networklessons/labs/tree/main/gns3vault-archive/OSPF/ospf-stub-area) | **USE** | Stub area. |
| [ospf-totally-stub](https://github.com/networklessons/labs/tree/main/gns3vault-archive/OSPF/ospf-totally-stub) | **USE** | Totally stubby. |
| [ospf-nssa-not-so-stubby-area](https://github.com/networklessons/labs/tree/main/gns3vault-archive/OSPF/ospf-nssa-not-so-stubby-area) | **USE** | NSSA — exam favorite. |
| [ospf-totally-nssa](https://github.com/networklessons/labs/tree/main/gns3vault-archive/OSPF/ospf-totally-nssa) | **USE** | Totally NSSA. |
| [ospf-lsa-type-3-summarization](https://github.com/networklessons/labs/tree/main/gns3vault-archive/OSPF/ospf-lsa-type-3-summarization) | **USE** | Inter-area summarization. |
| [ospf-lsa-type-5-summarization](https://github.com/networklessons/labs/tree/main/gns3vault-archive/OSPF/ospf-lsa-type-5-summarization) | **USE** | External-route summarization. |
| [ospf-summarization-discard-route](https://github.com/networklessons/labs/tree/main/gns3vault-archive/OSPF/ospf-summarization-discard-route) | **USE** | Discard route auto-generation. |
| [ospf-virtual-link](https://github.com/networklessons/labs/tree/main/gns3vault-archive/OSPF/ospf-virtual-link) | **USE** | Virtual link. |
| [ospf-virtual-link-and-summarization](https://github.com/networklessons/labs/tree/main/gns3vault-archive/OSPF/ospf-virtual-link-and-summarization) | **BONUS** | Combination scenario. |
| [ospf-suppress-forward-address](https://github.com/networklessons/labs/tree/main/gns3vault-archive/OSPF/ospf-suppress-forward-address) | **BONUS** | Niche but exam-eligible. |
| [ospf-flood-reduction](https://github.com/networklessons/labs/tree/main/gns3vault-archive/OSPF/ospf-flood-reduction) | **BONUS** | Flood reduction. |
| [ospf-demand-circuit](https://github.com/networklessons/labs/tree/main/gns3vault-archive/OSPF/ospf-demand-circuit) | **SKIP** | Demand circuit is for dial-up. Obsolete. |
| All `ospf-over-frame-relay-*` (5 labs) | **SKIP** | Frame Relay removed from ENCOR. The network-type concept is covered in the next ENCOR labs below — use `containerlab/labs/ospf/cisco/ospf-network-type-*` instead (modern, no FR). |

---

### Week 5 — BGP

GNS3vault BGP folder has **50 labs** — overkill for ENCOR. Pick these:

| Lab | Status | Notes |
|---|---|---|
| [bgp-basic](https://github.com/networklessons/labs/tree/main/gns3vault-archive/BGP/bgp-basic) | **USE** | Start here. |
| [ebgp-external-bgp](https://github.com/networklessons/labs/tree/main/gns3vault-archive/BGP/ebgp-external-bgp) | **USE** | eBGP. |
| [ibgp-internal-bgp](https://github.com/networklessons/labs/tree/main/gns3vault-archive/BGP/ibgp-internal-bgp) | **USE** | iBGP. |
| [bgp-ebgp-multihop](https://github.com/networklessons/labs/tree/main/gns3vault-archive/BGP/bgp-ebgp-multihop) | **USE** | Multihop + loopback peering. |
| [bgp-update-source](https://github.com/networklessons/labs/tree/main/gns3vault-archive/BGP/bgp-update-source) | **USE** | Update-source. |
| [bgp-next-hop-self](https://github.com/networklessons/labs/tree/main/gns3vault-archive/BGP/bgp-next-hop-self) | **USE** | `next-hop-self` — fundamental. |
| [bgp-route-reflectors](https://github.com/networklessons/labs/tree/main/gns3vault-archive/BGP/bgp-route-reflectors) | **USE** | RR. |
| [bgp-attribute-weight](https://github.com/networklessons/labs/tree/main/gns3vault-archive/BGP/bgp-attribute-weight) | **USE** | Weight — Cisco-only. |
| [bgp-attribute-local-preference](https://github.com/networklessons/labs/tree/main/gns3vault-archive/BGP/bgp-attribute-local-preference) | **USE** | LOCAL_PREF. |
| [bgp-attribute-as-path](https://github.com/networklessons/labs/tree/main/gns3vault-archive/BGP/bgp-attribute-as-path) | **USE** | AS-path prepending. |
| [bgp-attribute-med](https://github.com/networklessons/labs/tree/main/gns3vault-archive/BGP/bgp-attribute-med) | **USE** | MED. |
| [bgp-attribute-origin](https://github.com/networklessons/labs/tree/main/gns3vault-archive/BGP/bgp-attribute-origin) | **USE** | Origin code. |
| [bgp-communities](https://github.com/networklessons/labs/tree/main/gns3vault-archive/BGP/bgp-communities) | **USE** | Communities — exam favorite. |
| [bgp-communities-no-export](https://github.com/networklessons/labs/tree/main/gns3vault-archive/BGP/bgp-communities-no-export), [no-advertise](https://github.com/networklessons/labs/tree/main/gns3vault-archive/BGP/bgp-communities-no-advertise), [local-as](https://github.com/networklessons/labs/tree/main/gns3vault-archive/BGP/bgp-communities-local-as) | **USE** | Well-known communities. |
| [bgp-md5-authentication](https://github.com/networklessons/labs/tree/main/gns3vault-archive/BGP/bgp-md5-authentication) | **USE** | Auth. |
| [bgp-filtering-extended-access-list](https://github.com/networklessons/labs/tree/main/gns3vault-archive/BGP/bgp-filtering-extended-access-list), [bgp-as-path-access-list](https://github.com/networklessons/labs/tree/main/gns3vault-archive/BGP/bgp-as-path-access-list) | **USE** | Path filtering. |
| [bgp-peer-group](https://github.com/networklessons/labs/tree/main/gns3vault-archive/BGP/bgp-peer-group) | **USE** | Peer groups. |
| [bgp-aggregation](https://github.com/networklessons/labs/tree/main/gns3vault-archive/BGP/bgp-aggregation) | **USE** | Aggregation. |
| [bgp-confederations](https://github.com/networklessons/labs/tree/main/gns3vault-archive/BGP/bgp-confederations) | **BONUS** | Confederations — exam-eligible but rare in practice. |
| [bgp-advanced](https://github.com/networklessons/labs/tree/main/gns3vault-archive/BGP/bgp-advanced) | **BONUS** | Multi-feature capstone. |
| All other BGP labs (~25) | **POST-ENCOR / BONUS** | Excellent depth, save for after the cert. |

---

### Week 6 — EIGRP + IS-IS

| Lab | Status | Notes |
|---|---|---|
| `gns3vault-archive/EIGRP/` (23 labs) | **USE 3** | EIGRP is light on ENCOR. Pick: `eigrp-basic`, `eigrp-authentication`, `eigrp-summarization`. |
| `containerlab/labs/isis/cisco/is-is-single-area-two-routers` | **USE** | IS-IS isn't really in ENCOR but a one-look-at-it lab is useful. |

---

### Week 7 — First-hop redundancy + Services

From `gns3vault-archive/Network Services/`:

| Lab | Status | Notes |
|---|---|---|
| [hot-standby-routing-protocol](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Network%20Services/hot-standby-routing-protocol) | **USE** | HSRP basics. |
| [vrrp-virtual-router-redundancy-protocol](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Network%20Services/vrrp-virtual-router-redundancy-protocol) | **USE** | VRRP. |
| [glbp-gateway-load-balancing-protocol](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Network%20Services/glbp-gateway-load-balancing-protocol) | **USE** | GLBP — exam-eligible. |
| [dhcp-server](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Network%20Services/dhcp-server) | **USE** | DHCP server on IOS. |
| [dhcp-relay](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Network%20Services/dhcp-relay) | **USE** | DHCP relay (`ip helper-address`). |
| [nat-pat-overload](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Network%20Services/nat-pat-overload), [nat-dynamic](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Network%20Services/nat-dynamic), [nat-static](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Network%20Services/nat-static) | **USE** | NAT — basic ENCOR. |
| [ip-service-level-agreement-sla](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Network%20Services/ip-service-level-agreement-sla) | **USE** | IP SLA — exam-eligible. |
| [policy-based-routing](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Network%20Services/policy-based-routing) | **USE** | PBR. |
| [proxy-arp](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Network%20Services/proxy-arp) | **USE** | Proxy ARP. |
| [floating-static-routes](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Network%20Services/floating-static-routes) | **USE** | Floating statics. |

---

### Week 8 — Multicast

From `gns3vault-archive/Multicast/`:

| Lab | Status | Notes |
|---|---|---|
| [multicast-pim-dense-mode](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Multicast/multicast-pim-dense-mode) | **USE** | PIM-DM. |
| [multicast-pim-sparse-mode](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Multicast/multicast-pim-sparse-mode) | **USE** | PIM-SM — main ENCOR multicast topic. |
| [multicast-pim-sparse-dense-mode](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Multicast/multicast-pim-sparse-dense-mode) | **USE** | Sparse-dense. |
| [multicast-autorp](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Multicast/multicast-autorp), [auto-rp-listener](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Multicast/auto-rp-listener) | **USE** | Auto-RP. |
| [multicast-pim-bootstrap-router](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Multicast/multicast-pim-bootstrap-router) | **USE** | BSR. |
| [multicast-pim-dr-election](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Multicast/multicast-pim-dr-election) | **USE** | PIM DR election. |
| [multicast-rpf-failure](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Multicast/multicast-rpf-failure) | **USE** | RPF check — exam favorite. |
| [multicast-tunneling](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Multicast/multicast-tunneling) | **BONUS** | Multicast over GRE. |

**Replaces my Lab I6 entirely. Excellent coverage.**

---

### Week 9 — Wireless

No external labs. DevNet Always-On 9800 sandbox only.

---

### Week 10 — Tunnels / VRF / MPLS

From `gns3vault-archive/Tunneling/` and `gns3vault-archive/MPLS/`:

| Lab | Status | Notes |
|---|---|---|
| [gre-tunnel-basic](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Tunneling/gre-tunnel-basic) | **USE** | GRE basics. |
| [gre-over-ipsec](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Tunneling/gre-over-ipsec) | **USE** | GRE+IPsec — replaces my Lab V2. |
| [encrypted-gre-tunnel](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Tunneling/encrypted-gre-tunnel) | **USE** | Alternative encrypted-GRE approach. |
| [site-to-site-ipsec-vpn](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Tunneling/site-to-site-ipsec-vpn) | **USE** | Plain S2S IPsec. |
| [vrf-lite](https://github.com/networklessons/labs/tree/main/gns3vault-archive/MPLS/vrf-lite) | **USE** | VRF-Lite — replaces my Lab V1. |
| [vrf-routing](https://github.com/networklessons/labs/tree/main/gns3vault-archive/MPLS/vrf-routing) | **USE** | VRF inter-VRF routing. |
| [mpls-ldp](https://github.com/networklessons/labs/tree/main/gns3vault-archive/MPLS/mpls-ldp) | **USE** | LDP basics. |
| [basic-mpls-vpn](https://github.com/networklessons/labs/tree/main/gns3vault-archive/MPLS/basic-mpls-vpn) | **USE** | MPLS L3VPN — referenced in ENCOR. |
| [mpls-vpn-pe-ce-using-ospf](https://github.com/networklessons/labs/tree/main/gns3vault-archive/MPLS/mpls-vpn-pe-ce-using-ospf) | **POST-ENCOR** | Deeper than ENCOR needs. |
| All `mpls-traffic-engineering-*`, `mpls-atom-*` | **POST-ENCOR** | TE / pseudowire — Post-ENCOR. |

---

### Week 11–12 — VXLAN / SD-Access

GNS3vault is too old to cover VXLAN. Use:

| Lab | Status | Notes |
|---|---|---|
| `containerlab/labs/vxlan/cisco/vxlan-underlay-ospf-31-overlay-false` | **ADAPT** | NX-OS — I'll rewrite to IOS-XE for your CSR1000v when you reach this week. |
| Hand-written A2 (LISP) and V3 (VXLAN flood-and-learn) | **VALIDATE** | I'll re-pass these when you reach Week 11. |

---

### Week 13 — Security

From `gns3vault-archive/Security/`:

| Lab | Status | Notes |
|---|---|---|
| [aaa-authentication](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Security/aaa-authentication) | **USE** | RADIUS/TACACS+ auth. Replaces my Lab S1. |
| [aaa-command-authorization](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Security/aaa-command-authorization) | **USE** | Command authz. |
| [aaa-exec-authorization](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Security/aaa-exec-authorization) | **USE** | EXEC authz. |
| [basic-zone-based-firewall](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Security/basic-zone-based-firewall) | **USE** | ZBF — replaces my Lab S3. |
| [control-place-policing](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Security/control-place-policing) | **USE** | CoPP (typo in folder name — "place" should be "plane"). Pairs with my Lab S2's L2 hardening. |
| [standard-access-list](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Security/standard-access-list), [extended-access-list](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Security/extended-access-list), [named-access-list](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Security/named-access-list), [time-based-access-list](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Security/time-based-access-list), [reflexive-access-list](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Security/reflexive-access-list) | **USE** | ACL family. ENCOR expects fluency. |
| [unicast-reverse-path-forwarding-urpf](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Security/unicast-reverse-path-forwarding-urpf) | **USE** | uRPF — exam-eligible. |
| [role-based-cli-access](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Security/role-based-cli-access) | **BONUS** | Parser views — minor topic. |
| [ios-firewall-cbac](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Security/ios-firewall-cbac), [tcp-intercept](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Security/tcp-intercept), [transparent-ios-firewall](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Security/transparent-ios-firewall) | **SKIP** | Deprecated IOS firewall features. |

From `gns3vault-archive/Switching/` for L2 hardening (also useful in Week 2):

| Lab | Status | Notes |
|---|---|---|
| [dhcp-snooping](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Switching/dhcp-snooping) | **USE** | L2 hardening. |
| (Port-security is covered in CCNP Switch lab and inside the broader `ccnp-switch-lab` capstone.) | | |

---

### Week 14 — Automation & Telemetry

From `gns3vault-archive/Network Management/`:

| Lab | Status | Notes |
|---|---|---|
| [ntp-network-time-protocol](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Network%20Management/ntp-network-time-protocol) | **USE** | NTP. |
| [syslog-server-logging](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Network%20Management/syslog-server-logging), [system-message-logging](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Network%20Management/system-message-logging) | **USE** | Logging. |
| [snmpv2-server](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Network%20Management/snmpv2-server), [snmpv3-server](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Network%20Management/snmpv3-server) | **USE** | SNMP — exam topic. |
| [eem-scripting](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Network%20Management/eem-scripting), [eem-scripting-event-detector](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Network%20Management/eem-scripting-event-detector) | **USE** | EEM — replaces my Lab AU3. |
| [kron-task-scheduler](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Network%20Management/kron-task-scheduler) | **USE** | Kron scheduler — exam-eligible. |
| [configuration-archive-to-tftp](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Network%20Management/configuration-archive-to-tftp) | **USE** | Config archive — exam-eligible. |
| [cdp-cisco-discovery-protocol](https://github.com/networklessons/labs/tree/main/gns3vault-archive/Network%20Management/cdp-cisco-discovery-protocol) | **USE** | CDP. (LLDP not in repo — read OCG.) |
| *No NETCONF/RESTCONF/Ansible labs* | — | Keep my Labs AU1, AU2. These are CSR1000v-specific and modern — I'll validate when you reach them. |

---

## Big-picture summary

| ENCOR area | Best source | Hand-written labs to keep |
|---|---|---|
| L2 (STP/VLAN/EtherChannel/L2-security) | **GNS3vault Switching folder** | None — fully covered |
| OSPF | **GNS3vault OSPF folder** | None — fully covered |
| BGP | **GNS3vault BGP folder** | None — fully covered |
| EIGRP / IS-IS | **GNS3vault EIGRP folder + 1 containerlab IS-IS** | None |
| FHRP (HSRP/VRRP/GLBP) | **GNS3vault Network Services folder** | None |
| Multicast | **GNS3vault Multicast folder** | None — fully covered |
| Services (NAT/DHCP/IP SLA/PBR) | **GNS3vault Network Services folder** | None |
| Tunnels / VRF / MPLS | **GNS3vault Tunneling + MPLS folders** | None |
| Wireless | DevNet sandbox | None |
| VXLAN / SD-Access | Hand-written (A2, V3) — modern topics, GNS3vault too old | A2, V3 (I'll validate) |
| Security | **GNS3vault Security folder** | S2 partial (port-security only) |
| Automation (NETCONF/RESTCONF/Ansible) | Hand-written (AU1, AU2) — modern topics | AU1, AU2 (I'll validate) |
| EEM / SNMP / NTP / Syslog | **GNS3vault Network Management folder** | None |

**Net result:** roughly **85% of your lab work moves to pre-validated external labs with full descriptions**. The remaining 15% is modern topics (VXLAN, NETCONF, RESTCONF, Ansible, LISP) that GNS3vault is too old to cover — I write those hand-written labs and validate them before you reach each week.

---

## How I help you with these labs

1. **Pre-week briefing.** Each week, I list the 4–8 labs to run (from the right category folder), in order, with rationale ("do bgp-basic before bgp-attribute-as-path because…").
2. **Lab walkthrough on demand.** Paste the `.md` file's scenario or task list, I explain anything unclear before you start.
3. **Real-time debugging.** Paste `show` output during the lab, I diagnose against the goal.
4. **Final-config comparison.** If you finish and want a review, paste your config + the `final-configs/R<N>.txt`, I diff them and explain meaningful differences.
5. **Notes & Anki extraction.** After each lab session, I extract the 5–10 highest-value concepts into `ENCOR-Jeff-Kish-Notes.md` and turn them into Anki cards in `ENCOR-Practice-Questions.md`.
6. **Gap-filling labs.** For modern topics not in GNS3vault, I write focused single-image labs and validate them before you touch them.

---

## Open questions for you

1. Want me to **rebuild `ENCOR-Study-Plan.md`** so each week's "Lab" section directly references these external lab folders (with links) instead of my hand-written labs? Cleaner than maintaining two documents.
2. Should I **archive my hand-written labs** that are now redundant (most of `ENCOR-Labs-EVE-NG.md`) and keep only the modern/gap-filling ones (A2, V3, AU1, AU2, S2 partial)? Reduces clutter, less for me to maintain (and break).
3. Want to do the **extension to 16 weeks** at the same time — add buffer weeks?

Any combination of (1), (2), (3) — say which and I'll do them in one pass.
