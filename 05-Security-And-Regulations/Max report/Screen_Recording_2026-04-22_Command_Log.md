# Screen Recording 2026-04-22 Command Log Reconstruction

Source files used:

- `Screen Recording 2026-04-22 110207.mp4`
- `Screen Recording 2026-04-22 112214.mp4`
- `viewable logical topology.png`
- `Network Design Main final version.pkt`
- `Phase_2_Packet_Tracer_Network_Report.md`

Important note: the video is a scrolling Packet Tracer command-history table. I extracted frames from the video and OCR-read the normalized command column. Some OCR text was corrected manually using the visible frames and the topology/report. The `.pkt` file did not expose readable plain-text running configs, so this is a reconstruction from the recording, not a direct export from Packet Tracer.

Coverage marker: this reconstruction now covers visible command-history rows from `1` through `2957`. Rows `1` through `433` came from `Screen Recording 2026-04-22 110207.mp4`; rows `433` through `2957` came from `Screen Recording 2026-04-22 112214.mp4`. The second recording overlaps at row `433`. In the continuation section, repeated `enable`, `configure terminal`, `end`, `write memory`, repeated `show`, and repeated `ping` commands are intentionally summarized instead of copied every time.

## Devices Seen In The Topology

| Device | Role shown in topology |
| --- | --- |
| `Switch-L3-CORE` / `3650-24PS Switch-L3-CORE` | Core Layer 3 switch, central routing/VLAN point |
| `ROUTER A` / `ISR4331 ROUTER A` | Edge router between core/internal network and ISP |
| `ISP` / `ISR4331 ISP` | Simulated internet/upstream router |
| `Switch-L2-A` | Access switch for Study/Management side |
| `Switch-L2-B` | Access switch for Production side |
| `Switch-L2-C` | Access switch for Support A side |
| `Switch-L2-D` | Access switch for Support B side |
| `DMZ-Switch` | DMZ access switch for HTTP server |
| `DNS`, `DHCP`, `RADIUS`, `FTP`, `HTTP` servers | Internal/DMZ services |
| `MFD` printer | Printer endpoint |
| `STUDY-PC-*`, `PROD-PC-*`, `SUPA-PC-*`, `SUPB-PC-*` | End-user PCs |

## Reconstructed Commands By Device

### `Switch-L3-CORE`

Initial setup:

```ios
enable
configure terminal
hostname Switch-L3-CORE
```

Routed uplink toward `ROUTER A`:

```ios
enable
configure terminal
interface GigabitEthernet1/0/1
no switchport
ip address 192.168.10.194 255.255.255.252
no shutdown
end
write memory
ping 192.168.10.193
```

VLAN database:

```ios
configure terminal
vlan 10
name Management_Secretariat
vlan 20
name Study
vlan 30
name Production
vlan 40
name Support_A
vlan 50
name Support_B
vlan 60
name DMZ_Network
vlan 70
name General_Internal_Servers
vlan 80
name iSCSI_Network
vlan 99
name Management_Devices
end
write memory
```

SVI/gateway configuration visible in the recording:

```ios
configure terminal
interface Vlan10
ip address 192.168.10.1 255.255.255.240
no shutdown

interface Vlan20
ip address 192.168.10.17 255.255.255.240
no shutdown

interface Vlan30
ip address 192.168.10.33 255.255.255.224
no shutdown

interface Vlan40
ip address 192.168.10.65 255.255.255.240
no shutdown

interface Vlan50
ip address 192.168.10.81 255.255.255.240
no shutdown

interface Vlan60
ip address 192.168.10.97 255.255.255.240
no shutdown

interface Vlan70
ip address 192.168.10.113 255.255.255.240
no shutdown

interface Vlan80
ip address 192.168.10.129 255.255.255.240
no shutdown

interface Vlan99
ip address 192.168.10.177 255.255.255.240
no shutdown
end
write memory
```

Trunk/access-port work shown on the core:

```ios
configure terminal
interface GigabitEthernet1/0/2
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40,50,60,70,80,99
no shutdown
end
write memory

configure terminal
interface GigabitEthernet1/0/3
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40,50,60,70,80,99
no shutdown
end
write memory

configure terminal
interface GigabitEthernet1/0/4
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40,50,60,70,80,99
no shutdown
end
write memory

configure terminal
interface GigabitEthernet1/0/5
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40,50,60,70,80,99
no shutdown
end
write memory
```

Later correction/experimentation on `GigabitEthernet1/0/2`:

```ios
configure terminal
interface GigabitEthernet1/0/2
switchport mode access
switchport access vlan 10
no shutdown
end
write memory

configure terminal
interface GigabitEthernet1/0/2
default interface GigabitEthernet1/0/2
interface GigabitEthernet1/0/2
switchport mode access
switchport access vlan 10
no shutdown
end
write memory
```

Routing and spanning-tree commands:

```ios
configure terminal
ip routing
end
write memory

configure terminal
ip route 0.0.0.0 0.0.0.0 203.0.113.2
end
write memory

configure terminal
ip route 0.0.0.0 0.0.0.0 192.168.10.193
end
write memory

show spanning-tree interface
show spanning-tree
configure terminal
spanning-tree portfast
end
write memory

configure terminal
spanning-tree vlan 10 root primary
end
write memory
```

Verification commands seen:

```ios
show ip interface brief
show running-config
show ip route static
```

VLAN 10 mask correction attempts shown late in the video:

```ios
configure terminal
interface Vlan10
no ip address 192.168.10.1 255.255.255.240
ip address 192.168.10.1 255.255.255.0
no shutdown
end
write memory

configure terminal
interface Vlan10
no ip address 192.168.10.1 255.255.255.0
ip address 192.168.10.1 255.255.255.240
no shutdown
end
write memory
```

### `ROUTER A`

Initial edge/internal interface setup:

```ios
enable
configure terminal
interface GigabitEthernet0/0/0
ip address 203.0.113.1 255.255.255.252
no shutdown

interface GigabitEthernet0/0/1
ip address 192.168.10.193 255.255.255.252
no shutdown
end
write memory
```

Default route and NAT setup:

```ios
configure terminal
ip route 0.0.0.0 0.0.0.0 203.0.113.2
end
write memory

configure terminal
interface GigabitEthernet0/0/1
ip nat inside
exit
interface GigabitEthernet0/0/0
ip nat outside
exit
access-list 1 permit 192.168.10.0 0.0.0.255
ip nat inside source list 1 interface GigabitEthernet0/0/0 overload
end
write memory
```

Troubleshooting/correction commands visible:

```ios
show ip interface brief
show running-config | section nat
show running-config | section access-list

configure terminal
no ip nat inside
no ip nat outside
exit

configure terminal
interface GigabitEthernet0/0/1
no ip address 192.168.10.193 255.255.255.252
ip address 192.168.10.193 255.255.255.0
exit
write memory

configure terminal
no ip nat inside source static 203.0.113.2 8.8.8.8
end
write memory
```

Static routes back to the internal VLANs, using the core-side routed link as next hop. The OCR cuts off the final octets, but the topology indicates the next hop should be `192.168.10.194`:

```ios
configure terminal
interface GigabitEthernet0/0/1
ip nat inside
exit

no ip route 192.168.10.0 255.255.255.0 192.168.10.194
ip route 192.168.10.0 255.255.255.240 192.168.10.194
ip route 192.168.10.16 255.255.255.240 192.168.10.194
ip route 192.168.10.32 255.255.255.224 192.168.10.194
ip route 192.168.10.64 255.255.255.240 192.168.10.194
ip route 192.168.10.80 255.255.255.240 192.168.10.194
ip route 192.168.10.96 255.255.255.240 192.168.10.194
ip route 192.168.10.112 255.255.255.240 192.168.10.194
ip route 192.168.10.128 255.255.255.240 192.168.10.194
ip route 192.168.10.176 255.255.255.240 192.168.10.194
end
write memory
```

Connectivity checks:

```ios
ping 203.0.113.2
ping 8.8.8.8
show ip interface brief
show running-config
show ip route static
show interfaces status
```

### `ISP`

WAN interface setup:

```ios
enable
configure terminal
interface GigabitEthernet0/0/0
ip address 203.0.113.2 255.255.255.252
no shutdown
exit
ip route 8.8.8.8 255.255.255.255 Null0
end
write memory
```

Loopback/internet-test address:

```ios
enable
configure terminal
interface Loopback0
ip address 8.8.8.8 255.255.255.255
no shutdown
exit
```

Connectivity checks:

```ios
ping 8.8.8.8
show ip interface brief
```

### `Switch-L2-A`

Trunk to the core:

```ios
enable
configure terminal
interface FastEthernet0/1
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40,50,60,70,80,99
no shutdown
end
write memory
```

Access port work visible for VLAN 10:

```ios
enable
configure terminal
interface FastEthernet0/2
switchport mode access
switchport access vlan 10
no shutdown
end
write memory

enable
configure terminal
interface FastEthernet0/2
switchport mode access
switchport access vlan 10
spanning-tree portfast
no shutdown
exit
interface FastEthernet0/1
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40,50,60,70,80,99
no shutdown
exit
write memory
```

VLAN database also appears to have been repeated on this access switch:

```ios
enable
configure terminal
vlan 10
name Management_Secretariat
vlan 20
name Study
vlan 30
name Production
vlan 40
name Support_A
vlan 50
name Support_B
vlan 60
name DMZ_Network
vlan 70
name General_Internal_Servers
vlan 80
name iSCSI_Network
vlan 99
name Management_Devices
end
write memory
```

### `Switch-L2-B`

Trunk to the core:

```ios
enable
configure terminal
interface FastEthernet0/1
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40,50,60,70,80,99
no shutdown
end
write memory
```

### `Switch-L2-C`

Trunk to the core:

```ios
enable
configure terminal
interface FastEthernet0/1
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40,50,60,70,80,99
no shutdown
end
write memory
```

### `Switch-L2-D`

Trunk to the core:

```ios
enable
configure terminal
interface FastEthernet0/1
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40,50,60,70,80,99
no shutdown
end
write memory
```

## Continuation From Second Recording: Rows `434` Through `2957`

The second recording starts at row `433` again and continues to row `2957`. It appears to replace/correct much of the earlier addressing plan. The important change is that the design moves from the earlier mixed `/28` plan to a cleaner VLAN plan using mostly `/27` networks, a `/29` storage network, and a routed core-to-router link using `192.168.10.249/30` and `192.168.10.250/30`.

### Commands Intentionally Condensed

I did not list every repeated instance of these:

```ios
enable
configure terminal
end
write memory
copy running-config startup-config
show running-config
show ip interface brief
show vlan brief
show ip route
show interfaces trunk
ping ...
```

They appear throughout the second recording as mode changes, saves, and checks.

### `Switch-L3-CORE`: Old VLAN/SVI Cleanup

The recording shows removal of the earlier VLAN/SVI layout and old routed-link/default-route settings:

```ios
no interface Vlan10
no vlan 10
no interface Vlan20
no vlan 20
no interface Vlan30
no vlan 30
no interface Vlan40
no vlan 40
no interface Vlan50
no vlan 50
no interface Vlan60
no vlan 60
no interface Vlan70
no vlan 70
no interface Vlan80
no vlan 80
no interface Vlan99
no vlan 99

interface GigabitEthernet1/0/1
no ip address 192.168.10.194 255.255.255.252

no ip route 0.0.0.0 0.0.0.0 192.168.10.193
```

### `Switch-L3-CORE`: Revised VLANs And SVIs

```ios
vlan 10
name Management_Study
vlan 20
name Production
vlan 30
name Support_A
vlan 40
name Support_B
vlan 60
name DMZ_Network
vlan 70
name Internal_Servers
vlan 80
name iSCSI_Network
vlan 99
name Network_Management
```

```ios
interface Vlan10
description Default Gateway for Management & Study VLAN
ip address 192.168.10.1 255.255.255.224
no shutdown

interface Vlan20
description Default Gateway for Production VLAN
ip address 192.168.10.33 255.255.255.224
no shutdown

interface Vlan30
description Default Gateway for Support A VLAN
ip address 192.168.10.65 255.255.255.224
no shutdown

interface Vlan40
description Default Gateway for Support B VLAN
ip address 192.168.10.97 255.255.255.224
no shutdown

interface Vlan60
description Default Gateway for DMZ Network VLAN
ip address 192.168.10.193 255.255.255.224
no shutdown

interface Vlan70
description Default Gateway for Internal Servers VLAN
ip address 192.168.10.129 255.255.255.224
no shutdown

interface Vlan80
description Default Gateway for iSCSI Network VLAN
ip address 192.168.10.225 255.255.255.248
no shutdown

interface Vlan99
description Default Gateway for Network Management VLAN
ip address 192.168.10.161 255.255.255.224
no shutdown
```

Core routed link and default route:

```ios
interface GigabitEthernet1/0/1
no switchport
ip address 192.168.10.250 255.255.255.252
no shutdown

ip route 0.0.0.0 0.0.0.0 192.168.10.249
```

### `ROUTER A`: Revised Edge/Internal Addressing

The router is reworked away from `203.0.113.1/30` and the old inside `192.168.10.193/30` link:

```ios
interface GigabitEthernet0/0/0
no ip address 203.0.113.1 255.255.255.252
ip address 10.0.0.1 255.255.255.252
ip nat outside
no shutdown

interface GigabitEthernet0/0/1
no ip address 192.168.10.193 255.255.255.252
ip address 192.168.10.249 255.255.255.252
ip nat inside
no shutdown

no ip route 0.0.0.0 0.0.0.0 203.0.113.2
ip route 0.0.0.0 0.0.0.0 10.0.0.2
```

Static routes back to internal VLANs via the core:

```ios
ip route 192.168.10.0 255.255.255.224 192.168.10.250
ip route 192.168.10.32 255.255.255.224 192.168.10.250
ip route 192.168.10.64 255.255.255.224 192.168.10.250
ip route 192.168.10.96 255.255.255.224 192.168.10.250
ip route 192.168.10.128 255.255.255.224 192.168.10.250
ip route 192.168.10.160 255.255.255.224 192.168.10.250
ip route 192.168.10.192 255.255.255.224 192.168.10.250
ip route 192.168.10.224 255.255.255.248 192.168.10.250
```

### Access Switches: Management, Trunks, Access Ports

The second recording configures management SVI/default-gateway and trunks/access ports on the L2 switches. The pattern repeats per switch.

`Switch-L2-A`:

```ios
vlan 10
name Management_Study
vlan 20
name Production
vlan 99
name Network_Management

interface Vlan99
description Management IP for Switch-L2-A
ip address 192.168.10.163 255.255.255.224
no shutdown
ip default-gateway 192.168.10.161

interface FastEthernet0/1
switchport mode trunk
switchport trunk allowed vlan 10,20,99
switchport nonegotiate
no shutdown
```

`Switch-L2-B`:

```ios
vlan 10
name Management_Study
vlan 20
name Production
vlan 99
name Network_Management

interface Vlan99
description Management IP for Switch-L2-B
ip address 192.168.10.164 255.255.255.224
no shutdown
ip default-gateway 192.168.10.161

interface FastEthernet0/1
description Uplink to Switch-L3-CORE Gig1/0/3
switchport mode trunk
switchport trunk allowed vlan 10,20,99
switchport nonegotiate
no shutdown
```

Access port pattern shown repeatedly:

```ios
interface FastEthernet0/2
description Connected to PC in Management_Study (VLAN 10)
switchport mode access
switchport access vlan 10
switchport port-security
switchport port-security maximum 1
switchport port-security violation restrict
spanning-tree portfast
no shutdown

interface FastEthernet0/3
description Connected to PC in Production (VLAN 20)
switchport mode access
switchport access vlan 20
switchport port-security
switchport port-security maximum 1
switchport port-security violation restrict
spanning-tree portfast
no shutdown
```

Bulk access-port pattern also appears:

```ios
interface range FastEthernet0/2 - 14
description Access Port for VLAN 10 PC
switchport mode access
switchport access vlan 10
switchport port-security
switchport port-security maximum 1
switchport port-security violation restrict
spanning-tree portfast
no shutdown
```

Production range pattern:

```ios
interface range FastEthernet0/2 - 11
description Access Port for VLAN 20 PC
switchport mode access
switchport access vlan 20
switchport port-security
switchport port-security maximum 1
switchport port-security violation restrict
spanning-tree portfast
no shutdown
```

Printer port:

```ios
interface FastEthernet0/12
description Printer Port - VLAN 50
switchport mode access
switchport access vlan 50
switchport port-security
switchport port-security maximum 1
switchport port-security violation restrict
spanning-tree portfast
no shutdown
```

### DMZ Switch

The DMZ switch is configured with VLAN 60 management and logging/time settings:

```ios
vlan 60
name DMZ_VLAN

interface Vlan60
ip address 192.168.10.194 255.255.255.224
no shutdown

ip default-gateway 192.168.10.193

logging 192.168.10.130
service timestamps log datetime msec
ntp server 192.168.10.130
```

DMZ trunking is corrected on both sides:

```ios
interface GigabitEthernet0/1
description Uplink to L3-CORE-SW
switchport mode trunk
switchport trunk allowed vlan add 60
switchport trunk allowed vlan add 99
switchport trunk native vlan 99
no shutdown
```

Core-side DMZ trunk:

```ios
interface GigabitEthernet1/0/10
description Link to DMZ-L2-Switch
switchport mode trunk
switchport trunk native vlan 99
switchport trunk allowed vlan add 60
switchport trunk allowed vlan add 99
no shutdown
```

### Server/Service Ports

The recording shows at least the RADIUS server port being corrected:

```ios
interface GigabitEthernet1/0/7
description Access Port for RADIUS Server
switchport mode access
switchport access vlan 70
no shutdown
```

DHCP helper address appears later:

```ios
ip helper-address 192.168.10.131
```

### AAA, SSH, Console, And Password Hardening Attempts

AAA/RADIUS is attempted, then simplified to local login:

```ios
aaa new-model
no radius server RADIUS
no aaa authentication login default
no aaa authorization exec default
no username admin

radius server RADIUS
address ipv4 192.168.10.132
key cisco123
exit

no radius server RADIUS_SERVER_GROUP_1
aaa new-model
aaa authentication login default local

username admin privilege 15 secret admin
line vty 0 15
login authentication default
transport input ssh
```

SSH/domain/key work:

```ios
ip domain-name mynetwork.local
crypto key generate rsa
```

Router/console hardening examples also appear:

```ios
hostname RouterA
ip domain-name cisco.com
crypto key generate rsa

line console 0
password cisco
login
exec-timeout 5 0
logging synchronous

line vty 0 4
password cisco
login local
exec-timeout 5 0
transport input ssh

login block-for 120 attempts 5 within 60

username admin secret cisco
username admin secret 123456789

security passwords min-length 9
enable secret 123456789
service password-encryption
```

Weak passwords such as `cisco`, `admin`, `12345`, `0000`, and `123456789` appear in the command log and should be treated as lab/demo values, not acceptable production passwords.

### Logging And Time

```ios
logging 192.168.10.130
ntp server 192.168.10.130
service timestamps log datetime msec
show ntp status
```

### CoPP / Control Plane Policy Attempt

The recording contains an attempted CoPP-style policy:

```ios
mls qos

ip access-list extended OSPF_COPP
permit ospf any any

ip access-list extended EIGRP_COPP
permit eigrp any any

ip access-list extended MANAGEMENT_COPP
permit tcp any any eq 22
permit udp any any eq 161

class-map match-all OSPF_CLASS
match access-group name OSPF_COPP

class-map match-all EIGRP_CLASS
match access-group name EIGRP_COPP

class-map match-all MANAGEMENT_CLASS
match access-group name MANAGEMENT_COPP

policy-map COPP_POLICY
class OSPF_CLASS
class EIGRP_CLASS
class MANAGEMENT_CLASS
class class-default

service-policy input
```

Packet Tracer support for this should be verified because some commands may not fully apply on the simulated platform.

### ACLs: Internet, DMZ, Servers, Departments

Internet inbound ACL:

```ios
ip access-list extended ACL_INTERNET_IN
remark Inbound traffic from Internet
permit tcp any host 192.168.10.195 eq 80
permit tcp any host 192.168.10.195 eq 443
permit tcp any any established
permit udp any any eq 53
deny ip any any

interface GigabitEthernet0/0/0
ip access-group ACL_INTERNET_IN in
```

DMZ ACL summary:

```ios
ip access-list extended ACL_DMZ_IN
remark Controls traffic from DMZ
deny ip 192.168.10.192 0.0.0.31 192.168.10.0 0.0.0.31
deny ip 192.168.10.192 0.0.0.31 192.168.10.32 0.0.0.31
deny ip 192.168.10.192 0.0.0.31 192.168.10.64 0.0.0.31
deny ip 192.168.10.192 0.0.0.31 192.168.10.96 0.0.0.31
deny ip 192.168.10.192 0.0.0.31 host 192.168.10.130
deny ip 192.168.10.192 0.0.0.31 host 192.168.10.131
deny ip 192.168.10.192 0.0.0.31 host 192.168.10.226
permit tcp 192.168.10.192 0.0.0.31 any established
permit tcp 192.168.10.192 0.0.0.31 any eq 80
permit tcp 192.168.10.192 0.0.0.31 any eq 443
permit udp 192.168.10.192 0.0.0.31 any eq 53
deny ip any any

interface Vlan60
ip access-group ACL_DMZ_IN in
```

Internal server/DNS/DHCP/RADIUS ACL attempts:

```ios
ip access-list extended ACL_DNS_DHCP_RADIUS_IN
remark Controls traffic FROM DNS, DHCP and Radius servers
permit udp host 192.168.10.130 any eq 53
permit tcp host 192.168.10.130 any eq 53
permit udp host 192.168.10.131 any eq 67
permit udp host 192.168.10.131 any eq 68
permit udp host 192.168.10.132 any eq 1812
permit udp host 192.168.10.132 any eq 1813
permit tcp 192.168.10.128 0.0.0.31 any established
permit ip 192.168.10.128 0.0.0.31 192.168.10.0 0.0.0.127
deny ip any any

interface Vlan70
ip access-group ACL_DNS_DHCP_RADIUS_IN in
```

Traffic-to-servers ACL:

```ios
ip access-list extended ACL_TO_SERVERS
remark Controls traffic TO servers
permit udp any host 192.168.10.131 eq 67
permit udp any host 192.168.10.130 eq 53
permit tcp any host 192.168.10.130 eq 53
permit udp any host 192.168.10.132 eq 1812
permit udp any host 192.168.10.132 eq 1813
permit tcp any 192.168.10.128 0.0.0.31 eq 22
permit tcp any 192.168.10.128 0.0.0.31 eq 443
deny ip any any

interface Vlan70
ip access-group ACL_TO_SERVERS out
```

Storage ACL:

```ios
ip access-list extended ACL_STORAGE_IN
remark Controls traffic from Storage Server
permit tcp host 192.168.10.226 any established
permit tcp host 192.168.10.226 any eq 21
deny ip any any

interface Vlan80
ip access-group ACL_STORAGE_IN in
```

Management/Study ACL summary:

```ios
ip access-list extended ACL_MANAGEMENT_SECRETARIAT_STUDY_IN
remark Controls traffic from Management/Secretariat/Study department
deny ip 192.168.10.0 0.0.0.31 192.168.10.32 0.0.0.31
deny ip 192.168.10.0 0.0.0.31 192.168.10.64 0.0.0.31
deny ip 192.168.10.0 0.0.0.31 192.168.10.96 0.0.0.31
permit ip 192.168.10.0 0.0.0.31 192.168.10.192 0.0.0.31
permit udp 192.168.10.0 0.0.0.31 host 192.168.10.130 eq 53
permit tcp 192.168.10.0 0.0.0.31 host 192.168.10.130 eq 53
permit udp 192.168.10.0 0.0.0.31 host 192.168.10.132 eq 1812
permit udp 192.168.10.0 0.0.0.31 host 192.168.10.132 eq 1813
permit tcp 192.168.10.0 0.0.0.31 host 192.168.10.226 eq 21
permit tcp 192.168.10.0 0.0.0.31 any established
permit ip 192.168.10.0 0.0.0.31 any
deny ip any any

interface Vlan10
ip access-group ACL_MANAGEMENT_SECRETARIAT_STUDY_IN in
```

Production ACL summary:

```ios
ip access-list extended ACL_PRODUCTION_IN
remark Controls traffic from Production
deny ip 192.168.10.32 0.0.0.31 192.168.10.0 0.0.0.31
deny ip 192.168.10.32 0.0.0.31 192.168.10.64 0.0.0.31
deny ip 192.168.10.32 0.0.0.31 192.168.10.96 0.0.0.31
permit ip 192.168.10.32 0.0.0.31 192.168.10.192 0.0.0.31
permit udp 192.168.10.32 0.0.0.31 host 192.168.10.130 eq 53
permit tcp 192.168.10.32 0.0.0.31 host 192.168.10.130 eq 53
permit udp 192.168.10.32 0.0.0.31 host 192.168.10.131 eq 67
permit udp 192.168.10.32 0.0.0.31 host 192.168.10.132 eq 1812
permit udp 192.168.10.32 0.0.0.31 host 192.168.10.132 eq 1813
permit tcp 192.168.10.32 0.0.0.31 host 192.168.10.226 eq 21
permit tcp 192.168.10.32 0.0.0.31 any established
permit ip 192.168.10.32 0.0.0.31 any
deny ip any any

interface Vlan20
ip access-group ACL_PRODUCTION_IN in
```

Support A and Support B follow the same ACL pattern, using these source subnets:

```ios
ACL_SUPPORT_A_IN source: 192.168.10.64 0.0.0.31
ACL_SUPPORT_B_IN source: 192.168.10.96 0.0.0.31
```

Late ACL edits add DHCP permit sequence lines to several ACLs:

```ios
ip access-list extended ACL_SUPPORT_A_IN
54 permit udp any any eq 67
55 permit udp any any eq 68

ip access-list extended ACL_SUPPORT_B_IN
54 permit udp any any eq 67
55 permit udp any any eq 68

ip access-list extended ACL_MANAGEMENT_SECRETARIAT_STUDY_IN
54 permit udp any any eq 67
55 permit udp any any eq 68
```

The final visible rows are:

```text
2936 ip access-list extended ACL_SUPPORT_A_IN
2938 54 permit udp any any eq 67
2939 55 permit udp any any eq 68
2941 write memory
2942 show access-lists ACL_SUPPORT_A_IN
2944 ip access-list extended ACL_SUPPORT_B_IN
2945 54 permit udp any any eq 67
2946 55 permit udp any any eq 68
2948 write memory
2950 ip access-list extended ACL_MANAGEMENT_SECRETARIAT_STUDY_IN
2951 54 permit udp any any eq 67
2952 55 permit udp any any eq 68
2954 write memory
2955 show access-lists
2956 PROD-PC-1: ipconfig /all
2957 Switch-L2-B: enable
```

## Inconsistencies / Things To Verify In Packet Tracer

| Item | What the recording shows | Why to verify |
| --- | --- | --- |
| VLAN subnet plan | Recording uses many `/28` SVIs: VLAN 10 `.1/28`, VLAN 20 `.17/28`, VLAN 40 `.65/28`, VLAN 50 `.81/28`, VLAN 60 `.97/28`, VLAN 70 `.113/28`, VLAN 80 `.129/28`, VLAN 99 `.177/28`; VLAN 30 uses `.33/27`. | The existing report describes a different later plan for several VLANs, especially DMZ/server/storage/management. |
| `ROUTER A` inside interface mask | Initially `/30`, later changed to `/24`. | The /30 design matches the point-to-point link to `Switch-L3-CORE`; the /24 change may have been troubleshooting. |
| Core `GigabitEthernet1/0/2` | First configured as trunk, later defaulted and made access VLAN 10. | This may reflect a correction, or it may have broken a trunk depending on the actual cable. |
| NAT | NAT was configured, inspected, and partly removed/reworked. | Final NAT state should be checked with `show running-config | section nat`. |
| Static routes on `ROUTER A` | Several routes to internal subnets appear, but OCR cut off the next-hop final octets. | The likely next hop is `192.168.10.194`, but Packet Tracer should be checked. |
| DMZ switch | Device exists in topology, but I did not see a clear dedicated DMZ-switch configuration block in the sampled recording. | Check `DMZ-Switch` directly in the `.pkt`. |

## Practical Verification Commands To Run In Packet Tracer

Run these on each device to confirm the final state:

```ios
show running-config
show ip interface brief
show vlan brief
show interfaces trunk
show ip route
show running-config | section nat
show access-lists
show spanning-tree
```
