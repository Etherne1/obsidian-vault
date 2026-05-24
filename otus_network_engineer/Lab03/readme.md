 
 ### Уточнение по топологии
Нумерация интерфейсов в используемых образах отличается от реального оборудования, что создает  слоности при выполнении ДЗ.  Пришлось менять нумерацию интерфейсов. Вариант выбрал не самый оптимальный, но хорошего варианта тут не было. Вместо топологии:

![](https://github.com/Etherne1/otus_network_engineer/blob/main/Lab03/Pasted%20image%2020241020151723.png?raw=true)  

Получилась следующая:  
![](https://github.com/Etherne1/otus_network_engineer/blob/main/Lab03/Pasted%20image%2020241020152404.png?raw=true)

Т.е. нумерация интерфейсов на роутерах сменилась с формата Gi0/0/1 на e0/1; нумерация интерфейсов на свитчах с F0/18 на e8/1 и с F0/5 на e5/0 соответственно.
Логически схема не изменилась, но форму топологии поменял, для наглядности.
В связи со сменой формата, пришлось менять ее и в таблицах(а так же в заданиях):

###  Addressing Table
| Device | Interface  | IP Address   | Subnet Mask     | Default Gateway |
| ------ | ---------- | ------------ | --------------- | --------------- |
| R1     | et0/0      | 10.0.0.1     | 255.255.255.252 | N/A             |
| R1     | et0/1      | N/A          | N/A             | N/A             |
| R1     | et0/1.100  | 192.168.1.1  | 255.255.255.192 | N/A             |
| R1     | et0/1.200  | 192.168.1.65 | 255.255.255.224 | N/A             |
| R1     | et0/1.1000 | N/A          | N/A             | N/A             |
| R2     | et0/0      | 10.0.0.2     | 255.255.255.252 | N/A             |
| R2     | et0/1      | 192.168.1.97 | 255.255.255.240 | N/A             |
| S1     | VLAN 200   | 192.168.1.66 | 255.255.255.224 | 192.168.1.65    |
| S2     | VLAN 1     | blank        | blank           | blank           |
| PC-A   | NIC        | DHCP         | DHCP            | DHCP            |
| PC-B   | NIC        | DHCP         | DHCP            | DHCP            |

### VLAN Table
|VLAN|Name|Interface Assigned|
|---|---|---|
|1|N/A|S2: e8/1|
|100|Clients|S1: e6/0|
|200|Management|S1: VLAN 200|
|999|Parking_Lot|S1: all others|
|1000|Native|N/A|


## Part 1: Build the Network and Configure Basic Device Settings

<details>
  <summary> Step 1: Establish an addressing scheme</summary>
 Subnet the network 192.168.1.0/24 to meet the following requirements: 

a. One subnet, “Subnet A”, supporting 58 hosts (the client VLAN at R1).
Subnet A: 
```
192.168.1.0/26
```

Record the first IP address in the Addressing Table for R1 et0/1.100.

b. One subnet, “Subnet B”, supporting 28 hosts (the management VLAN at R1).
Subnet B: 
```
192.168.1.64/27
```

Record the first IP address in the Addressing Table for R1 et0/1.200. Record the second IP address in the Address Table for S1 VLAN 200 and enter the associated default gateway.

c. One subnet, “Subnet C”, supporting 12 hosts (the client network at R2).
Subnet C: 
```
192.168.1.96/28
```

Record the first IP address in the Addressing Table for R2 et0/1.
  
</details>

<details>
  <summary> Step 2: Cable the network as shown in the topology.</summary>
  Done.
 </details>
 
<details>
 <summary> Step 3: Configure basic settings for each router.</summary>


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

</details>

<details>
  <summary> Step 4:  Configure Inter-VLAN Routing on R1:</summary>
  
a. Activate interface et0/1 on the router.

```
Router(config)#int eth 0/1
Router(config-if)#no sh
Router(config-if)#
*Oct 20 16:21:22.079: %LINK-3-UPDOWN: Interface Ethernet0/1, changed state to up
*Oct 20 16:21:23.081: %LINEPROTO-5-UPDOWN: Line protocol on Interface Ethernet0/1, changed state to up
```


b. Configure sub-interfaces for each VLAN as required by the IP addressing table. All sub-interfaces use 802.1Q encapsulation and are assigned the first usable address from the IP address pool you have calculated. Ensure the sub-interface for the native VLAN does not have an IP address assigned. Include a description for each sub-interface.
```
int et0/1.100
enc dot 100
desc Clients
ip add 192.168.1.1 255.255.255.192
int et0/1.200
enc dot 200
desc Management
ip add 192.168.1.65 255.255.255.224
int et0/1.1000    
enc dot 1000
desc Native
```
c. Verify the sub-interfaces are operational.

  

```
Router#sh int desc | i 0/1
Et0/1                 up           up
Et0/1.100             up           up       Clients
Et0/1.200             up           up       Management
Et0/1.1000            up           up       Native
```
  </details>

<details>
  <summary> Step 5:  Configure et0/1 on R2, then et0/0 and static routing for both routers:</summary>


a. Configure et0/1 on R2 with the first IP address of Subnet C you calculated earlier.
```
R2(config)#int et0/1
R2(config-if)#ip add 192.168.1.97
R2(config-if)#ip add 192.168.1.97 255.255.255.240
R2(config-if)#no sh
*Oct 20 17:15:55.678: %LINK-3-UPDOWN: Interface Ethernet0/1, changed state to up
*Oct 20 17:15:56.679: %LINEPROTO-5-UPDOWN: Line protocol on Interface Ethernet0/1, changed state to up
```


b. Configure interface et0/0 for each router based on the IP Addressing table above.

```
R1(config)#int et0/0
R1(config-if)#ip add 10.0.0.1 255.255.255.252
R1(config-if)#no sh
*Oct 20 17:19:33.770: %LINK-3-UPDOWN: Interface Ethernet0/0, changed state to up
*Oct 20 17:19:34.770: %LINEPROTO-5-UPDOWN: Line protocol on Interface Ethernet0/0, changed state to up
```

```
R2(config)#int et0/0
R2(config-if)#ip add 10.0.0.2 255.255.255.252
R2(config-if)#no sh
*Oct 20 17:23:03.484: %LINK-3-UPDOWN: Interface Ethernet0/0, changed state to up
*Oct 20 17:23:04.488: %LINEPROTO-5-UPDOWN: Line protocol on Interface Ethernet0/0, changed state to up
```

c. Configure a default route on each router pointed to the IP address of et0/0 on the other router.

```
R1(config)#ip route 0.0.0.0 0.0.0.0 10.0.0.2
```

```
R2(config)#ip route 0.0.0.0 0.0.0.0 10.0.0.1
```

d. Verify static routing is working by pinging R2’s et0/1 address from R1.

```
R1#ping 10.0.0.2
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.0.0.2, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/1 ms
```

e.  Save the running configuration to the startup configuration file.

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

<details>
  <summary>  Step 6: Configure basic settings for each switch.</summary>

a.      Assign a device name to the switch.

b.      Disable DNS lookup to prevent the router from attempting to translate incorrectly entered commands as though they were host names.

c.      Assign **class** as the privileged EXEC encrypted password.

d.      Assign **cisco** as the console password and enable login.

e.      Assign **cisco** as the VTY password and enable login.

f.       Encrypt the plaintext passwords.

g.      Create a banner that warns anyone accessing the device that unauthorized access is prohibited.

h.      Save the running configuration to the startup configuration file.

i.        Set the clock on the switch to today’s time and date.

**Note**: Use the question mark (**?**) to help with the correct sequence of parameters needed to execute this command.

j.        Copy the running configuration to the startup configuration.


### Все это сделано в рамках п.3, т.к. кроме hostname, первоначальную конфигурацию вводил через MultiExec.


  </details>

<details>
  <summary>  Step 7: Create VLANs on S1.</summary>
Note: S2 is only configured with basic settings.

a. Create and name the required VLANs on switch 1 from the table above.
```
S1(config)#vlan 1
S1(config-vlan)#vlan 100
S1(config-vlan)#name Clients
S1(config-vlan)#vlan 200
S1(config-vlan)#name Management
S1(config-vlan)#vlan 999
S1(config-vlan)#name Parking_Lot
S1(config-vlan)#vlan 1000
S1(config-vlan)#name Native
```

b. Configure and activate the management interface on S1 (VLAN 200) using the second IP address from the subnet calculated earlier. Additionally, set the default gateway on S1.

```
S1(config)#int vlan 200
S1(config-if)#ip add 192.168.1.66 255.255.255.224
S1(config-if)#no sh
*Oct 20 17:43:23.068: %LINK-3-UPDOWN: Interface Vlan200, changed state to down
```

c. Configure and activate the management interface on S2 (VLAN 1) using the second IP address from the subnet calculated earlier. Additionally, set the default gateway on S2
```
S1(config-if)#ip add 192.168.1.66 255.255.255.224
S1(config)#ip default-gateway 192.168.1.65
```

d. Assign all unused ports on S1 to the Parking_Lot VLAN, configure them for static access mode, and administratively deactivate them. On S2, administratively deactivate all the unused ports.

```
S1(config)#int Et0/0
S1(config-if)#sh
S1(config-if)# sw ac vlan 999
S1(config-if)#int Et0/1
S1(config-if)#sh
S1(config-if)# sw ac vlan 999
S1(config-if)#int Et0/2
S1(config-if)#sh
S1(config-if)# sw ac vlan 999
S1(config-if)#int Et0/3
S1(config-if)#sh
S1(config-if)# sw ac vlan 999
S1(config-if)#int Et1/0
S1(config-if)#sh
S1(config-if)# sw ac vlan 999
S1(config-if)#int Et1/1
S1(config-if)#sh
S1(config-if)# sw ac vlan 999
S1(config-if)#int Et1/2
S1(config-if)#sh
S1(config-if)# sw ac vlan 999
S1(config-if)#int Et1/3
S1(config-if)#sh
S1(config-if)# sw ac vlan 999
S1(config-if)#int Et2/0
S1(config-if)#sh
S1(config-if)# sw ac vlan 999
S1(config-if)#int Et2/1
S1(config-if)#sh
S1(config-if)# sw ac vlan 999
S1(config-if)#int Et2/2
S1(config-if)#sh
S1(config-if)# sw ac vlan 999
S1(config-if)#int Et2/3
S1(config-if)#sh
S1(config-if)# sw ac vlan 999
S1(config-if)#int Et3/0
S1(config-if)#sh
S1(config-if)# sw ac vlan 999
S1(config-if)#int Et3/1
S1(config-if)#sh
S1(config-if)# sw ac vlan 999
S1(config-if)#int Et3/2
S1(config-if)#sh
S1(config-if)# sw ac vlan 999
S1(config-if)#int Et3/3
S1(config-if)#sh
S1(config-if)# sw ac vlan 999
S1(config-if)#int Et4/0
S1(config-if)#sh
S1(config-if)# sw ac vlan 999
S1(config-if)#int Et4/1
S1(config-if)#sh
S1(config-if)# sw ac vlan 999
S1(config-if)#int Et4/2
S1(config-if)#sh
S1(config-if)# sw ac vlan 999
S1(config-if)#int Et4/3
S1(config-if)#sh
S1(config-if)# sw ac vlan 999
S1(config-if)#int Et5/1
S1(config-if)#sh
S1(config-if)# sw ac vlan 999
S1(config-if)#int Et5/2
S1(config-if)#sh
S1(config-if)# sw ac vlan 999
S1(config-if)#int Et5/3
S1(config-if)#sh
S1(config-if)# sw ac vlan 999
S1(config-if)#int Et6/0
S1(config-if)#sh
S1(config-if)# sw ac vlan 999
S1(config-if)#int Et6/2
S1(config-if)#sh
S1(config-if)# sw ac vlan 999
S1(config-if)#int Et6/3
S1(config-if)#sh
S1(config-if)# sw ac vlan 999
S1(config-if)#int Et7/0
S1(config-if)#sh
S1(config-if)# sw ac vlan 999
S1(config-if)#int Et7/1
S1(config-if)#sh
S1(config-if)# sw ac vlan 999
S1(config-if)#int Et7/2
S1(config-if)#sh
S1(config-if)# sw ac vlan 999
S1(config-if)#int Et7/3
S1(config-if)#sh
S1(config-if)# sw ac vlan 999
S1(config-if)#int Et8/0
S1(config-if)#sh
S1(config-if)# sw ac vlan 999
S1(config-if)#int Et8/1
S1(config-if)#sh
S1(config-if)# sw ac vlan 999
S1(config-if)#int Et8/2
S1(config-if)#sh
S1(config-if)# sw ac vlan 999
S1(config-if)#int Et8/3
S1(config-if)#sh
S1(config-if)# sw ac vlan 999
S1(config-if)#int Et9/0
S1(config-if)#sh
S1(config-if)# sw ac vlan 999
S1(config-if)#int Et9/1
S1(config-if)#sh
S1(config-if)# sw ac vlan 999
S1(config-if)#int Et9/2
S1(config-if)#sh
S1(config-if)# sw ac vlan 999
S1(config-if)#int Et9/3
S1(config-if)#sh
S1(config-if)# sw ac vlan 999
```
```
S2(config)#int Et0/0
S2(config-if)# sh
S2(config-if)#int Et0/1
S2(config-if)# sh
S2(config-if)#int Et0/2
S2(config-if)# sh
S2(config-if)#int Et0/3
S2(config-if)# sh
S2(config-if)#int Et1/0
S2(config-if)# sh
S2(config-if)#int Et1/1
S2(config-if)# sh
S2(config-if)#int Et1/2
S2(config-if)# sh
S2(config-if)#int Et1/3
S2(config-if)# sh
S2(config-if)#int Et2/0
S2(config-if)# sh
S2(config-if)#int Et2/1
S2(config-if)# sh
S2(config-if)#int Et2/2
S2(config-if)# sh
S2(config-if)#int Et2/3
S2(config-if)# sh
S2(config-if)#int Et3/0
S2(config-if)# sh
S2(config-if)#int Et3/1
S2(config-if)# sh
S2(config-if)#int Et3/2
S2(config-if)# sh
S2(config-if)#int Et3/3
S2(config-if)# sh
S2(config-if)#int Et4/0
S2(config-if)# sh
S2(config-if)#int Et4/1
S2(config-if)# sh
S2(config-if)#int Et4/2
S2(config-if)# sh
S2(config-if)#int Et4/3
S2(config-if)# sh
S2(config-if)#int Et5/1
S2(config-if)# sh
S2(config-if)#int Et5/2
S2(config-if)# sh
S2(config-if)#int Et5/3
S2(config-if)# sh
S2(config-if)#int Et6/0
S2(config-if)# sh
S2(config-if)#int Et6/1
S2(config-if)# sh
S2(config-if)#int Et6/2
S2(config-if)# sh
S2(config-if)#int Et6/3
S2(config-if)# sh
S2(config-if)#int Et7/0
S2(config-if)# sh
S2(config-if)#int Et7/1
S2(config-if)# sh
S2(config-if)#int Et7/2
S2(config-if)# sh
S2(config-if)#int Et7/3
S2(config-if)# sh
S2(config-if)#int Et8/0
S2(config-if)# sh
S2(config-if)#int Et8/2
S2(config-if)# sh
S2(config-if)#int Et8/3
S2(config-if)# sh
S2(config-if)#int Et9/0
S2(config-if)# sh
S2(config-if)#int Et9/1
S2(config-if)# sh
S2(config-if)#int Et9/2
S2(config-if)# sh
S2(config-if)#int Et9/3
S2(config-if)# sh
 
```

  </details>

<details>
 <summary>  Step 8: Assign VLANs to the correct switch interfaces.</summary>
 a.      Assign used ports to the appropriate VLAN (specified in the VLAN table above) and configure them for static access mode.
 
S2(config-if)#sw ac vlan 1
S1(config-if)#sw ac vlan 100


b.      Verify that the VLANs are assigned to the correct interfaces.
Why is interface F0/5 listed under VLAN 1?  
Потому что VLAN=1 используется по-умолчанию, а в задании пока не было информации, на что еще надо перенастраивать порт.
  </details>

<details>
  <summary> Step 9: Manually configure S1’s interface Et5/0 as an 802.1Q trunk. </summary>


a. Change the switchport mode on the interface to force trunking.
```
S1(config-if)#sw trunk encapsulation dot1q
S1(config-if)#sw mode tr
```

b. As a part of the trunk configuration, set the native VLAN to 1000.
```
S1(config-if)#sw tr nat vlan 1000
```

c. As another part of trunk configuration, specify that VLANs 100, 200, and 1000 are allowed to cross the trunk.
```
S1(config-if)#sw tr al vlan 100,200,1000
```

d. Save the running configuration to the startup configuration file.
```
S1(config-if)#do wr
Building configuration...
Compressed configuration from 3449 bytes to 1399 bytes[OK]
```
e. Verify trunking status.

```
S1#sh int tr

Port        Mode             Encapsulation  Status        Native vlan
Et5/0       on               802.1q         trunking      1000

Port        Vlans allowed on trunk
Et5/0       100,200,1000

Port        Vlans allowed and active in management domain
Et5/0       100,200,1000

Port        Vlans in spanning tree forwarding state and not pruned
Et5/0       100,200,1000
```

At this point, what IP address would the PC’s have if they were connected to the network using DHCP?
```
link-local IPv4   169.254.x.x/16
Но эмуляция хоста в eve-ng мне просто 0.0.0.0/0 показывает
```
 </details>




## Part 2: Configure and verify two DHCPv4 Servers on R1

<details>
  <summary>
   Step 1: Configure R1 with DHCPv4 pools for the two supported subnets. Only the DHCP Pool for subnet A is given below </summary>


a. Exclude the first five useable addresses from each address pool.
```

R1(config)#ip dhcp excluded-address 192.168.1.1
R1(config)#ip dhcp excluded-address 192.168.1.5
R1(config)#ip dhcp excluded-address 192.168.1.65
R1(config)#ip dhcp excluded-address 192.168.1.70

```
b. Create the DHCP pool (Use a unique name for each pool).
```
R1(config)#ip dhcp pool R1_clients-LAN
R1(dhcp-config)#ip dhcp pool R2_clients-LAN
```
c. Specify the network that this DHCP server is supporting.
```
R1(config)#ip dhcp pool R1_clients-LAN
R1(dhcp-config)#network 192.168.1.0 /26
R1(dhcp-config)#ip dhc pool R2_clients-LAN
R1(dhcp-config)#network 192.168.1.64 /27
```

d. Configure the domain name as ccna-lab.com
```
R1(config)#ip dhcp pool R1_clients-LAN
R1(dhcp-config)#domain-name ccna-lab.com
R1(dhcp-config)#ip dhcp pool R2_clients-LAN
R1(dhcp-config)#domain-name ccna-lab.com
```

e. Configure the appropriate default gateway for each DHCP pool.

```
R1(config)#ip dhcp pool R1_clients-LAN
R1(dhcp-config)#default-router 192.168.1.1
R1(dhcp-config)#ip dhcp pool R2_clients-LAN
R1(dhcp-config)#default-router 192.168.1.65
```

f. Configure the lease time for 2 days 12 hours and 30 minutes.
```
R1(config)#ip dhcp pool R1_clients-LAN
R1(dhcp-config)#lease 2 12 30
R1(dhcp-config)#ip dhcp pool R2_clients-LAN
R1(dhcp-config)#lease 2 12 30
```

g. Next, configure the second DHCPv4 Pool using the pool name R2_Client_LAN and the calculated network, default-router and use the same domain name and lease time from the previous DHCP pool.

#### Смысл конструкции "Only the DHCP Pool for subnet A is given below" можно только угадать, дойдя до конца инструкции. Аналогично про название пула для клиентов R1. Очень странная конструкция.   
Поэтому настройки проведены в рамках предыдущих шагов.
 </details>


<details>
  <summary>
  Step 2: Save your configuration    </summary>

```
R1#wr
Building configuration...
[OK]
```

 </details>
 

 <details>
  <summary> Step 3: Verify the DHCPv4 Server configuration  </summary>
  a. Issue the command **show ip dhcp pool** to examine the pool details.



```
R1#show ip dhcp pool

Pool R1_clients-LAN :
 Utilization mark (high/low)    : 100 / 0
 Subnet size (first/next)       : 0 / 0
 Total addresses                : 62
 Leased addresses               : 1
 Pending event                  : none
 1 subnet is currently in the pool :
 Current index        IP address range                    Leased addresses
 192.168.1.3          192.168.1.1      - 192.168.1.62      1

Pool R2_clients-LAN :
 Utilization mark (high/low)    : 100 / 0
 Subnet size (first/next)       : 0 / 0
 Total addresses                : 30
 Leased addresses               : 1
 Pending event                  : none
 1 subnet is currently in the pool :
 Current index        IP address range                    Leased addresses
 192.168.1.99         192.168.1.97     - 192.168.1.126     1
```


b. Issue the command **show ip dhcp bindings** to examine established DHCP address assignments.

неправильная команда 
```
R1#show ip dhcp bindings
                       ^
% Invalid input detected at '^' marker.

R1#show ip dhcp binding
Bindings from all pools not associated with VRF:
IP address          Client-ID/              Lease expiration        Type
                    Hardware address/
                    User name
```

c. Issue the command **show ip dhcp server** **statistics** to examine DHCP messages.

```
R1#show ip dhcp server statistics
Memory usage         25003
Address pools        2
Database agents      0
Automatic bindings   0
Manual bindings      0
Expired bindings     0
Malformed messages   0
Secure arp entries   0

Message              Received
BOOTREQUEST          0
DHCPDISCOVER         0
DHCPREQUEST          0
DHCPDECLINE          0
DHCPRELEASE          0
DHCPINFORM           0

Message              Sent
BOOTREPLY            0
DHCPOFFER            0
DHCPACK              0
DHCPNAK              0
R1#
```

 </details>
 
 <details>
  <summary>Step 4: Attempt to acquire an IP address from DHCP on PC-A</summary>
  a. Open a command prompt on PC-A and issue the command **ipconfig /renew**.


в моем случае команда другая
```
VPCS> ip dhcp -x

VPCS> ip dhcp -r
DDORA IP 192.168.1.2/26 GW 192.168.1.1
```

b. Once the renewal process is complete, issue the command **ipconfig** to view the new IP information.

```
VPCS> show ip

NAME        : VPCS[1]
IP/MASK     : 192.168.1.2/26
GATEWAY     : 192.168.1.1
DNS         :
DHCP SERVER : 192.168.1.1
DHCP LEASE  : 217768, 217800/108900/190575
DOMAIN NAME : ccna-lab.com
MAC         : 00:50:79:66:68:05
LPORT       : 20000
RHOST:PORT  : 127.0.0.1:30000
MTU         : 1500

```
c. Test connectivity by pinging R1’s G0/0/1 interface IP address.
```
VPCS> ping 192.168.1.1

84 bytes from 192.168.1.1 icmp_seq=1 ttl=255 time=0.374 ms
84 bytes from 192.168.1.1 icmp_seq=2 ttl=255 time=0.539 ms
84 bytes from 192.168.1.1 icmp_seq=3 ttl=255 time=0.587 ms
```
   </details>

## Part 3: Configure and verify two DHCPv4 Servers on R1

 <details><summary>
Step 1: Configure R2 as a DHCP relay agent for the LAN on E0/1</summary>
a. Configure the **ip helper-address** command on E0/1 specifying R1’s E0/0 IP address.

```
R2(config)#int et0/1
R2(config-if)#ip helper-address 10.0.0.1
```

b. Save your configuration.

```
R2#wr
Building configuration...
[OK]
```

   </details>

 <details><summary>
Step 2: Attempt to acquire an IP address from DHCP on PC-B </summary>



a. Open a command prompt on PC-B and issue the command **ipconfig /renew**.
```
VPCS> dhcp -x

VPCS> dhcp -r
DDORA IP 192.168.1.98/27 GW 192.168.1.97

```
b.      Once the renewal process is complete, issue the command **ipconfig** to view the new IP information.
```
VPCS> show ip

NAME        : VPCS[1]
IP/MASK     : 192.168.1.98/27
GATEWAY     : 192.168.1.97
DNS         :
DHCP SERVER : 10.0.0.1
DHCP LEASE  : 217675, 217800/108900/190575
MAC         : 00:50:79:66:68:06
LPORT       : 20000
RHOST:PORT  : 127.0.0.1:30000
MTU         : 1500

```

c. Test connectivity by pinging R1’s E0/1 interface IP address.

```
VPCS> ping 192.168.0.1

*10.0.0.1 icmp_seq=1 ttl=254 time=5.702 ms (ICMP type:11, code:0, TTL expired in transit)
*10.0.0.1 icmp_seq=2 ttl=254 time=6.582 ms (ICMP type:11, code:0, TTL expired in transit)
*10.0.0.1 icmp_seq=3 ttl=254 time=6.016 ms (ICMP type:11, code:0, TTL expired in transit)

```

d. Issue the **show ip dhcp binding** on R1 to verify DHCP bindings.
```

R1#show ip dhcp binding
Bindings from all pools not associated with VRF:
IP address          Client-ID/              Lease expiration        Type
                    Hardware address/
                    User name
192.168.1.2         0100.5079.6668.05       Nov 19 2024 09:15 PM    Automatic
192.168.1.98        0100.5079.6668.06       Nov 19 2024 09:15 PM    Automatic

```

e. Issue the **show ip dhcp server statistics** on R1 and R2 to verify DHCP messages.


```

R1#show ip dhcp binding
Bindings from all pools not associated with VRF:
IP address          Client-ID/              Lease expiration        Type
                    Hardware address/
                    User name
192.168.1.2         0100.5079.6668.05       Nov 19 2024 09:15 PM    Automatic
192.168.1.98        0100.5079.6668.06       Nov 19 2024 09:15 PM    Automatic
R1#show ip dhcp server statistics
Memory usage         41487
Address pools        2
Database agents      0
Automatic bindings   2
Manual bindings      0
Expired bindings     0
Malformed messages   0
Secure arp entries   0

Message              Received
BOOTREQUEST          0
DHCPDISCOVER         4
DHCPREQUEST          2
DHCPDECLINE          0
DHCPRELEASE          0
DHCPINFORM           0

Message              Sent
BOOTREPLY            0
DHCPOFFER            2
DHCPACK              2
DHCPNAK              0




R2#show ip dhcp server statistics
Memory usage         22429
Address pools        0
Database agents      0
Automatic bindings   0
Manual bindings      0
Expired bindings     0
Malformed messages   0
Secure arp entries   0

Message              Received
BOOTREQUEST          0
DHCPDISCOVER         0
DHCPREQUEST          0
DHCPDECLINE          0
DHCPRELEASE          0
DHCPINFORM           0

Message              Sent
BOOTREPLY            0
DHCPOFFER            0
DHCPACK              0
DHCPNAK              0

```

