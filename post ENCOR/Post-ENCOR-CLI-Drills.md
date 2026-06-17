# Post-ENCOR CLI Drills (multi-vendor)

**Goal:** muscle memory. Each drill is timed. Target time = "production-engineer fluent" — if you're slower, repeat it next day.

**Format:** scenario, then the exact commands. Cisco IOS-XE default. **MikroTik** and **Juniper** equivalents noted where the comparison is instructive — these are the languages you'll actually see in Russia/EU.

**Workflow:**
- One drill per day during weeks 1-13 (≈65 drills, plus 3 weekend reviews)
- Drill 1× from memory → check answer → repeat next day if you missed any line.
- Tag completed drills in the tracker: `D-NN ✓`.

---

## D-01 — Create a VRF with route-leaking (Week 1)

**Scenario:** create VRF `CUST_A` (RD `65001:1`, RT both 100), assign `Gi0/1` to it with `10.0.0.1/24`, leak the shared VRF `SHARED` (RT 999) into it.

**Cisco IOS-XE:**
```
vrf definition CUST_A
 rd 65001:1
 address-family ipv4
  route-target export 65001:100
  route-target import 65001:100
  route-target import 65001:999
 exit-address-family
!
interface Gi0/1
 vrf forwarding CUST_A
 ip address 10.0.0.1 255.255.255.0
 no shut
```

**MikroTik:**
```
/ip vrf
add name=CUST_A rd=65001:1 \
    import-route-targets=65001:100,65001:999 \
    export-route-targets=65001:100
/ip address
add address=10.0.0.1/24 interface=ether1 vrf=CUST_A
```

**Juniper:**
```
set routing-instances CUST_A instance-type vrf
set routing-instances CUST_A route-distinguisher 65001:1
set routing-instances CUST_A vrf-target target:65001:100
set routing-instances CUST_A vrf-import IMPORT_SHARED
set routing-instances CUST_A interface ge-0/0/1.0
set interfaces ge-0/0/1.0 family inet address 10.0.0.1/24
```

**Target time:** 90 seconds per vendor.

---

## D-02 — BFD on OSPF (Week 1)

```
interface Gi0/2
 bfd interval 300 min_rx 300 multiplier 3
!
router ospf 1
 bfd all-interfaces
```

**Verify:** `show bfd neighbors detail` (look for `Async` mode, `Up` state, `Echo` flag).

**MikroTik:** `/routing/bfd/configuration add interfaces=ether2 transmit-interval=300ms`.

**Target time:** 30 seconds.

---

## D-03 — Prefix-list + route-map for BGP outbound (Week 2)

**Scenario:** announce only `10.0.0.0/8 ge 24 le 24` (i.e. `/24`s) to neighbor `192.0.2.1`, set local-pref 200.

```
ip prefix-list ANNOUNCE_24 seq 10 permit 10.0.0.0/8 ge 24 le 24
!
route-map TO_PEER permit 10
 match ip address prefix-list ANNOUNCE_24
 set local-preference 200
!
router bgp 65000
 neighbor 192.0.2.1 route-map TO_PEER out
```

**Target time:** 60 seconds.

---

## D-04 — Policy-based routing (Week 2)

**Scenario:** TCP/80 from `10.10.10.0/24` out `Gi0/2`, all else default routing. Backup nexthop tracked.

```
ip sla 1
 icmp-echo 10.20.20.1 source-interface Gi0/2
 frequency 5
ip sla schedule 1 life forever start-time now
track 1 ip sla 1 reachability
!
ip access-list extended WEB
 permit tcp 10.10.10.0 0.0.0.255 any eq 80
!
route-map PBR_WEB permit 10
 match ip address WEB
 set ip next-hop verify-availability 10.20.20.1 1 track 1
!
interface Gi0/3
 ip policy route-map PBR_WEB
```

**Target time:** 2 minutes.

---

## D-05 — Mutual OSPF↔EIGRP redistribution with tag loop-prevention (Week 3)

```
router ospf 1
 redistribute eigrp 100 subnets route-map FROM_EIGRP
!
router eigrp 100
 redistribute ospf 1 metric 1000000 10 255 1 1500 route-map FROM_OSPF
!
route-map FROM_EIGRP deny 10
 match tag 200
route-map FROM_EIGRP permit 20
 set tag 100
!
route-map FROM_OSPF deny 10
 match tag 100
route-map FROM_OSPF permit 20
 set tag 200
```

**Verify:** `show ip route ospf | i E2` shows routes with tag 100; loop is gone.

**Target time:** 90 seconds.

---

## D-06 — Named-mode EIGRP with SHA-256 (Week 4)

```
key chain EIGRP_KEYS
 key 1
  key-string SECRET_KEY_2026
  cryptographic-algorithm hmac-sha-256
!
router eigrp LAB4
 address-family ipv4 unicast autonomous-system 100
  af-interface Gi0/1
   authentication mode hmac-sha-256
   authentication key-chain EIGRP_KEYS
  exit-af-interface
  network 0.0.0.0
```

**Verify:** `show eigrp protocols` → `Hello timer is X, Hold time is X, HMAC-SHA-256`.

**Target time:** 60 seconds.

---

## D-07 — EIGRP stub on a spoke (Week 4)

```
router eigrp LAB4
 address-family ipv4 unicast autonomous-system 100
  eigrp stub connected summary
```

**Verify on hub:** `show ip eigrp neighbors detail` → spoke listed as `Stub Peer Advertising (CONNECTED SUMMARY) Routes`.

**Target time:** 20 seconds.

---

## D-08 — OSPFv3 dual-AF + key-chain auth (Week 5)

```
key chain OSPF_KEYS
 key 1
  key-string OSPF_SECRET
  cryptographic-algorithm hmac-sha-256
!
router ospfv3 1
 address-family ipv4 unicast
  router-id 1.1.1.1
 exit-address-family
 address-family ipv6 unicast
  router-id 1.1.1.1
 exit-address-family
!
interface Gi0/1
 ospfv3 1 ipv4 area 0
 ospfv3 1 ipv6 area 0
 ospfv3 authentication key-chain OSPF_KEYS
```

**Verify:** `show ospfv3 ipv4 neighbor` and `show ospfv3 ipv6 neighbor` both `FULL`.

**Target time:** 90 seconds.

---

## D-09 — OSPF NSSA with type-7 → type-5 translation (Week 6)

```
router ospf 1
 area 2 nssa
 redistribute static subnets
!
ip route 203.0.113.0 255.255.255.0 Null0
```

**Verify on ABR:** `show ip ospf database nssa-external` (Type-7 in area 2), `show ip ospf database external` (Type-5 in area 0).

**Target time:** 30 seconds.

---

## D-10 — Virtual link across area-2 (Week 6)

```
! On ABR_A (between area 0 and area 2)
router ospf 1
 area 2 virtual-link 3.3.3.3
!
! On ABR_B (between area 2 and area 3)
router ospf 1
 area 2 virtual-link 1.1.1.1
```

**Verify:** `show ip ospf virtual-links` → `Up`.

**Then remove and re-design without the virtual-link.**

**Target time:** 60 seconds.

---

## D-11 — IS-IS L1/L2 with wide metrics + SHA auth (Week 7)

```
router isis BACKBONE
 net 49.0001.1921.6800.1001.00
 metric-style wide
 is-type level-1-2
!
key chain ISIS_KEYS
 key 1
  key-string ISIS_SECRET
  cryptographic-algorithm hmac-sha-256
!
interface Gi0/1
 ip router isis BACKBONE
 ipv6 router isis BACKBONE
 isis authentication mode md5
 isis authentication key-chain ISIS_KEYS
```

**Juniper:**
```
set protocols isis level 2 wide-metrics-only
set protocols isis interface ge-0/0/1.0 level 1 disable
set protocols isis interface ge-0/0/1.0 level 2 metric 10
set protocols isis interface ge-0/0/1.0 hello-authentication-type hmac-sha-256
set protocols isis interface ge-0/0/1.0 hello-authentication-key "ISIS_SECRET"
```

**Verify:** `show isis neighbors` → `L1L2`, state `UP`.

**Target time:** 2 minutes.

---

## D-12 — BGP with IPv4 + IPv6 AF, RR client (Week 8)

```
router bgp 65020
 bgp router-id 2.2.2.2
 no bgp default ipv4-unicast
 neighbor 1.1.1.1 remote-as 65020
 neighbor 1.1.1.1 update-source Loopback0
 neighbor 2001:db8::1 remote-as 65020
 neighbor 2001:db8::1 update-source Loopback0
 !
 address-family ipv4 unicast
  neighbor 1.1.1.1 activate
  neighbor 1.1.1.1 route-reflector-client
  neighbor 1.1.1.1 next-hop-self
 exit-address-family
 !
 address-family ipv6 unicast
  neighbor 2001:db8::1 activate
  neighbor 2001:db8::1 route-reflector-client
  neighbor 2001:db8::1 next-hop-self
 exit-address-family
```

**Target time:** 2 minutes.

---

## D-13 — AS-path regex filter (Week 8)

```
ip as-path access-list 10 deny _64500_
ip as-path access-list 10 permit .*
!
router bgp 65020
 address-family ipv4 unicast
  neighbor 192.0.2.1 filter-list 10 in
```

**Verify:** `show bgp ipv4 unicast regexp _64500_` returns no routes from that peer.

**Target time:** 30 seconds.

---

## D-14 — BGP community-based policy (Week 8)

```
ip community-list standard NO_EXPORT permit 65020:200
!
route-map FROM_PEER deny 10
 match community NO_EXPORT
route-map FROM_PEER permit 20
 set community 65020:100 additive
!
router bgp 65020
 address-family ipv4 unicast
  neighbor 192.0.2.1 send-community both
  neighbor 192.0.2.1 route-map FROM_PEER in
```

**Target time:** 60 seconds.

---

## D-15 — BGP fast-converge (Week 8)

```
router bgp 65020
 bgp bestpath as-path multipath-relax
 bgp graceful-restart
 timers bgp 10 30
 !
 address-family ipv4 unicast
  maximum-paths 4
  maximum-paths ibgp 4
  bgp additional-paths receive
  bgp additional-paths send
```

**Target time:** 30 seconds.

---

## D-16 — BGP debug & state diagnosis (Week 9)

**One-liner used to start a controlled debug:**
```
debug bgp ipv4 unicast updates 192.0.2.1 in
debug ip tcp transactions
```

**Then narrow:**
```
show ip bgp neighbors 192.0.2.1 | i BGP state|holdtime|Last
show tcp brief
```

**Diagnostic flow when stuck in Active:**
```
ping 192.0.2.1 source Loopback0
telnet 192.0.2.1 179 /source-interface Loopback0
show ip access-lists | i 179
```

**Target time:** 60 seconds to type from memory.

---

## D-17 — IPv6 BGP with link-local next-hop (Week 9)

```
router bgp 65020
 neighbor fe80::1%Gi0/1 remote-as 65030
 !
 address-family ipv6 unicast
  neighbor fe80::1%Gi0/1 activate
  neighbor fe80::1%Gi0/1 route-map IPV6_IN in
```

**Verify:** `show bgp ipv6 unicast neighbors fe80::1%Gi0/1` → Established.

**Target time:** 30 seconds.

---

## D-18 — MPLS underlay: OSPF + LDP (Week 10)

```
mpls label range 100 999
mpls ldp router-id Loopback0 force
!
router ospf 1
 router-id 1.1.1.1
 network 1.1.1.1 0.0.0.0 area 0
 network 10.0.0.0 0.0.0.255 area 0
!
interface Gi0/1
 mpls ip
 mpls label protocol ldp
```

**Verify:**
```
show mpls ldp neighbor
show mpls ldp bindings
show mpls forwarding-table
```

**Target time:** 90 seconds.

---

## D-19 — MPLS L3VPN PE config (Week 10)

```
vrf definition VPN_RED
 rd 65000:10
 address-family ipv4
  route-target both 65000:10
 exit-address-family
!
router bgp 65000
 neighbor 2.2.2.2 remote-as 65000
 neighbor 2.2.2.2 update-source Loopback0
 !
 address-family vpnv4 unicast
  neighbor 2.2.2.2 activate
  neighbor 2.2.2.2 send-community extended
 exit-address-family
 !
 address-family ipv4 vrf VPN_RED
  neighbor 192.168.10.2 remote-as 65010
  neighbor 192.168.10.2 activate
 exit-address-family
!
interface Gi0/2
 vrf forwarding VPN_RED
 ip address 192.168.10.1 255.255.255.252
```

**Verify:** `show bgp vpnv4 unicast all summary` and `show ip route vrf VPN_RED`.

**Target time:** 3 minutes (the most important drill of the whole plan).

---

## D-20 — Verify L3VPN forwarding (Week 10)

```
show mpls forwarding-table vrf VPN_RED
show bgp vpnv4 unicast vrf VPN_RED 10.0.0.0/24
traceroute vrf VPN_RED 10.0.0.1
```

The traceroute must show MPLS labels in the path.

**Target time:** 30 seconds.

---

## D-21 — DMVPN Phase 3 hub (Week 11)

```
interface Tunnel0
 ip address 172.16.0.1 255.255.255.0
 no ip redirects
 ip mtu 1400
 ip nhrp authentication NHRP_KEY
 ip nhrp map multicast dynamic
 ip nhrp network-id 100
 ip nhrp redirect
 tunnel source Gi0/0
 tunnel mode gre multipoint
 tunnel key 100
 tunnel protection ipsec profile DMVPN_IPSEC
!
router eigrp 100
 network 172.16.0.0
 !
interface Tunnel0
 no ip split-horizon eigrp 100
 no ip next-hop-self eigrp 100
```

**Target time:** 2 minutes.

---

## D-22 — DMVPN Phase 3 spoke (Week 11)

```
interface Tunnel0
 ip address 172.16.0.2 255.255.255.0
 no ip redirects
 ip mtu 1400
 ip nhrp authentication NHRP_KEY
 ip nhrp map 172.16.0.1 198.51.100.1
 ip nhrp map multicast 198.51.100.1
 ip nhrp nhs 172.16.0.1
 ip nhrp network-id 100
 ip nhrp shortcut
 tunnel source Gi0/0
 tunnel mode gre multipoint
 tunnel key 100
 tunnel protection ipsec profile DMVPN_IPSEC
```

**Target time:** 90 seconds.

---

## D-23 — IKEv2 profile for DMVPN (Week 11)

```
crypto ikev2 proposal IKEV2_PROP
 encryption aes-cbc-256
 integrity sha512
 group 19
!
crypto ikev2 policy IKEV2_POL
 proposal IKEV2_PROP
!
crypto ikev2 keyring KEYRING
 peer DMVPN
  address 0.0.0.0 0.0.0.0
  pre-shared-key local DMVPN_PSK
  pre-shared-key remote DMVPN_PSK
!
crypto ikev2 profile IKEV2_PROF
 match identity remote address 0.0.0.0
 authentication remote pre-share
 authentication local pre-share
 keyring local KEYRING
!
crypto ipsec transform-set TS esp-aes 256 esp-sha512-hmac
 mode transport
!
crypto ipsec profile DMVPN_IPSEC
 set transform-set TS
 set pfs group19
 set ikev2-profile IKEV2_PROF
```

**Verify:** `show crypto ikev2 sa detailed` and `show crypto ipsec sa`.

**Target time:** 3 minutes.

---

## D-24 — MikroTik IKEv2 site-to-site (Week 11)

**Useful comparison drill — same goal, different syntax:**

```
/ip ipsec profile
add name=IKEV2 dh-group=ecp256 enc-algorithm=aes-256 hash-algorithm=sha256
/ip ipsec proposal
add name=IKEV2 auth-algorithms=sha256 enc-algorithms=aes-256-gcm pfs-group=ecp256
/ip ipsec peer
add name=PEER1 address=203.0.113.1/32 exchange-mode=ike2 profile=IKEV2
/ip ipsec identity
add peer=PEER1 secret=PSK_HERE
/ip ipsec policy
add src-address=10.10.10.0/24 dst-address=10.20.20.0/24 \
    peer=PEER1 proposal=IKEV2 tunnel=yes action=encrypt
```

**Target time:** 3 minutes.

---

## D-25 — TACACS+ AAA full config (Week 12)

```
aaa new-model
!
tacacs server PRIMARY
 address ipv4 10.99.99.10
 key TACACS_SECRET
 single-connection
 timeout 5
!
aaa group server tacacs+ TAC_GRP
 server name PRIMARY
!
aaa authentication login default group TAC_GRP local
aaa authentication enable default group TAC_GRP enable
aaa authorization config-commands
aaa authorization exec default group TAC_GRP local
aaa authorization commands 1 default group TAC_GRP local
aaa authorization commands 15 default group TAC_GRP local
aaa accounting exec default start-stop group TAC_GRP
aaa accounting commands 15 default start-stop group TAC_GRP
!
ip tacacs source-interface Loopback0
```

**Target time:** 2 minutes.

---

## D-26 — Hardening one-liner block (Week 12)

```
no ip http server
no ip http secure-server
no service pad
no ip source-route
no ip bootp server
no ip domain-lookup
no cdp run
no lldp run
!
service password-encryption
service tcp-keepalives-in
service tcp-keepalives-out
service timestamps log datetime msec localtime show-timezone
service timestamps debug datetime msec localtime show-timezone
!
ip ssh version 2
ip ssh time-out 60
ip ssh authentication-retries 2
crypto key generate rsa modulus 4096
!
login block-for 60 attempts 3 within 30
login on-failure log
login on-success log
!
line vty 0 15
 transport input ssh
 exec-timeout 10 0
!
banner motd ^
*** Authorized access only. All activity is logged. ***
^
```

**Target time:** type from memory — aim for 2 minutes; this saves your job on a 3am hardening audit.

---

## D-27 — CoPP policy (Week 12)

```
ip access-list extended COPP_MGMT
 permit tcp any any eq 22
 permit udp any any eq snmp
 permit udp any any eq ntp
ip access-list extended COPP_ROUTING
 permit ospf any any
 permit tcp any any eq bgp
 permit tcp any eq bgp any
 permit eigrp any any
 permit udp any any eq 646
 permit tcp any any eq 646
ip access-list extended COPP_MONITORING
 permit icmp any any echo
 permit icmp any any echo-reply
ip access-list extended COPP_UNDESIRABLE
 permit ip any any option any-options
 permit icmp any any redirect
!
class-map match-any COPP_MGMT
 match access-group name COPP_MGMT
class-map match-any COPP_ROUTING
 match access-group name COPP_ROUTING
class-map match-any COPP_MONITORING
 match access-group name COPP_MONITORING
class-map match-any COPP_UNDESIRABLE
 match access-group name COPP_UNDESIRABLE
!
policy-map COPP_POLICY
 class COPP_ROUTING
  police 5000000 conform-action transmit exceed-action drop
 class COPP_MGMT
  police 2000000 conform-action transmit exceed-action drop
 class COPP_MONITORING
  police 500000 conform-action transmit exceed-action drop
 class COPP_UNDESIRABLE
  police 32000 conform-action drop exceed-action drop
 class class-default
  police 1000000 conform-action transmit exceed-action drop
!
control-plane
 service-policy input COPP_POLICY
```

**Verify:** `show policy-map control-plane input`.

**Target time:** 3 minutes (the longest drill — expected).

---

## D-28 — IPv6 First-Hop Security on a vIOS L2 access port (Week 12)

```
ipv6 nd raguard policy HOST_RA
 device-role host
!
ipv6 dhcp guard policy HOST_DHCP
 device-role client
!
ipv6 snooping policy SNOOP_POLICY
 security-level guard
 tracking enable
!
ipv6 source-guard policy SG_POLICY
!
interface range Gi0/0 - 23
 ipv6 nd raguard attach-policy HOST_RA
 ipv6 dhcp guard attach-policy HOST_DHCP
 ipv6 snooping attach-policy SNOOP_POLICY
 ipv6 source-guard attach-policy SG_POLICY
```

**Verify:** `show ipv6 snooping policies`, `show ipv6 neighbors binding`.

**Target time:** 2 minutes.

---

## D-29 — SNMPv3 with auth+priv (Week 13)

```
snmp-server group ADMIN_GRP v3 priv read ADMIN_VIEW
snmp-server view ADMIN_VIEW iso included
snmp-server user monitor_user ADMIN_GRP v3 auth sha SNMP_AUTH_PASS priv aes 128 SNMP_PRIV_PASS
snmp-server host 10.99.99.20 version 3 priv monitor_user
snmp-server enable traps
!
snmp-server location MOSCOW-DC1-RACK7
snmp-server contact noc@example.com
```

**Verify on LibreNMS / from a test box:**
```
snmpwalk -v3 -l authPriv -u monitor_user -a SHA -A SNMP_AUTH_PASS -x AES -X SNMP_PRIV_PASS 10.0.0.1 1.3.6.1.2.1.1
```

**Target time:** 90 seconds.

---

## D-30 — Flexible NetFlow record + monitor + export (Week 13)

```
flow record FNF_RECORD
 match ipv4 source address
 match ipv4 destination address
 match ipv4 protocol
 match transport source-port
 match transport destination-port
 match ipv4 tos
 collect counter bytes long
 collect counter packets long
 collect interface input
 collect interface output
 collect routing source as
 collect routing destination as
 collect routing next-hop address ipv4
 collect timestamp sys-uptime first
 collect timestamp sys-uptime last
!
flow exporter FNF_EXPORTER
 destination 10.99.99.30
 transport udp 2055
 source Loopback0
 template data timeout 60
 export-protocol netflow-v9
!
flow monitor FNF_MONITOR
 record FNF_RECORD
 exporter FNF_EXPORTER
 cache timeout active 60
 cache timeout inactive 15
!
interface Gi0/1
 ip flow monitor FNF_MONITOR input
 ip flow monitor FNF_MONITOR output
```

**Verify:** `show flow monitor FNF_MONITOR cache format table`.

**Target time:** 3 minutes.

---

## D-31 — IP SLA + Tracker + Conditional static route (Week 13)

```
ip sla 10
 icmp-echo 198.51.100.1 source-interface Gi0/0
 frequency 5
 threshold 1000
ip sla schedule 10 life forever start-time now
!
track 10 ip sla 10 reachability
 delay down 10 up 5
!
ip route 0.0.0.0 0.0.0.0 198.51.100.1 track 10
ip route 0.0.0.0 0.0.0.0 203.0.113.1 100
```

**Verify:** `show ip sla statistics 10`, `show track 10`. Pull the primary cable — default route should swap to backup.

**Target time:** 90 seconds.

---

## D-32 — DHCP relay across VRFs (Week 13)

```
interface Vlan10
 vrf forwarding CUST_A
 ip address 10.10.10.1 255.255.255.0
 ip helper-address vrf MGMT 10.99.99.50 global
```

**Target time:** 30 seconds.

---

## D-33 — gNMI subscribe (Week 13, automation pivot starts here)

**On Cisco IOS-XE:**
```
gnmi-yang
gnmi-yang server
gnmi-yang port 9339
gnmi-yang secure-server
gnmi-yang secure-port 9339
```

**From your Linux box:**
```bash
gnmic -a 10.0.0.1:9339 \
      --skip-verify \
      --username admin --password admin \
      subscribe --path "/interfaces/interface/state/counters" \
                --stream-mode sample --sample-interval 10s
```

**Target time:** 60 seconds.

---

## D-34 — NETCONF over SSH from `ncclient` (Week 15)

```python
from ncclient import manager
import xml.etree.ElementTree as ET

with manager.connect(
    host="10.0.0.1", port=830, username="admin", password="admin",
    hostkey_verify=False, device_params={"name": "iosxe"}
) as m:
    filter_xml = """
    <filter>
      <interfaces xmlns="http://openconfig.net/yang/interfaces">
        <interface>
          <state/>
        </interface>
      </interfaces>
    </filter>"""
    reply = m.get(filter_xml)
    print(reply.xml)
```

**Target time:** 90 seconds (typed from memory).

---

## D-35 — RESTCONF GET with requests (Week 15)

```python
import requests, urllib3
urllib3.disable_warnings()

URL = "https://10.0.0.1/restconf/data/ietf-interfaces:interfaces"
HDR = {"Accept": "application/yang-data+json"}
r = requests.get(URL, headers=HDR, auth=("admin","admin"), verify=False)
print(r.json())
```

**Target time:** 60 seconds.

---

## D-36 — Nornir + NAPALM get-config (Week 15)

```python
from nornir import InitNornir
from nornir_napalm.plugins.tasks import napalm_get
from nornir_utils.plugins.functions import print_result

nr = InitNornir(config_file="config.yaml")
result = nr.run(task=napalm_get, getters=["config"])
print_result(result)
```

`config.yaml`:
```yaml
inventory:
  plugin: SimpleInventory
  options:
    host_file: "inventory/hosts.yaml"
    group_file: "inventory/groups.yaml"
runner:
  plugin: threaded
  options:
    num_workers: 20
```

**Target time:** 2 minutes.

---

## D-37 — Ansible playbook: backup configs from Cisco + MikroTik (Week 15)

```yaml
---
- name: Backup Cisco IOS-XE
  hosts: cisco_iosxe
  gather_facts: false
  tasks:
    - cisco.ios.ios_command:
        commands: show running-config
      register: cfg
    - copy:
        content: "{{ cfg.stdout[0] }}"
        dest: "configs/{{ inventory_hostname }}-{{ ansible_date_time.iso8601 }}.cfg"

- name: Backup MikroTik
  hosts: mikrotik
  gather_facts: false
  connection: community.routeros.api
  tasks:
    - community.routeros.api:
        path: "/export"
      register: cfg
    - copy:
        content: "{{ cfg | to_nice_yaml }}"
        dest: "configs/{{ inventory_hostname }}-{{ ansible_date_time.iso8601 }}.rsc"
```

Run with: `ansible-playbook -i inventory.yaml backup.yaml`.

**Target time:** 3 minutes.

---

## D-38 — Git workflow drill (Week 15)

```bash
git checkout -b feature/add-vrf-cust-c
# ... edit playbooks ...
git add playbooks/vrf.yaml
git commit -m "Add VRF CUST_C with RD 65001:3 and RT 65001:300"
git push -u origin feature/add-vrf-cust-c
# After merge:
git checkout main
git pull --rebase
git branch -d feature/add-vrf-cust-c
```

**Target time:** 30 seconds (this should be reflexive by Week 16).

---

## D-39 — pyang explore a YANG module (Week 15)

```bash
pyang -f tree -p ~/yang-modules/standard ~/yang-modules/standard/ietf/ietf-routing.yang | head -60
pyang -f sample-xml-skeleton ~/yang-modules/standard/ietf/ietf-ospf.yang > ospf-skel.xml
```

**Target time:** 30 seconds.

---

## D-40 — `gnmic` capabilities check (Week 15)

```bash
gnmic -a 10.0.0.1:9339 --skip-verify -u admin -p admin capabilities
gnmic -a 10.0.0.1:9339 --skip-verify -u admin -p admin get \
      --path "/interfaces/interface[name=GigabitEthernet1]/state/counters"
```

**Target time:** 30 seconds.

---

## D-41 — Promtheus scrape config for gnmic exporter (Week 15)

```yaml
scrape_configs:
  - job_name: 'gnmic'
    static_configs:
      - targets: ['localhost:9804']
```

`gnmic` config (`gnmic.yaml`):
```yaml
targets:
  10.0.0.1:9339:
    username: admin
    password: admin
    insecure: true
subscriptions:
  if_counters:
    paths:
      - "/interfaces/interface/state/counters"
    stream-mode: sample
    sample-interval: 10s
outputs:
  prom_out:
    type: prometheus
    listen: ":9804"
```

Start with: `gnmic --config gnmic.yaml subscribe`.

**Target time:** 90 seconds.

---

## D-42 — Cisco-equivalent commands on Juniper for daily ops (Week 7 + 16)

| Cisco IOS-XE | Juniper Junos |
|---|---|
| `show interface brief` | `show interfaces terse` |
| `show ip route` | `show route` |
| `show ip route bgp` | `show route protocol bgp` |
| `show ip bgp summary` | `show bgp summary` |
| `show ip bgp neighbors X` | `show bgp neighbor X` |
| `show ip ospf neighbor` | `show ospf neighbor` |
| `show running-config` | `show configuration` |
| `configure terminal` | `configure exclusive` |
| `copy run start` | `commit` |
| `do show ...` (in config mode) | `run show ...` |
| `show logging` | `show log messages` |
| `show interface counters` | `show interfaces extensive` |
| `clear ip bgp X soft in` | `clear bgp neighbor X soft` |
| `debug ip bgp updates in X` | `monitor traffic interface X detail` + `traceoptions` in BGP |

**Drill action:** flashcard format. Cisco → Junos and Junos → Cisco. 20 minutes total over a few days.

---

## D-43 — Cisco vs MikroTik vs Juniper "show route" comparison (Week 16)

| Goal | Cisco | MikroTik | Juniper |
|---|---|---|---|
| Show full routing table | `show ip route` | `/ip route print` | `show route` |
| Show BGP table | `show bgp ipv4 unicast` | `/routing bgp advertisements print` | `show route table inet.0 protocol bgp` |
| Show OSPF neighbors | `show ip ospf neighbor` | `/routing ospf neighbor print` | `show ospf neighbor` |
| Show interface counters | `show interfaces Gi0/1` | `/interface print stats` | `show interfaces ge-0/0/1 detail` |
| Reset BGP | `clear ip bgp 10.0.0.1 soft in` | `/routing bgp peer reset 0` | `clear bgp neighbor 10.0.0.1 soft` |
| Save config | `write memory` | `/system backup save name=...` | `commit` |

**Drill:** spend a coffee break doing this from memory once per week from Week 7 onward.

---

## D-44 — Save & restore: TFTP push (Week 16)

```
copy running-config tftp://10.99.99.40/R1-config-backup.cfg
copy tftp://10.99.99.40/R1-good-config.cfg running-config
```

**Or with SCP (production):**
```
ip scp server enable
copy running-config scp://admin@10.99.99.40/R1-config.cfg
```

**Target time:** 20 seconds.

---

## D-45 — Configuration rollback (IOS-XE archive) (Week 16)

```
archive
 path bootflash:/archive/R1
 maximum 14
 write-memory
 time-period 1440
!
! Trigger:
show archive
configure replace bootflash:/archive/R1-1
```

**Target time:** 60 seconds.

---

## D-46 — Multiple-context troubleshooting one-liner cheat (Week 16)

```
! Tunnel/IPsec breaks
show crypto session detail
show crypto ikev2 sa detailed
show dmvpn detail
debug crypto ikev2 error

! BGP suddenly missing prefixes
show bgp ipv4 unicast summary
show bgp ipv4 unicast neighbors X received-routes
show ip route bgp

! Multicast not working
show ip pim neighbor
show ip mroute X.X.X.X
show ip igmp groups

! Interface drops
show interfaces Gi0/1 | i errors|drops|reset
show platform hardware fed switch active fwd-asic resource ...
```

**Target:** memorize the first command of each block.

---

## D-47 to D-60 — Weekly review combos

Once you finish drills D-01 to D-46, the remaining drills are **combinations**. Pick one per week from D-14 onward as a 5-minute warm-up:

| ID | Combo |
|---|---|
| D-47 | VRF + BGP-PE + RT import — Week 10 quick-rebuild |
| D-48 | OSPF NSSA + redistribute static + verify Type-7→5 — Week 6 |
| D-49 | DMVPN spoke from scratch (no IPsec) — Week 11 |
| D-50 | DMVPN spoke + IPsec IKEv2 — Week 11 |
| D-51 | TACACS+ + CoPP — Week 12 |
| D-52 | SNMPv3 + IP SLA + tracker — Week 13 |
| D-53 | Flexible NetFlow + export — Week 13 |
| D-54 | gNMI subscribe + Prometheus scrape — Week 15 |
| D-55 | Ansible backup → Git commit — Week 15 |
| D-56 | Nornir state-gather report — Week 15 |
| D-57 | NETCONF GET + parse with `xmltodict` — Week 15 |
| D-58 | Full L3VPN end-to-end (Lab 10 mini-rebuild) — Week 16 |
| D-59 | BGP gauntlet (Lab 9 mini-rebuild) — Week 16 |
| D-60 | Capstone deployment via Ansible playbook (Lab 16 dress rehearsal) — Week 16 |

---

**Total: 46 unique drills + 14 weekly combos = 60 drills.**

**Daily discipline:**
1. Open this file.
2. Pick today's drill (one per day during weeks 1-14 lines up; Week 15-16 mostly use combos).
3. Type from memory; check; repeat if wrong.
4. Mark in `Post-ENCOR-Progress-Tracker.csv`.

By Week 16 your fingers know every command in this file. That's the goal — you don't think "what's the syntax", you think about the architecture.
