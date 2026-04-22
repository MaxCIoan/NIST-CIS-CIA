# Phase 2 Packet Tracer Network Report

## Report Banner

| Field | Details |
| --- | --- |
| Project | Phase 2 Packet Tracer network security review |
| Main evidence | `Network Design Main final version.pkt`, `viewable logical topology.png`, `Screen_Recording_2026-04-22_Command_Log.md`, `Final report_wip.md` |
| Review focus | Topology, VLANs, IP plan, routing, NAT, ACLs, servers, authentication, management access, monitoring, redundancy, and Belgian compliance posture |
| Important limitation | The command log is reconstructed from Packet Tracer recordings and OCR. The final `.pkt` running configuration should still be checked before final submission. |

## Executive Summary

The Packet Tracer network is built around one central Layer 3 core switch, one edge router, four access switches, one DMZ switch, internal servers, storage, and separated user VLANs. The main security idea is segmentation: users, servers, storage, DMZ, and network management are placed into different VLANs, then ACLs control which VLANs can communicate.

The strongest parts of the design are VLAN separation, DMZ isolation, SSH/VTY hardening attempts, port security, logging/NTP, and ACL filtering. These controls show a clear defense-in-depth approach.

The weakest parts are the single Layer 3 core switch, the single edge router, weak lab/demo passwords, incomplete Packet Tracer-limited RADIUS/AAA, and several broad ACL permits that should be tightened in a real network.

| Overall Area | Grade | Reason |
| --- | --- | --- |
| Segmentation | Green | VLANs exist for users, servers, DMZ, storage, and management. |
| Access control | Yellow | ACLs exist, but some broad `permit ip ... any` rules reduce least privilege. |
| Authentication / AAA | Red | RADIUS is planned and attempted, but Packet Tracer cannot fully prove real centralized AAA. |
| Resilience | Red | One core switch and one edge router create major single points of failure. |
| Monitoring | Yellow | Syslog/NTP are configured, but there is no full SIEM, alerting, or retention evidence. |

## Main Network Design

| Area | Final / Important Setting | Purpose |
| --- | --- | --- |
| Core routing | `Switch-L3-CORE` performs inter-VLAN routing using SVIs | Central gateway for all VLANs |
| Internet edge | `ROUTER A` connects the internal network to the ISP | Internet routing and NAT point |
| DMZ | VLAN 60 with HTTP server `192.168.10.195` | Keeps public-facing services separate from internal users |
| Internal servers | VLAN 70 | DNS, DHCP, RADIUS, logging, and time services |
| Storage | VLAN 80 | FTP/storage server isolation |
| Network management | VLAN 99 | Management IPs for switches and network devices |
| Access security | Port security, PortFast, restricted trunks | Reduces basic Layer 2 misuse |

## Device Inventory And Roles

| Device | Important IP / Interface Settings | Role / Security Notes |
| --- | --- | --- |
| `Switch-L3-CORE` | `Vlan10 192.168.10.1/27`, `Vlan20 192.168.10.33/27`, `Vlan30 192.168.10.65/27`, `Vlan40 192.168.10.97/27`, `Vlan60 192.168.10.193/27`, `Vlan70 192.168.10.129/27`, `Vlan80 192.168.10.225/29`, `Vlan99 192.168.10.161/27`, routed link `G1/0/1 192.168.10.250/30` | Main routing switch, VLAN gateways, ACL placement, trunks to access and DMZ switches |
| `ROUTER A` | Outside `G0/0/0 10.0.0.1/30`, inside `G0/0/1 192.168.10.249/30`, default route to `10.0.0.2`, internal routes back via `192.168.10.250` | Edge router, NAT inside/outside, internet ACL attempt, SSH/VTY hardening |
| `ISP` | Earlier testing used `203.0.113.2/30` and loopback `8.8.8.8/32`; later design points Router A default route to `10.0.0.2` | Simulated upstream internet |
| `Switch-L2-A` | Management IP `192.168.10.163/27`, gateway `192.168.10.161`, trunk allows VLANs `10,20,99` | Access switch for user VLANs, management VLAN, port security |
| `Switch-L2-B` | Management IP `192.168.10.164/27`, gateway `192.168.10.161`, trunk allows VLANs `10,20,99` | Access switch for user VLANs, management VLAN, port security |
| `Switch-L2-C` | Access/trunk switch for Support A side | Same access-security pattern as the other L2 switches |
| `Switch-L2-D` | Access/trunk switch for Support B side | Same access-security pattern as the other L2 switches |
| `DMZ-Switch` | `Vlan60 192.168.10.194/27`, gateway `192.168.10.193`, trunk native VLAN `99`, allowed VLANs `60,99` | DMZ switch for HTTP server, logging/NTP configured |
| `DNS Server` | `192.168.10.130` | DNS, syslog/logging target, NTP/time source |
| `DHCP Server` | `192.168.10.131` | DHCP service, reached by `ip helper-address` from user VLANs |
| `RADIUS Server` | `192.168.10.132` | AAA/RADIUS attempt; Packet Tracer support is limited |
| `HTTP Server` | `192.168.10.195` | Public web service placed inside the DMZ |
| `FTP/Storage Server` | `192.168.10.226` | Storage/FTP service isolated in VLAN 80 |
| End devices | Study, Production, Support A, Support B PCs and MFD printer | User and endpoint environment |
| UPS units | 3 APC Smart-UPS units documented | Power continuity, not full high availability |

## VLAN And IP Plan

The final design moved from earlier mixed `/28` test addressing to a cleaner plan using mostly `/27` networks, a `/29` storage network, and a `/30` routed link between `ROUTER A` and `Switch-L3-CORE`.

| VLAN | Name | Subnet | Gateway | Main Use |
| ---: | --- | --- | --- | --- |
| 10 | `Management_Study` | `192.168.10.0/27` | `192.168.10.1` | Management, Secretariat, and Study users |
| 20 | `Production` | `192.168.10.32/27` | `192.168.10.33` | Production users |
| 30 | `Support_A` | `192.168.10.64/27` | `192.168.10.65` | Support A users |
| 40 | `Support_B` | `192.168.10.96/27` | `192.168.10.97` | Support B users |
| 50 | Printer VLAN | Not fully consistent in recordings | Verify in Packet Tracer | MFD/printer |
| 60 | `DMZ_Network` | `192.168.10.192/27` | `192.168.10.193` | DMZ and HTTP server |
| 70 | `Internal_Servers` | `192.168.10.128/27` | `192.168.10.129` | DNS, DHCP, RADIUS, logging/time |
| 80 | `iSCSI_Network` | `192.168.10.224/29` | `192.168.10.225` | FTP/storage |
| 99 | `Network_Management` | `192.168.10.160/27` | `192.168.10.161` | Network-device management |
| Routed link | Router-core transit | `192.168.10.248/30` | Router `192.168.10.249`, core `192.168.10.250` | Point-to-point routing between edge and core |

## Sectors, PCs, Switches, VLANs

| Sector | PCs / Hosts Needed | VLAN | Subnet | Gateway | Access Switch |
| --- | ---: | ---: | --- | --- | --- |
| Management / Secretariat / Study | 13 PCs | 10 | `192.168.10.0/27` | `192.168.10.1` | Likely L2-A/L2-B; verify exact port map in `.pkt` |
| Production | 10 PCs | 20 | `192.168.10.32/27` | `192.168.10.33` | Likely L2-A/L2-B; verify exact port map in `.pkt` |
| Support A | 10 PCs documented, subnet allows more hosts | 30 | `192.168.10.64/27` | `192.168.10.65` | Likely L2-C |
| Support B | 10 PCs documented, subnet allows more hosts | 40 | `192.168.10.96/27` | `192.168.10.97` | Likely L2-D |
| MFD Printer | 1 printer | 50 | Needs final verification | Needs final verification | Printer access port shown, but final VLAN 50 addressing is inconsistent |
| DMZ | HTTP server + DMZ switch | 60 | `192.168.10.192/27` | `192.168.10.193` | DMZ-Switch |
| Internal Servers | DNS, DHCP, RADIUS | 70 | `192.168.10.128/27` | `192.168.10.129` | Core/server ports |
| Storage / FTP | FTP storage server | 80 | `192.168.10.224/29` | `192.168.10.225` | Core/server port |
| Network Management | Router/core/switch management | 99 | `192.168.10.160/27` | `192.168.10.161` | Management SVI network |

## Routing And NAT

| Device | Route / NAT Setting | Meaning |
| --- | --- | --- |
| `Switch-L3-CORE` | `ip routing` | Enables inter-VLAN routing |
| `Switch-L3-CORE` | `ip route 0.0.0.0 0.0.0.0 192.168.10.249` | Sends unknown traffic to Router A |
| `ROUTER A` | `ip route 0.0.0.0 0.0.0.0 10.0.0.2` | Sends internet traffic to the ISP |
| `ROUTER A` | Routes to `192.168.10.0/27`, `.32/27`, `.64/27`, `.96/27`, `.128/27`, `.160/27`, `.192/27`, `.224/29` via `192.168.10.250` | Sends internal VLAN return traffic back to the core |
| `ROUTER A` | `ip nat inside` on the internal interface, `ip nat outside` on the ISP interface | Marks NAT direction |
| `ROUTER A` | `ip nat inside source list 1 interface ... overload` was attempted earlier | PAT/NAT for internal users |

The routing design is logical: user VLANs and server VLANs use the core switch as their default gateway, the core sends non-local traffic to Router A, and Router A sends internet traffic to the ISP. Internal return routes on Router A point back to the core.

## Servers And Services

| Server | VLAN | IP | Role | Security Notes |
| --- | ---: | --- | --- | --- |
| DNS Server | 70 | `192.168.10.130` | DNS, Syslog, NTP | Central logging and time service are documented on this server. |
| DHCP Server | 70 | `192.168.10.131` | DHCP for workstation VLANs | User VLANs depend on `ip helper-address` toward this server. |
| RADIUS Server | 70 | `192.168.10.132` | Planned AAA/RADIUS | Hardware/IP exists, but Packet Tracer cannot fully validate real RADIUS service. |
| HTTP Server | 60 | `192.168.10.195` | Public-facing web service in DMZ | HTTP/HTTPS are exposed through ACL policy. |
| FTP / Storage Server | 80 | `192.168.10.226` | Shared storage / FTP | FTP user evidence conflicts in older documents; final service user should be checked in `.pkt`. |
| DMZ Switch SVI | 60 | `192.168.10.194` | DMZ switch management | Default gateway documented as `192.168.10.193`. |

## ACL And Filtering Summary

The ACL design uses a default segmentation idea: allow required services, deny direct department-to-department movement, isolate the DMZ, and reduce inbound internet exposure. The intent is good, but some rules are still broader than ideal.

| ACL | Applied To | What It Allows | What It Blocks / Controls |
| --- | --- | --- | --- |
| `ACL_INTERNET_IN` | Router internet interface inbound | HTTP/HTTPS to `192.168.10.195`, established TCP, DNS | Blocks most other inbound internet traffic |
| `ACL_DMZ_IN` | `Vlan60` inbound | DMZ web/DNS style traffic and established flows | Blocks DMZ from directly reaching internal user/server networks |
| `ACL_DNS_DHCP_RADIUS_IN` | `Vlan70` inbound | DNS `53`, DHCP `67/68`, RADIUS `1812/1813`, established server traffic | Blocks unwanted traffic originating from the server VLAN |
| `ACL_TO_SERVERS` | `Vlan70` outbound | Access to DHCP, DNS, RADIUS, SSH/HTTPS to servers | Blocks other traffic to the server VLAN |
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

Main ACL risk: broad rules such as `permit ip ... any` weaken least privilege. They are acceptable for a lab demonstration but should be replaced with service-specific permits in a real network.

## Layer 2 And Access-Port Security

| Control | Setting | Purpose |
| --- | --- | --- |
| Trunks | `switchport mode trunk`, allowed VLAN lists, native VLAN `99`, `switchport nonegotiate` | Prevents automatic trunk negotiation and limits VLANs on uplinks |
| Access ports | `switchport mode access`, `switchport access vlan X` | Places PCs, printer, and servers in the correct VLAN |
| Port security | `switchport port-security`, `maximum 1`, `violation restrict` | Limits each access port to one device/MAC |
| PortFast | `spanning-tree portfast` | Lets end-device ports come up faster |
| Unused/cleaned ports | `default interface ...`, `no shutdown` where needed | Resets or enables selected ports |
| DMZ trunk | Native VLAN `99`, allowed VLANs `60,99` | Keeps DMZ traffic separated from user VLANs |

Recommended improvement: add DHCP snooping, Dynamic ARP Inspection, BPDU Guard, Root Guard, and stricter unused-port shutdown where supported.

## Users, Admins, Privilege, AAA

| Setting | Seen In Commands | Security Meaning |
| --- | --- | --- |
| Local admin | `username admin privilege 15 secret admin`, also `username admin secret cisco` / `123456789` attempts | Local administrator account for device login |
| AAA | `aaa new-model`, `aaa authentication login default local` | Uses local authentication after RADIUS attempts |
| RADIUS | Server `192.168.10.132`, key `cisco123` | Central login was attempted, but Packet Tracer cannot fully prove real RADIUS/AD behavior |
| SSH | `ip domain-name ...`, `crypto key generate rsa`, `transport input ssh` | Enables encrypted remote management |
| Console | `line console 0`, `password cisco`, `exec-timeout 5 0`, `logging synchronous` | Local console access with timeout/log readability |
| Login blocking | `login block-for 120 attempts 5 within 60` | Slows repeated failed login attempts |
| Password controls | `security passwords min-length 9`, `service password-encryption` | Basic password hardening |

Important risk: passwords such as `cisco`, `admin`, `12345`, `0000`, `cisco123`, and `123456789` are weak lab/demo values. For a real environment they must be replaced with strong unique secrets, centralized AAA, and ideally MFA for privileged access.

## Logging, Monitoring, And Time

| Control | Evidence | Assessment |
| --- | --- | --- |
| Syslog | `logging 192.168.10.130` | Good basic logging direction, but retention/alerting is not proven. |
| NTP | `ntp server 192.168.10.130` | Good for consistent timestamps. |
| Timestamps | `service timestamps log datetime msec` | Improves incident investigation quality. |
| Monitoring gap | No SIEM, IDS/IPS, or alert workflow shown | Acceptable for Packet Tracer lab, insufficient for production. |

## Defense Explained For Non-Technical Readers

The network is defended like a building with separate rooms and locked doors. Each department is placed in its own room, called a VLAN. Production, Support A, Support B, servers, storage, management, and the public web server are separated so one area cannot freely walk into another.

The ACLs are the door rules. They say things like: users may ask the DNS server for names, may get an IP address from DHCP, may reach the web server in the DMZ, and may use approved services. They should not freely browse into another department or directly access sensitive servers.

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
| Access layer | Multiple L2 switches exist, so one access switch failure affects only part of the user network | No evidence of redundant uplinks or EtherChannel for every access switch |
| Core | One Layer 3 core switch | Major single point of failure |
| Edge router | One Router A | Internet and DMZ access depend on one router |
| DNS/DHCP/RADIUS | Separate server roles exist | No backup DNS, DHCP, or RADIUS server is shown |
| HTTP/FTP services | Separate DMZ and storage services exist | No failover service is shown |
| Power | UPS units are documented | UPS helps power continuity but is not network high availability |

Plain-language conclusion: the network has some resilience because users are spread across several access switches, but it does not have true high availability. If the core switch fails, most routing between departments and services stops. If Router A fails, internet and DMZ access stop.

## Belgian Security Framework Compliance Grading

| Control Area | Belgian Reference | Current State | Grade | Recommendation |
| --- | --- | --- | --- | --- |
| Asset inventory | CyFun Identify, ISO 27001 asset management | Devices and servers are listed, but exact switch-to-sector mapping still needs final confirmation | Yellow | Export device inventory from Packet Tracer and map every port, switch, sector, and server. |
| Network segmentation | NIS2 risk measures, CyFun Protect, ISO 27001 access control | VLANs and ACLs separate departments, servers, DMZ, storage, and management | Green | Keep segmentation and verify ACL order with packet tests. |
| Least privilege | CyFun Protect, ISO 27001 access control | ACLs deny cross-department traffic, but some `permit ip ... any` rules remain | Yellow | Replace broad permits with service-specific rules. |
| Privileged access | NIS2 privileged account expectations, CyFun IAM | SSH/VTY hardening exists, but weak demo passwords are documented | Red | Use strong unique local users, centralized AAA, and MFA where possible. |
| Central authentication | NIS2, CyFun IAM | RADIUS is planned/attempted but not fully testable in Packet Tracer | Red | In a real deployment, use AD/RADIUS/TACACS+ with break-glass local accounts. |
| Logging and monitoring | NIS2 incident detection, CyFun Detect | Syslog/NTP configured on DNS server | Yellow | Use a dedicated SIEM/syslog server with alerting and retention. |
| DMZ protection | CyFun Protect, ISO 27001 network security | HTTP server isolated in VLAN 60 with router/core ACL controls | Green | Confirm inbound internet rules expose only required services. |
| Business continuity | NIS2 continuity, CyFun Recover | No redundant core, edge router, or backup services documented | Red | Add second core switch, redundant uplinks, router HA, and backup services. |
| Data protection | GDPR security of processing | Basic network controls exist; no proof of encryption, backups, or account review | Yellow | Add encryption, backup evidence, access review, and incident response evidence. |
| Vulnerability hardening | CyFun Protect, ISO 27001 technical controls | Port security, trunk restriction, and SSH are documented | Green | Add DHCP snooping, DAI, BPDU Guard, Root Guard, and configuration backups where supported. |

## Key Findings

| ID | Finding | Severity | Evidence | Compliance Impact | Recommendation |
| --- | --- | --- | --- | --- | --- |
| F1 | Single L3 core switch is a major single point of failure | High | All inter-VLAN routing depends on `Switch-L3-CORE` | NIS2/CyFun continuity weakness | Add a second core switch, redundant uplinks, and HSRP/VRRP where possible. |
| F2 | Single edge router is a major dependency | High | Internet/DMZ access depends on `ROUTER A` | Availability and continuity weakness | Add router/firewall HA or a documented failover path. |
| F3 | RADIUS/AAA is not fully proven | High | RADIUS server and AAA commands appear, but Packet Tracer cannot fully validate real RADIUS/AD | Weak centralized access control | Use real RADIUS/TACACS+/AD outside Packet Tracer. |
| F4 | Weak example credentials are documented | High | `cisco`, `admin`, `cisco123`, `123456789` appear in command evidence | Poor privileged access hygiene | Replace with strong unique secrets and remove demo credentials. |
| F5 | ACLs include broad permit rules | Medium | `permit ip ... any` appears after specific service permits | Least privilege not fully enforced | Replace broad permits with service-specific rules and test with `show access-lists`. |
| F6 | DNS server also hosts Syslog/NTP | Medium | DNS server reused as central logging/time service | Service concentration risk | Use dedicated logging/time services or backup services. |
| F7 | Printer VLAN is not fully consistent | Medium | VLAN 50 appears in commands but final addressing is unclear | Documentation reliability issue | Confirm printer VLAN IP plan and gateway in Packet Tracer. |
| F8 | DMZ isolation is documented and useful | Positive | VLAN 60, HTTP server, DMZ switch, ACLs | Good segmentation control | Keep DMZ isolation and verify inbound rules. |
| F9 | Layer 2 protections are documented | Positive | Port security, PortFast, restricted trunks | Good access-layer hardening | Keep controls and add DHCP snooping/DAI/BPDU Guard where possible. |

## Documentation Inconsistencies To Resolve

| Item | What Changed Or Conflicts | What To Do |
| --- | --- | --- |
| Earlier VLAN subnet plan | Recording shows earlier `/28` SVI attempts before the final `/27` plan | Treat the later `/27` plan from `Final report_wip.md` as the working final plan, then confirm in `.pkt`. |
| Router A addressing | Earlier `203.0.113.1/30` and later `10.0.0.1/30` appear | Treat `10.0.0.1/30` toward ISP and `192.168.10.249/30` toward core as the final design. |
| Core-to-router link | Earlier `192.168.10.193/30` / `192.168.10.194/30` appears, later replaced | Treat `192.168.10.249/30` and `192.168.10.250/30` as final. |
| Switch model names | Older documents vary between model names | Verify exact Packet Tracer device model in the `.pkt`. |
| FTP account | Older evidence mentions different FTP users | Check actual FTP service users in Packet Tracer. |
| Printer VLAN | VLAN 50 appears but final subnet/gateway are unclear | Confirm final printer IP plan and include it in the final topology evidence. |

## Final Verification Checklist For Packet Tracer

| Check | What To Verify In Packet Tracer | Status |
| --- | --- | --- |
| Device names | Router A, ISP router, L3-CORE, L2-Switch-A/B/C/D, DMZ-Switch | To verify |
| Switch-to-sector map | Which access switch serves Management/Study, Production, Support A, Support B | To verify |
| VLAN IDs | Confirm VLAN 10/20/30/40/50/60/70/80/99 assignment | To verify |
| Final gateways | Confirm SVI gateways `.1`, `.33`, `.65`, `.97`, `.129`, `.161`, `.193`, `.225` | To verify |
| Router-core link | Confirm Router A `192.168.10.249/30` and core `192.168.10.250/30` | To verify |
| Server IPs | Confirm DNS `.130`, DHCP `.131`, RADIUS `.132`, HTTP `.195`, FTP `.226` | To verify |
| NAT | Run `show running-config | section nat` on Router A | To verify |
| Static routes | Confirm Router A return routes point to `192.168.10.250` | To verify |
| ACL placement | Confirm ACLs are applied inbound/outbound on the correct SVIs/interfaces | To verify |
| VTY security | Confirm SSH-only, no Telnet, timeout, login local, and strong usernames | To verify |
| Weak passwords | Search configs for `cisco`, `admin`, `admin123`, `cisco123`, `123456789` | To verify |
| Logging | Confirm router/core/DMZ switch send logs to `192.168.10.130` | To verify |
| NTP | Confirm network devices use `192.168.10.130` as NTP server | To verify |
| Redundancy | Confirm whether any EtherChannel, HSRP, backup core, or backup router exists | To verify |
| FTP users | Confirm whether actual FTP user is `ftpuser`, `security`, or another account | To verify |

## Practical Verification Commands

Run these on the relevant Packet Tracer devices before final submission:

```ios
show running-config
show ip interface brief
show vlan brief
show interfaces trunk
show ip route
show running-config | section nat
show access-lists
show spanning-tree
show ntp status
```

## Final Conclusion

The design demonstrates a solid learning-level secure network. VLAN segmentation, DMZ isolation, ACL filtering, port security, SSH management, centralized logging/time, and basic login hardening are all present.

For a real production network, the biggest improvements would be strong unique passwords, real centralized AAA, tighter ACLs, redundant core and edge devices, backup DNS/DHCP/RADIUS services, and better monitoring. The current Packet Tracer design is good for demonstrating security concepts and a basic-to-partial CyFun/NIS2 posture, but it is not fully resilient or production-grade yet.
