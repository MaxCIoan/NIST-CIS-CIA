# Final Report

Source evidence:

- `Screen_Recording_2026-04-22_Command_Log.md`
- `viewable logical topology.png`
- `Network Design Main final version.pkt`
- `Phase_2_Packet_Tracer_Network_Report.md`

This report summarizes the important configuration only. Repeated commands such as `enable`, `configure terminal`, `end`, `write memory`, `show ...`, and repeated pings are not listed unless they matter for the design.

## Executive Summary

The Packet Tracer network is built around one central Layer 3 core switch, one edge router, four access switches, one DMZ switch, internal servers, and separated user VLANs. The main security idea is segmentation: users, servers, storage, DMZ, and network management are placed into different VLANs, then ACLs control which VLANs may talk to each other.

The strongest parts are VLAN separation, DMZ isolation, SSH/VTY hardening attempts, port security, logging/NTP, and ACLs. The weakest parts are the single core switch, weak demo passwords, incomplete/Packet Tracer-limited RADIUS, and several broad ACL permits that should be tightened in a real network.

## Main Network Design

| Area | Final/Important Setting | Purpose |
| --- | --- | --- |
| Core routing | `Switch-L3-CORE` performs inter-VLAN routing using SVIs | Central gateway for all VLANs |
| Internet edge | `ROUTER A` connects internal network to ISP | Internet/edge routing and NAT point |
| DMZ | VLAN 60 with HTTP server `192.168.10.195` | Keeps public-facing server separate from internal users |
| Internal servers | VLAN 70 | DNS, DHCP, RADIUS, logging/time services |
| Storage | VLAN 80 | FTP/storage server isolation |
| Management | VLAN 99 | Management IPs for network devices |
| Access security | Port security, portfast, trunk restrictions | Reduces basic Layer 2 misuse |

## Device Settings

| Device | Important IP / Interface Settings | Important Security / Role Settings |
| --- | --- | --- |
| `Switch-L3-CORE` | `Vlan10 192.168.10.1/27`, `Vlan20 192.168.10.33/27`, `Vlan30 192.168.10.65/27`, `Vlan40 192.168.10.97/27`, `Vlan60 192.168.10.193/27`, `Vlan70 192.168.10.129/27`, `Vlan80 192.168.10.225/29`, `Vlan99 192.168.10.161/27`, routed link `G1/0/1 192.168.10.250/30` | Main routing switch, VLAN gateways, ACL placement, trunks to access/DMZ switches |
| `ROUTER A` | Outside `G0/0/0 10.0.0.1/30`, inside `G0/0/1 192.168.10.249/30`, default route to `10.0.0.2`, internal routes back via `192.168.10.250` | Edge routing, NAT inside/outside, internet ACL attempt, SSH/VTY hardening |
| `ISP` | Earlier test used `203.0.113.2/30` and loopback `8.8.8.8/32`; later design points router default route to `10.0.0.2` | Simulated internet/upstream |
| `Switch-L2-A` | Management IP `192.168.10.163/27`, gateway `192.168.10.161`, trunk allows VLANs `10,20,99` | Access switch, port security, management VLAN |
| `Switch-L2-B` | Management IP `192.168.10.164/27`, gateway `192.168.10.161`, trunk allows VLANs `10,20,99` | Access switch, port security, management VLAN |
| `Switch-L2-C` | Access/trunk switch for Support A side | Same trunk/access security pattern |
| `Switch-L2-D` | Access/trunk switch for Support B side | Same trunk/access security pattern |
| `DMZ-Switch` | `Vlan60 192.168.10.194/27`, gateway `192.168.10.193`, trunk native VLAN `99`, allowed VLANs `60,99` | DMZ switch for HTTP server, logging/NTP configured |
| `DNS Server` | `192.168.10.130` | DNS, syslog/logging target, NTP/time source |
| `DHCP Server` | `192.168.10.131` | DHCP service, reached by `ip helper-address` |
| `RADIUS Server` | `192.168.10.132` | AAA/RADIUS attempt, Packet Tracer support limited |
| `HTTP Server` | `192.168.10.195` | Public web service inside DMZ |
| `FTP/Storage Server` | `192.168.10.226` | Storage/FTP service in VLAN 80 |

## VLAN And IP Plan

| VLAN | Name | Subnet | Gateway | Main Use |
| ---: | --- | --- | --- | --- |
| 10 | `Management_Study` | `192.168.10.0/27` | `192.168.10.1` | Management/Secretariat/Study users |
| 20 | `Production` | `192.168.10.32/27` | `192.168.10.33` | Production users |
| 30 | `Support_A` | `192.168.10.64/27` | `192.168.10.65` | Support A users |
| 40 | `Support_B` | `192.168.10.96/27` | `192.168.10.97` | Support B users |
| 50 | Printer VLAN | Not fully consistent in recordings | Verify | MFD/printer |
| 60 | `DMZ_Network` | `192.168.10.192/27` | `192.168.10.193` | DMZ and HTTP server |
| 70 | `Internal_Servers` | `192.168.10.128/27` | `192.168.10.129` | DNS, DHCP, RADIUS |
| 80 | `iSCSI_Network` | `192.168.10.224/29` | `192.168.10.225` | FTP/storage |
| 99 | `Network_Management` | `192.168.10.160/27` | `192.168.10.161` | Network-device management |

## Routing And NAT

| Device | Route / NAT Setting | Meaning |
| --- | --- | --- |
| `Switch-L3-CORE` | `ip routing` | Enables inter-VLAN routing |
| `Switch-L3-CORE` | `ip route 0.0.0.0 0.0.0.0 192.168.10.249` | Sends unknown traffic to Router A |
| `ROUTER A` | `ip route 0.0.0.0 0.0.0.0 10.0.0.2` | Sends internet traffic to ISP |
| `ROUTER A` | Routes to `192.168.10.0/27`, `.32/27`, `.64/27`, `.96/27`, `.128/27`, `.160/27`, `.192/27`, `.224/29` via `192.168.10.250` | Sends internal VLAN return traffic back to the core |
| `ROUTER A` | `ip nat inside` on internal interface, `ip nat outside` on ISP interface | Marks NAT direction |
| `ROUTER A` | `ip nat inside source list 1 interface ... overload` was attempted earlier | PAT/NAT for internal users |

## ACL Summary

| ACL | Applied To | What It Allows | What It Blocks / Controls |
| --- | --- | --- | --- |
| `ACL_INTERNET_IN` | Router internet interface inbound | HTTP/HTTPS to `192.168.10.195`, established TCP, DNS | Blocks other inbound internet traffic |
| `ACL_DMZ_IN` | `Vlan60` inbound | DMZ web/DNS style traffic and established flows | Blocks DMZ from reaching internal user/server networks directly |
| `ACL_DNS_DHCP_RADIUS_IN` | `Vlan70` inbound | DNS `53`, DHCP `67/68`, RADIUS `1812/1813`, established server traffic | Blocks unwanted server-originated traffic |
| `ACL_TO_SERVERS` | `Vlan70` outbound | Access to DHCP, DNS, RADIUS, SSH/HTTPS to servers | Blocks other traffic to server VLAN |
| `ACL_STORAGE_IN` | `Vlan80` inbound | FTP/storage traffic from `192.168.10.226`, established TCP | Blocks unwanted storage-originated traffic |
| `ACL_MANAGEMENT_SECRETARIAT_STUDY_IN` | `Vlan10` inbound | DNS, RADIUS, FTP, DMZ access, internet/established traffic | Blocks direct department-to-department traffic |
| `ACL_PRODUCTION_IN` | `Vlan20` inbound | DNS, DHCP, RADIUS, FTP, DMZ access, internet/established traffic | Blocks direct access to other user departments |
| `ACL_SUPPORT_A_IN` | `Vlan30` inbound | Same pattern as Production using source `192.168.10.64/27` | Blocks direct access to other user departments |
| `ACL_SUPPORT_B_IN` | `Vlan40` inbound | Same pattern as Production using source `192.168.10.96/27` | Blocks direct access to other user departments |

Late ACL edits added DHCP permits to several user ACLs:

| ACL | Added Rules |
| --- | --- |
| `ACL_SUPPORT_A_IN` | `54 permit udp any any eq 67`, `55 permit udp any any eq 68` |
| `ACL_SUPPORT_B_IN` | `54 permit udp any any eq 67`, `55 permit udp any any eq 68` |
| `ACL_MANAGEMENT_SECRETARIAT_STUDY_IN` | `54 permit udp any any eq 67`, `55 permit udp any any eq 68` |

## Users And Administrative Access

| Setting | Seen In Commands | Security Meaning |
| --- | --- | --- |
| Local admin | `username admin privilege 15 secret admin`, also `username admin secret cisco` / `123456789` attempts | Local administrator account for device login |
| AAA | `aaa new-model`, `aaa authentication login default local` | Uses local authentication after RADIUS attempts |
| RADIUS | Server `192.168.10.132`, key `cisco123` | Central login was attempted, but Packet Tracer cannot fully prove real RADIUS/AD behavior |
| SSH | `ip domain-name ...`, `crypto key generate rsa`, `transport input ssh` | Enables encrypted remote management |
| Console | `line console 0`, `password cisco`, `exec-timeout 5 0`, `logging synchronous` | Local console access with timeout/log readability |
| Login blocking | `login block-for 120 attempts 5 within 60` | Slows repeated failed login attempts |
| Password controls | `security passwords min-length 9`, `service password-encryption` | Basic password hardening |

Important risk: passwords such as `cisco`, `admin`, `12345`, `0000`, `cisco123`, and `123456789` are weak lab values. For a real environment they must be replaced with strong unique secrets.

## Layer 2 And Access-Port Security

| Control | Setting | Purpose |
| --- | --- | --- |
| Trunks | `switchport mode trunk`, allowed VLAN lists, native VLAN `99`, `switchport nonegotiate` | Prevents automatic trunk negotiation and limits VLANs on uplinks |
| Access ports | `switchport mode access`, `switchport access vlan X` | Places PCs/printer/servers in the correct VLAN |
| Port security | `switchport port-security`, `maximum 1`, `violation restrict` | Limits each access port to one device/MAC |
| PortFast | `spanning-tree portfast` | Lets end-device ports come up faster |
| Disabled/cleaned ports | `default interface ...`, `no shutdown` where needed | Resets or enables selected ports |

## Defense Explained For Non-Technical Readers

The network is defended like a building with separate rooms and locked doors. Each department is placed in its own room, called a VLAN. Production, Support A, Support B, servers, storage, management, and the public web server are separated so that one area cannot freely walk into another.

The ACLs are the door rules. They say things like: users may ask the DNS server for names, may get an IP address from DHCP, may reach the web server in the DMZ, and may use approved services. But they should not freely browse into another department or directly access sensitive servers.

The DMZ is a visitor area. The public HTTP server lives there because it is the machine most exposed to outside traffic. If that web server is attacked, the ACLs try to stop the attacker from moving further into internal departments and servers.

SSH protects administrator logins by encrypting remote management. Port security helps stop someone from plugging extra unauthorized devices into switch ports. Logging and NTP help with investigation because device events are sent to a central server and timestamps are synchronized.

## Penetration Resistance Explained

If an attacker tries to enter from the internet, the internet ACL should only allow web traffic to the HTTP server and block everything else. This reduces the number of exposed services.

If an attacker compromises a normal PC, VLAN separation and department ACLs make lateral movement harder. For example, a Production PC should not freely connect to Management or Support networks.

If an attacker compromises the DMZ web server, the DMZ ACL should prevent easy access to internal departments, DNS/DHCP/RADIUS, and storage.

If an attacker tries to brute-force device logins, login blocking, SSH-only management, local authentication, password encryption, and timeouts help reduce risk. However, the weak demo passwords remain a serious weakness.

## Redundancy And Resilience

| Component | What Exists | Limitation |
| --- | --- | --- |
| Access layer | Multiple L2 switches exist, so one access switch failure affects only part of the user network | No evidence of redundant uplinks/EtherChannel for every access switch |
| Core | One Layer 3 core switch | Major single point of failure |
| Edge router | One Router A | Internet/DMZ access depends on one router |
| Servers | Separate DNS, DHCP, RADIUS, HTTP, FTP roles exist | No backup DNS/DHCP/RADIUS shown |
| Power | UPS units are documented in the earlier report | UPS helps power continuity but is not network high availability |

Plain-language conclusion: the network has some resilience because users are spread across several access switches, but it does not have true high availability. If the core switch fails, most routing between departments and services stops. If Router A fails, internet and DMZ access stop.

## Most Important Risks

| Risk | Severity | Why It Matters | Fix |
| --- | --- | --- | --- |
| Single core switch | High | One failure can break most of the network | Add second core switch, redundant links, HSRP/VRRP if possible |
| Weak passwords | High | Easy to guess in a real attack | Use strong unique secrets and remove demo passwords |
| RADIUS not fully proven | High | Central authentication is only partly demonstrated | Use real RADIUS/TACACS+/AD outside Packet Tracer |
| Broad ACL permits | Medium | `permit ip ... any` can weaken least privilege | Replace with service-specific rules |
| DMZ and server ACL complexity | Medium | Complex ACLs are easy to misorder | Test with `show access-lists` and packet flows |
| Limited monitoring | Medium | Syslog/NTP exists, but no SIEM/alerting | Add real log retention and alerting |

## Final Conclusion

The design demonstrates a solid learning-level secure network: VLAN segmentation, DMZ isolation, ACL filtering, port security, SSH management, centralized logging/time, and basic login hardening are all present.

For a real production network, the biggest improvements would be stronger passwords, real centralized AAA, tighter ACLs, redundant core/edge devices, and better monitoring. The current Packet Tracer design is good for demonstrating security concepts, but not fully resilient or production-grade yet.
