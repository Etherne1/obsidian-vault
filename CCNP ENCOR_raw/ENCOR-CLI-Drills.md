# ENCOR 350-401 — CLI Configuration Drills

**Purpose:** Short, focused config exercises that build muscle memory. Each drill targets one concept, takes 5–15 minutes, and reuses the EVE-NG topology style from `ENCOR-Labs-EVE-NG.md` (vIOS-L2, vIOS-L3, CSR1000v 17.3.04a).

**When to use:** Pass 2 only. Don't touch these during Pass 1 — you don't have a mental model yet and the drills won't stick.

**Three modes in Pass 2:**
- **Warmup (before a lab session):** pick 1–2 drills from the same block as the GNS3vault lab you're about to run. Type from memory before opening EVE-NG.
- **Cooldown (after a lab session):** pick 1 drill from the same block. No notes — from memory only. If you fail, it becomes an Anki card.
- **Marathon (Pass 2 Blocks 11–12):** 5–8 drills per session, mixed domains, no peeking. By the capstone you should breeze through all 60 cold.

**How to run a drill:**
1. Spin up just the devices the drill needs (don't load a full lab unless the drill maps to one).
2. Try the goal *first without looking* at the solution.
3. Compare your config to the solution.
4. Verify with the listed show commands.
5. Mark it done in your tracker: `D-NN ✓`. Repeat the next day if you had to peek.

**Drill structure:** Each drill has:
- **Goal** — one-line objective
- **Devices/Interfaces** — what to touch
- **Maps to lab** — which `ENCOR-Labs-EVE-NG.md` lab uses this topology, if any
- **Solution config** — paste-ready
- **Verify with** — show/debug commands

**Naming conventions** (consistent with the labs file):
- Loopback0 IP = `<router-num>.<router-num>.<router-num>.<router-num>/32`
- Lab links use `Gi0/1`, `Gi0/2`, etc.
- Default `enable secret Cisco123!`, `username admin secret Cisco123!`

**Drill index by domain**

| # | Domain | Drill |
|---|---|---|
| 1 | Foundations | Bring up a clean vIOS-L3 baseline |
| 2 | Foundations | Bring up a clean CSR1000v baseline |
| 3 | Foundations | Enable SSH-only management |
| 4 | L2 | Create VLANs and assign access ports |
| 5 | L2 | Configure a 802.1Q trunk with allowed VLAN list |
| 6 | L2 | Set native VLAN to an unused VLAN and tag it |
| 7 | L2 | Force RPVST+ and make a switch root for specific VLANs |
| 8 | L2 | Configure MST region with two instances |
| 9 | L2 | Apply PortFast + BPDU Guard to all access ports |
| 10 | L2 | Apply Root Guard on a distribution downlink |
| 11 | L2 | Build a Layer-2 LACP EtherChannel |
| 12 | L2 | Build a Layer-3 LACP EtherChannel with IP |
| 13 | L2 | Configure UDLD aggressive on fiber uplinks |
| 14 | L3 | OSPF single-area between three routers |
| 15 | L3 | Set OSPF reference bandwidth to 10 Gbps |
| 16 | L3 | Configure OSPF stub area |
| 17 | L3 | Configure OSPF NSSA with local redistribution |
| 18 | L3 | OSPF inter-area summarization at ABR |
| 19 | L3 | OSPF filtering with prefix-list distribute-list |
| 20 | L3 | OSPFv3 (IPv6) basic adjacency |
| 21 | L3 | eBGP between two ASes with loopback peering |
| 22 | L3 | iBGP with `next-hop-self` |
| 23 | L3 | Apply a route-map with set local-preference |
| 24 | L3 | BGP community tagging on outbound advertisements |
| 25 | L3 | BGP route-reflector for iBGP scaling |
| 26 | L3 | HSRP v2 with preempt and tracking |
| 27 | L3 | VRRP with priority and authentication |
| 28 | L3 | Static NAT for an inside server |
| 29 | L3 | PAT (NAT overload) for inside subnet |
| 30 | Multicast | Enable PIM-SM with a static RP |
| 31 | Multicast | IGMP snooping on access switch |
| 32 | Services | NTP client with authentication |
| 33 | Services | Syslog to a remote server with timestamps |
| 34 | Services | SNMPv3 user with authPriv |
| 35 | Services | IP SLA ICMP echo + Object Tracking |
| 36 | Services | Flexible NetFlow with explicit record/exporter/monitor |
| 37 | Overlay | GRE tunnel between two routers |
| 38 | Overlay | IPsec VTI between two CSR1000v |
| 39 | Overlay | GRE-over-IPsec transport mode |
| 40 | Overlay | LISP basic ITR/ETR registration to Map-Server |
| 41 | Virtualization | Create two VRFs with route leaking |
| 42 | Virtualization | VRF-aware static route |
| 43 | Network Assurance | Local SPAN session |
| 44 | Network Assurance | RSPAN across two switches |
| 45 | Network Assurance | ERSPAN from a CSR1000v to a remote collector |
| 46 | Network Assurance | Enable model-driven telemetry to a gRPC collector |
| 47 | Security | AAA with RADIUS group + local fallback |
| 48 | Security | DHCP Snooping with one trusted uplink |
| 49 | Security | Dynamic ARP Inspection + IP Source Guard |
| 50 | Security | Port-security with sticky MAC and restrict mode |
| 51 | Security | CoPP for ICMP and routing protocols |
| 52 | Security | uRPF strict on an interface |
| 53 | Security | Zone-Based Firewall with one zone-pair |
| 54 | Security | 802.1X with MAB fallback on a switchport |
| 55 | Automation | Enable NETCONF + RESTCONF on CSR1000v |
| 56 | Automation | EEM applet on interface down event |
| 57 | Automation | EEM timer cron applet |
| 58 | Automation | Save running-config periodically via EEM |
| 59 | Automation | Add a basic Guest Shell Python script |
| 60 | Automation | Configure a periodic telemetry subscription via CLI |

---

## 1. Foundations

### Drill 1 — Bring up a clean vIOS-L3 baseline

**Goal:** Bring R1 to a state where SSH works and OSPF can run.
**Devices:** R1 (vIOS-L3).
**Maps to lab:** A1 (Enterprise Campus Baseline).

```
enable
configure terminal
hostname R1
no ip domain lookup
ip domain name lab.local
crypto key generate rsa modulus 2048
ip ssh version 2
username admin privilege 15 secret Cisco123!
enable secret Cisco123!
line con 0
 logging synchronous
 exec-timeout 0 0
line vty 0 4
 login local
 transport input ssh
!
interface Loopback0
 ip address 1.1.1.1 255.255.255.255
end
write memory
```

**Verify with:**
- `show ip interface brief` → Lo0 up/up.
- `show ip ssh` → version 2.
- `show running-config | include username|enable secret` → both present.

---

### Drill 2 — Bring up a clean CSR1000v baseline

**Goal:** Same as Drill 1 but on CSR1000v (license, throughput note).
**Devices:** CSR-1 (CSR1000v 17.3.04a).

```
enable
configure terminal
hostname CSR-1
no ip domain lookup
ip domain name lab.local
crypto key generate rsa modulus 2048
ip ssh version 2
username admin privilege 15 secret Cisco123!
enable secret Cisco123!
license boot level network-advantage addon dna-advantage
platform hardware throughput level MB 1000
line con 0
 logging synchronous
 exec-timeout 0 0
line vty 0 4
 login local
 transport input ssh
!
interface Loopback0
 ip address 1.1.1.1 255.255.255.255
end
write memory
```

**Verify with:**
- `show platform hardware throughput level` → 1000 MB.
- `show license summary` → network-advantage + dna-advantage.

---

### Drill 3 — Enable SSH-only management

**Goal:** Disable telnet, lock down VTY, set timeouts, ACL VTY to a management subnet.
**Devices:** any router.

```
ip access-list standard MGMT-IN
 permit 192.0.2.0 0.0.0.255
 deny any log
!
line vty 0 4
 access-class MGMT-IN in
 transport input ssh
 exec-timeout 5 0
 login local
!
no ip http server
no ip http secure-server   ! (re-enable for RESTCONF labs later)
```

**Verify with:**
- `telnet <router>` from outside MGMT → refused.
- `show ip access-lists MGMT-IN` → counter increments on attempts.
- `show line vty 0` → exec timeout 0:05:00.

---

## 2. Layer 2

### Drill 4 — Create VLANs and assign access ports

**Goal:** Create VLAN 10 (Users), 20 (Voice), 99 (Native-unused), and place Gi0/3 in VLAN 10 with voice VLAN 20.
**Devices:** ACC1 (vIOS-L2).
**Maps to lab:** I2.

```
vlan 10
 name USERS
vlan 20
 name VOICE
vlan 99
 name NATIVE-UNUSED
!
interface GigabitEthernet0/3
 description Workstation+Phone
 switchport mode access
 switchport access vlan 10
 switchport voice vlan 20
 spanning-tree portfast
 spanning-tree bpduguard enable
```

**Verify with:**
- `show vlan brief`.
- `show interfaces Gi0/3 switchport` → Access VLAN 10, Voice VLAN 20.

---

### Drill 5 — 802.1Q trunk with allowed VLAN list

**Goal:** Trunk Gi0/1 between ACC1 and DIST1; allow only VLANs 10, 20, 30.
**Devices:** ACC1 + DIST1.

```
! On both ends
interface GigabitEthernet0/1
 description trunk
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport nonegotiate
 switchport trunk allowed vlan 10,20,30
```

**Verify with:**
- `show interfaces trunk` — Allowed VLANs on trunking links match.
- `show interfaces Gi0/1 switchport`.

---

### Drill 6 — Set native VLAN to unused VLAN and tag it

**Goal:** Move native VLAN to 99 and tag it on the trunk to block VLAN hopping.
**Devices:** ACC1, DIST1.

```
vlan 99
 name NATIVE-UNUSED
!
interface GigabitEthernet0/1
 switchport trunk native vlan 99
!
vlan dot1q tag native    ! global — tags native VLAN
```

**Verify with:**
- `show interfaces trunk` → Native VLAN: 99.
- `show vlan dot1q tag native`.
- Wireshark on the trunk → native frames now carry tag 99.

---

### Drill 7 — Force RPVST+ and set root for specific VLANs

**Goal:** DIST1 primary root for VLANs 10, 20; secondary root for VLAN 30.
**Devices:** DIST1.

```
spanning-tree mode rapid-pvst
!
spanning-tree vlan 10,20 root primary
spanning-tree vlan 30 root secondary
```

**Verify with:**
- `show spanning-tree vlan 10` → DIST1 is "This bridge is the root".
- `show spanning-tree summary` → mode rapid-pvst.

---

### Drill 8 — MST region with two instances

**Goal:** All switches in region "LAB", revision 1; VLANs 10/20 in MST1, VLAN 30 in MST2.
**Devices:** All switches.
**Maps to lab:** I1.

```
spanning-tree mode mst
spanning-tree mst configuration
 name LAB
 revision 1
 instance 1 vlan 10,20
 instance 2 vlan 30
 exit
!
spanning-tree mst 1 priority 0    ! on DIST1
spanning-tree mst 2 priority 0    ! on DIST2
```

**Verify with:**
- `show spanning-tree mst configuration` → Region LAB rev 1.
- `show spanning-tree mst 1` → roles per port.

---

### Drill 9 — PortFast + BPDU Guard on all access ports

**Goal:** Apply edge-port hardening globally.
**Devices:** ACC1.

```
spanning-tree portfast default              ! applies to access ports
spanning-tree portfast bpduguard default    ! applies BPDU Guard to portfast-enabled ports
!
errdisable recovery cause bpduguard
errdisable recovery interval 60
```

**Verify with:**
- `show spanning-tree summary` → Portfast default enabled / BPDU Guard default enabled.
- `show errdisable recovery`.

---

### Drill 10 — Root Guard on a distribution downlink

**Goal:** Prevent any switch connected to DIST1 Gi0/5 from becoming root.
**Devices:** DIST1.

```
interface GigabitEthernet0/5
 spanning-tree guard root
```

**Verify with:**
- `show spanning-tree inconsistentports` → empty until violation.
- Inject a low-priority bridge → port enters `root-inconsistent`.

---

### Drill 11 — Layer-2 LACP EtherChannel

**Goal:** Bundle Gi0/1 and Gi0/2 between ACC1 and DIST1 as Po1, LACP active.
**Devices:** ACC1, DIST1.
**Maps to lab:** I2.

```
interface range GigabitEthernet0/1 - 2
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

**Verify with:**
- `show etherchannel summary` → Po1 SU, members P.
- `show lacp neighbor` → both ports up, partner active.

---

### Drill 12 — Layer-3 LACP EtherChannel

**Goal:** Build a routed Po between DIST1 and DIST2 with IP `10.0.0.21/30` on DIST1 side.
**Devices:** DIST1, DIST2.
**Maps to lab:** I2.

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

DIST2 mirrors with 10.0.0.22.

**Verify with:**
- `show etherchannel summary` → Po12 RU.
- `show ip ospf neighbor` → adjacency over Po12.

---

### Drill 13 — UDLD aggressive on fiber uplinks

**Goal:** Enable UDLD aggressive globally for fiber, then explicit on Gi0/1.
**Devices:** ACC1.

```
udld aggressive       ! global — fiber only by default
!
interface GigabitEthernet0/1
 udld port aggressive
```

**Verify with:**
- `show udld Gi0/1` → Mode: Aggressive, Neighbor Detected.
- `show udld neighbors`.

---

## 3. Layer 3 — IGP

### Drill 14 — OSPF single-area on three routers

**Goal:** R1-R2-R3 chain, single area 0, loopbacks in OSPF.
**Devices:** R1, R2, R3.
**Maps to lab:** I3 (variant).

```
! R2 (middle)
router ospf 1
 router-id 2.2.2.2
 passive-interface default
 no passive-interface GigabitEthernet0/1
 no passive-interface GigabitEthernet0/2
 network 2.2.2.2 0.0.0.0 area 0
 network 10.12.12.0 0.0.0.3 area 0
 network 10.23.23.0 0.0.0.3 area 0
```

R1, R3 use same pattern with their own loopbacks/p2p subnets.

**Verify with:**
- `show ip ospf neighbor` on R2 → two FULL neighbors.
- `show ip route ospf` → see all loopbacks via OSPF.

---

### Drill 15 — Set OSPF reference bandwidth to 10 Gbps

**Goal:** Make 1G interfaces cost 10 and 10G interfaces cost 1.
**Devices:** all OSPF speakers.

```
router ospf 1
 auto-cost reference-bandwidth 10000
```

**Verify with:**
- `show ip ospf interface Gi0/1` → Cost: 10 (for 1G), 1 (for 10G).
- Note: must be applied on **all** routers; OSPF will warn if mismatched.

---

### Drill 16 — OSPF stub area

**Goal:** Make Area 1 a totally-stubby area.
**Devices:** ABR (R2) + Area 1 router (R1).

```
! R2 (ABR)
router ospf 1
 area 1 stub no-summary
!
! R1 (inside Area 1)
router ospf 1
 area 1 stub
```

**Verify with:**
- `show ip ospf` on R1 → "It is a stub area".
- `show ip route` on R1 → only intra-area + 0.0.0.0/0 default; no Type-3 or Type-5.

---

### Drill 17 — OSPF NSSA with redistribution

**Goal:** R4 inside NSSA area 2, redistributes a static into OSPF as Type-7.
**Devices:** R4 (in NSSA), R3 (NSSA ABR).
**Maps to lab:** I3.

```
! R4
ip route 44.44.44.44 255.255.255.255 Null0
router ospf 1
 area 2 nssa
 redistribute static subnets
!
! R3 (ABR for Area 2)
router ospf 1
 area 2 nssa
```

**Verify with:**
- `show ip ospf database nssa-external` on R4 → Type-7 LSA for 44.44.44.44.
- `show ip route ospf` on R1 (Area 0) → 44.44.44.44 as O E2 (translated to Type-5 by R3).

---

### Drill 18 — OSPF inter-area summarization at ABR

**Goal:** R2 (ABR Area1↔Area0) summarizes Area 1 loopbacks 1.1.1.0/30 into a single advertisement.
**Devices:** R2.

```
router ospf 1
 area 1 range 1.1.1.0 255.255.255.252
```

**Verify with:**
- `show ip route ospf` on R3 (in Area 0) → single 1.1.1.0/30 instead of individual /32s.
- `show ip ospf database summary` on R3.

---

### Drill 19 — OSPF filtering with prefix-list distribute-list

**Goal:** R1 should not install 44.44.44.44/32 into its RIB.
**Devices:** R1.

```
ip prefix-list DENY-44 seq 5 deny 44.44.44.44/32
ip prefix-list DENY-44 seq 10 permit 0.0.0.0/0 le 32
!
router ospf 1
 distribute-list prefix DENY-44 in
```

**Verify with:**
- `show ip route 44.44.44.44` → "% Network not in table".
- `show ip ospf database external | begin 44` → LSA still in DB (filter is RIB-only).

---

### Drill 20 — OSPFv3 (IPv6) basic adjacency

**Goal:** Bring up OSPFv3 between R1 and R2 over IPv6 link-local.
**Devices:** R1, R2.

```
ipv6 unicast-routing
!
interface GigabitEthernet0/1
 ipv6 enable
 ipv6 address 2001:DB8:12::1/64
 ospfv3 1 ipv6 area 0
!
router ospfv3 1
 router-id 1.1.1.1
 address-family ipv6 unicast
  passive-interface default
  no passive-interface GigabitEthernet0/1
 exit-address-family
```

**Verify with:**
- `show ospfv3 neighbor`.
- `show ipv6 route ospf`.

---

## 4. BGP

### Drill 21 — eBGP with loopback peering

**Goal:** R1 (AS65001) ↔ R4 (AS65002), peer via loopbacks, use `ebgp-multihop`.
**Devices:** R1, R4.
**Maps to lab:** I4.

```
! R1
ip route 4.4.4.4 255.255.255.255 GigabitEthernet0/1 10.14.14.4
router bgp 65001
 bgp router-id 1.1.1.1
 neighbor 4.4.4.4 remote-as 65002
 neighbor 4.4.4.4 ebgp-multihop 2
 neighbor 4.4.4.4 update-source Loopback0
 address-family ipv4
  network 1.1.1.1 mask 255.255.255.255
  neighbor 4.4.4.4 activate
```

R4 mirrors.

**Verify with:**
- `show ip bgp summary` → State Established.
- `show ip bgp` → 1.1.1.1/32 and 4.4.4.4/32 both visible.

---

### Drill 22 — iBGP with `next-hop-self`

**Goal:** R2 is iBGP RR; R1, R3 are clients. R3 has eBGP to AS65002. `next-hop-self` on R3 toward R2.
**Devices:** R2, R3.

```
! R3
router bgp 65001
 neighbor 2.2.2.2 remote-as 65001
 neighbor 2.2.2.2 update-source Loopback0
 address-family ipv4
  neighbor 2.2.2.2 next-hop-self
  neighbor 2.2.2.2 activate
```

**Verify with:**
- On R1: `show ip bgp 4.4.4.4` → next-hop = 3.3.3.3 (R3's loopback), reachable via IGP.

---

### Drill 23 — Route-map setting local preference

**Goal:** R1 prefers paths from R2 over R3 by setting LP to 200 inbound from R2.
**Devices:** R1.

```
route-map FROM-R2 permit 10
 set local-preference 200
!
router bgp 65001
 address-family ipv4
  neighbor 2.2.2.2 route-map FROM-R2 in
!
! After change, clear soft:
clear ip bgp 2.2.2.2 soft in
```

**Verify with:**
- `show ip bgp` → entries via R2 now show LocPrf 200, marked best.

---

### Drill 24 — BGP community tagging on outbound

**Goal:** R3 tags its eBGP-out advertisements with community 65001:100, and ensures community is sent.
**Devices:** R3.

```
ip community-list 1 permit 65001:100
!
route-map TO-EBGP permit 10
 set community 65001:100 additive
!
router bgp 65001
 address-family ipv4
  neighbor 10.34.34.4 route-map TO-EBGP out
  neighbor 10.34.34.4 send-community both
```

**Verify with:**
- On R4 (eBGP peer): `show ip bgp 3.3.3.3` → Community: 65001:100.

---

### Drill 25 — Route-reflector for iBGP scaling

**Goal:** R2 is RR, R1 and R3 are clients (peer-group based).
**Devices:** R2.

```
router bgp 65001
 neighbor IBGP peer-group
 neighbor IBGP remote-as 65001
 neighbor IBGP update-source Loopback0
 address-family ipv4
  neighbor IBGP route-reflector-client
  neighbor 1.1.1.1 peer-group IBGP
  neighbor 3.3.3.3 peer-group IBGP
  neighbor IBGP activate
  neighbor 1.1.1.1 activate
  neighbor 3.3.3.3 activate
```

**Verify with:**
- `show ip bgp neighbors 1.1.1.1 | include reflector` → "Route-Reflector Client".
- On R1: `show ip bgp 4.4.4.4` → ORIGINATOR_ID = 3.3.3.3, CLUSTER_LIST = 2.2.2.2.

---

## 5. FHRP

### Drill 26 — HSRPv2 with preempt and tracking

**Goal:** VLAN 10 SVI; DIST1 active (priority 110), DIST2 standby (default 100), tracking uplink.
**Devices:** DIST1, DIST2.
**Maps to lab:** I5.

```
! DIST1
track 1 interface GigabitEthernet0/1 line-protocol
!
interface Vlan10
 ip address 192.168.10.2 255.255.255.0
 standby version 2
 standby 10 ip 192.168.10.1
 standby 10 priority 110
 standby 10 preempt delay minimum 60
 standby 10 authentication md5 key-string Cisco123!
 standby 10 track 1 decrement 20
```

DIST2 same but priority default and IP `.3`.

**Verify with:**
- `show standby brief` → DIST1 Active, DIST2 Standby.
- `shutdown` G0/1 on DIST1 → track decrement → DIST2 preempts.

---

### Drill 27 — VRRP with priority and authentication

**Goal:** VLAN 20 SVI; DIST2 master (priority 110), DIST1 backup (100), MD5 auth.
**Devices:** DIST1, DIST2.

```
! DIST2
interface Vlan20
 ip address 192.168.20.3 255.255.255.0
 vrrp 20 ip 192.168.20.1
 vrrp 20 priority 110
 vrrp 20 authentication md5 key-string Cisco123!
```

DIST1 same but priority 100, IP `.2`.

**Verify with:**
- `show vrrp brief` → DIST2 Master, DIST1 Backup.

---

## 6. NAT

### Drill 28 — Static NAT for an inside server

**Goal:** Map outside 198.51.100.50 → inside 192.168.10.10 (a web server).
**Devices:** CSR-EDGE.

```
interface GigabitEthernet2
 description INSIDE
 ip nat inside
interface GigabitEthernet3
 description OUTSIDE
 ip nat outside
!
ip nat inside source static 192.168.10.10 198.51.100.50
```

**Verify with:**
- `show ip nat translations` → Static entry.
- From outside: `curl http://198.51.100.50` → reaches 192.168.10.10.

---

### Drill 29 — PAT (NAT overload) for inside subnet

**Goal:** Translate all 192.168.10.0/24 hosts behind the outside interface IP.
**Devices:** CSR-EDGE.

```
access-list 10 permit 192.168.10.0 0.0.0.255
!
ip nat inside source list 10 interface GigabitEthernet3 overload
```

**Verify with:**
- Generate traffic from inside hosts; `show ip nat translations` shows TCP/UDP entries with port translation.
- `show ip nat statistics`.

---

## 7. Multicast

### Drill 30 — PIM-SM with static RP

**Goal:** Enable PIM-SM on R1, R2, R3 with R2's loopback as RP.
**Devices:** R1, R2, R3.
**Maps to lab:** I6.

```
ip multicast-routing distributed
ip pim rp-address 2.2.2.2
!
interface Loopback0
 ip pim sparse-mode
interface GigabitEthernet0/1
 ip pim sparse-mode
interface GigabitEthernet0/2
 ip pim sparse-mode
```

**Verify with:**
- `show ip pim neighbor` → expected PIM neighbors.
- `show ip pim rp mapping` → static RP 2.2.2.2.
- After source/receiver join: `show ip mroute`.

---

### Drill 31 — IGMP snooping on access switch

**Goal:** Ensure IGMP snooping is enabled (default is on, but explicit and limit) on ACC1.
**Devices:** ACC1.

```
ip igmp snooping
ip igmp snooping vlan 10
!
interface GigabitEthernet0/3
 ip igmp snooping report-suppression
```

**Verify with:**
- `show ip igmp snooping vlan 10`.
- `show ip igmp snooping groups`.

---

## 8. Services

### Drill 32 — NTP client with authentication

**Goal:** R1 syncs to server 192.0.2.10 with MD5 key 1.
**Devices:** R1.

```
clock timezone MSK 3 0
ntp authentication-key 1 md5 Cisco123!
ntp authenticate
ntp trusted-key 1
ntp server 192.0.2.10 key 1
ntp source Loopback0
```

**Verify with:**
- `show ntp associations` → server in sys peer.
- `show ntp status` → stratum incremented.
- `show ntp authentication`.

---

### Drill 33 — Syslog to remote server with timestamps

**Goal:** Send levels 0–6 to 192.0.2.100, include msec timestamps.
**Devices:** R1.

```
service timestamps log datetime msec localtime show-timezone
logging host 192.0.2.100 transport udp port 514
logging trap informational
logging source-interface Loopback0
```

**Verify with:**
- `show logging` → Trap logging: level informational, host 192.0.2.100.
- On syslog server: messages arriving with millisecond timestamps.

---

### Drill 34 — SNMPv3 user with authPriv

**Goal:** Create group ENCOR-G and user ENCOR-U with SHA auth + AES-128 priv.
**Devices:** R1.

```
snmp-server group ENCOR-G v3 priv read READ_VIEW
snmp-server view READ_VIEW iso included
snmp-server user ENCOR-U ENCOR-G v3 auth sha Cisco123! priv aes 128 Cisco123!
snmp-server host 192.0.2.100 version 3 priv ENCOR-U
snmp-server enable traps
```

**Verify with:**
- From a Linux box: `snmpwalk -v3 -u ENCOR-U -l authPriv -a SHA -A Cisco123! -x AES -X Cisco123! 1.1.1.1 sysDescr`.
- `show snmp user`.

---

### Drill 35 — IP SLA ICMP echo + Object Tracking

**Goal:** Probe 8.8.8.8 every 5 s; track 10 reflects reachability; condition a static default route.
**Devices:** R1.

```
ip sla 10
 icmp-echo 8.8.8.8 source-interface Loopback0
 threshold 1000
 timeout 1500
 frequency 5
ip sla schedule 10 life forever start-time now
!
track 10 ip sla 10 reachability
 delay down 2 up 5
!
ip route 0.0.0.0 0.0.0.0 198.51.100.1 track 10
```

**Verify with:**
- `show ip sla statistics 10`.
- `show track 10`.
- `show ip route 0.0.0.0` → conditional route active.

---

### Drill 36 — Flexible NetFlow

**Goal:** Define record / exporter / monitor and attach to Gi0/1 input.
**Devices:** CSR-1.
**Maps to lab:** NA3.

```
flow record FNF-REC
 match ipv4 source address
 match ipv4 destination address
 match ipv4 protocol
 match transport source-port
 match transport destination-port
 collect counter bytes
 collect counter packets
 collect timestamp absolute first
 collect timestamp absolute last
!
flow exporter FNF-EXP
 destination 192.0.2.100
 source Loopback0
 transport udp 9996
 export-protocol netflow-v9
!
flow monitor FNF-MON
 record FNF-REC
 exporter FNF-EXP
 cache timeout active 60
!
interface GigabitEthernet1
 ip flow monitor FNF-MON input
 ip flow monitor FNF-MON output
```

**Verify with:**
- `show flow monitor FNF-MON cache` → top talkers.
- `show flow exporter FNF-EXP statistics`.

---

## 9. Overlays

### Drill 37 — GRE tunnel between two routers

**Goal:** Build Tu0 between R1 and R2 with 172.16.0.0/30 transit.
**Devices:** R1, R2.

```
! R1
interface Tunnel0
 ip address 172.16.0.1 255.255.255.252
 tunnel source GigabitEthernet0/1
 tunnel destination 198.51.100.2
 tunnel mode gre ip
```

R2 mirrors with `.2` and reversed source/dest.

**Verify with:**
- `show interfaces Tunnel0` → up/up, GRE/IP.
- `ping 172.16.0.2`.
- `show ip route` → tunnel routed.

---

### Drill 38 — IPsec VTI

**Goal:** Replace Drill 37's GRE tunnel with an IPsec VTI (no GRE, route-based IPsec).
**Devices:** R1, R2 (CSR1000v).

```
crypto ikev2 proposal IKE-PROP
 encryption aes-cbc-256
 integrity sha256
 group 14
!
crypto ikev2 policy IKE-POL
 proposal IKE-PROP
!
crypto ikev2 keyring IKE-KEY
 peer R2
  address 198.51.100.2
  pre-shared-key Cisco123!
!
crypto ikev2 profile IKE-PROF
 match identity remote address 198.51.100.2 255.255.255.255
 authentication local pre-share
 authentication remote pre-share
 keyring local IKE-KEY
!
crypto ipsec transform-set TS esp-aes 256 esp-sha256-hmac
 mode tunnel
!
crypto ipsec profile IPSEC-PROF
 set transform-set TS
 set ikev2-profile IKE-PROF
!
interface Tunnel0
 ip address 172.16.0.1 255.255.255.252
 tunnel source GigabitEthernet1
 tunnel destination 198.51.100.2
 tunnel mode ipsec ipv4
 tunnel protection ipsec profile IPSEC-PROF
```

**Verify with:**
- `show crypto ikev2 sa` → READY.
- `show crypto ipsec sa` → encrypts/decrypts increment.

---

### Drill 39 — GRE-over-IPsec transport mode

**Goal:** Run GRE inside IPsec transport mode (save 20 bytes per packet).
**Devices:** R1, R2.
**Maps to lab:** V2.

```
crypto ipsec transform-set TS esp-aes 256 esp-sha256-hmac
 mode transport
!
crypto ipsec profile IPSEC-PROF
 set transform-set TS
!
interface Tunnel0
 ip address 172.16.0.1 255.255.255.252
 tunnel source GigabitEthernet1
 tunnel destination 198.51.100.2
 tunnel mode gre ip
 tunnel protection ipsec profile IPSEC-PROF
```

**Verify with:**
- `show interface Tunnel0` → Encapsulation GRE/IP, Tunnel protection via IPsec (profile IPSEC-PROF).
- Wireshark: ESP between R1 and R2; decrypted payload is GRE.

---

### Drill 40 — LISP basic ITR/ETR + Map-Server registration

**Goal:** Single CSR registers its EID `10.10.10.0/24` to Map-Server 100.100.100.100.
**Devices:** CSR-FE1.
**Maps to lab:** A2.

```
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
```

**Verify with:**
- `show ip lisp database` → 10.10.10.0/24 locator Lo0.
- `show ip lisp map-cache` → entries populated when traffic flows.

---

## 10. Virtualization

### Drill 41 — Two VRFs with route leaking

**Goal:** Create CUST-A (RD 65000:1), CUST-B (RD 65000:2); leak SHARED prefix between them.
**Devices:** CSR-PE.
**Maps to lab:** V1.

```
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
  route-target import 65000:99
 exit-address-family
!
! Mark a shared prefix on one VRF with the shared RT
route-map SHARED-EXPORT permit 10
 match ip address prefix-list SHARED-PFX
 set extcommunity rt 65000:99 additive
!
ip prefix-list SHARED-PFX seq 5 permit 10.50.50.0/24
!
! Use VPNv4 BGP to propagate (PE-style, even on one box)
router bgp 65000
 address-family vpnv4
  no auto-summary
 address-family ipv4 vrf CUST-A
  redistribute connected route-map SHARED-EXPORT
 address-family ipv4 vrf CUST-B
  redistribute connected
```

**Verify with:**
- `show ip route vrf CUST-A` and `show ip route vrf CUST-B` → 10.50.50.0/24 in both VRFs.
- `show ip bgp vpnv4 all` → see RD-tagged prefixes.

---

### Drill 42 — VRF-aware static route

**Goal:** Static route in CUST-A pointing to next-hop in CUST-A.
**Devices:** CSR-PE.

```
ip route vrf CUST-A 10.99.99.0 255.255.255.0 10.1.1.254
```

**Verify with:**
- `show ip route vrf CUST-A static`.
- `ping vrf CUST-A 10.99.99.1`.

---

## 11. Network Assurance

### Drill 43 — Local SPAN session

**Goal:** Mirror Gi0/3 in/out to Gi0/4 on ACC1.
**Devices:** ACC1.
**Maps to lab:** NA2.

```
monitor session 1 source interface GigabitEthernet0/3 both
monitor session 1 destination interface GigabitEthernet0/4
```

**Verify with:**
- `show monitor session 1`.

---

### Drill 44 — RSPAN across two switches

**Goal:** Mirror ACC1 Gi0/3 across an RSPAN VLAN 999 to ACC2 Gi0/4.
**Devices:** ACC1, ACC2 (with trunk in between carrying VLAN 999).
**Maps to lab:** NA2.

```
! Both switches
vlan 999
 name RSPAN
 remote-span
!
! ACC1 (source side)
monitor session 1 source interface GigabitEthernet0/3 both
monitor session 1 destination remote vlan 999
!
! ACC2 (dest side)
monitor session 2 source remote vlan 999
monitor session 2 destination interface GigabitEthernet0/4
```

**Verify with:**
- `show monitor session all`.
- Trunk between switches allows VLAN 999 (`show interfaces trunk`).

---

### Drill 45 — ERSPAN to remote collector

**Goal:** Mirror CSR-1 Gi2 to remote collector 192.0.2.200 with ERSPAN ID 100.
**Devices:** CSR-1.
**Maps to lab:** NA2.

```
monitor session 1 type erspan-source
 source interface GigabitEthernet2 both
 no shutdown
 destination
  erspan-id 100
  ip address 192.0.2.200
  origin ip address 1.1.1.1
```

**Verify with:**
- `show monitor session 1` → State up.
- Wireshark on collector → GRE-encapsulated frames with ERSPAN ID 100.

---

### Drill 46 — Model-driven telemetry subscription

**Goal:** Push interface statistics every 1 second to a gRPC collector at 192.0.2.100:57500.
**Devices:** CSR-1.
**Maps to lab:** NA3.

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

**Verify with:**
- `show telemetry ietf subscription all` → status Valid.
- `show telemetry ietf subscription 101 receiver` → Connecting → Connected.

---

## 12. Security

### Drill 47 — AAA with RADIUS group + local fallback

**Goal:** Authenticate VTY against RADIUS at 192.0.2.50; fall back to local user if RADIUS unreachable.
**Devices:** R1.
**Maps to lab:** S1.

```
aaa new-model
!
radius server RAD1
 address ipv4 192.0.2.50 auth-port 1812 acct-port 1813
 key Cisco123!
!
aaa group server radius RAD
 server name RAD1
!
aaa authentication login VTY-AUTH group RAD local
aaa authorization exec VTY-EXEC group RAD local if-authenticated
aaa accounting exec default start-stop group RAD
!
username backupadmin privilege 15 secret BackupCisco123!
!
line vty 0 4
 login authentication VTY-AUTH
 authorization exec VTY-EXEC
 transport input ssh
```

**Verify with:**
- `test aaa group RAD <user> <pass> new-code` → returns PASS/FAIL with reason.
- `debug aaa authentication` while logging in.

---

### Drill 48 — DHCP Snooping with trusted uplink

**Goal:** Enable snooping on VLAN 10; only Gi0/1 is trusted (uplink to legit DHCP server).
**Devices:** ACC1.
**Maps to lab:** S2.

```
ip dhcp snooping
ip dhcp snooping vlan 10
no ip dhcp snooping information option   ! avoid Option-82 issues with non-snooping-aware servers
!
interface GigabitEthernet0/1
 ip dhcp snooping trust
```

**Verify with:**
- `show ip dhcp snooping` → enabled, VLANs, trust state per port.
- `show ip dhcp snooping binding` after a client leases.

---

### Drill 49 — DAI + IPSG

**Goal:** Enable DAI for VLAN 10, IPSG on access ports.
**Devices:** ACC1.

```
ip arp inspection vlan 10
ip arp inspection validate src-mac dst-mac ip
!
interface GigabitEthernet0/1
 ip arp inspection trust
!
interface range GigabitEthernet0/3 - 24
 ip verify source           ! IPSG: drops if src IP not in snooping binding
```

**Verify with:**
- `show ip arp inspection` → enabled, drops counter.
- `show ip verify source`.

---

### Drill 50 — Port-security with sticky MAC and restrict

**Goal:** Limit to 2 MACs per access port, sticky learning, restrict mode (log without err-disable).
**Devices:** ACC1.

```
interface range GigabitEthernet0/3 - 24
 switchport mode access
 switchport port-security
 switchport port-security maximum 2
 switchport port-security violation restrict
 switchport port-security mac-address sticky
 switchport port-security aging time 60
 switchport port-security aging type inactivity
```

**Verify with:**
- `show port-security interface Gi0/3`.
- Plug a third MAC → violations counter increments, port stays up.

---

### Drill 51 — CoPP for ICMP and routing protocols

**Goal:** Police ICMP to 8 kbps; permit OSPF/BGP without limit; drop everything else to RP.
**Devices:** R1.

```
ip access-list extended COPP-ICMP
 permit icmp any any
ip access-list extended COPP-ROUTING
 permit ospf any any
 permit tcp any any eq bgp
 permit tcp any eq bgp any
!
class-map match-any COPP-ICMP-CLASS
 match access-group name COPP-ICMP
class-map match-any COPP-ROUTING-CLASS
 match access-group name COPP-ROUTING
!
policy-map COPP-IN
 class COPP-ROUTING-CLASS
  ! No police = unlimited
 class COPP-ICMP-CLASS
  police 8000 conform-action transmit exceed-action drop
 class class-default
  drop
!
control-plane
 service-policy input COPP-IN
```

**Verify with:**
- `show policy-map control-plane input` → counters per class.
- Flood ping from outside → policed.

---

### Drill 52 — uRPF strict on an interface

**Goal:** Enable strict-mode uRPF on Gi0/1 (untrusted edge).
**Devices:** R1.

```
interface GigabitEthernet0/1
 ip verify unicast source reachable-via rx
```

**Verify with:**
- `show ip interface Gi0/1 | include verify` → "IP verify source reachable-via RX".
- Source-spoofed traffic gets dropped (`show ip traffic | include drop`).

---

### Drill 53 — Zone-Based Firewall with one zone-pair

**Goal:** INSIDE → OUTSIDE permits HTTP/HTTPS/DNS/ICMP; default deny.
**Devices:** CSR-EDGE.
**Maps to lab:** S3.

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

**Verify with:**
- `show policy-map type inspect zone-pair sessions`.
- Test from INSIDE: HTTPS works, SSH outbound blocked.

---

### Drill 54 — 802.1X with MAB fallback

**Goal:** Switchport Gi0/5 runs 802.1X; falls back to MAB if no supplicant; uses RADIUS server.
**Devices:** ACC1.

```
aaa new-model
radius server ISE
 address ipv4 192.0.2.50 auth-port 1812 acct-port 1813
 key Cisco123!
aaa group server radius RAD
 server name ISE
aaa authentication dot1x default group RAD
aaa authorization network default group RAD
!
dot1x system-auth-control
!
interface GigabitEthernet0/5
 switchport mode access
 switchport access vlan 10
 authentication port-control auto
 authentication host-mode multi-auth
 authentication order dot1x mab
 authentication priority dot1x mab
 mab
 dot1x pae authenticator
 spanning-tree portfast
```

**Verify with:**
- `show authentication sessions interface Gi0/5 details`.

---

## 13. Automation

### Drill 55 — Enable NETCONF + RESTCONF on CSR1000v

**Goal:** Make CSR-1 accept NETCONF/SSH on 830 and RESTCONF/HTTPS on 443.
**Devices:** CSR-1.
**Maps to lab:** AU1.

```
ip http secure-server
ip http authentication local
restconf
netconf-yang
netconf-yang feature candidate-datastore
username api privilege 15 secret Cisco123!
```

**Verify with:**
- `show netconf-yang sessions` after ncclient connects.
- `show platform software yang-management process` → all `Running`.
- From a host:
  ```bash
  curl -k -u api:'Cisco123!' \
    -H 'Accept: application/yang-data+json' \
    https://<csr-ip>/restconf/data/Cisco-IOS-XE-native:native/hostname
  ```

---

### Drill 56 — EEM applet on interface down

**Goal:** When Gi0/1 goes down, log a warning + capture diagnostic commands.
**Devices:** R1.
**Maps to lab:** AU3.

```
event manager applet IF-DOWN-G01
 event syslog pattern "Interface GigabitEthernet0/1, changed state to down"
 action 1.0 syslog priority warnings msg "EEM: G0/1 is DOWN — running diagnostics"
 action 1.1 cli command "enable"
 action 1.2 cli command "show ip interface brief"
 action 1.3 cli command "show log | tail 20"
 action 1.4 syslog priority informational msg "$_cli_result"
```

**Verify with:**
- `event manager run IF-DOWN-G01` (manual trigger) or `shutdown` Gi0/1.
- `show event manager history events`.

---

### Drill 57 — EEM cron applet

**Goal:** Every 30 minutes, log uptime and CPU.
**Devices:** R1.

```
event manager applet HEALTH-CHECK
 event timer cron name HEALTH cron-entry "*/30 * * * *"
 action 1.0 cli command "enable"
 action 1.1 cli command "show clock"
 action 1.2 cli command "show processes cpu sorted | exclude 0.00"
 action 1.3 syslog msg "EEM HEALTH-CHECK: $_cli_result"
```

**Verify with:**
- `show event manager history events`.
- Logs every 30 min.

---

### Drill 58 — Auto-save running-config via EEM

**Goal:** Save startup-config 60 seconds after any config change.
**Devices:** R1.

```
event manager applet AUTO-SAVE
 event syslog pattern "%SYS-5-CONFIG_I"
 action 1.0 wait 60
 action 1.1 cli command "enable"
 action 1.2 cli command "write memory"
 action 1.3 syslog msg "EEM AUTO-SAVE: startup-config refreshed"
```

**Verify with:**
- Make a config change → 60 s later, `show event manager history events` shows execution.

---

### Drill 59 — Guest Shell Python script

**Goal:** From Guest Shell, run a Python script that pushes a loopback config via `dohost`.
**Devices:** CSR-1 (IOx must be enabled — `iox` in global).

```
! Enable IOx + Guest Shell first
configure terminal
iox
end
!
guestshell enable
!
guestshell run python3
>>> import cli
>>> cli.configurep(["interface Loopback123", "ip address 123.123.123.123 255.255.255.255"])
>>> print(cli.execute("show ip interface brief | include Loopback123"))
>>> exit()
```

**Verify with:**
- `show ip interface brief | include Loopback123` from regular CLI.
- `show iox-service` and `show app-hosting list`.

---

### Drill 60 — Periodic telemetry subscription via CLI

**Goal:** Push CPU process info every 5 seconds to a collector.
**Devices:** CSR-1.

```
telemetry ietf subscription 200
 encoding encode-kvgpb
 filter xpath /process-cpu-ios-xe-oper:cpu-usage/cpu-utilization
 stream yang-push
 update-policy periodic 5000
 receiver ip address 192.0.2.100 57500 protocol grpc-tcp
```

**Verify with:**
- `show telemetry ietf subscription 200`.
- `show telemetry ietf subscription 200 receiver`.
- On collector (e.g., Pipeline / Telegraf / custom gRPC consumer): CPU metrics arriving every 5 s.

---

## Drill map by Pass 2 block

| Pass 2 block | Topic | Drills |
|---|---|---|
| Block 1 | STP + Guards | 7, 8, 9, 10 |
| Block 2 | VLANs + EtherChannel | 4, 5, 6, 11, 12, 13 |
| Block 3 | OSPF | 14, 15, 16, 17, 18, 19, 20 |
| Block 4 | BGP | 21, 22, 23, 24, 25 |
| Block 5 | Multicast | 30, 31 |
| Block 6 | FHRP + NAT | 26, 27, 28, 29 |
| Block 7 | IP Services | 32, 33, 34, 35, 36 |
| Block 8 | Overlays + VRF | 37, 38, 39, 40, 41, 42 |
| Block 9 | Wireless | — (DevNet sandbox, no CLI drills) |
| Block 10 | Network Assurance | 43, 44, 45, 46 |
| Block 11 | Security | 47, 48, 49, 50, 51, 52, 53, 54 |
| Block 12 | Automation | 1, 2, 3 (bootstrap refresh), 55, 56, 57, 58, 59, 60 |

**Always start with Drills 1–3 in Block 1** — you'll re-paste the baseline config constantly across every block. Get it in muscle memory first.

Run each drill at least twice — once with the solution open, once blind. If the blind pass takes more than double the expected time, mark it for re-drill at the next session. Don't move to the next block with more than 2 drills still failing.
