
 ### Уточнение по топологии
Нумерация интерфейсов в используемых образах отличается от реального оборудования, что создает  слоности при выполнении ДЗ.  Пришлось менять нумерацию интерфейсов. Вариант выбрал не самый оптимальный, но хорошего варианта тут не было. Вместо топологии:

![](https://github.com/Etherne1/otus_network_engineer/blob/main/Lab03/Pasted%20image%2020241020151723.png?raw=true)  

Получилась следующая:  
![](https://github.com/Etherne1/otus_network_engineer/blob/main/Lab03/Pasted%20image%2020241020152404.png?raw=true)

Т.е. нумерация интерфейсов на роутерах сменилась с формата Gi0/0/1 на e0/1; нумерация интерфейсов на свитчах с F0/18 на e8/1 и с F0/5 на e5/0 соответственно.
Логически схема не изменилась, но форму топологии поменял, для наглядности.
В связи со сменой формата, пришлось менять ее и в таблицах(а так же в заданиях):

###  Addressing Table
|Device|Interface|IPv6 Address|
|---|---|---|
|R1|et0/0|2001:db8:acad:2::1/64|
|R1|et0/0|fe80::1|
|R1|et0/1|2001:db8:acad:1::1/64|
|R1|et0/1|fe80::1|
|R2|et0/0|2001:db8:acad:2::2/64|
|R2|et0/0|fe80::2|
|R2|et0/1|2001:db8:acad:3::1/64|
|R2|et0/1|fe80::1|
|PC-A|NIC|DHCP|
|PC-B|NIC|DHCP|

## Part 1: Build the Network and Configure Basic Device Settings

<details>
  <summary>  Step 1: Cable the network as shown in the topology. </summary>

Attach the devices as shown in the topology diagram, and cable as necessary.

</details>


<details>
  <summary>Step 2: Configure basic settings for each switch. (Optional)
</summary>



a.      Assign a device name to the switch.

b.      Disable DNS lookup to prevent the router from attempting to translate incorrectly entered commands as though they were host names.

c.      Assign **class** as the privileged EXEC encrypted password.

d.      Assign **cisco** as the console password and enable login.

e.      Assign **cisco** as the VTY password and enable login.

f.       Encrypt the plaintext passwords.

g.      Create a banner that warns anyone accessing the device that unauthorized access is prohibited.

h.      Shutdown all unused ports

i.        Save the running configuration to the startup configuration file.

```
ena
clock set 14:40:00 20 october 2024
conf t
no ip domain-lookup
banner motd "unauthorized access is prohibited"
line vty 0 4
 login local
 password cisco
 line con 0 
 password cisco
 logging syn
enable secret cisco
service password-encryption
end
wr
```

```
S1(config)#int Et0/0
S1(config-if)#sh
S1(config-if)#int Et0/1
S1(config-if)#sh
S1(config-if)#int Et0/2
S1(config-if)#sh
S1(config-if)#int Et0/3
S1(config-if)#sh
S1(config-if)#int Et1/0
S1(config-if)#sh
S1(config-if)#int Et1/1
S1(config-if)#sh
S1(config-if)#int Et1/2
S1(config-if)#sh
S1(config-if)#int Et1/3
S1(config-if)#sh
S1(config-if)#int Et2/0
S1(config-if)#sh
S1(config-if)#int Et2/1
S1(config-if)#sh
S1(config-if)#int Et2/2
S1(config-if)#sh
S1(config-if)#int Et2/3
S1(config-if)#sh
S1(config-if)#int Et3/0
S1(config-if)#sh
S1(config-if)#int Et3/1
S1(config-if)#sh
S1(config-if)#int Et3/2
S1(config-if)#sh
S1(config-if)#int Et3/3
S1(config-if)#sh
S1(config-if)#int Et4/0
S1(config-if)#sh
S1(config-if)#int Et4/1
S1(config-if)#sh
S1(config-if)#int Et4/2
S1(config-if)#sh
S1(config-if)#int Et4/3
S1(config-if)#sh
S1(config-if)#int Et5/1
S1(config-if)#sh
S1(config-if)#int Et5/2
S1(config-if)#sh
S1(config-if)#int Et5/3
S1(config-if)#sh
S1(config-if)#int Et6/0
S1(config-if)#sh
S1(config-if)#int Et6/2
S1(config-if)#sh
S1(config-if)#int Et6/3
S1(config-if)#sh
S1(config-if)#int Et7/0
S1(config-if)#sh
S1(config-if)#int Et7/1
S1(config-if)#sh
S1(config-if)#int Et7/2
S1(config-if)#sh
S1(config-if)#int Et7/3
S1(config-if)#sh
S1(config-if)#int Et8/0
S1(config-if)#sh
S1(config-if)#int Et8/1
S1(config-if)#sh
S1(config-if)#int Et8/2
S1(config-if)#sh
S1(config-if)#int Et8/3
S1(config-if)#sh
S1(config-if)#int Et9/0
S1(config-if)#sh
S1(config-if)#int Et9/1
S1(config-if)#sh
S1(config-if)#int Et9/2
S1(config-if)#sh
S1(config-if)#int Et9/3
S1(config-if)#sh
```
```
S2(config)#int Et0/0
S2(config-if)#sh
S2(config-if)#int Et0/1
S2(config-if)#sh
S2(config-if)#int Et0/2
S2(config-if)#sh
S2(config-if)#int Et0/3
S2(config-if)#sh
S2(config-if)#int Et1/0
S2(config-if)#sh
S2(config-if)#int Et1/1
S2(config-if)#sh
S2(config-if)#int Et1/2
S2(config-if)#sh
S2(config-if)#int Et1/3
S2(config-if)#sh
S2(config-if)#int Et2/0
S2(config-if)#sh
S2(config-if)#int Et2/1
S2(config-if)#sh
S2(config-if)#int Et2/2
S2(config-if)#sh
S2(config-if)#int Et2/3
S2(config-if)#sh
S2(config-if)#int Et3/0
S2(config-if)#sh
S2(config-if)#int Et3/1
S2(config-if)#sh
S2(config-if)#int Et3/2
S2(config-if)#sh
S2(config-if)#int Et3/3
S2(config-if)#sh
S2(config-if)#int Et4/0
S2(config-if)#sh
S2(config-if)#int Et4/1
S2(config-if)#sh
S2(config-if)#int Et4/2
S2(config-if)#sh
S2(config-if)#int Et4/3
S2(config-if)#sh
S2(config-if)#int Et5/1
S2(config-if)#sh
S2(config-if)#int Et5/2
S2(config-if)#sh
S2(config-if)#int Et5/3
S2(config-if)#sh
S2(config-if)#int Et6/0
S2(config-if)#sh
S2(config-if)#int Et6/1
S2(config-if)#sh
S2(config-if)#int Et6/2
S2(config-if)#sh
S2(config-if)#int Et6/3
S2(config-if)#sh
S2(config-if)#int Et7/0
S2(config-if)#sh
S2(config-if)#int Et7/1
S2(config-if)#sh
S2(config-if)#int Et7/2
S2(config-if)#sh
S2(config-if)#int Et7/3
S2(config-if)#sh
S2(config-if)#int Et8/0
S2(config-if)#sh
S2(config-if)#int Et8/2
S2(config-if)#sh
S2(config-if)#int Et8/3
S2(config-if)#sh
S2(config-if)#int Et9/0
S2(config-if)#sh
S2(config-if)#int Et9/1
S2(config-if)#sh
S2(config-if)#int Et9/2
S2(config-if)#sh
S2(config-if)#int Et9/3
S2(config-if)#sh
```

</details>

 <details>
  <summary>Step 3: Configure basic settings for each router.</summary>
  сделано в рамках предыдущего пункта через multi exec </details>


<details>
  <summary>Step 4: Configure interfaces and routing for both routers.</summary>
a.      Configure the Et0/0 and Et0/1 interfaces on R1 and R2 with the IPv6 addresses specified in the table above.

```
R1(config)#int e0/0
R1(config-if)#ipv6 ena
R1(config-if)#ipv6 unicast-routing
R1(config-if)#ipv6 add 2001:db8:acad:2::1/64
R1(config-if)#ipv6 add fe80::1 link-local
R1(config-if)#no sh  

R1(config)#int et0/1
R1(config-if)#ipv6 ena
R1(config-if)#ipv6 add 2001:db8:acad:1::1/64
R1(config-if)#ipv6 add fe80::1 link-local
R1(config-if)#no sh

```

```
R2(config)#int e0/0
R2(config-if)#ipv6 ena
R2(config-if)#ipv6 addr 2001:db8:acad:2::2/64
R2(config-if)#ipv6 addr fe80::2 link-local
R2(config-if)#no sh
R2(config-if)#int e0/1
R2(config-if)#ipv6 ena
R2(config-if)#ipv6 addr 2001:db8:acad:3::1/64
R2(config-if)#ipv6 addr fe80::1 link-local
R2(config-if)#no sh
```



b.      Configure a default route on each router pointed to the IP address of Et0/0 on the other router.
```
R1(config)#ipv6 route 0::0/0 2001:db8:acad:2::2
```
```
R2(config)#ipv6 route 0::0/0 2001:db8:acad:2::1
```


c.      Verify routing is working by pinging R2’s Et0/1 address from R1
```
R1#ping 2001:db8:acad:2::2
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 2001:DB8:ACAD:2::2, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/1 ms
```

d.      Save the running configuration to the startup configuration file.
```
R1#wr
Building configuration...
[OK]
```

```
R2#wr
Building configuration...
[OK]
```
</details>


 ## Part 2: Verify SLAAC Address Assignment from R1
 <details>


```
PC-A#sh ipv6 interface brief
Ethernet0/0            [up/up]
    FE80::A8BB:CCFF:FE00:5000
    2001:DB8:ACAD:1:A8BB:CCFF:FE00:5000
Ethernet0/1            [administratively down/down]
    unassigned
Ethernet0/2            [administratively down/down]
    unassigned
Ethernet0/3            [administratively down/down]
    unassigned
```


Where did the host-id portion of the address come from?

```
PC-A#sh int e0/0 | i add
  Hardware is AmdP2, address is aabb.cc00.5000 (bia aabb.cc00.5000)
```

</details>



 ## Part 3: Configure and Verify a DHCPv6 server on R1
 <details>
 <summary> Step 1: Examine the configuration of PC-A in more detail. </summary>

```
PC-A>sh ipv6 interface
Ethernet0/0 is up, line protocol is up
  IPv6 is enabled, link-local address is FE80::A8BB:CCFF:FE00:5000
  No Virtual link-local address(es):
  Stateless address autoconfig enabled
  Global unicast address(es):
    2001:DB8:ACAD:1:A8BB:CCFF:FE00:5000, subnet is 2001:DB8:ACAD:1::/64 [EUI/CAL/PRE]
      valid lifetime 2591963 preferred lifetime 604763
  Joined group address(es):
    FF02::1
    FF02::1:FF00:5000
  MTU is 1500 bytes
  ICMP error messages limited to one every 100 milliseconds
  ICMP redirects are enabled
  ICMP unreachables are sent
  ND DAD is enabled, number of DAD attempts: 1
  ND reachable time is 30000 milliseconds (using 30000)
  ND NS retransmit interval is 1000 milliseconds
  Default router is FE80::1 on Ethernet0/0
```
 </details>
  <details>
 <summary> Step 2: Configure R1 to provide stateless DHCPv6 for PC-A.</summary>

```
R1(config)#ipv6 dhcp pool R1-STATELESS
R1(config-dhcpv6)#dns-server 2001:db8:acad::254
R1(config-dhcpv6)#domain-name STATELESS.com
R1(config-dhcpv6)#exit
R1(config)#int e0/1
R1(config-if)#ipv6 nd other-config-flag
R1(config-if)#ipv6 dhcp server R1-STATELESS
```


```
PC-A>sh ipv6 interface
Ethernet0/0 is up, line protocol is up
  IPv6 is enabled, link-local address is FE80::A8BB:CCFF:FE00:5000
  No Virtual link-local address(es):
  Stateless address autoconfig enabled
  Global unicast address(es):
    2001:DB8:ACAD:1:A8BB:CCFF:FE00:5000, subnet is 2001:DB8:ACAD:1::/64 [EUI/CAL/PRE]
      valid lifetime 2591886 preferred lifetime 604686
  Joined group address(es):
    FF02::1
    FF02::1:FF00:5000
  MTU is 1500 bytes
  ICMP error messages limited to one every 100 milliseconds
  ICMP redirects are enabled
  ICMP unreachables are sent
  ND DAD is enabled, number of DAD attempts: 1
  ND reachable time is 30000 milliseconds (using 30000)
  ND NS retransmit interval is 1000 milliseconds
  Default router is FE80::1 on Ethernet0/0

PC-A> sh ipv6 dhcp interface
Ethernet0/0 is in client mode
  Prefix State is IDLE (0)
  Information refresh timer expires in 23:58:33
  Address State is IDLE
  List of known servers:
    Reachable via address: FE80::1
    DUID: 00030001AABBCC003000
    Preference: 0
    Configuration parameters:
      DNS server: 2001:DB8:ACAD::254
      Domain name: STATELESS.com
      Information refresh time: 0
  Prefix Rapid-Commit: disabled
  Address Rapid-Commit: disabled
```

 </details>


## Part 4: Configure a stateful DHCPv6 server on R1
<details>
 <summary>a. Create a DHCPv6 pool on R1 for the 2001:db8:acad:3:aaaa::/80 network</summary>
 
```
R1(config)#ipv6 dhcp pool R2-STATEFUL
R1(config-dhcpv6)#address prefix 2001:db8:acad:3:aaa::/80
R1(config-dhcpv6)#dns-server 2001:db8:acad::254
R1(config-dhcpv6)#domain-name STATEFUL.com
```
</details>

<details>
 <summary>b. Assign the DHCPv6 pool</summary>

```
R1(config)#int e0/0
R1(config-if)#ipv6 dhcp pool R2-STATEFUL
```
</details>

## Part 5: Configure and verify DHCPv6 relay on R2.


<details>
 <summary>Step 1: Power on PC-B and examine the SLAAC address that it generates.</summary>
 

 ```
Router#sh ipv6 interface brief
Ethernet0/0            [up/up]
    FE80::A8BB:CCFF:FE00:6000
Ethernet0/1            [administratively down/down]
    unassigned
Ethernet0/2            [administratively down/down]
    unassigned
Ethernet0/3            [administratively down/down]
    unassigned
```

   </details>
   
<details>
 <summary> Step 2: Configure R2 as a DHCP relay agent for the LAN on G0/0/1. </summary>

```
R2(config)# interface e0/1
R2(config-if)#ipv6 nd managed-config-flag
R2(config-if)#ipv6 dhcp relay destination 2001:db8:acad:2::1 e0/0
```


   </details>

<details>
 <summary>Step 3: Attempt to acquire an IPv6 address from DHCPv6 on PC-B.</summary>


```
PC-B#sh ipv6 dhcp interface
Ethernet0/0 is in client mode
  Prefix State is IDLE
  Address State is OPEN
  Renew for address will be sent in 11:51:25
  List of known servers:
    Reachable via address: FE80::1
    DUID: 00030001AABBCC003000
    Preference: 0
    Configuration parameters:
      IA NA: IA ID 0x00030001, T1 43200, T2 69120
        Address: 2001:DB8:ACAD:3:AAA:B861:B80E:DA33/128
                preferred lifetime 86400, valid lifetime 172800
                expires at Dec 23 2024 05:24 PM (172285 seconds)
      DNS server: 2001:DB8:ACAD::254
      Domain name: STATEFUL.com
      Information refresh time: 0
  Prefix Rapid-Commit: disabled
  Address Rapid-Commit: disabled
```







  Test connectivity by pinging R1’s G0/0/1 interface IP address.
```
PC-B# ping 2001:DB8:ACAD:1::1
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 2001:DB8:ACAD:1::1, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/4/19 ms
```
   </details>