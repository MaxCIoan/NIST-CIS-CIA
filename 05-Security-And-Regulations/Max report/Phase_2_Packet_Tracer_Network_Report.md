# 🛡️ Phase 2 Packet Tracer Network Report

## 📌 Report Banner

| Field | Details |
| --- | --- |
| Project | Phase 2 Packet Tracer network security review |
| Main evidence | `Final_Networking_Analysis_Report.pdf`, `Network_Security_Final.pdf` |
| Review focus | Devices, servers, VLANs, sectors, authentication, redundancy, VTY/SSH, ACLs, Belgian compliance posture |
| Important limitation | This report is based on the two PDF reports. The actual `.pka/.pkt` running configuration should still be checked in Packet Tracer before final submission. |

---

## 🧭 Executive Summary

The network is designed around a **single Layer 3 core switch**, multiple **Cisco 2960 access switches**, an **edge router**, separated **department VLANs**, a **DMZ**, and dedicated server VLANs for services such as DNS, DHCP, FTP, HTTP, Syslog/NTP, and a planned RADIUS/AAA server.

Overall security design is **partially compliant** with Belgian cybersecurity expectations because segmentation, ACLs, logging, SSH/VTY hardening, and a DMZ are documented. The biggest weaknesses are the **single core switch dependency**, **Packet Tracer limitations for real RADIUS/AD**, broad ACL lines such as `permit ip ... any`, and some documentation inconsistencies that must be verified in the actual PKA.

| Overall Area | Grade | Reason |
| --- | --- | --- |
| Segmentation | 🟢 Green | VLANs exist for departments, servers, DMZ, storage, and management. |
| Access control | 🟡 Yellow | ACLs are documented, but some rules still allow broad outbound access. |
| Authentication / AAA | 🔴 Red | RADIUS is planned/provisioned, but Packet Tracer cannot fully simulate real RADIUS/AD. |
| Resilience | 🔴 Red | Single core switch means a central failure can disconnect the network. |
| Monitoring | 🟡 Yellow | Syslog/NTP exists, but Packet Tracer logging is limited. |

---

## 🖥️ Device Inventory

| Device / Group | Type | Role | Evidence / Notes |
| --- | --- | --- | --- |
| ISR4331 / ISR331 Router A | Edge router | Internet gateway, NAT/PAT, DMZ routing, perimeter ACLs | PDF names vary between ISR4331 and ISR331. Verify exact model in PKA. |
| ISP Router | Router | Simulated upstream internet connection | Mentioned as `ISR 331 ISP`. |
| L3-CORE Switch | Layer 3 core switch | Central inter-VLAN routing, SVI gateways, trunks to access switches | Named as Cisco 3650-24PS in one PDF and Cisco 1850-24PS in another. Verify model in PKA. |
| L2-Switch-A | Cisco 2960 access switch | Access layer switch for users | Exact sector mapping must be verified in PKA. |
| L2-Switch-B | Cisco 2960 access switch | Access layer switch for users | Exact sector mapping must be verified in PKA. |
| L2-Switch-C | Cisco 2960 access switch | Access layer switch for users | Exact sector mapping must be verified in PKA. |
| L2-Switch-D | Cisco 2960 access switch | Access layer switch for users | Exact sector mapping must be verified in PKA. |
| DMZ-SWITCH | Cisco 2960 access switch | Dedicated DMZ switch for HTTP server | Management IP documented as `192.168.10.194`. |
| End devices | PCs / laptops / printers | User workstations and printer access | 45 Dell laptops are budgeted; site survey lists 43 computers across sectors. |
| UPS units | APC Smart-UPS | Power backup for servers and switches | 3 UPS units documented. |

---

## 🗂️ Sectors, PCs, Switches, VLANs

### ✅ Recommended Working Mapping

The later security report and ACL section consistently use this VLAN mapping:

| Sector | PCs / Hosts Needed | VLAN | Subnet | Gateway | Access Switch |
| --- | ---: | ---: | --- | --- | --- |
| Management / Secretariat / Study | 13 PCs | 10 | `192.168.10.0/27` | `192.168.10.1` | Likely one of L2-Switch-A/B/C/D; verify in PKA |
| Production | 10 PCs | 20 | `192.168.10.32/27` | `192.168.10.33` | Likely one of L2-Switch-A/B/C/D; verify in PKA |
| Support A | 10 PCs documented in site survey, 20 hosts in IP table | 30 | `192.168.10.64/27` | `192.168.10.65` | Likely one of L2-Switch-A/B/C/D; verify in PKA |
| Support B | 10 PCs documented in site survey, 20 hosts in IP table | 40 | `192.168.10.96/27` | `192.168.10.97` | Likely one of L2-Switch-A/B/C/D; verify in PKA |
| MFD Printer | 1 printer | 50 | `192.168.11.0/27` | `192.168.11.1` | Not clearly mapped in PDFs |
| DMZ | HTTP server + DMZ switch | 60 | `192.168.10.192/27` | `192.168.10.193` | DMZ-SWITCH |
| Internal Servers | DNS, DHCP, RADIUS | 70 | `192.168.10.128/27` | `192.168.10.129` | Direct/core server ports |
| Storage / FTP | FTP storage server | 80 | `192.168.10.224/29` | `192.168.10.225` | Direct/core server port |
| Network Management | Router/core/switch management | 99 | `192.168.10.160/27` | `192.168.10.161` | Management VLAN |

### ⚠️ Documentation Inconsistency

One IP table in `Final_Networking_Analysis_Report.pdf` labels Study as VLAN 20, Production as VLAN 30, and Support A/B as VLAN 40. Later sections and ACLs use VLAN 10 for Management/Secretariat/Study, VLAN 20 for Production, VLAN 30 for Support A, and VLAN 40 for Support B. The actual PKA should be treated as the final source of truth.

---

## 🧱 Servers And Services

| Server | VLAN | IP | Role | Security Notes |
| --- | ---: | --- | --- | --- |
| DNS Server | 70 | `192.168.10.130` | DNS, Syslog, NTP | Central logging and time service are documented on this server. |
| DHCP Server | 70 | `192.168.10.131` | DHCP for workstation VLANs | L3 core uses `ip helper-address` on workstation SVIs. |
| RADIUS Server | 70 | `192.168.10.132` | Planned AAA/RADIUS | Hardware/IP exists, but Packet Tracer cannot fully simulate real RADIUS service. |
| HTTP Server | 60 | `192.168.10.195` | Public-facing web service in DMZ | HTTP and HTTPS are documented as enabled. |
| FTP / Storage Server | 80 | `192.168.10.226` | Shared storage / FTP | FTP user evidence conflicts: `ftpuser` in one PDF and `security` in another. |
| DMZ Switch SVI | 60 | `192.168.10.194` | DMZ switch management | Default gateway documented as `192.168.10.193`. |

---

## 🔐 Users, Admins, Privilege, AAA

| Area | Documented Setting | Status | Risk |
| --- | --- | --- | --- |
| FTP account | `ftpuser` in one guide; `security` in security test | 🟡 Verify | Conflicting documentation. Check actual FTP service users in PKA. |
| Device local admin | Example shows `username admin password admin123` | 🔴 Weak if used | `admin123` is weak and should not be accepted in a real environment. |
| Enable secret | Example shows `enable secret 123456789` | 🔴 Weak if used | Predictable secret; replace with strong unique secret. |
| Console password | Example shows `password cisco` | 🔴 Weak if used | Default Cisco-style password is non-compliant. |
| VTY password | Example shows `password cisco` + `login local` | 🔴 Weak if used | Requires strong local user database and SSH-only access. |
| AAA | `aaa new-model`, local authentication examples | 🟡 Partial | Local AAA can be demonstrated, but not real external RADIUS. |
| RADIUS | Server at `192.168.10.132` planned/provisioned | 🔴 Not fully implemented | Packet Tracer cannot validate real centralized RADIUS/AD. |

---

## 🌐 VLANs, Trunks, VTY, ACLs

| Control | Evidence | Grade |
| --- | --- | --- |
| VLAN segmentation | VLANs 10, 20, 30, 40, 60, 70, 80, 99 documented | 🟢 Green |
| Inter-VLAN routing | Performed by L3-CORE using SVIs | 🟡 Yellow because single core is a critical dependency |
| Trunking | Trunks between L3 core and access switches are manually configured | 🟢 Green |
| Native VLAN | VLAN 99 documented as native VLAN | 🟡 Yellow because native VLAN should ideally be unused and not also management |
| DTP/VTP | Manual trunks, `switchport nonegotiate`, VTP transparent/off documented | 🟢 Green |
| VTY | `line vty 0 4`, `transport input ssh`, `login local`, timeout documented | 🟡 Yellow if weak local credentials remain |
| Telnet | Replaced/prohibited in documentation | 🟢 Green |
| Port security | `maximum 1`, sticky MAC, violation shutdown documented | 🟢 Green |
| Unused ports | Shutdown and assigned/marked unused | 🟢 Green |
| ACLs | Department, DMZ, server, internet ACLs documented | 🟡 Yellow because some ACLs include broad `permit ip ... any` rules |

---

## 🧯 Redundancy And Failure Points

| Component | Redundancy Status | Impact If It Fails | Grade |
| --- | --- | --- | --- |
| L3-CORE Switch | No redundant core implemented | Most/all inter-VLAN routing and access switch connectivity fails | 🔴 Red |
| Edge Router A | No redundant edge router documented | Internet/DMZ access fails | 🔴 Red |
| DNS/DHCP/Syslog/NTP Server | DNS server also performs Syslog/NTP; DHCP is separate but same server VLAN | Name resolution/logging/time or addressing services may fail | 🟡 Yellow |
| RADIUS Server | Single planned server | Central AAA would fail if actually used | 🔴 Red |
| Access Switches A-D | Multiple access switches exist | One sector/access group may fail, but others can remain online | 🟡 Yellow |
| UPS | 3 UPS units documented | Provides power backup, but not full HA | 🟡 Yellow |

Main conclusion: **there is no high availability at the core layer**. If the central L3 switch dies, the network loses routing between VLANs and likely loses access to most services.

---

## 🇧🇪 Belgian Security Framework Compliance Grading

| Control Area | Belgian Reference | Current State | Grade | Recommendation |
| --- | --- | --- | --- | --- |
| Asset inventory | CyFun Identify, ISO 27001 asset management | Devices and servers are listed, but exact switch-to-sector mapping is incomplete | 🟡 Yellow | Export device inventory from PKA and map each port/switch/sector. |
| Network segmentation | NIS2 risk measures, CyFun Protect, ISO 27001 access control | VLANs and ACLs separate departments, servers, DMZ, and management | 🟢 Green | Keep VLANs, but verify ACL order and remove broad permits. |
| Least privilege | CyFun Protect, ISO 27001 access control | ACLs deny cross-department traffic, but still allow internet and broad `any` flows | 🟡 Yellow | Replace broad permits with service-specific rules. |
| Privileged access | NIS2 MFA/privileged account expectations, CyFun IAM | SSH/VTY hardening exists, but weak example passwords are documented | 🔴 Red | Use strong unique local users, privilege levels, and MFA/RADIUS in real deployment. |
| Central authentication | NIS2, CyFun IAM | RADIUS planned, but not actually testable in Packet Tracer | 🔴 Red | In real design, deploy AD/RADIUS/TACACS+ and fallback local break-glass accounts. |
| Logging and monitoring | NIS2 incident detection, CyFun Detect | Syslog/NTP configured on DNS server | 🟡 Yellow | Use a dedicated SIEM/syslog server with alerting and retention. |
| DMZ protection | CyFun Protect, ISO 27001 network security | HTTP server isolated in VLAN 60 with router/ACL controls | 🟢 Green | Confirm inbound internet rules expose only required services. |
| Business continuity | NIS2 continuity, CyFun Recover | No redundant core/router/server design documented | 🔴 Red | Add second core switch, redundant uplinks, router HA, backup services. |
| Data protection | GDPR security of processing | Basic network controls exist; no evidence of encryption, backups, or access review | 🟡 Yellow | Add encryption, backup, account review, and incident response evidence. |
| Vulnerability hardening | CyFun Protect, ISO 27001 technical controls | Port security and unused-port shutdown are documented | 🟢 Green | Add DHCP snooping, DAI, BPDU Guard, and Root Guard where Packet Tracer supports it. |

---

## 🚦 Key Findings

| ID | Finding | Severity | Evidence | Compliance Impact |
| --- | --- | --- | --- | --- |
| F1 | Single L3 core switch is a major single point of failure | 🔴 High | Design says redundancy is only future potential | NIS2/CyFun continuity weakness |
| F2 | RADIUS/AAA is not truly implemented in Packet Tracer | 🔴 High | PDFs state Packet Tracer lacks real RADIUS service | Weak centralized access control |
| F3 | Weak example credentials are documented | 🔴 High | `cisco`, `admin123`, `123456789` appear in examples | Poor privileged access hygiene |
| F4 | ACLs include broad permit rules | 🟡 Medium | `permit ip ... any` appears after specific service permits | Least privilege not fully enforced |
| F5 | DNS server also hosts Syslog/NTP | 🟡 Medium | DNS server is reused as central Syslog/NTP | Service concentration risk |
| F6 | Documentation conflicts with itself | 🟡 Medium | VLAN table, switch model, FTP username inconsistencies | Audit evidence reliability issue |
| F7 | DMZ isolation is documented and tested | 🟢 Positive | VLAN 60, HTTP server, DMZ switch, NAT/ACLs documented | Good segmentation control |
| F8 | Layer 2 protections are documented | 🟢 Positive | Port security, sticky MAC, unused-port shutdown | Good access-layer hardening |

---

## ✅ Final Verification Checklist For The PKA

| Check | What To Verify In Packet Tracer | Status |
| --- | --- | --- |
| Device names | Router A, ISP router, L3-CORE, L2-Switch-A/B/C/D, DMZ-SWITCH | ⬜ |
| Switch-to-sector map | Which access switch serves Management/Study, Production, Support A, Support B | ⬜ |
| VLAN IDs | Confirm VLAN 10/20/30/40/50/60/70/80/99 assignment | ⬜ |
| Server IPs | Confirm DNS `.130`, DHCP `.131`, RADIUS `.132`, HTTP `.195`, FTP `.226` | ⬜ |
| ACL placement | Confirm ACLs are applied inbound on correct SVIs/interfaces | ⬜ |
| VTY security | Confirm SSH-only, no Telnet, timeout, login local, strong usernames | ⬜ |
| Weak passwords | Search configs for `cisco`, `admin123`, `123456789` | ⬜ |
| Redundancy | Confirm whether any EtherChannel, HSRP, backup core, or backup router exists | ⬜ |
| Logging | Confirm router/core/DMZ switch send logs to `192.168.10.130` | ⬜ |
| FTP users | Confirm whether actual FTP user is `ftpuser`, `security`, or another account | ⬜ |

---

## 🎯 Bottom Line

The Packet Tracer design shows a good learning-level security architecture: VLAN segmentation, a DMZ, ACLs, SSH/VTY hardening, port security, and central logging are all present in the documentation. For a Belgian compliance view, it reaches a **basic-to-partial CyFun/NIS2 posture**, but it is not fully compliant because authentication is not truly centralized, passwords/examples are weak, monitoring is basic, and resilience is weak due to the single-core design.

