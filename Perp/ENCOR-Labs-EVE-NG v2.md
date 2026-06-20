# ENCOR 350-401 — EVE-NG Lab Book

**Platform:** EVE-NG (Community or Pro).
**Scope:** Pure Cisco. Single-vendor for ENCOR study. Multi-vendor interop is intentionally moved to the Post-ENCOR phase so you don't fight emulator bugs and inter-vendor bugs at the same time.
**Images assumed available (only these three):**
- `vIOS-L2` (vIOS-L2 Adventerprise, e.g. `vios_l2-adventerprisek9-m`) — switch features: STP/MST/RPVST+, trunks, LACP/EtherChannel, port-security, DHCP snoop, DAI, IPSG, basic L3 on SVIs and routed ports (OSPF/EIGRP), HSRP/VRRP.
- `vIOS-L3` (vIOS Adventerprise, e.g. `vios-adventerprisek9-m`) — pure router: OSPF, EIGRP, BGP, VRF-Lite, GRE, IPsec, multicast, redistribution. No `switchport` command.
- `CSR1000v 17.3.04a` (csr1000v-universalk9.17.03.04a) — modern IOS-XE router: VXLAN, LISP, ZBF, NETCONF/RESTCONF, model-driven telemetry, EEM, NAT, IPsec, MPLS.

## Image Matrix — which image for which lab

| Lab | Image used | Why |
|---|---|---|
| A1 Campus baseline | CORE/DIST = vIOS-L2 (L3 features on SVI/routed ports), ACC = vIOS-L2 | DIST must run trunks down + OSPF up — only L2 image supports `switchport`. CORE could be vIOS-L3, but using L2 everywhere here keeps topology consistent for I1/I2 reuse. |
| A2 SD-Access concept (LISP+VXLAN) | CSR1000v ×3 | LISP and modern VXLAN require IOS-XE. |
| A3 StackWise/HSRP convergence | vIOS-L2 ×2 + vIOS-L2 access | HSRP + SVI testing. (Real StackWise Virtual cannot be emulated — we simulate convergence behavior only.) |
| V1 VRF-Lite | vIOS-L3 or CSR1000v | Pure routing/VRF feature. |
| V2 GRE+IPsec | vIOS-L3 or CSR1000v | Tunnel + crypto features. CSR preferred for modern IKEv2. |
| V3 VXLAN flood-and-learn | CSR1000v ×2 | VXLAN NVE only on IOS-XE. |
| I1 STP / MST / Guards | vIOS-L2 everywhere | All L2 features. |
| I2 VLAN / Trunk / LACP / L3 Po | vIOS-L2 everywhere | `switchport` + `channel-group` required. |
| I3 OSPF multi-area / OSPFv3 | vIOS-L3 or CSR1000v | Pure routing. |
| I4 BGP policy | vIOS-L3 or CSR1000v | Pure routing. |
| I5 HSRP/VRRP + tracking | vIOS-L2 (DIST pair) | FHRP on SVIs. |
| I6 PIM-SM multicast | vIOS-L3 or CSR1000v | Multicast routing. |
| NA1 NTP/Syslog/SNMPv3/IP SLA | any | Works on all three. |
| NA2 SPAN/RSPAN/ERSPAN | vIOS-L2 (SPAN/RSPAN), CSR or vIOS-L3 (ERSPAN destination) | SPAN is L2-switch feature. |
| NA3 Flexible NetFlow + telemetry | CSR1000v | Model-driven telemetry / gNMI only on IOS-XE. |
| S1 AAA (RADIUS) | any router + Linux host | Need a Linux container/VM as RADIUS server. (Use any Linux EVE-NG image — `linux-tinycore` or a Docker node.) |
| S2 L2 hardening + CoPP | vIOS-L2 (DHCP snoop/DAI/IPSG/port-sec/CoPP all run on the L2 image) | All features supported. |
| S3 Zone-Based Firewall | CSR1000v | ZBF is IOS-XE. |
| AU1 NETCONF/RESTCONF | CSR1000v | IOS-XE only. |
| AU2 Ansible push | CSR1000v + vIOS-L3 + vIOS-L2 | Mixed pool to prove Ansible works across IOS-classic and IOS-XE. |
| AU3 EEM | any | EEM is in all three. |

**Wireless (ENCOR Domain 5):** No free emulated WLC fits in 8 GB RAM. Use Cisco DevNet Always-On 9800 sandbox (web GUI + SSH, free, accessible from RU via VPN) for any hands-on you want. Study path: read the OCG + watch Jeff Kish's wireless section + spend one afternoon on the sandbox. Don't try to add an emulated WLC to EVE-NG for ENCOR — it's not worth the pain.

**Things vIOS-L2 cannot do (so don't waste time looking):** no real ASIC (CPU forwarding, so don't benchmark throughput), no StackWise/VSS, no QoS hardware queues, no MACsec, limited platform-specific show output. Everything in the ENCOR blueprint that you can configure with CLI works.

**Conventions used in every lab:**

- Hostnames are short and meaningful. Loopback0 = router-id, always `<router-num>.<router-num>.<router-num>.<router-num>` (R1 → 1.1.1.1).
- Management is out-of-band on `Gi0/0` to the EVE-NG `Cloud0` (we won't reconfigure it inside labs — leave it as DHCP).
- Lab links use `Gi0/1`, `Gi0/2`, etc.
- Default credentials: `admin / Cisco123!`, `enable secret Cisco123!`.
- All configs assume a fresh boot — paste from top to bottom.
- Where I write `! ===== R1 =====`, that's a paste boundary.

**Lab index**

| ID | Domain | Title |
|---|---|---|
| A1 | Architecture | Enterprise Campus Baseline (3-tier) |
| A2 | Architecture | SD-Access Underlay/Overlay Concept Lab |
| A3 | Architecture | StackWise Virtual / HSRP Convergence Test |
| V1 | Virtualization | VRF-Lite + Inter-VRF Routing |
| V2 | Virtualization | GRE-over-IPsec Site-to-Site |
| V3 | Virtualization | VXLAN Flood-and-Learn between CSR1000v |
| I1 | Infrastructure | STP / RPVST+ / MST + Guards |
| I2 | Infrastructure | VLANs, Trunks, LACP, Layer-3 Port-channel |
| I3 | Infrastructure | OSPF Multi-Area + Filtering + OSPFv3 |
| I4 | Infrastructure | eBGP / iBGP with Policy & Communities |
| I5 | Infrastructure | HSRP + VRRP + Tracking |
| I6 | Infrastructure | PIM-SM Multicast with Static RP |
| NA1 | Network Assurance | NTP + Syslog + SNMPv3 + IP SLA |
| NA2 | Network Assurance | SPAN / RSPAN / ERSPAN |
| NA3 | Network Assurance | Flexible NetFlow + Model-Driven Telemetry |
| S1 | Security | AAA — Local + RADIUS Fallback |
| S2 | Security | Layer-2 Hardening (DHCP snoop / DAI / IPSG / port-sec / CoPP) |
| S3 | Security | Zone-Based Firewall on CSR1000v |
| AU1 | Automation | NETCONF + RESTCONF on CSR1000v |
| AU2 | Automation | Ansible push: OSPF + VLAN config |
| AU3 | Automation | EEM applet — interface-state syslog reaction |

---

# A — Architecture

## Lab A1 — Enterprise Campus Baseline (3-tier)

**Goal:** Build a small 3-tier campus you'll reuse for several infrastructure labs.

**Topology**
```
                 CORE1 (vIOS-L2)            CORE2 (vIOS-L2)
                 Lo0 1.1.1.1                Lo0 2.2.2.2
                   |     \                /     |
                   |      \              /      |
                 DIST1 (vIOS-L2)-----DIST2 (vIOS-L2)
                 Lo0 3.3.3.3        Lo0 4.4.4.4
                  /    \              /    \
              ACC1 (vIOS-L2)  ACC2 (vIOS-L2)  ACC3 (vIOS-L2)
              VLANs 10,20 / 30,40 / 50

      Host PCs (Linux nodes in EVE-NG):
        PC1 → ACC1 Gi0/1  VLAN 10
        PC2 → ACC2 Gi0/1  VLAN 30
        PC3 → ACC3 Gi0/1  VLAN 50
```

**Links**
| From | If | To | If | Notes |
|---|---|---|---|---|
| CORE1 | Gi0/1 | CORE2 | Gi0/1 | L3, 10.0.0.0/30 |
| CORE1 | Gi0/2 | DIST1 | Gi0/1 | L3, 10.0.0.4/30 |
| CORE1 | Gi0/3 | DIST2 | Gi0/1 | L3, 10.0.0.8/30 |
| CORE2 | Gi0/2 | DIST1 | Gi0/2 | L3, 10.0.0.12/30 |
| CORE2 | Gi0/3 | DIST2 | Gi0/2 | L3, 10.0.0.16/30 |
| DIST1 | Gi0/3 | DIST2 | Gi0/3 | L3, 10.0.0.20/30 |
| DIST1 | Gi0/4 | ACC1 | Gi0/1 | 802.1Q trunk |
| DIST1 | Gi0/5 | ACC2 | Gi0/1 | 802.1Q trunk |
| DIST2 | Gi0/4 | ACC2 | Gi0/2 | 802.1Q trunk (HSRP redundancy) |
| DIST2 | Gi0/5 | ACC3 | Gi0/1 | 802.1Q trunk |

**Note on images:** All seven nodes are vIOS-L2. On CORE and DIST we use the L2 image because DIST has to do `switchport`/trunks down to ACC *and* L3/OSPF up to CORE — vIOS-L2 handles both (`no switchport` on the dist↔core uplinks turns the port into a routed L3 interface, exactly like a real Catalyst). CORE is L2 in this lab only for consistency (saves you having to remember which image is which); you could swap CORE to vIOS-L3 with no config change since the core ports are all `no switchport` routed anyway.

**Addressing**
- p2p core/dist subnets: 10.0.0.0/24 sliced as /30s above.
- SVI on DIST pair (HSRP later): VLAN10 192.168.10.0/24, VLAN20 192.168.20.0/24, VLAN30 .30.0/24, VLAN40 .40.0/24, VLAN50 .50.0/24.
- Loopbacks: CORE1 1.1.1.1, CORE2 2.2.2.2, DIST1 3.3.3.3, DIST2 4.4.4.4, ACC1 11.11.11.11, ACC2 12.12.12.12, ACC3 13.13.13.13.

**Base config — CORE1**
```
hostname CORE1
no ip domain lookup
ip domain name lab.local
crypto key generate rsa modulus 2048
username admin privilege 15 secret Cisco123!
enable secret Cisco123!
line vty 0 4
 login local
 transport input ssh
!
interface Loopback0
 ip address 1.1.1.1 255.255.255.255
!
interface GigabitEthernet0/1
 description to CORE2
 no switchport
 ip address 10.0.0.1 255.255.255.252
 no shutdown
interface GigabitEthernet0/2
 description to DIST1
 no switchport
 ip address 10.0.0.5 255.255.255.252
 no shutdown
interface GigabitEthernet0/3
 description to DIST2
 no switchport
 ip address 10.0.0.9 255.255.255.252
 no shutdown
!
router ospf 1
 router-id 1.1.1.1
 passive-interface default
 no passive-interface GigabitEthernet0/1
 no passive-interface GigabitEthernet0/2
 no passive-interface GigabitEthernet0/3
 network 1.1.1.1 0.0.0.0 area 0
 network 10.0.0.0 0.0.0.255 area 0
!
end
```

Repeat the same pattern for CORE2 / DIST1 / DIST2 with their respective IPs/loopbacks. (You'll do the access switches in Lab I2.)

**Verification**
- `show ip ospf neighbor` on each L3 device → 3 FULL/DR or FULL/BDR neighbors on the core, 4 on the dist pair after I2 is done.
- `show ip route ospf` → see all loopbacks reachable.
- `traceroute 4.4.4.4 source 1.1.1.1` from CORE1 → 2 hops via either path; run twice to see ECMP variation.

---

## Lab A2 — SD-Access Underlay/Overlay Concept Lab

**Goal:** Manually build what DNA Center would automate — IS-IS underlay + LISP+VXLAN overlay between three CSR1000v "fabric edges" with one acting as control plane.

**Topology**
```
                CP/Border (CSR-CP)
                 Lo0 100.100.100.100
                   /             \
                  /               \
            FE1 (CSR-FE1)----FE2 (CSR-FE2)
            Lo0 1.1.1.1       Lo0 2.2.2.2
              |                   |
            Host1               Host2
            10.10.10.10/24      10.20.20.20/24
            (EID space)         (EID space)
```

**Addressing**
- Underlay (IS-IS L2): 172.16.0.0/16 sliced as /30 p2p, loopbacks as router-ids.
- Overlay EIDs: 10.10.10.0/24 (FE1), 10.20.20.0/24 (FE2).
- VXLAN VNI 4097 for the overlay (LISP-mapped automatically when using `instance-id 4097`).

**Base config — CSR-FE1 (overlay only — assume underlay IS-IS already up)**
```
hostname CSR-FE1
ip routing
!
router lisp
 locator-set RLOC
  IPv4-interface Loopback0
 exit-locator-set
 instance-id 4097
  service ipv4
   eid-table default
   database-mapping 10.10.10.0/24 locator-set RLOC
   map-cache 0.0.0.0/0 map-request
   itr map-resolver 100.100.100.100
   itr
   etr map-server 100.100.100.100 key Cisco123!
   etr
   exit-service-ipv4
  exit-instance-id
 exit-router-lisp
!
interface Loopback0
 ip address 1.1.1.1 255.255.255.255
!
interface GigabitEthernet2
 description Host-Facing
 ip address 10.10.10.1 255.255.255.0
 no shutdown
```

**Base config — CSR-CP (Map-Server / Map-Resolver)**
```
router lisp
 site SITE-A
  authentication-key Cisco123!
  eid-record 10.10.10.0/24 accept-more-specifics
  eid-record 10.20.20.0/24 accept-more-specifics
 exit-site
 ipv4 map-server
 ipv4 map-resolver
```

**Verification**
- `show ip lisp` and `show ip lisp database` on each FE — should list its local EID.
- On CP: `show lisp site` should show both sites with **Y**es registered.
- From Host1 ping Host2 → on FE1: `show ip lisp map-cache` should show 10.20.20.0/24 → RLOC 2.2.2.2.
- Wireshark on underlay → outer UDP 4789 (VXLAN) carries inner LISP-encapsulated traffic.

---

## Lab A3 — StackWise Virtual / HSRP Convergence Test

**Goal:** Compare two redundancy approaches on the DIST layer of Lab A1 — chassis virtualization (StackWise Virtual concept — emulate with HSRP+MEC) versus pure HSRP. Measure failover time.

**Method**
- Build the A1 topology. PC1 (Linux) pings 8.8.8.8 (or PC3) continuously with `ping -i 0.2`.
- Trigger failover:
  - **Scenario A:** `shutdown` on DIST1 G0/4 — ACC1 reconverges via STP & HSRP.
  - **Scenario B:** repeat but with `standby 10 preempt delay minimum 60` on DIST1.
- Record dropped pings → compare RPVST+ (~2 s typical) vs MST (faster after `spanning-tree mst hello-time 1`) vs HSRP `preempt delay`.

**Show commands**
- `show standby brief`, `show spanning-tree`, `show etherchannel summary`.
- On Linux PC: `mtr -n 192.168.50.1` while toggling links — note path change.

---

# V — Virtualization

## Lab V1 — VRF-Lite + Inter-VRF Routing

**Goal:** Run two customer VRFs (CUST-A, CUST-B) on a single PE (CSR1000v), with overlapping IP space, and provide controlled leaking between them via route-targets.

**Topology**
```
   CE-A1 ---\                          /--- CE-B1
              PE1 (CSR1000v)
   CE-A2 ---/                          \--- CE-B2

   CE-A* uses 10.1.1.0/24
   CE-B* uses 10.1.1.0/24   (overlap on purpose)
```

**PE1 config**
```
hostname PE1
vrf definition CUST-A
 rd 65000:1
 address-family ipv4
  route-target export 65000:1
  route-target import 65000:1
  route-target import 65000:99
 exit-address-family
!
vrf definition CUST-B
 rd 65000:2
 address-family ipv4
  route-target export 65000:2
  route-target import 65000:2
 exit-address-family
!
interface GigabitEthernet2
 description to CE-A1
 vrf forwarding CUST-A
 ip address 10.1.1.1 255.255.255.0
 no shutdown
interface GigabitEthernet3
 description to CE-A2
 vrf forwarding CUST-A
 ip address 10.1.2.1 255.255.255.0
 no shutdown
interface GigabitEthernet4
 description to CE-B1
 vrf forwarding CUST-B
 ip address 10.1.1.1 255.255.255.0
 no shutdown
!
router bgp 65000
 address-family ipv4 vrf CUST-A
  redistribute connected
 address-family ipv4 vrf CUST-B
  redistribute connected
!
end
```

**Verification**
- `show ip route vrf CUST-A` and `show ip route vrf CUST-B` — note overlapping 10.1.1.0/24 entries are isolated.
- `ping vrf CUST-A 10.1.1.10` works; `ping 10.1.1.10` (global table) does NOT.
- Add controlled leak: `route-target import 65000:1` under CUST-B → see one prefix appear.

---

## Lab V2 — GRE-over-IPsec Site-to-Site

**Topology**
```
   HQ-LAN 192.168.10.0/24 - HQ (CSR1000v) === Internet === BRANCH (CSR1000v) - BR-LAN 192.168.20.0/24
                            Lo0 1.1.1.1                     Lo0 2.2.2.2
                            Tu0  172.16.0.1/30              Tu0  172.16.0.2/30
```

**HQ config**
```
crypto isakmp policy 10
 encr aes 256
 authentication pre-share
 group 14
 hash sha256
crypto isakmp key Cisco123! address 198.51.100.2
!
crypto ipsec transform-set TS esp-aes 256 esp-sha256-hmac
 mode transport
!
crypto ipsec profile IPSEC-PROF
 set transform-set TS
!
interface Tunnel0
 ip address 172.16.0.1 255.255.255.252
 tunnel source 198.51.100.1
 tunnel destination 198.51.100.2
 tunnel protection ipsec profile IPSEC-PROF
!
router ospf 1
 network 172.16.0.0 0.0.0.3 area 0
 network 192.168.10.0 0.0.0.255 area 0
```
Mirror on BRANCH (swap source/destination/IPs).

**Verification**
- `show crypto isakmp sa` → QM_IDLE.
- `show crypto ipsec sa` → encrypts/decrypts counters increment after pinging across.
- `show ip ospf neighbor` → adjacency over Tu0 (note tunnel transport saves on full-tunnel mode bytes).
- `show interface Tunnel0` → encap GRE/IP, transport IPsec.

---

## Lab V3 — VXLAN Flood-and-Learn between CSR1000v

**Goal:** Build a two-VTEP VXLAN with multicast control plane (flood-and-learn) — not EVPN. Builds intuition for what fabric overlay actually carries.

**Topology**
```
   Host-A (VLAN 100) - VTEP1 (CSR) =====IP underlay===== VTEP2 (CSR) - Host-B (VLAN 100)
                       Lo0 1.1.1.1                       Lo0 2.2.2.2
                       VNI 10100 mcast 239.1.1.100
```

**Underlay:** OSPF over Gi0/2 link, multicast PIM-SM with static RP at Lo0 of VTEP1.

**VTEP1**
```
ip multicast-routing distributed
ip pim rp-address 1.1.1.1
!
interface GigabitEthernet2
 ip address 10.0.0.1 255.255.255.252
 ip pim sparse-mode
 ip ospf 1 area 0
!
interface Loopback0
 ip address 1.1.1.1 255.255.255.255
 ip pim sparse-mode
 ip ospf 1 area 0
!
bridge-domain 100
!
interface nve1
 no shutdown
 source-interface Loopback0
 member vni 10100 mcast-group 239.1.1.100
!
interface GigabitEthernet3
 description to Host-A
 service instance 100 ethernet
  encapsulation dot1q 100
  rewrite ingress tag pop 1 symmetric
  bridge-domain 100
```
Mirror on VTEP2.

**Verification**
- `show nve interface nve1 detail`, `show nve peers` → peer appears after first frame.
- `show nve vni` → state Up, VNI 10100 mapped to BD 100.
- `show ip mroute 239.1.1.100` → (*,G) on RP, (S,G) when traffic flows.
- Wireshark on G0/2 → outer IP/UDP 4789 + VXLAN + inner Ethernet.

---

# I — Infrastructure

## Lab I1 — STP / RPVST+ / MST + Guards

**Topology**
```
                  ROOT-PRI (DIST1 vIOS-L2)
                  /                \
              ACC1 (vIOS-L2)----ACC2 (vIOS-L2)
                  \                /
                  ROOT-SEC (DIST2 vIOS-L2)
              -- diamond redundant L2 mesh, all four nodes vIOS-L2 --
   PC1 attached to ACC1 G0/3 (PortFast + BPDU Guard)
```

VLANs 10, 20, 30 trunked everywhere.

**Phase 1 — RPVST+**
```
! On all four switches:
spanning-tree mode rapid-pvst
vlan 10,20,30
!
! On DIST1:
spanning-tree vlan 10,20 priority 0
spanning-tree vlan 30 priority 4096
! On DIST2:
spanning-tree vlan 10,20 priority 4096
spanning-tree vlan 30 priority 0
!
! On ACC1 G0/3 (host-facing):
interface GigabitEthernet0/3
 switchport mode access
 switchport access vlan 10
 spanning-tree portfast
 spanning-tree bpduguard enable
!
! On any uplink that must never receive a superior BPDU from outside the trusted core:
interface GigabitEthernet0/1
 spanning-tree guard root
```

**Phase 2 — MST**
```
spanning-tree mode mst
spanning-tree mst configuration
 name LAB
 revision 1
 instance 1 vlan 10,20
 instance 2 vlan 30
spanning-tree mst 1 priority 0   ! on DIST1
spanning-tree mst 2 priority 0   ! on DIST2
```

**Verification**
- `show spanning-tree vlan 10` then `show spanning-tree mst 1` — compare port roles.
- Plug a rogue switch into ACC1 G0/3 → BPDU Guard err-disables, `show errdisable recovery`.
- `show spanning-tree inconsistentports` — exercise Root Guard by injecting a superior BPDU from a wrong direction.

---

## Lab I2 — VLANs, Trunks, LACP, Layer-3 Port-channel

**Goal:** Bundle the DIST1↔DIST2 link as both an L3 Po and the ACC1↔DIST1+DIST2 dual-homing as L2 LACP.

**ACC1 config**
```
hostname ACC1
vlan 10,20,30
!
interface range GigabitEthernet0/1 - 2
 description to DIST1 / DIST2
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk allowed vlan 10,20,30
 channel-protocol lacp
 channel-group 1 mode active
!
interface Port-channel1
 switchport trunk encapsulation dot1q
 switchport mode trunk
```

**DIST1 — Layer-3 Po to DIST2**
```
interface range GigabitEthernet0/3 - 4
 no switchport
 channel-group 12 mode active
!
interface Port-channel12
 no switchport
 ip address 10.0.0.21 255.255.255.252
 ip ospf 1 area 0
```

**Verification**
- `show etherchannel summary` → Po1 SU, Po12 RU (L3).
- `show interfaces trunk` on ACC1 — allowed VLANs match on both ends.
- `show etherchannel load-balance` then change to `port-channel load-balance src-dst-ip` and observe distribution with iperf flows.

---

## Lab I3 — OSPF Multi-Area + Filtering + OSPFv3

**Topology**
```
   R1 -- R2 -- R3 -- R4
   |     |          |
  Area1 Area0     Area2(NSSA)

   R1 Lo0 1.1.1.1, R2 2.2.2.2, R3 3.3.3.3, R4 4.4.4.4
   Links 10.12.12.0/30, 10.23.23.0/30, 10.34.34.0/30
   Loopback11 on R4 = 44.44.44.44/32 (redistributed from "static" to test NSSA Type-7)
```

**R2 (ABR Area1↔Area0)**
```
router ospf 1
 router-id 2.2.2.2
 area 1 range 1.1.1.0 255.255.255.252
 network 10.12.12.0 0.0.0.3 area 1
 network 10.23.23.0 0.0.0.3 area 0
 network 2.2.2.2 0.0.0.0 area 0
```

**R3 (ABR Area0↔Area2 NSSA)**
```
router ospf 1
 router-id 3.3.3.3
 area 2 nssa
 network 3.3.3.3 0.0.0.0 area 0
 network 10.23.23.0 0.0.0.3 area 0
 network 10.34.34.0 0.0.0.3 area 2
```

**R4 (NSSA — redistribute static)**
```
ip route 44.44.44.44 255.255.255.255 Null0
router ospf 1
 area 2 nssa
 redistribute static subnets
 network 4.4.4.4 0.0.0.0 area 2
 network 10.34.34.0 0.0.0.3 area 2
```

**Filtering — block 44.44.44.44 inbound on R1 via Distribute-list with prefix-list**
```
ip prefix-list DENY-44 seq 5 deny 44.44.44.44/32
ip prefix-list DENY-44 seq 10 permit 0.0.0.0/0 le 32
router ospf 1
 distribute-list prefix DENY-44 in
```

**OSPFv3 (address-families)**
```
ipv6 unicast-routing
router ospfv3 1
 address-family ipv6 unicast
  router-id 1.1.1.1
interface GigabitEthernet0/1
 ipv6 enable
 ospfv3 1 ipv6 area 0
```

**Verification**
- `show ip ospf neighbor`, `show ip ospf border-routers`.
- `show ip ospf database` — types 1/2 per area, type 3 from ABRs, type 5 nowhere in Area 2, type 7 from R4, converted to type 5 by R3.
- `show ip route ospf` — `O IA`, `O E2`, `O N2` markers visible.
- On R1: `show ip route 44.44.44.44` → no route (filtered).

---

## Lab I4 — eBGP / iBGP with Policy & Communities

**Topology**
```
              AS 65001                   AS 65002
        R1 ----- R2 (RR) ---- R3   <-eBGP->   R4
        Lo0 1   2 (RR-client) 3                4
        iBGP full of R1,R3 via R2 (RR)
```

Loopbacks 1.1.1.1, 2.2.2.2, 3.3.3.3, 4.4.4.4. Use OSPF inside AS65001 to provide IGP reachability for iBGP.

**R2 (Route Reflector)**
```
router bgp 65001
 bgp router-id 2.2.2.2
 neighbor IBGP peer-group
 neighbor IBGP remote-as 65001
 neighbor IBGP update-source Loopback0
 neighbor IBGP route-reflector-client
 neighbor 1.1.1.1 peer-group IBGP
 neighbor 3.3.3.3 peer-group IBGP
 address-family ipv4
  neighbor IBGP activate
  neighbor 1.1.1.1 activate
  neighbor 3.3.3.3 activate
```

**R3 (eBGP to R4 in AS65002)**
```
ip community-list 1 permit 65001:100
route-map FROM-R4 permit 10
 set local-preference 200
 set community 65001:100
!
router bgp 65001
 neighbor 10.34.34.4 remote-as 65002
 neighbor 2.2.2.2 remote-as 65001
 neighbor 2.2.2.2 update-source Loopback0
 address-family ipv4
  network 3.3.3.3 mask 255.255.255.255
  neighbor 10.34.34.4 activate
  neighbor 10.34.34.4 route-map FROM-R4 in
  neighbor 10.34.34.4 send-community both
  neighbor 2.2.2.2 activate
  neighbor 2.2.2.2 next-hop-self
```

**Verification**
- `show ip bgp summary` everywhere — Established, prefix counts sane.
- `show ip bgp` on R1 → 4.4.4.4 visible with LocPref 200, community 65001:100.
- Test best-path: prepend AS-path on R3 outbound, observe R1's path change to alternate exit if you also wire an exit on R2.
- `show ip bgp neighbors 2.2.2.2 advertised-routes` from R3 vs `received-routes` on R1.

---

## Lab I5 — HSRP + VRRP + Tracking

**Topology:** Reuse A1 distribution pair (DIST1/DIST2) + ACC1.

**DIST1**
```
interface Vlan10
 ip address 192.168.10.2 255.255.255.0
 standby version 2
 standby 10 ip 192.168.10.1
 standby 10 priority 110
 standby 10 preempt delay minimum 60
 standby 10 authentication md5 key-string Cisco123!
 standby 10 track 1 decrement 20
!
track 1 interface GigabitEthernet0/1 line-protocol
```

**DIST2**
```
interface Vlan10
 ip address 192.168.10.3 255.255.255.0
 standby version 2
 standby 10 ip 192.168.10.1
 standby 10 priority 100
 standby 10 preempt
```

**VRRP variant on VLAN20** — same idea, swap `standby` for `vrrp`.

**Verification**
- `show standby brief` → DIST1 Active, DIST2 Standby.
- `shutdown` on DIST1 G0/1 → track decrements priority by 20, DIST2 preempts.
- `debug standby events` while testing.

---

## Lab I6 — PIM-SM Multicast with Static RP

**Topology:** R1—R2—R3 chain. R2 = static RP. Source on R1 LAN, receiver on R3 LAN, group `239.10.10.10`.

**All routers**
```
ip multicast-routing distributed
ip pim rp-address 2.2.2.2
!
interface GigabitEthernet0/1
 ip pim sparse-mode
interface GigabitEthernet0/2
 ip pim sparse-mode
interface Loopback0
 ip pim sparse-mode
```

**R3 LAN-facing**
```
interface GigabitEthernet0/3
 ip pim sparse-mode
 ip igmp version 3
```

**Verification**
- From receiver Linux: `socat -u UDP4-RECV:5000,ip-add-membership=239.10.10.10:0.0.0.0 -` (joins).
- From source Linux: `iperf -c 239.10.10.10 -u -T 32 -t 30`.
- `show ip mroute` → `(*,239.10.10.10)` everywhere, `(192.168.x.x, 239.10.10.10)` once traffic flows.
- `show ip pim neighbor`, `show ip pim rp mapping`, `show ip igmp groups`.
- Capture on R2↔R3 link → see PIM Register / Register-Stop, then SPT switchover.

---

# NA — Network Assurance

## Lab NA1 — NTP + Syslog + SNMPv3 + IP SLA

Use Lab A1 baseline. Pick CORE1 as the lab's NTP/Syslog server target (or use an external Linux node with `chrony` + `rsyslog`).

**Every device**
```
clock timezone MSK 3 0
ntp server 1.1.1.1
ntp authenticate
ntp authentication-key 1 md5 Cisco123!
ntp trusted-key 1
!
logging host 192.0.2.100   ! a Linux syslog VM
logging trap informational
service timestamps log datetime msec localtime show-timezone
!
snmp-server group ENCOR-G v3 priv
snmp-server user ENCOR-U ENCOR-G v3 auth sha Cisco123! priv aes 128 Cisco123!
snmp-server host 192.0.2.100 version 3 priv ENCOR-U
snmp-server enable traps
!
ip sla 10
 icmp-echo 8.8.8.8 source-interface Loopback0
 frequency 5
ip sla schedule 10 life forever start-time now
track 10 ip sla 10 reachability
```

**Verification**
- `show ntp associations`, `show ntp status` → stratum 2+, sys peer set.
- `show snmp user`, then `snmpwalk -v3 -u ENCOR-U -l authPriv -a SHA -A Cisco123! -x AES -X Cisco123! 198.51.100.1 sysDescr`.
- `show ip sla statistics 10`, `show track 10`.
- On Linux syslog VM: `tail -f /var/log/network.log` — see authentication failures when you fat-finger a password.

---

## Lab NA2 — SPAN / RSPAN / ERSPAN

**SPAN (local)** on ACC1:
```
monitor session 1 source interface Gi0/3 both
monitor session 1 destination interface Gi0/4
```

**RSPAN** across ACC1→ACC2 (trunked VLAN):
```
! ACC1
vlan 999
 remote-span
monitor session 1 source interface Gi0/3 both
monitor session 1 destination remote vlan 999
! ACC2
vlan 999
 remote-span
monitor session 2 source remote vlan 999
monitor session 2 destination interface Gi0/4
```

**ERSPAN** (CSR1000v sender → Linux collector with Wireshark using `ip.proto == 47` + ERSPAN dissector):
```
monitor session 1 type erspan-source
 source interface Gi2 both
 no shutdown
 destination
  erspan-id 100
  ip address 192.0.2.200
  origin ip address 1.1.1.1
```

**Verification**
- `show monitor session all`.
- On dest interface plug a host running Wireshark — see mirrored frames.
- For ERSPAN: `wireshark -i eth0 -f "proto gre"` — see GRE-encap'd traffic with ERSPAN ID 100.

---

## Lab NA3 — Flexible NetFlow + Model-Driven Telemetry

**Flexible NetFlow on CSR1000v**
```
flow record FNF-REC
 match ipv4 source address
 match ipv4 destination address
 match transport source-port
 match transport destination-port
 match ipv4 protocol
 collect counter bytes
 collect counter packets
 collect timestamp absolute first
 collect timestamp absolute last
!
flow exporter FNF-EXP
 destination 192.0.2.100
 transport udp 9996
 source Loopback0
 export-protocol netflow-v9
!
flow monitor FNF-MON
 record FNF-REC
 exporter FNF-EXP
 cache timeout active 60
!
interface GigabitEthernet2
 ip flow monitor FNF-MON input
 ip flow monitor FNF-MON output
```

**Model-Driven Telemetry (gNMI/NETCONF dial-out, IOS-XE 17.x)**
```
netconf-yang
!
telemetry ietf subscription 101
 encoding encode-kvgpb
 filter xpath /interfaces-ios-xe-oper:interfaces/interface/statistics
 stream yang-push
 update-policy periodic 1000
 receiver ip address 192.0.2.100 57500 protocol grpc-tcp
```

**Verification**
- `show flow monitor FNF-MON cache` → top talkers visible.
- `show flow exporter FNF-EXP statistics`.
- On collector use `nfdump`, `ntopng`, or `softflowd` partner.
- `show telemetry ietf subscription all`, `show telemetry ietf subscription 101 receiver`.

---

# S — Security

## Lab S1 — AAA — Local + RADIUS Fallback

Add a Linux VM running FreeRADIUS at 192.0.2.50.

**Router (CSR or vIOS-L3)**
```
aaa new-model
!
radius server R1
 address ipv4 192.0.2.50 auth-port 1812 acct-port 1813
 key Cisco123!
!
aaa group server radius RAD
 server name R1
!
aaa authentication login VTY-AUTH group RAD local
aaa authentication enable default group RAD enable
aaa authorization exec VTY-AUTHZ group RAD local if-authenticated
aaa accounting exec default start-stop group RAD
!
line vty 0 4
 login authentication VTY-AUTH
 authorization exec VTY-AUTHZ
 transport input ssh
!
username backupadmin privilege 15 secret BackupCisco123!
```

**Verification**
- `test aaa group RAD <user> <pass> new-code` → PASS/FAIL with reason.
- SSH in as RADIUS user, then `enable`, then `show privilege`.
- `shutdown` the link to RADIUS → re-login uses local `backupadmin` fallback. Watch `debug aaa authentication`.

---

## Lab S2 — Layer-2 Hardening (DHCP snoop / DAI / IPSG / port-sec / CoPP)

**ACC1**
```
ip dhcp snooping
ip dhcp snooping vlan 10,20,30
no ip dhcp snooping information option
!
interface GigabitEthernet0/1   ! trunk to DIST1
 ip dhcp snooping trust
 ip arp inspection trust
!
interface range GigabitEthernet0/3 - 24
 switchport mode access
 switchport port-security
 switchport port-security maximum 3
 switchport port-security violation restrict
 switchport port-security mac-address sticky
 ip verify source
 storm-control broadcast level 1.00
 storm-control action shutdown
!
ip arp inspection vlan 10,20,30
ip arp inspection validate src-mac dst-mac ip
```

**CoPP on DIST1 (vIOS-L2 — CoPP works on the L2 image, applied at control-plane just like on a Catalyst)**
```
ip access-list extended COPP-ICMP
 permit icmp any any
class-map match-any COPP-CRITICAL
 match access-group name COPP-ICMP
policy-map COPP-IN
 class COPP-CRITICAL
  police 8000 conform-action transmit exceed-action drop
control-plane
 service-policy input COPP-IN
```

**Verification**
- `show ip dhcp snooping`, `show ip dhcp snooping binding`.
- `show ip arp inspection`, `show ip verify source`.
- `show port-security interface Gi0/3`.
- Trigger violation by spoofing a second MAC; `show errdisable recovery`.
- `show policy-map control-plane input` → conform/exceed counters.

---

## Lab S3 — Zone-Based Firewall on CSR1000v

**Topology:** CSR has INSIDE (Gi2) and OUTSIDE (Gi3). Permit only HTTP/HTTPS/DNS/ICMP from INSIDE→OUTSIDE; deny inbound.

```
zone security INSIDE
zone security OUTSIDE
!
ip access-list extended ACL-WEB
 permit tcp any any eq 80
 permit tcp any any eq 443
 permit udp any any eq 53
 permit icmp any any
!
class-map type inspect match-any CM-WEB
 match access-group name ACL-WEB
!
policy-map type inspect PM-IN-OUT
 class type inspect CM-WEB
  inspect
 class class-default
  drop log
!
zone-pair security IN-OUT source INSIDE destination OUTSIDE
 service-policy type inspect PM-IN-OUT
!
interface GigabitEthernet2
 zone-member security INSIDE
interface GigabitEthernet3
 zone-member security OUTSIDE
```

**Verification**
- From INSIDE host: `curl https://1.1.1.1` works; `nc -zv 1.1.1.1 22` blocked.
- `show policy-map type inspect zone-pair sessions` — see active flows + stateful info.
- `show zone-pair security`, `show class-map type inspect`.

---

# AU — Automation & Programmability

## Lab AU1 — NETCONF + RESTCONF on CSR1000v

**Enable on CSR1000v**
```
netconf-yang
netconf-yang feature candidate-datastore
restconf
ip http secure-server
username api privilege 15 secret Cisco123!
```

**Verify from a Linux VM (any IP-reachable host)**
```bash
# NETCONF hello
ssh -p 830 -s api@198.51.100.1 netconf
```
```python
# ncclient example
from ncclient import manager
with manager.connect(host="198.51.100.1", port=830, username="api",
                     password="Cisco123!", hostkey_verify=False) as m:
    print(m.get_config(source="running").data_xml[:2000])
```
```bash
# RESTCONF
curl -k -u api:'Cisco123!' \
  -H 'Accept: application/yang-data+json' \
  https://198.51.100.1/restconf/data/Cisco-IOS-XE-native:native/hostname
```

**Verification**
- `show platform software yang-management process` — confnd, ncsshd, etc. = Running.
- `show netconf-yang sessions detail` while ncclient is connected.
- `show restconf` (privileged).

---

## Lab AU2 — Ansible push: OSPF + VLAN config

**Inventory** (`hosts.ini`)
```
[csr]
csr1 ansible_host=198.51.100.1

[csr:vars]
ansible_user=api
ansible_password=Cisco123!
ansible_network_os=cisco.ios.ios
ansible_connection=network_cli
```

**Playbook** (`encor.yml`)
```yaml
- hosts: csr
  gather_facts: no
  tasks:
    - name: Ensure loopback
      cisco.ios.ios_l3_interfaces:
        config:
          - name: Loopback100
            ipv4: [{ address: 100.100.100.100/32 }]
        state: merged

    - name: Ensure OSPF
      cisco.ios.ios_ospfv2:
        config:
          processes:
            - process_id: 1
              router_id: 100.100.100.100
              areas:
                - area_id: '0'
        state: merged
```

Run: `ansible-playbook -i hosts.ini encor.yml`.

**Verification**
- Re-run → idempotent (changed=0).
- `show running-config | section router ospf`.
- Roll back via a second playbook with `state: deleted`.

---

## Lab AU3 — EEM applet — interface-state syslog reaction

```
event manager applet IF-DOWN-ALERT
 event syslog pattern "Interface GigabitEthernet0/1, changed state to down"
 action 1.0 syslog priority warnings msg "EEM: G0/1 down — auto-action firing"
 action 1.1 cli command "enable"
 action 1.2 cli command "show ip int brief"
 action 1.3 cli command "show log | tail 10"
 action 2.0 mail server "192.0.2.100" to "noc@lab.local" from "router@lab.local" subject "G0/1 DOWN on $_hostname" body "$_cli_result"
```

**Verification**
- `event manager run IF-DOWN-ALERT` (manual trigger) OR `shutdown` on Gi0/1.
- `show event manager history events`, `show event manager statistics server`.
- See action output in syslog.

---

## Lab cross-reference to weeks

| Week | Recommended labs |
|---|---|
| 1 | A1 |
| 2 | I1 |
| 3 | I2 |
| 4 | I3 |
| 5 | I4 |
| 6 | I5, I6 |
| 7 | NA1 |
| 8 | V2, V3 |
| 9 | (wireless GUI — DevNet sandbox) |
| 10 | A2 (+ optional A3) |
| 11 | NA2, NA3 |
| 12 | S1, S2 |
| 13 | S3 |
| 14 | V1, AU1, AU2, AU3 |
