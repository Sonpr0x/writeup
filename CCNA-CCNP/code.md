Description: logging config ccna lab project.
Index:

- Step 1: Basic setup
- Step 2: Create VLANs & Ports assignment
- Step 3: Truking & Inter-VLANs routing & HSRP
- Step 4: DHCP + Load balancing DHCP
- Step 5: Routing OSPF
- Step 6: Load blancing HSRP
- Step 7: NAT
- Step 8: PVST+ load balancing
- Step 9: Security access layer
- Step 10: Security network devices
- Step 11: ACL setup
...


### Step 1: Basic setup
> shutdown all interfaces
`R1, R2, SW0, SW1, SW2, CoreSW1, CoreSW2`
int range interface
shutdown
> basic setup devices
`R1, R2, SW0, SW1, SW2, CoreSW1, CoreSW2`
hostname
ip domain name !!!!
no ip domain lookup
service password-encryption	
logging synch

### Step 2: Create VLANs & Ports assignment

`SW0, SW1, SW2`
vlan 10
name Guess
vlan 12
name Officer
vlan 40
name Management
vlan 50
name Server
vlan 80
name IT
vlan 99
name Chef
vlan 100
name Store
vlan 101
name Native

`SW0`
intface f0/2
switchport vlan 20
intface g0/2
switchport vlan 10

`SW1`
intface f0/4
switchport vlan 22
intface f0/2
switchport vlan 80
intface g0/2
switchport vlan 10
intface f0/23
switchport vlan 50
intface f0/24
switchport vlan 50

`SW2` 

intface f0/5
switchport vlan 22
intface f0/2
switchport vlan 99
intface g0/2
switchport vlan 12

### Step 3: Truking & Inter-VLANs routing & HSRP CoreSW

> Trunking

`SW0`
int range f0/1,f0/4
switchport mode trunk
switchport trunk native vlan 101
switchport trunk allowed vlan 10,12,20,21,22,40,50,80,99,101
no shut

`SW1`
int range g0/1,f0/3
switchport mode trunk
switchport trunk native vlan 101
switchport trunk allowed vlan 10,12,20,21,22,40,50,80,99,101
no shut

`SW2`
int range f0/1,f0/3
switchport mode trunk
switchport trunk native vlan 101
switchport trunk allowed vlan 10,12,20,21,22,40,50,80,99,101
no shut

`CoreSW1`
int range f0/10-11
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk native vlan 101
switchport trunk allowed vlan 10,12,20,21,22,40,50,80,99,101
no shut

`CoreSW2`
int range f0/4-5
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk native vlan 101
switchport trunk allowed vlan 10,12,20,21,22,40,50,80,99,101
no shut

> Inter-VLANs Routing

`CoreSW1, CoreSW2`

interface vlan 10
ip add 192.168.10.1 255.255.255.0
interface vlan 12
ip add 192.168.12.1 255.255.255.0
interface vlan 20
ip add 192.168.20.1 255.255.255.0
interface vlan 40
ip add 192.168.40.1 255.255.255.0
interface vlan 50
ip add 192.168.50.1 255.255.255.0
interface vlan 80
ip add 192.168.80.1 255.255.255.0
interface vlan 99
ip add 192.168.99.1 255.255.255.0

ip routing

> HSRP

`CoreSW1`

interface vlan 10
ip add 192.168.10.3 255.255.255.0
standby 1 ip 192.168.10.1
standby 1 priority 110
standby 1 preetemp
interface vlan 12
ip add 192.168.12.3 255.255.255.0
standby 1 ip 192.168.12.1
standby 1 priority 110
standby 1 preetemp
interface vlan 20
ip add 192.168.20.3 255.255.255.0
standby 1 ip 192.168.20.1
standby 1 priority 110
standby 1 preetemp
interface vlan 40
ip add 192.168.40.3 255.255.255.0
standby 1 ip 192.168.40.1
standby 1 priority 110
standby 1 preetemp
interface vlan 50
ip add 192.168.50.3 255.255.255.0
standby 1 ip 192.168.50.1
standby 1 priority 110
standby 1 preetemp
interface vlan 80
ip add 192.168.80.3 255.255.255.0
standby 1 ip 192.168.80.1
standby 1 priority 110
standby 1 preetemp
interface vlan 99
ip add 192.168.99.3 255.255.255.0
standby 1 ip 192.168.99.1
standby 1 priority 110
standby 1 preetemp

`CoreSW2`

interface vlan 10
ip add 192.168.10.2 255.255.255.0
standby 1 ip 192.168.10.1
standby 1 preetemp
interface vlan 12
ip add 192.168.12.2 255.255.255.0
standby 1 ip 192.168.12.1
standby 1 preetemp
interface vlan 20
ip add 192.168.20.2 255.255.255.0
standby 1 ip 192.168.20.1
standby 1 preetemp
interface vlan 40
ip add 192.168.40.2 255.255.255.0
standby 1 ip 192.168.40.1
standby 1 preetemp
interface vlan 50
ip add 192.168.50.2 255.255.255.0
standby 1 ip 192.168.50.1
standby 1 preetemp
interface vlan 80
ip add 192.168.80.2 255.255.255.0
standby 1 ip 192.168.80.1
standby 1 preetemp
interface vlan 99
ip add 192.168.99.2 255.255.255.0
standby 1 ip 192.168.99.1
standby 1 preetemp

### Step 4: DHCP + Load balancing DHCP

`CoreSW1`
ip dhcp pool vPool10
network 192.168.10.0 255.255.255.0
default-router 192.168.1.1 255.255.255.0
dns-server 8.8.8.8
ip dhcp pool vPool20
network 192.168.20.0 255.255.255.0
default-router 192.168.20.1 255.255.255.0
dns-server 8.8.8.8
ip dhcp pool vPool21
network 192.168.21.0 255.255.255.0
default-router 192.168.21.1 255.255.255.0
dns-server 8.8.8.8
ip dhcp pool vPool22
network 192.168.22.0 255.255.255.0
default-router 192.168.22.1 255.255.255.0
dns-server 8.8.8.8

`CoreSW2`
ip dhcp pool vPool12
network 192.168.12.0 255.255.255.0
default-router 192.168.12.1 255.255.255.0
dns-server 8.8.8.8
ip dhcp pool vPool50
network 192.168.50.0 255.255.255.0
default-router 192.168.50.1 255.255.255.0
dns-server 8.8.8.8
ip dhcp pool vPool80
network 192.168.80.0 255.255.255.0
default-router 192.168.80.1 255.255.255.0
dns-server 8.8.8.8
ip dhcp pool vPool99
network 192.168.99.0 255.255.255.0
default-router 192.168.99.1 255.255.255.0
dns-server 8.8.8.8

### Step 5: Routing OSPF

`CoreSW1`
ip route ospf 1
network 192.168.0.0 0.0.255.255 area 0
network 172.16.1.0 0.0.0.255 area 0
network 172.16.2.0 0.0.0.255 area 0

`CoreSW2`
ip route ospf 1
network 192.168.0.0 0.0.255.255 area 0
network 172.16.3.0 0.0.0.255 area 0
network 172.16.4.0 0.0.0.255 area 0

`R1`
ip route ospf 1
network 100.0.0.0 0.0.0.7 area 0
network 172.16.1.0 0.0.0.255 area 0
network 172.16.4.0 0.0.0.255 area 0

`R2`
ip route ospf 1
network 101.0.0.0 0.0.0.3 area 0
network 172.16.2.0 0.0.0.255 area 0
network 172.16.3.0 0.0.0.255 area 0

### Step 6: Load blancing HSRP

`CoreSW1`
interface vlan 10
standby 2 ip 192.168.10.4
standby 2 preetemp
interface vlan 12
standby 2 ip 192.168.12.4
standby 2 preetemp
interface vlan 20
standby 2 ip 192.168.20.4
standby 2 preetemp
interface vlan 40
standby 2 ip 192.168.40.4
standby 2 preetemp
interface vlan 50
standby 2 ip 192.168.50.4
standby 2 preetemp
interface vlan 80
standby 2 ip 192.168.80.4
standby 2 preetemp
interface vlan 99
standby 2 ip 192.168.99.4
standby 2 preetemp

`CoreSW2`
interface vlan 10
standby 2 ip 192.168.10.4
standby 2 priority 110
standby 2 preetemp
interface vlan 12
standby 2 ip 192.168.12.4
standby 2 priority 110
standby 2 preetemp
interface vlan 20
standby 2 ip 192.168.20.4
standby 2 priority 110
standby 2 preetemp
interface vlan 40
standby 2 ip 192.168.40.4
standby 2 priority 110
standby 2 preetemp
interface vlan 50
standby 2 ip 192.168.50.4
standby 2 priority 110
standby 2 preetemp
interface vlan 80
standby 2 ip 192.168.80.4
standby 2 priority 110
standby 2 preetemp
interface vlan 99
standby 2 ip 192.168.99.4
standby 2 priority 110
standby 2 preetemp

### Step 7: NAT

> Static NAT + NAT overload

`R1`

ip nat inside source static tcp 192.168.50.11 80 100.0.0.3 80
access-list 1 permit 192.168.0.0 0.0.0.255
ip nat inside source list 1 interface s0/0/0 overload
int s0/0/0
ip nat outside
int g0/0
ip nat inside
int g0/1
ip nat inside

`R2`

ip nat inside source static tcp 192.168.50.11 80 101.0.0.3 80
access-list 1 permit 192.168.0.0 0.0.0.255
ip nat inside source list 1 interface s0/0/1 overload
int s0/0/1
ip nat outside
int g0/0
ip nat inside
int g0/1
ip nat inside

### Step 8: PVST+ load balancing

`SW0`
spanning-tree vlan 10,20 root primary
`SW1`
spanning-tree vlan 21,22,50,80 root primary
`SW2`
spanning-tree vlan 12,99 root primary

### Step 9: Security access layer

> Port security
`SW0, SW1, SW2`
int range all access port
switchport port-security	

> mitigate STP
`SW0, SW1, SW2`
int range <all access port>
switchport port-security
spanning tree portfast
spanning-tree bpduguard enable

> mitigate ARP attack
`SW0, SW1, SW2`
ip dhcp snooping
ip dhcp snooping <id vlan access>
ip arp inspection <id vlan access>
int range <trust-port> (port offer DHCP)
ip dhcp snooping trust <trust-port>
ip arp inspection trust <trust-port>

> mitigate CDP attack
`SW0, SW1, SW2`
int range <all access port>
no cdp enable (port)

> mitigate DHCP attack
`SW0, SW1, SW2`
ip dhcp snooping
ip dhcp snooping <id vlan access>
int range <trust-port> (port offer DHCP)
ip dhcp snooping trust <trust-port>
ip dhcp snooping limit rate 6 (port)

> mitigate vlans attack
`SW0, SW1, SW2, CoreSW1, CoreSW2`
int range <unused-port>
switchport access vlan 100
int range <trunk-port>
switchport mode trunk
switchport nonegotiate

### Step 10: Security network devices
`SW1, SW2, S3, R1, R2`
enable secret
ip domain name
service password-encryption
exec-timeout 3
login block-for 

> SSH
username secret
line vty 0 1
transport input ssh
### step 11: ACL
