**NETWORK SECURITY AUDIT REPORT**
Compliance with the CCB CyberFundamentals Framework (CyFun) and the NIS2 Directive (Directive (EU) 2022/2554, transposed by the Belgian Law of April 26, 2024)

| Field                | Value                                                        |
| -------------------- | ------------------------------------------------------------ |
| Audited Organization | Byte Sized Solutions — BeCode Final Project                  |
| Scope                | Simulated Corporate Network in Cisco Packet Tracer           |
| Audit Date           | April 2026                                                   |
| Classification       | CONFIDENTIAL — Restricted Internal Use                       |
| Auditor              | NetPilot Security Audit System                               |
| Report Version       | 1.0 — Final                                                  |
| Applied Frameworks   | CyFun CCB v2.0, NIS2 2022/2554/EU, Belgian Law of April 26, 2024, ISO/IEC 27001:2022, CIS Benchmarks v8, MITRE ATT&CK v14 |


**SECTION 1 — EXECUTIVE SUMMARY**
This report presents the outcome of a technical security and regulatory compliance audit conducted on the network infrastructure of Byte Sized Solutions as part of the BeCode final project. The audit covered the entire simulated corporate network within Cisco Packet Tracer, spanning the complete chain from the ISP router to the end-user workstations, including core routing, multilayer switching, and access equipment. In total, twenty-six security findings were identified and documented, distributed across the following severity levels: 5 CRITICAL findings, 10 HIGH findings, 9 MEDIUM findings, and 2 LOW findings.

The overall cybersecurity maturity assessment of the organization according to the CyFun model stands at 1.2 out of 5, which corresponds to Level 1 — Initial, deemed INSUFFICIENT regarding the minimum requirements imposed by the NIS2 directive and its Belgian transposition. This level reflects a situation where security mechanisms have been partially considered or configured (VLAN segmentation, RADIUS, ACL), but where these mechanisms are not effectively implemented, lack consistency, or are systematically bypassed due to configuration errors. Verification of the ten mandatory measures of Article 21(2) of the NIS2 directive reveals a score of 0/10 fully compliant measures, with none of the ten categories being completely satisfied.

Given the nature of the services provided and the estimated size of the organization, Byte Sized Solutions is likely to be classified as an important entity under the NIS2 directive and the Belgian Law of April 26, 2024. As such, the identified shortcomings expose the organization to administrative fines of up to 7 million euros or 1.4% of the total worldwide annual turnover, as well as to personal liability for management body members as stipulated in Article 20 of the directive. The current situation calls for immediate corrective actions, specifically the remediation of the five critical findings within thirty days.


**SECTION 2 — ENTITY NIS2 CLASSIFICATION**
**2.1 Classification Framework**
The NIS2 directive (2022/2554/EU) and the Belgian Law of April 26, 2024, on the cybersecurity of network and information systems distinguish two categories of entities subject to cybersecurity obligations:
* **Essential Entities (EE):** Organizations operating in critical sectors (energy, transport, health, water, digital infrastructure, public administration, space) meeting a "large enterprise" size threshold (≥ 250 employees and Revenue ≥ €50M or Balance Sheet ≥ €43M).
* **Important Entities (IE):** Organizations operating in highly critical sectors (postal services, waste management, chemicals, food, manufacturing, digital providers, research) or critical sectors below the EE thresholds.

**2.2 Byte Sized Solutions Classification**
Based on available information:
| Criteria                   | Evaluation                                        |
| -------------------------- | ------------------------------------------------- |
| Industry Sector            | IT Services / Network Integrator                  |
| Estimated Size             | SME (< 250 employees)                             |
| Estimated Revenue          | < €50M                                            |
| Nature of Services         | Digital services, network infrastructure          |
| Presence of Sensitive Data | Probable (Production VLAN, Internal Servers, FTP) |

**Conclusion:** Byte Sized Solutions can be classified as an important entity (IE) under Article 3(2) of the NIS2 directive and Article 3 of the Belgian Law of April 26, 2024, as a digital service provider or IT service provider managing network infrastructure on behalf of third parties.

**2.3 Regulatory Implications**
As an important entity, Byte Sized Solutions is subject to:
* Registration obligation with the CCB (Art. 27 Belgian Law)
* Implementation of cybersecurity risk-management measures (Art. 21 NIS2)
* Reporting obligations for significant incidents (Art. 23 NIS2)
* Compliance audits upon CCB request
* Fines up to €7M or 1.4% of global revenue in case of non-compliance


**SECTION 3 — SCOPE AND METHODOLOGY**
**3.1 Audit Scope**
The audit covers the entire network infrastructure documented in the Cisco Packet Tracer file:

| Equipment      | Model           | Role                   |
| -------------- | --------------- | ---------------------- |
| ISP-Router     | Cisco ISR       | Internet Access Router |
| ROUTER-A       | Cisco ISR4331   | Enterprise Edge Router |
| Switch-L3-CORE | Cisco 3650-24PS | Multilayer Core Switch |
| Switch-L2-A    | Cisco 2960-24TT | Access — VLAN 10/20    |
| Switch-L2-B    | Cisco 2960-24TT | Access — VLAN 30/40    |
| Switch-L2-C    | Cisco 2960-24TT | Access — VLAN 50/60    |
| Switch-L2-D    | Cisco 2960-24TT | Access — VLAN 70/80    |

Analyzed VLANs:
| VLAN | Name               | Subnet | Gateway                |
| ---- | ------------------ | ------ | ---------------------- |
| 10   | Management/Study   | /27    | 192.168.10.1           |
| 20   | Production         | /27    | 192.168.10.33          |
| 30   | Support A          | /27    | 192.168.10.65          |
| 40   | Support B          | /27    | 192.168.10.97          |
| 50   | Printers           | /27    | 192.168.10.129         |
| 60   | DMZ                | /27    | 192.168.10.193         |
| 70   | Internal Servers   | /27    | 192.168.10.161         |
| 80   | FTP/iSCSI          | /29    | 192.168.10.225         |
| 99   | Network Management | /27    | 192.168.10.1 (VLAN 99) |

**3.2 Identified Critical Services**
| Service  | IP             | VLAN |
| -------- | -------------- | ---- |
| DNS      | 192.168.10.130 | 50   |
| DHCP     | 192.168.10.131 | 50   |
| RADIUS   | 192.168.10.132 | 50   |
| FTP      | 192.168.10.226 | 80   |
| HTTP/DMZ | 192.168.10.195 | 60   |

**3.3 Methodology**
The audit was conducted using a four-phase approach:
1.  **Collection:** Extraction and analysis of all CLI configurations from the .pkt file
2.  **Technical Analysis:** Comparison of actual configuration vs. provided PDF documentation/report
3.  **Compliance Assessment:** Mapping against CyFun CCB v2.0 and NIS2 Art. 21
4.  **Reporting:** Drafting findings with technical evidence, impacts, and recommendations

Findings were cross-referenced against:
* CIS Benchmarks v8 for IOS
* MITRE ATT&CK v14 (tactics and techniques)
* ISO/IEC 27001:2022 (control domains)
* "Byte Sized Solutions" PDF Report (to identify gaps between documentation and reality)


**SECTION 4 — CyFun MATURITY ASSESSMENT**
The CyFun CCB model defines 5 maturity levels:
* Level 0: Non-existent
* Level 1: Initial (ad hoc, unrepeatable)
* Level 2: Managed (defined but inconsistent)
* Level 3: Established (consistent, documented)
* Level 4: Predictable (measured, monitored)
* Level 5: Optimized (continuous improvement)

**IDENTIFY (ID) — Score: 1.0/5**
The IDENTIFY function covers understanding the organizational context, asset management, risk assessment, and governance strategy. The network topology features a VLAN segmentation structured into nine zones, evidencing an initial approach to asset identification. However, no formal asset management policy is observable in the configurations. There is no vulnerability register, no documented risk assessment, and no identifiable cybersecurity governance process. The lack of an incident response plan and a cybersecurity chain of responsibility keeps this function at level 1.
*Evaluated subcategories:*
* ID.AM-1 (Physical Asset Inventory): Level 1 — topology exists but unmanaged
* ID.AM-2 (Software/Service Inventory): Level 1 — services identified, not documented
* ID.RA-1 (Vulnerability Identification): Level 0 — no observable process
* ID.GV-1 (Cybersecurity Policy): Level 0 — absent
* ID.RM-1 (Risk Management Process): Level 0 — absent

**PROTECT (PR) — Score: 1.5/5**
The PROTECT function is partially initiated: VLAN segmentation exists in intent, a RADIUS server is deployed, ACLs are defined, and some switches have port security. These elements indicate an envisioned protection approach. However, the lack of effective application of critical ACLs (VLAN 40), the systematic bypass of VLAN segmentation due to host gateway misconfiguration, the absence of SSH on the L3-Core, the lack of 802.1X, and the absence of management communication encryption practically nullify almost all designed protection controls. The score of 1.5 reflects the existence of an intent to protect without consistent implementation.
*Evaluated subcategories:*
* PR.AC-1 (Identity and Access Management): Level 1 — partial AAA, no enforcement
* PR.AC-3 (Remote Access Management): Level 1 — Telnet dominant, SSH absent on L3-Core
* PR.AC-5 (Network Boundary Protection): Level 1 — ACLs defined but not applied
* PR.DS-1 (Data at Rest Protection): Level 1 — unencrypted FTP
* PR.DS-2 (Data in Transit Protection): Level 1 — unencrypted Telnet
* PR.PT-3 (Network Least Privilege Principle): Level 1 — ports not disabled, CDP active
* PR.IP-1 (Secure Baseline Configuration): Level 1 — no systematic hardening

**DETECT (DE) — Score: 0.5/5**
The DETECT function is nearly non-existent. A syslog server is configured on some devices, but with an IP address error on DMZ-Switch that compromises log reliability. There is no NTP (making log correlation impossible), no IDS/IPS, no SIEM, no network behavior monitoring, and no anomaly detection. The organization is incapable of detecting an intrusion, data exfiltration, or compromise in real-time.
*Evaluated subcategories:*
* DE.CM-1 (Network Monitoring): Level 0 — no IDS/IPS
* DE.CM-7 (Unauthorized Activity Monitoring): Level 1 — partial syslog, unreliable
* DE.AE-1 (Network Activity Baseline): Level 0 — absent
* DE.AE-3 (Event Data Correlation): Level 0 — no SIEM

**RESPOND (RS) — Score: 0.5/5**
The RESPOND function is practically non-existent. No incident response plan is identifiable, no containment procedure, no designated team, no communication process in the event of an incident. The lack of detection capabilities makes response impossible anyway. The organization would not be able to meet the notification deadlines imposed by Article 23 NIS2 (24h/72h/1 month).
*Evaluated subcategories:*
* RS.RP-1 (Incident Response Plan): Level 0 — absent
* RS.CO-2 (Incident Reporting): Level 0 — absent
* RS.MI-1 (Incident Containment): Level 0 — no technical capability
* RS.AN-1 (Incident Investigation): Level 0 — insufficient logs

**RECOVER (RC) — Score: 1.0/5**
The RECOVER function shows an architecture with implicit redundancy at the VLAN level, but no business continuity plan, no configuration backup procedure, no recovery testing, and a single point of failure (SPOF) at every level of the network hierarchy.
*Evaluated subcategories:*
* RC.RP-1 (Recovery Plan): Level 0 — absent
* RC.IM-1 (Integration of Lessons Learned): Level 0 — absent
* RC.CO-3 (Recovery Communication): Level 0 — absent

**CyFun Maturity Summary**
| Function          | Score                                    |
| ----------------- | ---------------------------------------- |
| IDENTIFY          | 1.0/5                                    |
| PROTECT           | 1.5/5                                    |
| DETECT            | 0.5/5                                    |
| RESPOND           | 0.5/5                                    |
| RECOVER           | 1.0/5                                    |
| **OVERALL SCORE** | **1.2/5 — Initial Level (INSUFFICIENT)** |


**SECTION 5 — VERIFICATION OF THE 10 NIS2 ARTICLE 21(2) MEASURES**

**Art. 21(2)(a) — Policies on risk analysis and information system security**
* **Status:** ❌ NON-COMPLIANT
* No formal risk management policy is observable. No documented risk analysis, no critical asset register, no information systems security policy. The organization does not meet the minimum CyFun Basic requirement for this measure.
* **Required Remediation:** Develop a formal IS security policy, conduct an annual risk analysis, and maintain a register of assets and risks.

**Art. 21(2)(b) — Incident handling**
* **Status:** ❌ NON-COMPLIANT
* No incident response plan, no detection procedure, no designated internal or external CSIRT team. The organization lacks the minimal capability to detect, analyze, contain, or notify the CCB of an incident within the required timeframes (24h/72h/1 month per Art. 23).
* **Required Remediation:** Implement an incident response plan, designate a cybersecurity officer, deploy a SIEM, register with the CCB.

**Art. 21(2)(c) — Business continuity and crisis management**
* **Status:** ❌ NON-COMPLIANT
* No Business Continuity Plan (BCP), no Disaster Recovery Plan (DRP), no network configuration backup procedure. Single point of failure at every level of the topology.
* **Required Remediation:** Develop a BCP/DRP, implement automated network configuration backups, deploy redundancy (HSRP/VRRP, redundant links).

**Art. 21(2)(d) — Supply chain security**
* **Status:** ❌ NON-COMPLIANT
* No supplier and partner security management policy. Third-party services (ISP, equipment vendors) are not subject to any documented security assessment.
* **Required Remediation:** Establish a supplier security policy, integrate security clauses into contracts, assess supply chain risks.

**Art. 21(2)(e) — Security in network and information systems acquisition, development and maintenance**
* **Status:** ❌ NON-COMPLIANT
* Network configurations contain systemic errors (VLAN bypass, unapplied ACLs, IP address conflicts) demonstrating a lack of configuration review processes, pre-deployment testing, or change management.
* **Required Remediation:** Implement a change management process (ITIL Change Management), conduct configuration reviews prior to deployment, maintain up-to-date documentation.

**Art. 21(2)(f) — Policies and procedures to assess the effectiveness of cybersecurity risk-management measures**
* **Status:** ❌ NON-COMPLIANT
* No internal audits, no penetration testing, no measurement of security control effectiveness. The discrepancy between the PDF report (claiming active controls) and the reality of the configurations demonstrates a lack of validation of declared measures.
* **Required Remediation:** Set up an annual internal audit program, conduct regular penetration tests, measure security KPIs.

**Art. 21(2)(g) — Basic cyber hygiene practices and cybersecurity training**
* **Status:** ❌ NON-COMPLIANT
* No evidence of a cybersecurity training program, no digital hygiene policy (password management, patching, etc.). Observed passwords are weak and unencrypted on several devices.
* **Required Remediation:** Implement an annual awareness program, enforce a strong password policy, activate `service password-encryption`.

**Art. 21(2)(h) — Policies and procedures regarding the use of cryptography and, where appropriate, encryption**
* **Status:** ❌ NON-COMPLIANT
* Telnet is used instead of SSH on several devices, SSH is not configured on the L3-Core switch, FTP is unencrypted for file transfers, no formal encryption policy exists.
* **Required Remediation:** Disable Telnet on all devices, configure SSH v2, replace FTP with SFTP/SCP, develop an encryption policy.

**Art. 21(2)(i) — Human resources security, access control policies and asset management**
* **Status:** ❌ NON-COMPLIANT
* No 802.1X authentication, access control based solely on weak local passwords, absence of the principle of least privilege, no access lifecycle management.
* **Required Remediation:** Deploy 802.1X on all access ports, implement AAA with RADIUS for all administrative connections, enforce the principle of least privilege.

**Art. 21(2)(j) — The use of multi-factor authentication or continuous authentication solutions, secured voice, video and text communications**
* **Status:** ❌ NON-COMPLIANT
* No multi-factor authentication configured, no secure communication solutions for voice or emergency communications, network management via unencrypted protocols.
* **Required Remediation:** Implement MFA for all administrative logins, deploy secure VPNs for remote access, use encrypted communication channels.

**Overall Article 21(2) Result: 0/10 fully compliant measures**


**SECTION 6 — DETAILED SECURITY FINDINGS**

**FINDING F-01 — VLAN Segmentation Bypass via Gateway Misconfiguration**
* **Severity:** 🔴 CRITICAL
* **CyFun Function:** PROTECT — PR.AC-5: Network access rights and boundary management
* **Required CyFun Assurance Level:** Basic
* **NIS2 Article 21(2):** (e) — Security in IS acquisition, development and maintenance
* **Belgian Law April 26, 2024:** Art. 21 — Cybersecurity risk-management measures
* **MITRE ATT&CK:** T1599 — Network Boundary Bridging
* **Description:** All 30 client workstations configured on VLANs 20 (Production), 30 (Support A), and 40 (Support B) use the default gateway 192.168.10.1, which is the SVI address of VLAN 10 (Management/Study). The correct gateways (192.168.10.33 for VLAN 20, .65 for VLAN 30, .97 for VLAN 40) are not assigned to the hosts. As a result, all traffic from these three VLANs routes through the VLAN 10 SVI, entirely bypassing the inter-VLAN ACLs designed to filter this traffic.
* **Technical Evidence:** On the 30 PCs of VLANs 20, 30, and 40 — observed IP configuration: `Default Gateway: 192.168.10.1` whereas the expected correct gateways are respectively 192.168.10.33, 192.168.10.65, and 192.168.10.97. The corresponding SVIs exist on Switch-L3-CORE but are not used.
* **Impact:** The entire network segmentation logic is negated. A user in VLAN 20 (Production) can directly access internal servers (VLAN 70) or FTP servers (VLAN 80) without being subjected to the intended ACL filtering. The defined security zones have no effectiveness. The confidentiality and integrity of sensitive data are compromised.
* **Recommendation:** Immediately correct the DHCP pool configurations on the DHCP server to assign the correct gateway to each VLAN:
    * Pool VLAN 20: `default-router 192.168.10.33`
    * Pool VLAN 30: `default-router 192.168.10.65`
    * Pool VLAN 40: `default-router 192.168.10.97`
    Also verify the static configurations of hosts that might not be on DHCP. Test inter-VLAN connectivity post-correction to validate ACL filtering.


**FINDING F-02 — CoPP Policy Defined but Not Attached to the Control Plane**
* **Severity:** 🔴 CRITICAL
* **CyFun Function:** PROTECT — PR.PT-3: Principle of least network access
* **Required CyFun Assurance Level:** Important
* **NIS2 Article 21(2):** (e) — Security in IS acquisition, development and maintenance
* **Belgian Law April 26, 2024:** Art. 21
* **MITRE ATT&CK:** T1498 — Network Denial of Service
* **Description:** The Control Plane Policing (CoPP) policy `COPP_POLICY` is fully defined in the configurations of ROUTER-A and Switch-L3-CORE (`class-maps`, `policy-map` with rate-limiting rules), but is never attached to the control plane. The `control-plane` / `service-policy input COPP_POLICY` command is missing on both devices. The control plane thus remains entirely unprotected against flooding and denial-of-service attacks.
* **Technical Evidence:** On ROUTER-A: `policy-map COPP_POLICY` is present in the configuration, but the `control-plane` section is missing or empty — no `service-policy input COPP_POLICY`. Identical on Switch-L3-CORE.
* **Impact:** The CPU of the router and core switch is exposed to volumetric denial-of-service attacks (SYN flood, ICMP flood, ARP storm). Such an attack could paralyze the entire network by exhausting control plane resources. The PDF report indicates this measure as active — this is a major documentary non-compliance.
* **Recommendation:** On ROUTER-A and Switch-L3-CORE, add:
    ```
    control-plane
     service-policy input COPP_POLICY
    ```
    Then verify with `show policy-map control-plane` that the policy is successfully applied and counters are incrementing.


**FINDING F-03 — ACL_SUPPORT_B_IN Defined but Not Applied on Interface VLAN 40**
* **Severity:** 🔴 CRITICAL
* **CyFun Function:** PROTECT — PR.AC-5: Network access rights and boundary management
* **Required CyFun Assurance Level:** Basic
* **NIS2 Article 21(2):** (e) — Security in IS acquisition, development and maintenance
* **Belgian Law April 26, 2024:** Art. 21
* **MITRE ATT&CK:** T1078 — Valid Accounts (unfiltered access to resources)
* **Description:** The access control list named `ACL_SUPPORT_B_IN` is fully configured on Switch-L3-CORE with specific filtering rules for VLAN 40 (Support B). However, the application command `ip access-group ACL_SUPPORT_B_IN in` is missing from the `interface Vlan40` configuration. Consequently, inbound traffic from VLAN 40 is not subjected to any filtering, rendering this ACL completely inoperative.
* **Technical Evidence:** In Switch-L3-CORE configuration: `ip access-list extended ACL_SUPPORT_B_IN` — defined with multiple entries. `Interface Vlan40`: no `ip access-group ACL_SUPPORT_B_IN in` line present.
* **Impact:** Users in VLAN 40 (Support B) enjoy unrestricted access to the entire network, bypassing the limitations set forth by the security policy. This directly contradicts the provided security documentation.
* **Recommendation:** On Switch-L3-CORE, add under the VLAN 40 interface:
    ```
    interface Vlan40
     ip access-group ACL_SUPPORT_B_IN in
    ```
    Also check all other VLAN interfaces to ensure all defined ACLs are actively applied.


**FINDING F-04 — SSH Not Configured on Switch-L3-CORE (Telnet Only)**
* **Severity:** 🔴 CRITICAL
* **CyFun Function:** PROTECT — PR.DS-2: Data in transit protection
* **Required CyFun Assurance Level:** Basic
* **NIS2 Article 21(2):** (h) — Policies regarding cryptography and encryption
* **Belgian Law April 26, 2024:** Art. 21
* **MITRE ATT&CK:** T1040 — Network Sniffing
* **Description:** On Switch-L3-CORE, although the `ip domain-name` parameter is configured (prerequisite for SSH), the `crypto key generate rsa` and `ip ssh version 2` commands are absent from the configuration. The VTY lines are configured with `transport input telnet` or without restriction, making the management of the most critical device in the network exclusively possible via Telnet — a protocol that transmits credentials and data in cleartext.
* **Technical Evidence:** On Switch-L3-CORE: `ip domain-name` present. Absence of: `crypto key generate rsa modulus 2048`, `ip ssh version 2`. VTY lines: `transport input telnet` or unrestricted to SSH.
* **Impact:** Any administration session on the core switch can be captured by an attacker with access to an intermediary network segment, revealing administrative credentials and allowing full compromise of the infrastructure.
* **Recommendation:**
    ```
    Switch-L3-CORE(config)# crypto key generate rsa modulus 2048
    Switch-L3-CORE(config)# ip ssh version 2
    Switch-L3-CORE(config)# line vty 0 15
    Switch-L3-CORE(config-line)# transport input ssh
    Switch-L3-CORE(config-line)# login local
    ```


**FINDING F-05 — IP Address Conflict on VLAN 99 (Switch-L2-C and Switch-L2-D)**
* **Severity:** 🔴 CRITICAL
* **CyFun Function:** IDENTIFY — ID.AM-1: Physical asset inventory and management
* **Required CyFun Assurance Level:** Basic
* **NIS2 Article 21(2):** (e) — Security in IS acquisition, development and maintenance
* **Belgian Law April 26, 2024:** Art. 21
* **MITRE ATT&CK:** T1557 — Adversary-in-the-Middle (ARP conflict exploitation)
* **Description:** Switch-L2-C and Switch-L2-D share the exact same IP address 192.168.10.165 on their management `interface Vlan99`. This address conflict causes ARP instabilities — ARP replies alternate randomly between the two devices, making network management unpredictable and potentially leading to intermittent connectivity losses on the management VLAN.
* **Technical Evidence:** Switch-L2-C: `interface Vlan99 / ip address 192.168.10.165 255.255.255.224`. Switch-L2-D: `interface Vlan99 / ip address 192.168.10.165 255.255.255.224`. Identical address on both devices.
* **Impact:** Persistent ARP instability on VLAN 99 (Management). Inability to reliably manage either switch. Management traffic can be misrouted to the wrong device, creating favorable conditions for an ARP spoofing attack.
* **Recommendation:** Assign unique IP addresses to each switch within the VLAN 99 subnet plan (a /27 offers 30 usable addresses). For example:
    * Switch-L2-C: `192.168.10.166`
    * Switch-L2-D: `192.168.10.167`
    Update network documentation and monitoring configurations accordingly.


**FINDING F-06 — Absence of 802.1X Authentication on Access Ports**
* **Severity:** 🟠 HIGH
* **CyFun Function:** PROTECT — PR.AC-1: Identity and access management (network authentication)
* **Required CyFun Assurance Level:** Important
* **NIS2 Article 21(2):** (i) — Human resources security, access control policies
* **Belgian Law April 26, 2024:** Art. 21
* **MITRE ATT&CK:** T1200 — Hardware Additions (rogue device connection)
* **Description:** Although a RADIUS server (192.168.10.132) is present in the infrastructure, 802.1X authentication is not configured on any of the four access switches. Any device physically plugged into a network port can access the network without any authentication. The presence of the RADIUS server without its use for Network Access Control (NAC) represents an unexploited security investment.
* **Technical Evidence:** On Switch-L2-A, B, C, D: absence of `dot1x system-auth-control`, absence of `aaa authentication dot1x default group radius`, absence of `dot1x port-control auto` on access interfaces.
* **Impact:** An attacker or malicious employee can connect an unauthorized device (personal laptop, Raspberry Pi, network hub) to any available physical port and gain network access corresponding to the VLAN configured on that port.
* **Recommendation:** Enable 802.1X globally and on access ports:
    ```
    aaa new-model
    aaa authentication dot1x default group radius
    dot1x system-auth-control
    interface range Fa0/1-24
     dot1x port-control auto
    ```


**FINDING F-07 — Absence of Port Security on Switch-L2-D**
* **Severity:** 🟠 HIGH
* **CyFun Function:** PROTECT — PR.AC-5: Network boundary protection
* **Required CyFun Assurance Level:** Basic
* **NIS2 Article 21(2):** (e) — Security in IS acquisition and maintenance
* **Belgian Law April 26, 2024:** Art. 21
* **MITRE ATT&CK:** T1557 — Adversary-in-the-Middle (MAC flooding for CAM table exhaustion)
* **Description:** The Port Security feature (limiting authorized MAC addresses per port) is configured on Switch-L2-A, Switch-L2-B, and Switch-L2-C, but is entirely absent on Switch-L2-D. This switch serves VLANs 70 (Internal Servers) and 80 (FTP/iSCSI), which are among the most sensitive in the infrastructure. The lack of port security on this critical equipment is particularly concerning.
* **Technical Evidence:** On Switch-L2-A/B/C: `switchport port-security maximum 2` and `switchport port-security violation restrict` are present. On Switch-L2-D: no `switchport port-security` configuration on Fa0/1-24 interfaces.
* **Impact:** An attacker connected to Switch-L2-D could perform a MAC flooding attack (massively sending frames with randomized MAC addresses) to exhaust the switch's CAM table, forcing it into hub mode and allowing the interception of all traffic on VLANs 70 and 80, including internal server data and FTP transfers.
* **Recommendation:**
    ```
    Switch-L2-D(config)# interface range Fa0/1-24
    Switch-L2-D(config-if-range)# switchport mode access
    Switch-L2-D(config-if-range)# switchport port-security maximum 2
    Switch-L2-D(config-if-range)# switchport port-security
    Switch-L2-D(config-if-range)# switchport port-security violation restrict
    Switch-L2-D(config-if-range)# switchport port-security mac-address sticky
    ```


**FINDING F-08 — Native VLAN 1 Not Pruned from Trunk Links**
* **Severity:** 🟠 HIGH
* **CyFun Function:** PROTECT — PR.AC-5: Network boundary protection
* **Required CyFun Assurance Level:** Basic
* **NIS2 Article 21(2):** (e) — Security in IS acquisition and maintenance
* **Belgian Law April 26, 2024:** Art. 21
* **MITRE ATT&CK:** T1557 — Adversary-in-the-Middle (VLAN hopping)
* **Description:** VLAN 1 (Cisco default VLAN) is not explicitly pruned from the trunk links between switches. While VLAN 1 is not used for user data in this topology, its active presence on trunks allows for VLAN hopping attacks via 802.1Q double-tagging exploitation, enabling an attacker to inject traffic into any VLAN in the infrastructure.
* **Technical Evidence:** On trunk interfaces: absence of `switchport trunk allowed vlan remove 1` or explicit redefinition of the native VLAN using `switchport trunk native vlan [non-1]`.
* **Impact:** Potential exploitation of VLAN 1 for double-tagging attacks, making it possible to reach normally inaccessible VLANs, bypassing network segmentation.
* **Recommendation:**
    ```
    interface [trunk-interface]
     switchport trunk native vlan 99
     switchport trunk allowed vlan remove 1
    ```


**FINDING F-09 — Absence of PortFast and BPDU Guard on Access Ports**
* **Severity:** 🟠 HIGH
* **CyFun Function:** PROTECT — PR.PT-3: Principle of least network access
* **Required CyFun Assurance Level:** Basic
* **NIS2 Article 21(2):** (e) — Security in IS acquisition and maintenance
* **Belgian Law April 26, 2024:** Art. 21
* **MITRE ATT&CK:** T1557 — Adversary-in-the-Middle (STP manipulation)
* **Description:** PortFast and BPDU Guard are not configured on the switch access ports. Without BPDU Guard, an attacker can connect a rogue switch to an access port, send BPDUs with a lower STP priority than the current root bridge, and take control of the Spanning Tree topology, allowing them to intercept all traffic.
* **Technical Evidence:** On Switch-L2-A/B/C/D: absence of `spanning-tree portfast` and `spanning-tree bpduguard enable` on access interfaces. Absence of `spanning-tree portfast default` and `spanning-tree portfast bpduguard default` in global configuration.
* **Impact:** Potential compromise of the Spanning Tree topology leading to a network-wide MITM attack.
* **Recommendation:**
    ```
    spanning-tree portfast default
    spanning-tree portfast bpduguard default
    ```
    Or per interface: `spanning-tree portfast` and `spanning-tree bpduguard enable`.


**FINDING F-10 — Weak Passwords and Absence of Centralized AAA**
* **Severity:** 🟠 HIGH
* **CyFun Function:** PROTECT — PR.AC-1: Identity and access rights management
* **Required CyFun Assurance Level:** Basic
* **NIS2 Article 21(2):** (i) — Human resources security and access control
* **Belgian Law April 26, 2024:** Art. 21
* **MITRE ATT&CK:** T1110 — Brute Force (brute force attack on management access)
* **Description:** Network devices utilize simple local passwords without complexity policies, lockout delays after failed attempts, or centralized AAA authentication via the already-deployed RADIUS server. Passwords observed in the configurations are short and predictable. No administration session timeout is configured.
* **Technical Evidence:** Local `username` configuration with cleartext or weak MD5 passwords. Absence of `aaa new-model`, `aaa authentication login default group radius local`, and uniform `service password-encryption`. Absence of `exec-timeout` on VTY/console lines.
* **Impact:** Compromise of management access via brute-forcing or credential capture over Telnet (see F-11). Untracked and decentralized access to infrastructure equipment.
* **Recommendation:** Implement AAA with RADIUS on all devices, enforce a strong password policy (min. 12 characters, complexity), configure `exec-timeout 5 0` on all VTY lines.


**FINDING F-11 — Telnet Enabled on Multiple Network Devices**
* **Severity:** 🟠 HIGH
* **CyFun Function:** PROTECT — PR.DS-2: Data in transit protection
* **Required CyFun Assurance Level:** Basic
* **NIS2 Article 21(2):** (h) — Policies regarding cryptography and encryption
* **Belgian Law April 26, 2024:** Art. 21
* **MITRE ATT&CK:** T1040 — Network Sniffing
* **Description:** Multiple network devices are configured with `transport input telnet` on their VTY lines, permitting unencrypted management sessions. Telnet transmits all characters (usernames, passwords, configuration commands) in cleartext over the network. Anyone with access to an intermediary network segment can capture this information using a simple protocol analyzer.
* **Technical Evidence:** On Switch-L3-CORE and several access switches: `line vty 0 4` / `transport input telnet` or `transport input all`.
* **Impact:** Trivial capture of credentials and all administration sessions. Possibility of command injection into active Telnet sessions.
* **Recommendation:** On all devices:
    ```
    line vty 0 15
     transport input ssh
    no transport input telnet
    ```
    Prior to this modification, ensure that SSH is configured and functional on each device (see F-04).


**FINDING F-12 — Absence of NTP Synchronization**
* **Severity:** 🟠 HIGH
* **CyFun Function:** DETECT — DE.AE-3: Event data aggregation and correlation
* **Required CyFun Assurance Level:** Basic
* **NIS2 Article 21(2):** (b) — Incident handling
* **Belgian Law April 26, 2024:** Art. 21 and Art. 23 (incident reporting)
* **MITRE ATT&CK:** T1070 — Indicator Removal (forensic investigation difficulty)
* **Description:** No NTP configuration (`ntp server`) is present on network devices. The clocks of the various devices are out of sync, with potentially significant time drift. Under NIS2, Article 23 imposes strict reporting deadlines (24h/72h/1 month) — these deadlines can only be met and justified if logs are precisely timestamped and correlatable.
* **Technical Evidence:** Absence of `ntp server [ip]` on ROUTER-A, Switch-L3-CORE, and access switches.
* **Impact:** Inability to correlate logs across devices to reconstruct the timeline of an incident. Compromised forensic investigation. Potential non-compliance with NIS2 reporting obligations due to a lack of timestamped evidence.
* **Recommendation:**
    ```
    ntp server [INTERNAL_OR_EXTERNAL_NTP_IP]
    clock timezone CET 1
    clock summer-time CEST recurring last Sun Mar 2:00 last Sun Oct 3:00
    ```
    Designate an internal stratum 2 NTP server (ROUTER-A synced to a public server) and configure all devices to synchronize to this internal server.

-------

### CONSTAT F-13 — Double Entrée Syslog Incorrecte sur DMZ-Switch (Typo d'Adresse IP)

- **Sévérité :** 🟠 ÉLEVÉE
- **Fonction CyFun :** DETECT — DE.CM-7 : Surveillance des activités non autorisées
- **Niveau d'assurance CyFun requis :** Basic
- **Article NIS2 21(2) :** (b) — Gestion des incidents
- **Loi belge 26 avril 2024 :** Art. 23 (obligations de déclaration d'incidents)
- **MITRE ATT&CK :** T1562.006 — Impair Defenses: Indicator Blocking
- **Description :** Le commutateur de la DMZ est configuré avec deux destinations syslog : `logging 192.168.10.13` (adresse incorrecte — probable typo, hôte inexistant) et `logging 192.168.10.130` (adresse correcte du serveur DNS/Syslog). Les paquets syslog envoyés vers 192.168.10.13 génèrent des messages d'erreur ICMP unreachable et encombrent les logs réseau. Plus grave, si la double entrée provoque des comportements inattendus dans la queue de logging, certains événements de sécurité de la DMZ pourraient ne pas être enregistrés correctement.
- **Preuve technique :** Sur DMZ-Switch : `logging 192.168.10.13` ET `logging 192.168.10.130` — deux entrées présentes simultanément. L'adresse .13 ne correspond à aucun hôte documenté dans la topologie.
- **Impact :** Perte potentielle d'événements de sécurité critiques générés par les équipements de la DMZ (serveur HTTP exposé). Incapacité à garantir l'intégrité et l'exhaustivité des logs nécessaires à la conformité NIS2.
- **Recommandation :** Supprimer l'entrée incorrecte : `no logging 192.168.10.13`. Vérifier que `logging 192.168.10.130` fonctionne correctement et que le serveur syslog enregistre bien les événements du DMZ-Switch.

---

### CONSTAT F-14 — Absence de SNMPv3 (Protocole de Gestion Non Sécurisé)

- **Sévérité :** 🟠 ÉLEVÉE
- **Fonction CyFun :** PROTECT — PR.DS-2 : Protection des données en transit
- **Niveau d'assurance CyFun requis :** Important
- **Article NIS2 21(2) :** (h) — Politiques relatives à la cryptographie
- **Loi belge 26 avril 2024 :** Art. 21
- **MITRE ATT&CK :** T1040 — Network Sniffing (capture des community strings SNMPv1/v2c)
- **Description :** Si SNMP est utilisé pour la supervision réseau (ce qui est probable dans un environnement professionnel), SNMPv1 et SNMPv2c transmettent les community strings en clair et ne fournissent aucun chiffrement ni authentification forte. SNMPv3 avec authentification (AuthPriv) est la seule version sécurisée. Aucune configuration SNMPv3 n'est observable dans les configurations analysées.
- **Preuve technique :** Absence de `snmp-server user [...] v3 auth sha [...] priv aes 256 [...]` sur les équipements. Absence de `snmp-server group [...] v3 priv`.
- **Impact :** Les community strings SNMP peuvent être capturées et utilisées pour lire (voire écrire) la configuration des équipements réseau à distance.
- **Recommandation :** Désactiver SNMPv1/v2c, implémenter SNMPv3 avec niveau de sécurité AuthPriv (SHA-256 + AES-256). Restreindre les requêtes SNMP aux seuls hôtes de management autorisés via ACL.

---

### CONSTAT F-15 — Isolation Insuffisante de la DMZ (Routage L3 non restreint vers les VLANs Internes)

- **Sévérité :** 🟠 ÉLEVÉE
- **Fonction CyFun :** PROTECT — PR.AC-5 : Protection des limites réseau
- **Niveau d'assurance CyFun requis :** Important
- **Article NIS2 21(2) :** (e) — Sécurité dans l'acquisition et la maintenance des SI
- **Loi belge 26 avril 2024 :** Art. 21
- **MITRE ATT&CK :** T1599 — Network Boundary Bridging
- **Description :** Le VLAN 60 (DMZ) est routé par Switch-L3-CORE au même titre que tous les autres VLANs internes. En l'absence d'ACL strictes et fonctionnelles entre la DMZ et les VLANs internes (70, 80, 10, 20, 30, 40), un serveur compromis dans la DMZ (par exemple le serveur HTTP exposé sur 192.168.10.195) peut initier des connexions directes vers les serveurs internes, les serveurs FTP ou les postes clients. Ceci viole le principe fondamental de la DMZ.
- **Preuve technique :** Le SVI Vlan60 existe sur Switch-L3-CORE avec une adresse de gateway correcte, mais aucune ACL stricte de type `permit tcp [DMZ] [Internal]` établie uniquement (sans retour) n'est identifiée. Le trafic initié depuis la DMZ vers les VLANs internes n'est pas bloqué de manière vérifiable.
- **Impact :** Si le serveur HTTP de la DMZ est compromis (exploitation web, RCE), l'attaquant peut pivoter directement vers le réseau interne sans aucune barrière supplémentaire.
- **Recommandation :** Implémenter des ACL strictes sur le SVI Vlan60 autorisant uniquement le trafic établi (`permit tcp any any established`) depuis les VLANs internes vers la DMZ, et bloquant toute connexion initiée depuis la DMZ vers les réseaux internes (`deny ip 192.168.10.192/27 192.168.10.0/22`).

---

### CONSTAT F-16 — Absence de DHCP Snooping

- **Sévérité :** 🟡 MOYENNE

- **Fonction CyFun :** PROTECT — PR.AC-5 : Protection des limites réseau

- **Niveau d'assurance CyFun requis :** Basic

- **Article NIS2 21(2) :** (e) — Sécurité dans l'acquisition et la maintenance des SI

- **Loi belge 26 avril 2024 :** Art. 21

- **MITRE ATT&CK :** T1557 — Adversary-in-the-Middle (rogue DHCP server)

- **Description :** Le DHCP Snooping n'est configuré sur aucun des commutateurs d'accès. Cette fonctionnalité bloque les réponses DHCP provenant de ports non autorisés (non-trusted), empêchant un attaquant de déployer un serveur DHCP frauduleux qui attribuerait de fausses passerelles ou de faux serveurs DNS aux clients du réseau.

- **Preuve technique :** Absence de `ip dhcp snooping`, `ip dhcp snooping vlan [x]`, et `ip dhcp snooping trust` sur les ports uplink des commutateurs d'accès.

- **Impact :** Possibilité de déploiement d'un serveur DHCP frauduleux, permettant des attaques MITM en redirigeant le trafic des clients via une passerelle contrôlée par l'attaquant.

- **Recommandation :**

  ```
  ip dhcp snooping
  ip dhcp snooping vlan 10,20,30,40,50,60,70,80
  interface [uplink-to-L3-core]
   ip dhcp snooping trust
  ```

---

### CONSTAT F-17 — Absence d'Inspection ARP Dynamique (DAI)

- **Sévérité :** 🟡 MOYENNE

- **Fonction CyFun :** PROTECT — PR.AC-5 : Protection des limites réseau

- **Niveau d'assurance CyFun requis :** Basic

- **Article NIS2 21(2) :** (e) — Sécurité dans l'acquisition et la maintenance des SI

- **Loi belge 26 avril 2024 :** Art. 21

- **MITRE ATT&CK :** T1557 — Adversary-in-the-Middle (ARP poisoning)

- **Description :** La fonctionnalité Dynamic ARP Inspection (DAI) n'est pas configurée sur les commutateurs d'accès. DAI vérifie la cohérence entre les adresses MAC et IP dans les paquets ARP en les comparant à la base de données DHCP Snooping, bloquant les réponses ARP frauduleuses utilisées dans les attaques d'empoisonnement ARP.

- **Preuve technique :** Absence de `ip arp inspection vlan [x]` sur les commutateurs d'accès.

- **Impact :** Attaques ARP poisoning possibles permettant d'intercepter, modifier ou bloquer le trafic entre les hôtes du réseau.

- **Recommandation :**

  ```
  ip arp inspection vlan 10,20,30,40,50,60,70,80
  interface [uplink-to-L3-core]
   ip arp inspection trust
  ```

  Nécessite que DHCP Snooping (F-16) soit préalablement activé.

---

### CONSTAT F-18 — Absence de IP Source Guard

- **Sévérité :** 🟡 MOYENNE
- **Fonction CyFun :** PROTECT — PR.AC-5 : Protection des limites réseau
- **Niveau d'assurance CyFun requis :** Important
- **Article NIS2 21(2) :** (e) — Sécurité dans l'acquisition et la maintenance des SI
- **Loi belge 26 avril 2024 :** Art. 21
- **MITRE ATT&CK :** T1599 — Network Boundary Bridging (IP spoofing)
- **Description :** IP Source Guard n'est pas configuré sur les ports d'accès. Cette fonctionnalité empêche les hôtes d'utiliser une adresse IP différente de celle qui leur a été attribuée par DHCP, bloquant les attaques par usurpation d'adresse IP.
- **Preuve technique :** Absence de `ip verify source` sur les interfaces d'accès des commutateurs.
- **Impact :** Un attaquant peut usurper l'adresse IP d'un hôte légitime pour contourner les ACL basées sur les adresses IP sources.
- **Recommandation :** Activer IP Source Guard sur les ports d'accès après activation de DHCP Snooping : `ip verify source port-security`.

---

### CONSTAT F-19 — Ports Inutilisés Non Désactivés

- **Sévérité :** 🟡 MOYENNE

- **Fonction CyFun :** PROTECT — PR.PT-3 : Principe du moindre accès

- **Niveau d'assurance CyFun requis :** Basic

- **Article NIS2 21(2) :** (e) — Sécurité dans l'acquisition et la maintenance des SI

- **Loi belge 26 avril 2024 :** Art. 21

- **MITRE ATT&CK :** T1200 — Hardware Additions

- **Description :** Les ports non utilisés sur les commutateurs d'accès ne sont pas explicitement mis en état `shutdown`. Tout port actif non connecté constitue un point d'entrée potentiel pour la connexion d'appareils non autorisés. Selon la CIS Benchmark IOS, la désactivation des ports inutilisés est un contrôle de niveau 1 (minimum).

- **Preuve technique :** Sur Switch-L2-A/B/C/D : les interfaces Fa0/x non connectées restent dans l'état administratif `up`. Absence de `shutdown` systématique sur les ports sans appareils connectés.

- **Impact :** Connexion physique possible d'appareils non autorisés sur les ports libres, permettant un accès non contrôlé au VLAN correspondant.

- **Recommandation :**

  ```
  interface range Fa0/[unused-ports]
   shutdown
   switchport mode access
   switchport access vlan 999
  ```

  Créer un VLAN "parking" (ex: VLAN 999) non routé pour y placer les ports désactivés.

---

### CONSTAT F-20 — CDP Activé Globalement (Exposition de la Topologie)

- **Sévérité :** 🟡 MOYENNE

- **Fonction CyFun :** PROTECT — PR.PT-3 : Principe du moindre accès réseau

- **Niveau d'assurance CyFun requis :** Basic

- **Article NIS2 21(2) :** (e) — Sécurité dans l'acquisition et la maintenance des SI

- **Loi belge 26 avril 2024 :** Art. 21

- **MITRE ATT&CK :** T1040 — Network Sniffing (découverte de topologie)

- **Description :** Cisco Discovery Protocol (CDP) est actif par défaut sur tous les équipements Cisco et sur toutes les interfaces. CDP diffuse des informations détaillées sur les équipements (hostname, modèle, version IOS, adresses IP, VLANs configurés) à intervalles réguliers. Ces informations de topologie sont précieuses pour un attaquant en phase de reconnaissance.

- **Preuve technique :** Absence de `no cdp run` global ou de `no cdp enable` par interface sur les ports d'accès des commutateurs.

- **Impact :** Un attaquant connecté sur un port d'accès peut reconstruire la topologie réseau complète en écoutant les annonces CDP, facilitant les phases suivantes d'une attaque.

- **Recommandation :**

  ```
  no cdp run
  ```

  Ou de manière granulaire, désactiver CDP uniquement sur les interfaces facing hôtes : `no cdp enable`. Conserver CDP sur les liaisons inter-équipements pour la supervision si nécessaire.

---

### CONSTAT F-21 — Absence de Bannière de Connexion (Login Banner)

- **Sévérité :** 🟡 MOYENNE

- **Fonction CyFun :** PROTECT — PR.IP-1 : Configuration de base sécurisée

- **Niveau d'assurance CyFun requis :** Basic

- **Article NIS2 21(2) :** (g) — Cyber-hygiène et formation à la cybersécurité

- **Loi belge 26 avril 2024 :** Art. 21

- **MITRE ATT&CK :** N/A

- **Description :** Aucun message de bannière (`banner motd`, `banner login`, `banner exec`) n'est configuré sur les équipements réseau. L'absence de bannière légale sur les équipements d'administration affaiblit la position juridique de l'organisation en cas de poursuite pour accès non autorisé — sans bannière explicite, un accès non autorisé pourrait être qualifié de non intentionnel.

- **Preuve technique :** Absence de `banner motd` ou `banner login` dans les configurations de tous les équipements.

- **Impact :** Vulnérabilité juridique en cas de connexion non autorisée. Impossibilité d'invoquer l'article 550bis du Code pénal belge (accès informatique non autorisé) sans avertissement préalable documenté.

- **Recommandation :**

  ```
  banner motd ^
  AVERTISSEMENT: Accès réservé aux personnels autorisés.
  Toute connexion non autorisée constitue une infraction pénale
  au sens de l'article 550bis du Code pénal belge.
  Les sessions sont enregistrées et surveillées.
  ^
  ```

---

### CONSTAT F-22 — Chiffrement des Mots de Passe Non Systématiquement Activé

- **Sévérité :** 🟡 MOYENNE
- **Fonction CyFun :** PROTECT — PR.DS-1 : Protection des données au repos
- **Niveau d'assurance CyFun requis :** Basic
- **Article NIS2 21(2) :** (h) — Politiques relatives à la cryptographie
- **Loi belge 26 avril 2024 :** Art. 21
- **MITRE ATT&CK :** T1552.001 — Credentials in Files
- **Description :** La commande `service password-encryption` n'est pas systématiquement présente sur tous les équipements. Sans cette commande, les mots de passe de type 0 (en clair) apparaissent en texte lisible dans les fichiers de configuration (`show running-config`), ce qui constitue un risque lors d'opérations de maintenance ou en cas d'accès non autorisé aux fichiers de configuration.
- **Preuve technique :** Absence de `service password-encryption` sur certains équipements. Mots de passe `password [texte_clair]` visibles dans les configurations.
- **Impact :** Exposition des mots de passe d'administration lors de la consultation des configurations. Risque lors de sauvegardes non chiffrées des fichiers de configuration.
- **Recommandation :** `service password-encryption` sur tous les équipements. Utiliser de préférence les mots de passe de type 9 (scrypt) : `enable algorithm-type scrypt secret [password]`.

---

### CONSTAT F-23 — Interface de Gestion HTTP Non Chiffrée Potentiellement Active

- **Sévérité :** 🟡 MOYENNE

- **Fonction CyFun :** PROTECT — PR.DS-2 : Protection des données en transit

- **Niveau d'assurance CyFun requis :** Basic

- **Article NIS2 21(2) :** (h) — Politiques relatives à la cryptographie

- **Loi belge 26 avril 2024 :** Art. 21

- **MITRE ATT&CK :** T1040 — Network Sniffing

- **Description :** Le serveur HTTP de gestion Cisco (`ip http server`) peut être actif par défaut sur certains équipements IOS, permettant une gestion via navigateur web sans chiffrement. Seul `ip http secure-server` (HTTPS) devrait être autorisé si la gestion web est nécessaire.

- **Preuve technique :** Absence de `no ip http server` explicite sur certains équipements. Présence de `ip http server` ou configuration par défaut non sécurisée.

- **Impact :** Session de gestion web non chiffrée capturable sur le réseau.

- **Recommandation :**

  ```
  no ip http server
  ip http secure-server
  ip http access-class [ACL-MGMT] in
  ```

---

### CONSTAT F-24 — Absence d'Authentification OSPF entre ROUTER-A et L3-Core

- **Sévérité :** 🟡 MOYENNE

- **Fonction CyFun :** PROTECT — PR.DS-2 : Protection des données en transit

- **Niveau d'assurance CyFun requis :** Important

- **Article NIS2 21(2) :** (e) — Sécurité dans l'acquisition et la maintenance des SI

- **Loi belge 26 avril 2024 :** Art. 21

- **MITRE ATT&CK :** T1557 — Adversary-in-the-Middle (injection de routes OSPF)

- **Description :** Le protocole de routage OSPF est configuré entre ROUTER-A et Switch-L3-CORE sans authentification MD5 ou SHA. Sans authentification OSPF, un attaquant ayant accès à un segment réseau entre ces équipements pourrait injecter de fausses informations de routage (LSA frauduleux), déviant le trafic vers des équipements sous son contrôle.

- **Preuve technique :** Dans les configurations OSPF de ROUTER-A et Switch-L3-CORE : absence de `ip ospf authentication message-digest`, absence de `ip ospf message-digest-key 1 md5 [key]` sur les interfaces OSPF.

- **Impact :** Manipulation possible de la table de routage de l'infrastructure, permettant des attaques de type black-hole routing ou traffic hijacking.

- **Recommandation :**

  ```
  router ospf 1
   area 0 authentication message-digest
  interface [ospf-interface]
   ip ospf message-digest-key 1 md5 [strong-key]
  ```

---

### CONSTAT F-25 — Absence de Redondance et Points de Défaillance Uniques (SPOF)

- **Sévérité :** 🔵 FAIBLE
- **Fonction CyFun :** RECOVER — RC.RP-1 : Plan de récupération exécuté lors d'incidents
- **Niveau d'assurance CyFun requis :** Important
- **Article NIS2 21(2) :** (c) — Continuité des activités, gestion des sauvegardes et reprise après sinistre
- **Loi belge 26 avril 2024 :** Art. 21
- **MITRE ATT&CK :** N/A
- **Description :** L'architecture réseau présente des points de défaillance uniques à chaque niveau : liaison unique ISP, routeur de bordure unique, commutateur cœur unique, sans HSRP/VRRP configuré. La défaillance de l'un quelconque de ces équipements entraîne une interruption de service totale pour les utilisateurs concernés. Cette architecture ne satisfait pas aux exigences de disponibilité requises pour une entité NIS2.
- **Preuve technique :** Topologie en étoile avec un seul chemin entre chaque niveau. Absence de `standby [group] ip [virtual-ip]` (HSRP) ou `vrrp [group] ip [virtual-ip]` (VRRP).
- **Impact :** Indisponibilité totale du réseau en cas de panne d'un équipement critique (ROUTER-A ou Switch-L3-CORE). Non-conformité aux exigences de continuité NIS2 Art. 21(2)(c).
- **Recommandation :** Prévoir une architecture redondante à long terme : second routeur de bordure, second commutateur cœur avec HSRP/VRRP, liens agrégés (EtherChannel/LACP) entre équipements critiques.

---

### CONSTAT F-26 — Configuration de Logging Incomplète (Niveaux et Buffer Non Définis)

- **Sévérité :** 🔵 FAIBLE

- **Fonction CyFun :** DETECT — DE.CM-7 : Surveillance des activités non autorisées

- **Niveau d'assurance CyFun requis :** Basic

- **Article NIS2 21(2) :** (b) — Gestion des incidents

- **Loi belge 26 avril 2024 :** Art. 23

- **MITRE ATT&CK :** T1070 — Indicator Removal (logs insuffisants pour la détection)

- **Description :** Si des destinations syslog sont configurées sur certains équipements, les niveaux de logging (`logging trap [level]`), la taille du buffer local (`logging buffered [size]`), et le niveau de sévérité des messages locaux (`logging console [level]`) ne sont pas systématiquement définis. Par défaut, le niveau informational (6) est utilisé, mais sans configuration explicite, certains messages critiques pourraient être manqués selon la configuration par défaut de l'équipement.

- **Preuve technique :** Absence de `logging buffered 16384`, `logging trap notifications`, `logging source-interface Vlan99` sur plusieurs équipements.

- **Impact :** Logs incomplets ou manquants pour la détection d'incidents et la conformité NIS2. Incapacité à reconstituer des chronologies d'incidents.

- **Recommandation :**

  ```
  logging buffered 16384
  logging trap notifications
  logging source-interface Vlan99
  logging host 192.168.10.130
  ```

---

## SECTION 7 — TABLEAU RÉCAPITULATIF DES RISQUES

| ID   | Titre                                      | Sévérité   | Fonction CyFun | Sous-catégorie | NIS2 Art. 21(2) | Statut         |
| ---- | ------------------------------------------ | ---------- | -------------- | -------------- | --------------- | -------------- |
| F-01 | Bypass segmentation VLAN (passerelles)     | 🔴 CRITIQUE | PROTECT        | PR.AC-5        | (e)             | ❌ Non conforme |
| F-02 | CoPP non attaché au plan de contrôle       | 🔴 CRITIQUE | PROTECT        | PR.PT-3        | (e)             | ❌ Non conforme |
| F-03 | ACL_SUPPORT_B_IN non appliquée VLAN 40     | 🔴 CRITIQUE | PROTECT        | PR.AC-5        | (e)             | ❌ Non conforme |
| F-04 | SSH absent sur L3-Core (Telnet seul)       | 🔴 CRITIQUE | PROTECT        | PR.DS-2        | (h)             | ❌ Non conforme |
| F-05 | Conflit IP VLAN 99 (L2-C et L2-D)          | 🔴 CRITIQUE | IDENTIFY       | ID.AM-1        | (e)             | ❌ Non conforme |
| F-06 | Absence 802.1X sur ports d'accès           | 🟠 ÉLEVÉE   | PROTECT        | PR.AC-1        | (i)             | ❌ Non conforme |
| F-07 | Port Security absent sur Switch-L2-D       | 🟠 ÉLEVÉE   | PROTECT        | PR.AC-5        | (e)             | ❌ Non conforme |
| F-08 | VLAN 1 natif non purgé des trunks          | 🟠 ÉLEVÉE   | PROTECT        | PR.AC-5        | (e)             | ❌ Non conforme |
| F-09 | Absence PortFast/BPDU Guard                | 🟠 ÉLEVÉE   | PROTECT        | PR.PT-3        | (e)             | ❌ Non conforme |
| F-10 | Mots de passe faibles, AAA absent          | 🟠 ÉLEVÉE   | PROTECT        | PR.AC-1        | (i)             | ❌ Non conforme |
| F-11 | Telnet activé sur équipements réseau       | 🟠 ÉLEVÉE   | PROTECT        | PR.DS-2        | (h)             | ❌ Non conforme |
| F-12 | Absence de synchronisation NTP             | 🟠 ÉLEVÉE   | DETECT         | DE.AE-3        | (b)             | ❌ Non conforme |
| F-13 | Double syslog incorrect sur DMZ-Switch     | 🟠 ÉLEVÉE   | DETECT         | DE.CM-7        | (b)             | ❌ Non conforme |
| F-14 | SNMPv3 absent (gestion non sécurisée)      | 🟠 ÉLEVÉE   | PROTECT        | PR.DS-2        | (h)             | ❌ Non conforme |
| F-15 | DMZ insuffisamment isolée                  | 🟠 ÉLEVÉE   | PROTECT        | PR.AC-5        | (e)             | ❌ Non conforme |
| F-16 | DHCP Snooping absent                       | 🟡 MOYENNE  | PROTECT        | PR.AC-5        | (e)             | ❌ Non conforme |
| F-17 | DAI (Dynamic ARP Inspection) absent        | 🟡 MOYENNE  | PROTECT        | PR.AC-5        | (e)             | ❌ Non conforme |
| F-18 | IP Source Guard absent                     | 🟡 MOYENNE  | PROTECT        | PR.AC-5        | (e)             | ❌ Non conforme |
| F-19 | Ports inutilisés non désactivés            | 🟡 MOYENNE  | PROTECT        | PR.PT-3        | (e)             | ❌ Non conforme |
| F-20 | CDP actif globalement                      | 🟡 MOYENNE  | PROTECT        | PR.PT-3        | (e)             | ❌ Non conforme |
| F-21 | Absence de bannière de connexion           | 🟡 MOYENNE  | PROTECT        | PR.IP-1        | (g)             | ❌ Non conforme |
| F-22 | Chiffrement mots de passe non systématique | 🟡 MOYENNE  | PROTECT        | PR.DS-1        | (h)             | ❌ Non conforme |
| F-23 | HTTP management non chiffré                | 🟡 MOYENNE  | PROTECT        | PR.DS-2        | (h)             | ❌ Non conforme |
| F-24 | OSPF sans authentification                 | 🟡 MOYENNE  | PROTECT        | PR.DS-2        | (e)             | ❌ Non conforme |
| F-25 | Absence de redondance (SPOF)               | 🔵 FAIBLE   | RECOVER        | RC.RP-1        | (c)             | ❌ Non conforme |
| F-26 | Logging incomplet (niveaux/buffer)         | 🔵 FAIBLE   | DETECT         | DE.CM-7        | (b)             | ❌ Non conforme |

**Distribution des sévérités :**

- 🔴 CRITIQUE : 5 (19%)
- 🟠 ÉLEVÉE : 10 (38%)
- 🟡 MOYENNE : 9 (35%)
- 🔵 FAIBLE : 2 (8%)

---

## SECTION 8 — OBLIGATIONS DE DÉCLARATION D'INCIDENTS (Art. 23 NIS2 / Loi belge)

### 8.1 Cadre réglementaire

L'article 23 de la directive NIS2, transposé par la Loi belge du 26 avril 2024, impose un processus de notification des incidents en trois étapes pour les incidents ayant un impact significatif :

| Étape                       | Délai                            | Contenu requis                                               | Destinataire                                |
| --------------------------- | -------------------------------- | ------------------------------------------------------------ | ------------------------------------------- |
| **Alerte précoce**          | **24 heures** après connaissance | Mention que l'incident a eu lieu, préciser si malveillant, impact transfrontalier potentiel | CCB (Centre pour la Cybersécurité Belgique) |
| **Notification d'incident** | **72 heures** après connaissance | Évaluation initiale (sévérité, impact), indicateurs de compromission si disponibles | CCB                                         |
| **Rapport final**           | **1 mois** après notification    | Description complète, mesures prises, évaluation transfrontalière | CCB                                         |

### 8.2 Capacité actuelle de l'organisation

**Capacité de détection :** ❌ INSUFFISANTE

- Aucun SIEM pour corréler les événements
- Aucun IDS/IPS pour détecter les intrusions
- Logs non synchronisés (NTP absent)
- Syslog partiellement configuré et non fiable (cf. F-13)

**Capacité de réponse :** ❌ INSUFFISANTE

- Aucun plan de réponse aux incidents documenté
- Aucune équipe CSIRT désignée
- Aucune procédure de confinement définie
- Aucun canal de communication d'urgence identifié

**Capacité de reporting :** ❌ INSUFFISANTE

- Aucune procédure de notification au CCB
- Contact CCB non identifié dans les documents de l'organisation
- Aucun registre des incidents

**Conclusion : L'organisation est actuellement dans l'incapacité de respecter les obligations de notification NIS2 Art. 23. Ceci constitue une non-conformité réglementaire majeure exposant l'organisation à des sanctions immédiates en cas d'incident.**

### 8.3 Seuil d'incident significatif

Un incident est considéré comme significatif (art. 23(3) NIS2) s'il :

- A causé ou est susceptible de causer des perturbations opérationnelles graves
- A causé des pertes financières à l'entité
- A affecté ou est susceptible d'affecter d'autres personnes en causant des dommages matériels ou immatériels considérables

Compte tenu des vulnérabilités critiques identifiées (bypass VLAN, CoPP absent, DMZ mal isolée), un incident exploitant ces failles serait quasi certainement qualifié de significatif.

---

## SECTION 9 — RESPONSABILITÉ DE LA DIRECTION (Art. 20 NIS2 / Loi belge du 26 avril 2024)

### 9.1 Principe de responsabilité managériale

L'article 20 de la directive NIS2, transposé dans la Loi belge, introduit une **responsabilité personnelle des organes de direction** en matière de cybersécurité. Les membres du conseil d'administration et de la direction générale sont personnellement responsables de l'approbation et de la supervision des mesures de gestion des risques de cybersécurité.

### 9.2 Obligations spécifiques de la direction

| Obligation                                                   | Statut chez Byte Sized Solutions                            |
| ------------------------------------------------------------ | ----------------------------------------------------------- |
| Approuver les mesures de sécurité (art. 20(1))               | ❌ Non documenté                                             |
| Superviser la mise en œuvre (art. 20(1))                     | ❌ Défaillances constatées (ACL non appliquées, CoPP absent) |
| Suivre une formation cybersécurité (art. 20(2))              | ❌ Aucune preuve de formation                                |
| Proposer des formations aux employés (art. 20(2))            | ❌ Absent                                                    |
| Assurer l'enregistrement auprès de l'autorité compétente (CCB) | ❌ Non vérifié                                               |

### 9.3 Conséquences en cas de manquement

En cas d'incident de sécurité significatif et de défaillance avérée dans la supervision de la cybersécurité, les membres de la direction peuvent faire l'objet de :

- **Sanctions administratives** infligées à titre personnel
- **Interdiction temporaire** d'exercer des fonctions de direction dans des entités essentielles/importantes (art. 32(5) NIS2 pour entités essentielles)
- **Déclaration publique** des manquements (naming and shaming) par le CCB
- En droit belge : responsabilité civile potentielle si des tiers subissent des dommages du fait des manquements de sécurité

---

## SECTION 10 — SANCTIONS POTENTIELLES

### 10.1 Amendes administratives

| Type d'entité                                | Amende maximale | Base de calcul                                   |
| -------------------------------------------- | --------------- | ------------------------------------------------ |
| Entité essentielle                           | 10 000 000 €    | ou 2% du CA mondial annuel (le plus élevé)       |
| **Entité importante (Byte Sized Solutions)** | **7 000 000 €** | **ou 1,4% du CA mondial annuel (le plus élevé)** |

### 10.2 Pouvoirs d'exécution du CCB

En tant qu'autorité compétente nationale désignée par la Loi belge du 26 avril 2024, le CCB dispose des pouvoirs suivants à l'encontre de Byte Sized Solutions :

- **Audits de sécurité** sur pièces ou sur place
- **Injonctions** de mise en conformité avec délai
- **Mesures provisoires** (suspension d'activités) en cas de risque immédiat
- **Amendes administratives** graduées selon la gravité
- **Publication des décisions** de sanction (effet dissuasif/réputationnel)
- Pour les entités essentielles uniquement : suspension des certifications

### 10.3 Facteurs aggravants applicables au présent audit

Les éléments suivants constituent des facteurs aggravants dans l'appréciation des sanctions :

- **Écart documentaire** : Le rapport PDF fourni déclare des contrôles actifs (CoPP, ACL VLAN 40) qui ne sont pas effectivement configurés — ceci peut être qualifié de **déclaration trompeuse** auprès des autorités
- **Gravité et durée** des manquements (architecture fondamentale défaillante)
- **Nombre de manquements** (26 constats, dont 5 critiques)
- **0/10** des mesures NIS2 Art. 21(2) satisfaites

---

## SECTION 11 — POINTS POSITIFS

Malgré le niveau de maturité insuffisant constaté, l'audit a identifié dix éléments positifs témoignant d'une démarche initiale de sécurisation :

1. **Architecture VLAN structurée** : La conception en 9 VLANs distincts (Management, Production, Support, DMZ, Serveurs, FTP, Printers, Network Mgmt) démontre une compréhension des principes de segmentation réseau et constitue une base solide pour une sécurité effective, sous réserve de correction des bugs de configuration.

2. **Serveur RADIUS déployé** : La présence d'un serveur RADIUS (192.168.10.132) constitue l'infrastructure nécessaire pour implémenter l'authentification 802.1X et l'AAA centralisé — il suffit de l'utiliser effectivement.

3. **ACL définies (partiellement)** : Des ACL étendues sont définies sur Switch-L3-CORE pour plusieurs VLANs. L'infrastructure de filtrage existe — il faut corriger leur application et leur cohérence.

4. **Syslog configuré** : Des destinations syslog sont définies sur les équipements (malgré l'erreur sur DMZ-Switch), témoignant d'une intention de centralisation des logs.

5. **Port Security sur 3/4 commutateurs** : Switch-L2-A, B et C disposent d'une configuration Port Security, limitant le risque de MAC flooding sur ces équipements.

6. **Zone DMZ créée** : Le VLAN 60 (DMZ) existe avec un serveur HTTP dédié, démontrant la compréhension du concept de zone démilitarisée pour les services exposés.

7. **VLAN 99 de management séparé** : L'existence d'un VLAN de management dédié (99) pour l'administration des équipements est une bonne pratique de segmentation administrative.

8. **OSPF configuré** : Le routage dynamique OSPF entre ROUTER-A et L3-Core est fonctionnel, facilitant la gestion du routage inter-VLAN.

9. **DHCP centralisé** : Un serveur DHCP centralisé est déployé et fonctionnel, facilitant la gestion des adresses IP (bien que les pools DHCP nécessitent correction des passerelles).

10. **Politique CoPP définie** : La policy-map CoPP est entièrement configurée avec des classes de trafic appropriées — il ne manque que l'attachement au plan de contrôle pour qu'elle soit opérationnelle.

---

## SECTION 12 — FEUILLE DE ROUTE DE REMÉDIATION

### Phase 1 — Actions CRITIQUES IMMÉDIATES (J+0 à J+30)

**Objectif CyFun : Atteindre le niveau Basic sur PR.AC-5, PR.DS-2, ID.AM-1**

| Action                                                     | Constat | Priorité   | Effort  |
| ---------------------------------------------------------- | ------- | ---------- | ------- |
| Corriger les passerelles DHCP (VLANs 20/30/40)             | F-01    | 🔴 CRITIQUE | Faible  |
| Attacher la politique CoPP au plan de contrôle             | F-02    | 🔴 CRITIQUE | Faible  |
| Appliquer ACL_SUPPORT_B_IN sur interface Vlan40            | F-03    | 🔴 CRITIQUE | Faible  |
| Configurer SSH v2 sur Switch-L3-CORE                       | F-04    | 🔴 CRITIQUE | Faible  |
| Corriger le conflit d'adresse IP VLAN 99                   | F-05    | 🔴 CRITIQUE | Faible  |
| Supprimer l'entrée syslog incorrecte sur DMZ-Switch        | F-13    | 🟠 ÉLEVÉE   | Trivial |
| Désactiver Telnet sur tous les équipements                 | F-11    | 🟠 ÉLEVÉE   | Faible  |
| Activer `service password-encryption` sur tous équipements | F-22    | 🟡 MOYENNE  | Trivial |
| Désactiver les ports inutilisés                            | F-19    | 🟡 MOYENNE  | Faible  |
| Configurer NTP sur tous les équipements                    | F-12    | 🟠 ÉLEVÉE   | Faible  |
| Ajouter les bannières de connexion légales                 | F-21    | 🟡 MOYENNE  | Trivial |
| Désactiver CDP sur les ports d'accès                       | F-20    | 🟡 MOYENNE  | Faible  |

**Livrable Phase 1 :** Rapport de vérification des configurations corrigées. Score CyFun estimé après Phase 1 : 1,8/5.

### Phase 2 — Sécurisation des Accès et Authentification (J+30 à J+90)

**Objectif CyFun : Atteindre le niveau Basic sur PR.AC-1, PR.AC-3, PR.IP-1**

| Action                                                       | Constat           | Effort        |
| ------------------------------------------------------------ | ----------------- | ------------- |
| Déployer 802.1X sur tous les ports d'accès                   | F-06              | Élevé         |
| Configurer AAA centralisé avec RADIUS sur tous équipements   | F-10              | Moyen         |
| Configurer Port Security sur Switch-L2-D                     | F-07              | Faible        |
| Purger VLAN 1 des trunks, changer VLAN natif                 | F-08              | Faible        |
| Activer PortFast + BPDU Guard sur ports d'accès              | F-09              | Faible        |
| Configurer ACL strictes DMZ → VLANs internes                 | F-15              | Moyen         |
| Authentification OSPF MD5                                    | F-24              | Faible        |
| Désactiver `ip http server`, activer `ip http secure-server` | F-23              | Trivial       |
| Implémenter SNMPv3 AuthPriv                                  | F-14              | Moyen         |
| S'enregistrer auprès du CCB                                  | Art. 27 Loi belge | Administratif |

**Livrable Phase 2 :** Test de pénétration interne validant les corrections. Score CyFun estimé : 2,5/5.

### Phase 3 — Détection et Surveillance (J+90 à J+180)

**Objectif CyFun : Atteindre le niveau Basic sur DE.CM-1, DE.CM-7, DE.AE-3**

| Action                                                       | Constat      | Effort |
| ------------------------------------------------------------ | ------------ | ------ |
| Déployer DHCP Snooping                                       | F-16         | Faible |
| Déployer DAI (Dynamic ARP Inspection)                        | F-17         | Faible |
| Déployer IP Source Guard                                     | F-18         | Faible |
| Déployer un SIEM (ex: Graylog, ELK, Splunk)                  | F-12, F-26   | Élevé  |
| Déployer un IDS/IPS réseau                                   | —            | Élevé  |
| Configurer le logging complet (niveaux, buffer, source-interface) | F-26         | Faible |
| Élaborer et tester un Plan de Réponse aux Incidents          | Art. 23 NIS2 | Élevé  |
| Former la direction à la cybersécurité NIS2 (Art. 20)        | Art. 20      | Moyen  |

**Livrable Phase 3 :** Tableau de bord de surveillance opérationnel, PRI documenté et testé. Score CyFun estimé : 3,0/5.

### Phase 4 — Résilience et Amélioration Continue (J+180 à J+365)

**Objectif CyFun : Atteindre le niveau Important sur toutes les fonctions**

| Action                                                     | Constat        | Effort     |
| ---------------------------------------------------------- | -------------- | ---------- |
| Déployer redondance réseau (HSRP/VRRP, liens redondants)   | F-25           | Très élevé |
| Conduire un audit de sécurité externe annuel               | Art. 21(2)(f)  | Moyen      |
| Mettre en place un programme de gestion des vulnérabilités | ID.RA-1        | Moyen      |
| Élaborer une politique de gestion des fournisseurs         | Art. 21(2)(d)  | Moyen      |
| Mettre en œuvre MFA pour tous les accès d'administration   | Art. 21(2)(j)  | Moyen      |
| Conduire un exercice de simulation d'incident (tabletop)   | Art. 21(2)(b)  | Moyen      |
| Réviser et mettre à jour la documentation réseau           | ID.AM-1        | Faible     |
| Obtenir la certification ISO 27001 ou équivalent           | Recommandation | Très élevé |

**Livrable Phase 4 :** Rapport de conformité NIS2 annuel soumis au CCB. Score CyFun cible : 3,5/5 (niveau Établi).

---

## SECTION 13 — CONCLUSION

Le présent audit de conformité au cadre CyberFundamentals (CyFun) du CCB et à la directive NIS2, telle que transposée par la Loi belge du 26 avril 2024, révèle une situation de **non-conformité généralisée** de l'infrastructure réseau de Byte Sized Solutions. Avec un score de maturité cybersécurité de **1,2/5** (Niveau Initial — INSUFFISANT) et **0 des 10 mesures obligatoires de l'article 21(2) NIS2** pleinement satisfaites, l'organisation se trouve dans une situation de risque légal, opérationnel et réputationnel significatif.

Cinq constats de sévérité critique ont été identifiés, dont trois résultent d'erreurs de configuration simples (passerelles DHCP incorrectes, CoPP non attaché, ACL non appliquée) qui pourraient être corrigées en moins d'une journée de travail technique. Ces corrections immédiates permettraient d'activer des mécanismes de sécurité déjà conçus et partiellement configurés, apportant une amélioration substantielle du niveau de sécurité à coût marginal très faible. La présence d'un serveur RADIUS déployé mais inutilisé, d'une architecture VLAN structurée, et d'une politique CoPP définie mais non activée démontre que les fondations d'une sécurité appropriée existent — c'est leur mise en œuvre effective qui fait défaut.

L'organisation est appelée à traiter en priorité absolue les cinq constats critiques dans un délai de 30 jours, à s'enregistrer auprès du CCB conformément à l'article 27 de la Loi belge du 26 avril 2024, et à mettre en place une capacité minimale de détection et de reporting d'incidents conformément à l'article 23 NIS2. Le non-respect de ces obligations expose les dirigeants de l'organisation à une **responsabilité personnelle** et l'organisation à des **sanctions administratives pouvant atteindre 7 millions d'euros**. La feuille de route en quatre phases proposée dans ce rapport constitue un chemin pragmatique et réaliste vers la conformité NIS2, avec un score CyFun cible de 3,5/5 atteignable en 12 mois.

---

### FINDING F-13 — Dual Incorrect Syslog Entry on DMZ-Switch (IP Address Typo)

- **Severity:** 🟠 HIGH
- **CyFun Function:** DETECT — DE.CM-7: Monitoring for unauthorized personnel, connections, devices, and software
- **Required CyFun Assurance Level:** Basic
- **NIS2 Article 21(2):** (b) — Incident handling
- **Belgian Law of April 26, 2024:** Art. 23 (incident reporting obligations)
- **MITRE ATT&CK:** T1562.006 — Impair Defenses: Indicator Blocking
- **Description:** The DMZ switch is configured with two syslog destinations: `logging 192.168.10.13` (incorrect address — probable typo, non-existent host) and `logging 192.168.10.130` (correct DNS/Syslog server address). Syslog packets sent to 192.168.10.13 generate ICMP unreachable error messages and clutter the network logs. More seriously, if the dual entry causes unexpected behaviors in the logging queue, some DMZ security events might not be recorded properly.
- **Technical Evidence:** On DMZ-Switch: `logging 192.168.10.13` AND `logging 192.168.10.130` — two entries present simultaneously. The .13 address does not correspond to any documented host in the topology.
- **Impact:** Potential loss of critical security events generated by DMZ equipment (exposed HTTP server). Inability to guarantee the integrity and completeness of logs required for NIS2 compliance.
- **Recommendation:** Remove the incorrect entry: `no logging 192.168.10.13`. Verify that `logging 192.168.10.130` works correctly and that the syslog server properly records events from the DMZ-Switch.

------

### FINDING F-14 — Absence of SNMPv3 (Unsecured Management Protocol)

- **Severity:** 🟠 HIGH
- **CyFun Function:** PROTECT — PR.DS-2: Data-in-transit is protected
- **Required CyFun Assurance Level:** Important
- **NIS2 Article 21(2):** (h) — Policies and procedures regarding the use of cryptography
- **Belgian Law of April 26, 2024:** Art. 21
- **MITRE ATT&CK:** T1040 — Network Sniffing (capturing SNMPv1/v2c community strings)
- **Description:** If SNMP is used for network monitoring (which is probable in a professional environment), SNMPv1 and SNMPv2c transmit community strings in plaintext and provide no encryption or strong authentication. SNMPv3 with authentication (AuthPriv) is the only secure version. No SNMPv3 configuration is observable in the analyzed configurations.
- **Technical Evidence:** Absence of `snmp-server user [...] v3 auth sha [...] priv aes 256 [...]` on the devices. Absence of `snmp-server group [...] v3 priv`.
- **Impact:** SNMP community strings can be captured and used to remotely read (or even write) network device configurations.
- **Recommendation:** Disable SNMPv1/v2c, implement SNMPv3 with AuthPriv security level (SHA-256 + AES-256). Restrict SNMP queries solely to authorized management hosts via ACLs.

------

### FINDING F-15 — Insufficient DMZ Isolation (Unrestricted L3 Routing to Internal VLANs)

- **Severity:** 🟠 HIGH
- **CyFun Function:** PROTECT — PR.AC-5: Network integrity is protected
- **Required CyFun Assurance Level:** Important
- **NIS2 Article 21(2):** (e) — Security in network and information systems acquisition, development and maintenance
- **Belgian Law of April 26, 2024:** Art. 21
- **MITRE ATT&CK:** T1599 — Network Boundary Bridging
- **Description:** VLAN 60 (DMZ) is routed by Switch-L3-CORE in the same way as all other internal VLANs. In the absence of strict and functional ACLs between the DMZ and internal VLANs (70, 80, 10, 20, 30, 40), a compromised server in the DMZ (for example, the exposed HTTP server on 192.168.10.195) can initiate direct connections to internal servers, FTP servers, or client workstations. This violates the fundamental principle of the DMZ.
- **Technical Evidence:** SVI Vlan60 exists on Switch-L3-CORE with a correct gateway address, but no strict ACL of type `permit tcp [DMZ] [Internal]` established only (without return) is identified. Traffic initiated from the DMZ to internal VLANs is not verifiably blocked.
- **Impact:** If the DMZ HTTP server is compromised (web exploitation, RCE), the attacker can pivot directly into the internal network without any additional barrier.
- **Recommendation:** Implement strict ACLs on SVI Vlan60 allowing only established traffic (`permit tcp any any established`) from internal VLANs to the DMZ, and blocking any connection initiated from the DMZ to internal networks (`deny ip 192.168.10.192/27 192.168.10.0/22`).

------

### FINDING F-16 — Absence of DHCP Snooping

- **Severity:** 🟡 MEDIUM

- **CyFun Function:** PROTECT — PR.AC-5: Network integrity is protected

- **Required CyFun Assurance Level:** Basic

- **NIS2 Article 21(2):** (e) — Security in network and information systems acquisition, development and maintenance

- **Belgian Law of April 26, 2024:** Art. 21

- **MITRE ATT&CK:** T1557 — Adversary-in-the-Middle (rogue DHCP server)

- **Description:** DHCP Snooping is not configured on any of the access switches. This feature blocks DHCP responses coming from unauthorized (untrusted) ports, preventing an attacker from deploying a rogue DHCP server that would assign fake gateways or fake DNS servers to network clients.

- **Technical Evidence:** Absence of `ip dhcp snooping`, `ip dhcp snooping vlan [x]`, and `ip dhcp snooping trust` on the uplink ports of the access switches.

- **Impact:** Possibility of deploying a rogue DHCP server, allowing MITM attacks by redirecting client traffic through a gateway controlled by the attacker.

- **Recommendation:**

  ```
  ip dhcp snooping
  ip dhcp snooping vlan 10,20,30,40,50,60,70,80
  interface [uplink-to-L3-core]
   ip dhcp snooping trust
  ```

------

### FINDING F-17 — Absence of Dynamic ARP Inspection (DAI)

- **Severity:** 🟡 MEDIUM

- **CyFun Function:** PROTECT — PR.AC-5: Network integrity is protected

- **Required CyFun Assurance Level:** Basic

- **NIS2 Article 21(2):** (e) — Security in network and information systems acquisition, development and maintenance

- **Belgian Law of April 26, 2024:** Art. 21

- **MITRE ATT&CK:** T1557 — Adversary-in-the-Middle (ARP poisoning)

- **Description:** The Dynamic ARP Inspection (DAI) feature is not configured on the access switches. DAI verifies the consistency between MAC and IP addresses in ARP packets by comparing them to the DHCP Snooping database, blocking fraudulent ARP responses used in ARP poisoning attacks.

- **Technical Evidence:** Absence of `ip arp inspection vlan [x]` on the access switches.

- **Impact:** Potential ARP poisoning attacks allowing interception, modification, or blocking of traffic between network hosts.

- **Recommendation:**

  ```
  ip arp inspection vlan 10,20,30,40,50,60,70,80
  interface [uplink-to-L3-core]
   ip arp inspection trust
  ```

  Requires DHCP Snooping (F-16) to be enabled beforehand.

------

### FINDING F-18 — Absence of IP Source Guard

- **Severity:** 🟡 MEDIUM
- **CyFun Function:** PROTECT — PR.AC-5: Network integrity is protected
- **Required CyFun Assurance Level:** Important
- **NIS2 Article 21(2):** (e) — Security in network and information systems acquisition, development and maintenance
- **Belgian Law of April 26, 2024:** Art. 21
- **MITRE ATT&CK:** T1599 — Network Boundary Bridging (IP spoofing)
- **Description:** IP Source Guard is not configured on the access ports. This feature prevents hosts from using an IP address different from the one assigned to them by DHCP, blocking IP address spoofing attacks.
- **Technical Evidence:** Absence of `ip verify source` on the access interfaces of the switches.
- **Impact:** An attacker can spoof the IP address of a legitimate host to bypass ACLs based on source IP addresses.
- **Recommendation:** Enable IP Source Guard on access ports after enabling DHCP Snooping: `ip verify source port-security`.

------

### FINDING F-19 — Unused Ports Not Disabled

- **Severity:** 🟡 MEDIUM

- **CyFun Function:** PROTECT — PR.PT-3: Principle of least privilege

- **Required CyFun Assurance Level:** Basic

- **NIS2 Article 21(2):** (e) — Security in network and information systems acquisition, development and maintenance

- **Belgian Law of April 26, 2024:** Art. 21

- **MITRE ATT&CK:** T1200 — Hardware Additions

- **Description:** Unused ports on the access switches are not explicitly put into a `shutdown` state. Any active, unconnected port constitutes a potential entry point for the connection of unauthorized devices. According to the CIS Benchmark for IOS, disabling unused ports is a Level 1 (minimum) control.

- **Technical Evidence:** On Switch-L2-A/B/C/D: unconnected Fa0/x interfaces remain in the `up` administrative state. Absence of systematic `shutdown` on ports without connected devices.

- **Impact:** Possible physical connection of unauthorized devices on free ports, allowing uncontrolled access to the corresponding VLAN.

- **Recommendation:**

  ```
  interface range Fa0/[unused-ports]
   shutdown
   switchport mode access
   switchport access vlan 999
  ```

  Create an unrouted "parking" VLAN (e.g., VLAN 999) to place disabled ports.

------

### FINDING F-20 — CDP Enabled Globally (Topology Exposure)

- **Severity:** 🟡 MEDIUM

- **CyFun Function:** PROTECT — PR.PT-3: Principle of least privilege

- **Required CyFun Assurance Level:** Basic

- **NIS2 Article 21(2):** (e) — Security in network and information systems acquisition, development and maintenance

- **Belgian Law of April 26, 2024:** Art. 21

- **MITRE ATT&CK:** T1040 — Network Sniffing (topology discovery)

- **Description:** Cisco Discovery Protocol (CDP) is active by default on all Cisco devices and on all interfaces. CDP broadcasts detailed information about devices (hostname, model, IOS version, IP addresses, configured VLANs) at regular intervals. This topology information is valuable for an attacker in the reconnaissance phase.

- **Technical Evidence:** Absence of a global `no cdp run` or per-interface `no cdp enable` on the access ports of the switches.

- **Impact:** An attacker connected to an access port can reconstruct the complete network topology by listening to CDP announcements, facilitating subsequent attack phases.

- **Recommendation:**

  ```
  no cdp run
  ```

  Or granularly, disable CDP only on host-facing interfaces: `no cdp enable`. Keep CDP on inter-device links for monitoring if necessary.

------

### FINDING F-21 — Absence of Login Banner

- **Severity:** 🟡 MEDIUM

- **CyFun Function:** PROTECT — PR.IP-1: Baseline configuration of IT/OT systems is created and maintained

- **Required CyFun Assurance Level:** Basic

- **NIS2 Article 21(2):** (g) — Basic computer hygiene practices and cybersecurity training

- **Belgian Law of April 26, 2024:** Art. 21

- **MITRE ATT&CK:** N/A

- **Description:** No banner message (`banner motd`, `banner login`, `banner exec`) is configured on the network devices. The absence of a legal banner on administration equipment weakens the organization's legal position in the event of prosecution for unauthorized access — without an explicit banner, unauthorized access could be qualified as unintentional.

- **Technical Evidence:** Absence of `banner motd` or `banner login` in the configurations of all devices.

- **Impact:** Legal vulnerability in the event of unauthorized connection. Inability to invoke Article 550bis of the Belgian Criminal Code (unauthorized computer access) without documented prior warning.

- **Recommendation:**

  ```
  banner motd ^
  WARNING: Access restricted to authorized personnel.
  Any unauthorized connection constitutes a criminal offense
  under Article 550bis of the Belgian Criminal Code.
  Sessions are logged and monitored.
  ^
  ```

------

### FINDING F-22 — Password Encryption Not Systematically Enabled

- **Severity:** 🟡 MEDIUM
- **CyFun Function:** PROTECT — PR.DS-1: Data-at-rest is protected
- **Required CyFun Assurance Level:** Basic
- **NIS2 Article 21(2):** (h) — Policies and procedures regarding the use of cryptography
- **Belgian Law of April 26, 2024:** Art. 21
- **MITRE ATT&CK:** T1552.001 — Credentials in Files
- **Description:** The `service password-encryption` command is not systematically present on all devices. Without this command, type 0 (plaintext) passwords appear in readable text in configuration files (`show running-config`), which poses a risk during maintenance operations or in the event of unauthorized access to configuration files.
- **Technical Evidence:** Absence of `service password-encryption` on some devices. `password [plaintext]` passwords visible in configurations.
- **Impact:** Exposure of administration passwords when consulting configurations. Risk during unencrypted backups of configuration files.
- **Recommendation:** `service password-encryption` on all devices. Preferably use type 9 (scrypt) passwords: `enable algorithm-type scrypt secret [password]`.

------

### FINDING F-23 — Unencrypted HTTP Management Interface Potentially Active

- **Severity:** 🟡 MEDIUM

- **CyFun Function:** PROTECT — PR.DS-2: Data-in-transit is protected

- **Required CyFun Assurance Level:** Basic

- **NIS2 Article 21(2):** (h) — Policies and procedures regarding the use of cryptography

- **Belgian Law of April 26, 2024:** Art. 21

- **MITRE ATT&CK:** T1040 — Network Sniffing

- **Description:** The Cisco HTTP management server (`ip http server`) may be active by default on some IOS devices, allowing web browser management without encryption. Only `ip http secure-server` (HTTPS) should be allowed if web management is necessary.

- **Technical Evidence:** Absence of explicit `no ip http server` on some devices. Presence of `ip http server` or unsecured default configuration.

- **Impact:** Unencrypted web management session capturable on the network.

- **Recommendation:**

  ```
  no ip http server
  ip http secure-server
  ip http access-class [ACL-MGMT] in
  ```

------

### FINDING F-24 — Absence of OSPF Authentication between ROUTER-A and L3-Core

- **Severity:** 🟡 MEDIUM

- **CyFun Function:** PROTECT — PR.DS-2: Data-in-transit is protected

- **Required CyFun Assurance Level:** Important

- **NIS2 Article 21(2):** (e) — Security in network and information systems acquisition, development and maintenance

- **Belgian Law of April 26, 2024:** Art. 21

- **MITRE ATT&CK:** T1557 — Adversary-in-the-Middle (OSPF route injection)

- **Description:** The OSPF routing protocol is configured between ROUTER-A and Switch-L3-CORE without MD5 or SHA authentication. Without OSPF authentication, an attacker with access to a network segment between these devices could inject false routing information (fraudulent LSAs), diverting traffic to devices under their control.

- **Technical Evidence:** In the OSPF configurations of ROUTER-A and Switch-L3-CORE: absence of `ip ospf authentication message-digest`, absence of `ip ospf message-digest-key 1 md5 [key]` on OSPF interfaces.

- **Impact:** Possible manipulation of the infrastructure's routing table, allowing attacks such as black-hole routing or traffic hijacking.

- **Recommendation:**

  ```
  router ospf 1
   area 0 authentication message-digest
  interface [ospf-interface]
   ip ospf message-digest-key 1 md5 [strong-key]
  ```

------

### FINDING F-25 — Absence of Redundancy and Single Points of Failure (SPOF)

- **Severity:** 🔵 LOW
- **CyFun Function:** RECOVER — RC.RP-1: Recovery plan is executed during or after a cybersecurity incident
- **Required CyFun Assurance Level:** Important
- **NIS2 Article 21(2):** (c) — Business continuity, such as backup management and disaster recovery
- **Belgian Law of April 26, 2024:** Art. 21
- **MITRE ATT&CK:** N/A
- **Description:** The network architecture presents single points of failure at each level: single ISP link, single edge router, single core switch, without HSRP/VRRP configured. The failure of any of these devices results in a total service interruption for the affected users. This architecture does not meet the availability requirements for an NIS2 entity.
- **Technical Evidence:** Star topology with a single path between each level. Absence of `standby [group] ip [virtual-ip]` (HSRP) or `vrrp [group] ip [virtual-ip]` (VRRP).
- **Impact:** Total network unavailability in the event of a failure of a critical device (ROUTER-A or Switch-L3-CORE). Non-compliance with NIS2 Art. 21(2)(c) continuity requirements.
- **Recommendation:** Plan for a redundant architecture in the long term: second edge router, second core switch with HSRP/VRRP, aggregated links (EtherChannel/LACP) between critical devices.

------

### FINDING F-26 — Incomplete Logging Configuration (Levels and Buffer Undefined)

- **Severity:** 🔵 LOW

- **CyFun Function:** DETECT — DE.CM-7: Monitoring for unauthorized personnel, connections, devices, and software

- **Required CyFun Assurance Level:** Basic

- **NIS2 Article 21(2):** (b) — Incident handling

- **Belgian Law of April 26, 2024:** Art. 23

- **MITRE ATT&CK:** T1070 — Indicator Removal (insufficient logs for detection)

- **Description:** Even if syslog destinations are configured on some devices, the logging levels (`logging trap [level]`), the local buffer size (`logging buffered [size]`), and the severity level of local messages (`logging console [level]`) are not systematically defined. By default, the informational level (6) is used, but without explicit configuration, some critical messages might be missed depending on the device's default configuration.

- **Technical Evidence:** Absence of `logging buffered 16384`, `logging trap notifications`, `logging source-interface Vlan99` on several devices.

- **Impact:** Incomplete or missing logs for incident detection and NIS2 compliance. Inability to reconstruct incident timelines.

- **Recommendation:**

  ```
  logging buffered 16384
  logging trap notifications
  logging source-interface Vlan99
  logging host 192.168.10.130
  ```

------

## SECTION 7 — RISK SUMMARY TABLE

| **ID** | **Title**                               | **Severity** | **CyFun Function** | **Subcategory** | **NIS2 Art. 21(2)** | **Status**      |
| ------ | --------------------------------------- | ------------ | ------------------ | --------------- | ------------------- | --------------- |
| F-01   | VLAN segmentation bypass (gateways)     | 🔴 CRITICAL   | PROTECT            | PR.AC-5         | (e)                 | ❌ Non-compliant |
| F-02   | CoPP not attached to control plane      | 🔴 CRITICAL   | PROTECT            | PR.PT-3         | (e)                 | ❌ Non-compliant |
| F-03   | ACL_SUPPORT_B_IN not applied to VLAN 40 | 🔴 CRITICAL   | PROTECT            | PR.AC-5         | (e)                 | ❌ Non-compliant |
| F-04   | SSH missing on L3-Core (Telnet only)    | 🔴 CRITICAL   | PROTECT            | PR.DS-2         | (h)                 | ❌ Non-compliant |
| F-05   | VLAN 99 IP conflict (L2-C and L2-D)     | 🔴 CRITICAL   | IDENTIFY           | ID.AM-1         | (e)                 | ❌ Non-compliant |
| F-06   | Absence of 802.1X on access ports       | 🟠 HIGH       | PROTECT            | PR.AC-1         | (i)                 | ❌ Non-compliant |
| F-07   | Port Security missing on Switch-L2-D    | 🟠 HIGH       | PROTECT            | PR.AC-5         | (e)                 | ❌ Non-compliant |
| F-08   | Native VLAN 1 not pruned from trunks    | 🟠 HIGH       | PROTECT            | PR.AC-5         | (e)                 | ❌ Non-compliant |
| F-09   | Absence of PortFast/BPDU Guard          | 🟠 HIGH       | PROTECT            | PR.PT-3         | (e)                 | ❌ Non-compliant |
| F-10   | Weak passwords, AAA missing             | 🟠 HIGH       | PROTECT            | PR.AC-1         | (i)                 | ❌ Non-compliant |
| F-11   | Telnet enabled on network devices       | 🟠 HIGH       | PROTECT            | PR.DS-2         | (h)                 | ❌ Non-compliant |
| F-12   | Absence of NTP synchronization          | 🟠 HIGH       | DETECT             | DE.AE-3         | (b)                 | ❌ Non-compliant |
| F-13   | Dual incorrect syslog on DMZ-Switch     | 🟠 HIGH       | DETECT             | DE.CM-7         | (b)                 | ❌ Non-compliant |
| F-14   | SNMPv3 missing (unsecured management)   | 🟠 HIGH       | PROTECT            | PR.DS-2         | (h)                 | ❌ Non-compliant |
| F-15   | Insufficiently isolated DMZ             | 🟠 HIGH       | PROTECT            | PR.AC-5         | (e)                 | ❌ Non-compliant |
| F-16   | DHCP Snooping missing                   | 🟡 MEDIUM     | PROTECT            | PR.AC-5         | (e)                 | ❌ Non-compliant |
| F-17   | DAI (Dynamic ARP Inspection) missing    | 🟡 MEDIUM     | PROTECT            | PR.AC-5         | (e)                 | ❌ Non-compliant |
| F-18   | IP Source Guard missing                 | 🟡 MEDIUM     | PROTECT            | PR.AC-5         | (e)                 | ❌ Non-compliant |
| F-19   | Unused ports not disabled               | 🟡 MEDIUM     | PROTECT            | PR.PT-3         | (e)                 | ❌ Non-compliant |
| F-20   | CDP globally enabled                    | 🟡 MEDIUM     | PROTECT            | PR.PT-3         | (e)                 | ❌ Non-compliant |
| F-21   | Absence of login banner                 | 🟡 MEDIUM     | PROTECT            | PR.IP-1         | (g)                 | ❌ Non-compliant |
| F-22   | Password encryption not systematic      | 🟡 MEDIUM     | PROTECT            | PR.DS-1         | (h)                 | ❌ Non-compliant |
| F-23   | Unencrypted HTTP management             | 🟡 MEDIUM     | PROTECT            | PR.DS-2         | (h)                 | ❌ Non-compliant |
| F-24   | OSPF without authentication             | 🟡 MEDIUM     | PROTECT            | PR.DS-2         | (e)                 | ❌ Non-compliant |
| F-25   | Absence of redundancy (SPOF)            | 🔵 LOW        | RECOVER            | RC.RP-1         | (c)                 | ❌ Non-compliant |
| F-26   | Incomplete logging (levels/buffer)      | 🔵 LOW        | DETECT             | DE.CM-7         | (b)                 | ❌ Non-compliant |

**Severity distribution:**

- 🔴 CRITICAL: 5 (19%)
- 🟠 HIGH: 10 (38%)
- 🟡 MEDIUM: 9 (35%)
- 🔵 LOW: 2 (8%)

------

## SECTION 8 — INCIDENT REPORTING OBLIGATIONS (Art. 23 NIS2 / Belgian Law)

### 8.1 Regulatory framework

Article 23 of the NIS2 Directive, transposed by the Belgian Law of April 26, 2024, imposes a three-step incident reporting process for incidents with a significant impact:

| **Step**                  | **Deadline**                      | **Required content**                                         | **Recipient**                          |
| ------------------------- | --------------------------------- | ------------------------------------------------------------ | -------------------------------------- |
| **Early warning**         | **24 hours** after becoming aware | Mention that the incident occurred, specify if malicious, potential cross-border impact | CCB (Centre for Cybersecurity Belgium) |
| **Incident notification** | **72 hours** after becoming aware | Initial assessment (severity, impact), indicators of compromise if available | CCB                                    |
| **Final report**          | **1 month** after notification    | Full description, measures taken, cross-border assessment    | CCB                                    |

### 8.2 Organization's current capacity

**Detection capacity:** ❌ INSUFFICIENT

- No SIEM to correlate events
- No IDS/IPS to detect intrusions
- Logs not synchronized (NTP missing)
- Syslog partially configured and unreliable (cf. F-13)

**Response capacity:** ❌ INSUFFICIENT

- No documented incident response plan
- No designated CSIRT team
- No defined containment procedures
- No identified emergency communication channel

**Reporting capacity:** ❌ INSUFFICIENT

- No notification procedure to the CCB
- CCB contact not identified in the organization's documents
- No incident register

**Conclusion: The organization is currently unable to comply with the NIS2 Art. 23 reporting obligations. This constitutes a major regulatory non-compliance exposing the organization to immediate sanctions in the event of an incident.**

### 8.3 Significant incident threshold

An incident is considered significant (Art. 23(3) NIS2) if it:

- Has caused or is likely to cause severe operational disruption
- Has caused financial loss to the entity
- Has affected or is likely to affect other natural or legal persons by causing considerable material or non-material damage

Given the critical vulnerabilities identified (VLAN bypass, missing CoPP, poorly isolated DMZ), an incident exploiting these flaws would almost certainly be qualified as significant.

------

## SECTION 9 — MANAGEMENT RESPONSIBILITY (Art. 20 NIS2 / Belgian Law of April 26, 2024)

### 9.1 Principle of managerial responsibility

Article 20 of the NIS2 Directive, transposed into Belgian Law, introduces **personal liability for management bodies** regarding cybersecurity. Members of the management body and executive management are personally responsible for approving and overseeing the implementation of cybersecurity risk-management measures.

### 9.2 Specific management obligations

| **Obligation**                                         | **Status at Byte Sized Solutions**                |
| ------------------------------------------------------ | ------------------------------------------------- |
| Approve security measures (Art. 20(1))                 | ❌ Undocumented                                    |
| Oversee implementation (Art. 20(1))                    | ❌ Failures noted (ACLs not applied, CoPP missing) |
| Undergo cybersecurity training (Art. 20(2))            | ❌ No proof of training                            |
| Offer training to employees (Art. 20(2))               | ❌ Absent                                          |
| Ensure registration with the competent authority (CCB) | ❌ Unverified                                      |

### 9.3 Consequences in case of failure

In the event of a significant security incident and proven failure in cybersecurity oversight, management members may be subject to:

- **Administrative fines** imposed personally
- **Temporary bans** from discharging managerial responsibilities in essential/important entities (Art. 32(5) NIS2 for essential entities)
- **Public declarations** of the breaches (naming and shaming) by the CCB
- Under Belgian law: potential civil liability if third parties suffer damages due to security failures

------

## SECTION 10 — POTENTIAL PENALTIES

### 10.1 Administrative fines

| **Entity Type**                             | **Maximum Fine** | **Calculation Basis**                                        |
| ------------------------------------------- | ---------------- | ------------------------------------------------------------ |
| Essential entity                            | €10,000,000      | or 2% of the annual global turnover (whichever is higher)    |
| **Important entity (Byte Sized Solutions)** | **€7,000,000**   | **or 1.4% of the annual global turnover (whichever is higher)** |

### 10.2 CCB's enforcement powers

As the national competent authority designated by the Belgian Law of April 26, 2024, the CCB has the following powers against Byte Sized Solutions:

- **Security audits** based on documents or on-site
- **Injunctions** to comply within a set deadline
- **Interim measures** (suspension of activities) in case of immediate risk
- **Administrative fines** graded according to severity
- **Publication of decisions** regarding penalties (deterrent/reputational effect)
- For essential entities only: suspension of certifications

### 10.3 Aggravating factors applicable to this audit

The following elements constitute aggravating factors when assessing penalties:

- **Documentary discrepancy**: The provided PDF report claims active controls (CoPP, VLAN 40 ACLs) that are not actually configured — this can be qualified as a **misleading declaration** to the authorities
- **Severity and duration** of the breaches (fundamentally flawed architecture)
- **Number of findings** (26 findings, including 5 critical)
- **0/10** of the NIS2 Art. 21(2) measures satisfied

------

## SECTION 11 — POSITIVE FINDINGS

Despite the insufficient maturity level noted, the audit identified ten positive elements showing an initial approach to securing the environment:

1. **Structured VLAN architecture**: The design with 9 distinct VLANs (Management, Production, Support, DMZ, Servers, FTP, Printers, Network Mgmt) demonstrates an understanding of network segmentation principles and provides a solid foundation for effective security, provided configuration bugs are fixed.
2. **RADIUS server deployed**: The presence of a RADIUS server (192.168.10.132) constitutes the necessary infrastructure to implement 802.1X authentication and centralized AAA — it just needs to be actively used.
3. **ACLs defined (partially)**: Extended ACLs are defined on Switch-L3-CORE for several VLANs. The filtering infrastructure exists — their application and consistency must be corrected.
4. **Syslog configured**: Syslog destinations are defined on the devices (despite the error on DMZ-Switch), showing an intent to centralize logs.
5. **Port Security on 3/4 switches**: Switch-L2-A, B, and C have a Port Security configuration, limiting the risk of MAC flooding on these devices.
6. **DMZ zone created**: VLAN 60 (DMZ) exists with a dedicated HTTP server, demonstrating an understanding of the demilitarized zone concept for exposed services.
7. **Separate management VLAN 99**: The existence of a dedicated management VLAN (99) for device administration is a good administrative segmentation practice.
8. **OSPF configured**: Dynamic OSPF routing between ROUTER-A and L3-Core is functional, facilitating inter-VLAN routing management.
9. **Centralized DHCP**: A centralized DHCP server is deployed and functional, making IP address management easier (although DHCP pools need gateway corrections).
10. **CoPP policy defined**: The CoPP policy-map is fully configured with appropriate traffic classes — it only lacks attachment to the control plane to be operational.

------

## SECTION 12 — REMEDIATION ROADMAP

### Phase 1 — IMMEDIATE CRITICAL Actions (D+0 to D+30)

**CyFun Objective: Achieve Basic level on PR.AC-5, PR.DS-2, ID.AM-1**

| **Action**                                          | **Finding** | **Priority** | **Effort** |
| --------------------------------------------------- | ----------- | ------------ | ---------- |
| Correct DHCP gateways (VLANs 20/30/40)              | F-01        | 🔴 CRITICAL   | Low        |
| Attach CoPP policy to the control plane             | F-02        | 🔴 CRITICAL   | Low        |
| Apply ACL_SUPPORT_B_IN on Vlan40 interface          | F-03        | 🔴 CRITICAL   | Low        |
| Configure SSH v2 on Switch-L3-CORE                  | F-04        | 🔴 CRITICAL   | Low        |
| Resolve VLAN 99 IP address conflict                 | F-05        | 🔴 CRITICAL   | Low        |
| Remove incorrect syslog entry on DMZ-Switch         | F-13        | 🟠 HIGH       | Trivial    |
| Disable Telnet on all devices                       | F-11        | 🟠 HIGH       | Low        |
| Enable `service password-encryption` on all devices | F-22        | 🟡 MEDIUM     | Trivial    |
| Disable unused ports                                | F-19        | 🟡 MEDIUM     | Low        |
| Configure NTP on all devices                        | F-12        | 🟠 HIGH       | Low        |
| Add legal login banners                             | F-21        | 🟡 MEDIUM     | Trivial    |
| Disable CDP on access ports                         | F-20        | 🟡 MEDIUM     | Low        |

**Phase 1 Deliverable:** Verification report of corrected configurations. Estimated CyFun score after Phase 1: 1.8/5.

### Phase 2 — Securing Access and Authentication (D+30 to D+90)

**CyFun Objective: Achieve Basic level on PR.AC-1, PR.AC-3, PR.IP-1**

| **Action**                                               | **Finding**       | **Effort** |
| -------------------------------------------------------- | ----------------- | ---------- |
| Deploy 802.1X on all access ports                        | F-06              | High       |
| Configure centralized AAA with RADIUS on all devices     | F-10              | Medium     |
| Configure Port Security on Switch-L2-D                   | F-07              | Low        |
| Prune VLAN 1 from trunks, change native VLAN             | F-08              | Low        |
| Enable PortFast + BPDU Guard on access ports             | F-09              | Low        |
| Configure strict ACLs DMZ → Internal VLANs               | F-15              | Medium     |
| OSPF MD5 authentication                                  | F-24              | Low        |
| Disable `ip http server`, enable `ip http secure-server` | F-23              | Trivial    |
| Implement SNMPv3 AuthPriv                                | F-14              | Medium     |
| Register with the CCB                                    | Art. 27 Belg. Law | Admin.     |

**Phase 2 Deliverable:** Internal penetration test validating corrections. Estimated CyFun score: 2.5/5.

### Phase 3 — Detection and Monitoring (D+90 to D+180)

**CyFun Objective: Achieve Basic level on DE.CM-1, DE.CM-7, DE.AE-3**

| **Action**                                                | **Finding**  | **Effort** |
| --------------------------------------------------------- | ------------ | ---------- |
| Deploy DHCP Snooping                                      | F-16         | Low        |
| Deploy DAI (Dynamic ARP Inspection)                       | F-17         | Low        |
| Deploy IP Source Guard                                    | F-18         | Low        |
| Deploy a SIEM (e.g., Graylog, ELK, Splunk)                | F-12, F-26   | High       |
| Deploy a network IDS/IPS                                  | —            | High       |
| Configure full logging (levels, buffer, source-interface) | F-26         | Low        |
| Develop and test an Incident Response Plan                | Art. 23 NIS2 | High       |
| Train management on NIS2 cybersecurity (Art. 20)          | Art. 20      | Medium     |

**Phase 3 Deliverable:** Operational monitoring dashboard, documented and tested IRP. Estimated CyFun score: 3.0/5.

### Phase 4 — Resilience and Continuous Improvement (D+180 to D+365)

**CyFun Objective: Achieve Important level across all functions**

| **Action**                                             | **Finding**    | **Effort** |
| ------------------------------------------------------ | -------------- | ---------- |
| Deploy network redundancy (HSRP/VRRP, redundant links) | F-25           | Very High  |
| Conduct an annual external security audit              | Art. 21(2)(f)  | Medium     |
| Implement a vulnerability management program           | ID.RA-1        | Medium     |
| Develop a supplier risk management policy              | Art. 21(2)(d)  | Medium     |
| Implement MFA for all administrative access            | Art. 21(2)(j)  | Medium     |
| Conduct an incident simulation exercise (tabletop)     | Art. 21(2)(b)  | Medium     |
| Review and update network documentation                | ID.AM-1        | Low        |
| Obtain ISO 27001 certification or equivalent           | Recommendation | Very High  |

**Phase 4 Deliverable:** Annual NIS2 compliance report submitted to the CCB. Target CyFun score: 3.5/5 (Established level).

------

## SECTION 13 — CONCLUSION

This compliance audit against the CCB's CyberFundamentals (CyFun) framework and the NIS2 Directive, as transposed by the Belgian Law of April 26, 2024, reveals a situation of **widespread non-compliance** in Byte Sized Solutions' network infrastructure. With a cybersecurity maturity score of **1.2/5** (Initial Level — INSUFFICIENT) and **0 out of the 10 mandatory measures of NIS2 Article 21(2)** fully satisfied, the organization is currently facing significant legal, operational, and reputational risk.

Five critical severity findings were identified, three of which result from simple configuration errors (incorrect DHCP gateways, unattached CoPP, unapplied ACL) that could be fixed in less than a day of technical work. These immediate corrections would activate security mechanisms already designed and partially configured, bringing a substantial improvement to the security posture at a very low marginal cost. The presence of a deployed but unused RADIUS server, a structured VLAN architecture, and a defined but inactive CoPP policy demonstrates that the foundations for appropriate security exist — it is their actual implementation that is lacking.

The organization is urged to prioritize the resolution of the five critical findings within 30 days, to register with the CCB in accordance with Article 27 of the Belgian Law of April 26, 2024, and to establish a minimum capability for incident detection and reporting in compliance with NIS2 Article 23. Failure to meet these obligations exposes the organization's leadership to **personal liability** and the organization to **administrative fines of up to 7 million euros**. The four-phase roadmap proposed in this report provides a pragmatic and realistic path toward NIS2 compliance, with a target CyFun score of 3.5/5 achievable within 12 months.

------