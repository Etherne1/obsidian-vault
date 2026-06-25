# ENCOR Labs — Modern Topics Supplement

**Purpose:** hand-written EVE-NG labs for topics that GNS3vault doesn't cover — VXLAN, SD-Access concepts, ERSPAN, model-driven telemetry, NETCONF/RESTCONF, Ansible, and EEM. Everything else in the ENCOR lab plan is handled by GNS3vault.

**When to use this file:** Pass 2 only. Referenced directly from `ENCOR-Study-Plan-TwoPass.md` — don't open this until the relevant Pass 2 block.

**GNS3vault repo (for everything else):** https://github.com/networklessons/labs/tree/main/gns3vault-archive/

---

## Images required

| Image | Used in |
|---|---|
| `vIOS-L2` (vIOS-L2 Adventerprise) | NA2 (SPAN/RSPAN source) |
| `CSR1000v 17.3.04a` | V3, NA2 (ERSPAN), NA3, AU1, AU2, AU3 |
| `vIOS-L3` | AU2 (mixed inventory) |
| Linux VM or Docker container | AU1 (ncclient/curl), AU2 (Ansible control node), AU3 (mail target) |

**Conventions:**
- Loopback0 = router-id, always `X.X.X.X/32` where X = router number (R1 → 1.1.1.1)
- Management on `Gi0/0` to EVE-NG `Cloud0` — leave as DHCP, don't reconfigure inside labs
- Lab links use `Gi0/1`, `Gi0/2`, etc. On CSR1000v: `Gi1`, `Gi2`, `Gi3`
- Default credentials: `admin / Cisco123!`, `enable secret Cisco123!`
- Paste configs from top to bottom on a fresh boot

---

## Lab V3 — VXLAN Flood-and-Learn between CSR1000v

**Pass 2 block:** Block 8 (Overlays)

**Why hand-written:** VXLAN NVE interface is IOS-XE only. GNS3vault uses legacy IOS images that don't support it.

**Goal:** build a VXLAN segment between two CSR1000v routers, verify MAC learning over the overlay.

**Topology:**
```
CSR1 (NVE1) ---- Gi1 --- 10.0.12.0/30 --- Gi1 ---- CSR2 (NVE2)
  Lo0: 1.1.1.1                                        Lo0: 2.2.2.2
  VNI 10010, VLAN 10                                  VNI 10010, VLAN 10
```

**CSR1:**
```
interface Loopback0
 ip address 1.1.1.1 255.255.255.255
!
interface GigabitEthernet1
 ip address 10.0.12.1 255.255.255.252
 no shut
!
! Static route so the NVE VTEP can reach the remote VTEP
ip route 2.2.2.2 255.255.255.255 10.0.12.2
!
vlan 10
!
interface vlan 10
 ip address 192.168.10.1 255.255.255.0
 no shut
!
interface nve1
 no ip address
 source-interface Loopback0
 host-reachability protocol flood-learn
 member vni 10010
  ingress-replication protocol static
  peer-ip 2.2.2.2
!
interface GigabitEthernet2
 switchport mode access
 switchport access vlan 10
 no shut
```

**CSR2** (mirror — swap IPs):
```
interface Loopback0
 ip address 2.2.2.2 255.255.255.255
!
interface GigabitEthernet1
 ip address 10.0.12.2 255.255.255.252
 no shut
!
ip route 1.1.1.1 255.255.255.255 10.0.12.1
!
vlan 10
!
interface vlan 10
 ip address 192.168.10.2 255.255.255.0
 no shut
!
interface nve1
 no ip address
 source-interface Loopback0
 host-reachability protocol flood-learn
 member vni 10010
  ingress-replication protocol static
  peer-ip 1.1.1.1
!
interface GigabitEthernet2
 switchport mode access
 switchport access vlan 10
 no shut
```

**Verification:**
```
show nve peers
show nve vni
show mac address-table
ping 192.168.10.2 source vlan 10
```

**Expected:** `show nve peers` shows the remote VTEP as `UP`. After a ping, `show mac address-table` shows the remote MAC learned over the NVE peer.

**What if?** Break ingress-replication by removing the `peer-ip` statement on one side. Observe NVE peer goes `DOWN`. Restore and verify MAC table clears and relearns.

---

## Lab A2 — SD-Access Underlay/Overlay Concept

**Pass 2 block:** Block 8 (Overlays)

**Why hand-written:** LISP + modern VXLAN require IOS-XE. This is a conceptual lab — the goal is to see how SDA uses IS-IS as underlay and LISP+VXLAN as overlay, not to build a production fabric.

**Goal:** understand the two-layer SDA architecture by building a simplified version. IS-IS underlay carries reachability between VTEPs; LISP control plane handles endpoint registration; VXLAN encapsulates traffic.

**Topology:** 3 CSR1000v
```
CSR1 (Edge/VTEP) --- Gi1 --- CSR2 (Border/RR) --- Gi2 --- CSR3 (Edge/VTEP)
  Lo0: 1.1.1.1                 Lo0: 2.2.2.2                Lo0: 3.3.3.3
  EID: 10.1.1.0/24                                          EID: 10.3.3.0/24
```

**Step 1 — IS-IS underlay (all three CSRs):**
```
router isis UNDERLAY
 net 49.0001.0000.0000.000X.00    ! X = router number
 is-type level-2-only
 metric-style wide
!
interface Loopback0
 ip router isis UNDERLAY
!
interface GigabitEthernet1       ! or Gi2 on CSR3
 ip router isis UNDERLAY
 isis network point-to-point
```

Verify: `show isis neighbors` — CSR1↔CSR2 and CSR2↔CSR3 adjacencies up.

**Step 2 — LISP control plane (CSR1 and CSR3 as xTR, CSR2 as Map-Server/Map-Resolver):**

**CSR2 (Map-Server + Map-Resolver):**
```
router lisp
 site SITE_A
  eid-prefix 10.1.1.0/24
  authentication-key Cisco123!
 site SITE_C
  eid-prefix 10.3.3.0/24
  authentication-key Cisco123!
 ipv4 map-server
 ipv4 map-resolver
```

**CSR1 (xTR for 10.1.1.0/24):**
```
router lisp
 locator-set LOC1
  2.2.2.2 priority 1 weight 100    ! using CSR2 as map-server RLOC for simplicity
  exit
 eid-table default instance-id 0
  database-mapping 10.1.1.0/24 locator-set LOC1
  map-server 2.2.2.2 key Cisco123!
  map-resolver 2.2.2.2
  exit
 ipv4 itr map-resolver 2.2.2.2
 ipv4 etr map-server 2.2.2.2 key Cisco123!
 ipv4 etr
 ipv4 itr
```

CSR3: mirror of CSR1 with `10.3.3.0/24`.

**Step 3 — VXLAN data plane (CSR1 and CSR3):**
```
interface nve1
 source-interface Loopback0
 host-reachability protocol lisp     ! use LISP for MAC/IP→RLOC resolution
 member vni 20010 vrf default
```

**Verification:**
```
show lisp session
show lisp site
show lisp map-cache
show nve peers
```

**Key questions to answer from the output (write in your notes):**
1. What does the Map-Server know about each site's EID prefixes?
2. When CSR1 pings `10.3.3.1`, what LISP message does it send first and to where?
3. After the LISP map-reply, how does the VXLAN encapsulation know which VTEP to use?

This is the conceptual bridge between "Cisco SDA marketing slides" and "actual protocol behavior."

---

## Lab NA2 — SPAN / RSPAN / ERSPAN

**Pass 2 block:** Block 10 (Network Assurance)

**Why hand-written:** ERSPAN requires CSR1000v (IOS-XE only). SPAN/RSPAN covered in GNS3vault but ERSPAN is not — doing all three here keeps the comparison clean.

**Topology:**
```
ACC1 (vIOS-L2) --- Gi0/3 (monitored port) --- host
                --- Gi0/4 (SPAN destination) --- capture host

ACC2 (vIOS-L2) --- connected to ACC1 via trunk (RSPAN VLAN 999)

CSR1 (CSR1000v) --- Gi2 (monitored) --- Gi3 (toward Linux ERSPAN collector)
```

**Part 1 — Local SPAN on ACC1:**
```
monitor session 1 source interface Gi0/3 both
monitor session 1 destination interface Gi0/4
```

Verify: `show monitor session 1`. Plug Wireshark on Gi0/4 destination — see mirrored frames from Gi0/3.

**Part 2 — RSPAN across ACC1 → ACC2:**
```
! On ACC1
vlan 999
 remote-span
monitor session 1 source interface Gi0/3 both
monitor session 1 destination remote vlan 999

! On ACC2
vlan 999
 remote-span
monitor session 2 source remote vlan 999
monitor session 2 destination interface Gi0/4
```

Verify: `show monitor session all`. Traffic from ACC1's Gi0/3 appears on ACC2's Gi0/4 — crossing the trunk.

**Part 3 — ERSPAN on CSR1000v:**
```
monitor session 1 type erspan-source
 source interface GigabitEthernet2 both
 no shutdown
 destination
  erspan-id 100
  ip address 192.0.2.200        ! Linux collector IP
  origin ip address 1.1.1.1
```

Verify on collector: `wireshark -i eth0 -f "proto gre"` — see GRE-encapsulated traffic with ERSPAN ID 100.

**Comparison table to write in notes:**

| Feature | SPAN | RSPAN | ERSPAN |
|---|---|---|---|
| Crosses switches | No | Yes (VLAN) | Yes (IP) |
| Requires dedicated VLAN | No | Yes | No |
| Analyzer location | Same switch | Same L2 domain | Anywhere IP-reachable |
| IOS-XE required | No | No | Yes (source) |

---

## Lab NA3 — Flexible NetFlow + Model-Driven Telemetry

**Pass 2 block:** Block 10 (Network Assurance)

**Why hand-written:** model-driven telemetry (gNMI dial-out, `telemetry ietf subscription`) is IOS-XE 17.x only. GNS3vault images are too old.

**Platform:** CSR1000v 17.3.04a + Linux VM (Docker with nfdump/ntopng for NetFlow, gnmic for MDT).

**Part 1 — Flexible NetFlow:**
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

Verify:
```
show flow monitor FNF-MON cache
show flow exporter FNF-EXP statistics
```

On collector: `nfdump -r /var/cache/nfdump/nfcapd.* -s srcip` — top source IPs visible.

**Part 2 — Model-Driven Telemetry (gNMI dial-out):**
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

Verify:
```
show telemetry ietf subscription all
show telemetry ietf subscription 101 receiver
```

On collector using gnmic: `gnmic -a 192.0.2.1:57500 subscribe --path "/interfaces/interface/state/counters"` — see live counter stream.

**Key comparison to write in notes:**

| Aspect | NetFlow/IPFIX | SNMP polling | MDT (gNMI/gRPC) |
|---|---|---|---|
| Transport | UDP (lossy) | UDP | TCP (reliable) |
| Direction | Device pushes | Manager polls | Device pushes (dial-out) or manager subscribes (dial-in) |
| Granularity | Per-flow | Per-OID | Per-YANG path |
| Latency | Seconds (cache timeout) | Poll interval | Sub-second (sample interval) |
| Config overhead | Medium | Low | Higher (YANG paths) |

---

## Lab AU1 — NETCONF + RESTCONF on CSR1000v

**Pass 2 block:** Block 12 (Automation)

**Why hand-written:** NETCONF/RESTCONF is IOS-XE only and requires a Python environment on a Linux host — not possible in GNS3vault's self-contained topology model.

**Enable on CSR1000v:**
```
netconf-yang
netconf-yang feature candidate-datastore
restconf
ip http secure-server
username api privilege 15 secret Cisco123!
```

**Verify services are running:**
```
show platform software yang-management process
show netconf-yang sessions detail
show restconf
```

**NETCONF from Linux (ncclient):**
```bash
# Raw hello test
ssh -p 830 -s api@198.51.100.1 netconf
```

```python
from ncclient import manager

with manager.connect(
    host="198.51.100.1", port=830,
    username="api", password="Cisco123!",
    hostkey_verify=False,
    device_params={"name": "iosxe"}
) as m:
    # Get running config
    print(m.get_config(source="running").data_xml[:2000])

    # Get OSPF neighbors via OpenConfig YANG
    filter_xml = """
    <filter>
      <ospf xmlns="http://cisco.com/ns/yang/Cisco-IOS-XE-ospf-oper">
        <ospf-state>
          <ospf-instance>
            <ospf-area>
              <ospf-neighbor/>
            </ospf-area>
          </ospf-instance>
        </ospf-state>
      </ospf>
    </filter>"""
    reply = m.get(filter_xml)
    print(reply.xml)
```

**RESTCONF from Linux (curl):**
```bash
# Get hostname
curl -k -u api:'Cisco123!' \
  -H 'Accept: application/yang-data+json' \
  https://198.51.100.1/restconf/data/Cisco-IOS-XE-native:native/hostname

# Get interface list
curl -k -u api:'Cisco123!' \
  -H 'Accept: application/yang-data+json' \
  https://198.51.100.1/restconf/data/ietf-interfaces:interfaces

# Push a loopback (PUT)
curl -k -u api:'Cisco123!' -X PUT \
  -H 'Content-Type: application/yang-data+json' \
  -d '{"ietf-interfaces:interface":{"name":"Loopback99","type":"iana-if-type:softwareLoopback","ietf-ip:ipv4":{"address":[{"ip":"99.99.99.99","prefix-length":32}]}}}' \
  https://198.51.100.1/restconf/data/ietf-interfaces:interfaces/interface=Loopback99
```

**Verify the PUT worked:**
```
show interfaces Loopback99
show running-config | section Loopback99
```

**Notes to write:** NETCONF vs RESTCONF — transport (SSH/830 vs HTTPS/443), encoding (XML vs JSON), operations (get-config/edit-config vs HTTP GET/PUT/PATCH/DELETE). When you'd use each in production.

---

## Lab AU2 — Ansible push: OSPF + VLAN config

**Pass 2 block:** Block 12 (Automation)

**Why hand-written:** Ansible runs on a Linux control node — not possible inside a GNS3vault self-contained topology.

**Inventory** (`hosts.ini`):
```ini
[csr]
csr1 ansible_host=198.51.100.1

[vios]
r1 ansible_host=198.51.100.2

[csr:vars]
ansible_user=api
ansible_password=Cisco123!
ansible_network_os=cisco.ios.ios
ansible_connection=network_cli

[vios:vars]
ansible_user=admin
ansible_password=Cisco123!
ansible_network_os=cisco.ios.ios
ansible_connection=network_cli
```

**Playbook 1 — push a loopback and OSPF to CSR** (`push_ospf.yml`):
```yaml
- hosts: csr
  gather_facts: no
  tasks:
    - name: Ensure Loopback100
      cisco.ios.ios_l3_interfaces:
        config:
          - name: Loopback100
            ipv4:
              - address: 100.100.100.100/32
        state: merged

    - name: Ensure OSPF process
      cisco.ios.ios_ospfv2:
        config:
          processes:
            - process_id: 1
              router_id: 100.100.100.100
              areas:
                - area_id: '0'
        state: merged
```

Run: `ansible-playbook -i hosts.ini push_ospf.yml`

**Verify idempotency:** run again — `changed=0`. This is the key property of declarative automation.

**Playbook 2 — collect state** (`gather_state.yml`):
```yaml
- hosts: all
  gather_facts: no
  tasks:
    - name: Get running config
      cisco.ios.ios_command:
        commands: show running-config
      register: cfg

    - name: Save to file
      copy:
        content: "{{ cfg.stdout[0] }}"
        dest: "configs/{{ inventory_hostname }}.cfg"
```

Run: `ansible-playbook -i hosts.ini gather_state.yml` — configs saved to `configs/` directory.

**Playbook 3 — rollback** (`rollback.yml`):
```yaml
- hosts: csr
  gather_facts: no
  tasks:
    - name: Remove Loopback100
      cisco.ios.ios_l3_interfaces:
        config:
          - name: Loopback100
        state: deleted

    - name: Remove OSPF process
      cisco.ios.ios_ospfv2:
        config:
          processes:
            - process_id: 1
        state: deleted
```

**Notes to write:** idempotency — what it means and why it matters. `state: merged` vs `state: replaced` vs `state: deleted`. Why `ios_command` is imperative (not idempotent) while `ios_ospfv2` is declarative.

---

## Lab AU3 — EEM applet — interface-state syslog reaction

**Pass 2 block:** Block 12 (Automation)

**Why hand-written:** EEM works on all images but the interesting IOS-XE-specific variants (Python EEM, on-box telemetry triggers) require CSR1000v. GNS3vault has basic EEM labs but this one extends to IOS-XE specifics.

**Basic applet — react to interface down:**
```
event manager applet IF-DOWN-ALERT
 event syslog pattern "Interface GigabitEthernet0/1, changed state to down"
 action 1.0 syslog priority warnings msg "EEM: Gi0/1 down — auto-action firing"
 action 1.1 cli command "enable"
 action 1.2 cli command "show ip int brief"
 action 1.3 cli command "show log | tail 10"
```

**Timer-based applet — scheduled config archive:**
```
event manager applet CONFIG-BACKUP
 event timer cron name DAILY cron-entry "0 2 * * *"
 action 1.0 cli command "enable"
 action 1.1 cli command "copy running-config tftp://192.0.2.100/backup-$_host-$_localtime.cfg"
 action 1.2 syslog priority informational msg "EEM: config backup completed"
```

**CLI event applet — react to a specific command being typed:**
```
event manager applet CATCH-NO-SHUT
 event cli pattern "no shutdown" sync no skip no
 action 1.0 syslog priority informational msg "EEM: no shutdown typed on $_cli_msg"
```

**Verification:**
```
! Manual trigger
event manager run IF-DOWN-ALERT

! Or trigger naturally
interface GigabitEthernet0/1
 shutdown

! Check results
show event manager history events
show event manager statistics server
show logging | include EEM
```

**What if?** Add `action 2.0 mail server ...` to the IF-DOWN-ALERT applet and verify the mail action fires (requires a mail server VM in EVE-NG, or just verify the action is registered without a live mail target).

**Notes to write:** EEM event detectors available — syslog, timer, cli, interface, SNMP, OIR, routing. When you'd use EEM vs external automation (Ansible/Nornir) — EEM is on-box, zero-dependency, millisecond reaction; Ansible is off-box, requires connectivity, but more powerful and version-controlled.

---

## Lab cross-reference (Pass 2 blocks)

| Pass 2 block | Lab | Description |
|---|---|---|
| Block 8 — Overlays | V3 | VXLAN Flood-and-Learn |
| Block 8 — Overlays | A2 | SD-Access Underlay/Overlay Concept |
| Block 10 — Assurance | NA2 | SPAN / RSPAN / ERSPAN |
| Block 10 — Assurance | NA3 | Flexible NetFlow + Model-Driven Telemetry |
| Block 12 — Automation | AU1 | NETCONF + RESTCONF |
| Block 12 — Automation | AU2 | Ansible push |
| Block 12 — Automation | AU3 | EEM applets |
