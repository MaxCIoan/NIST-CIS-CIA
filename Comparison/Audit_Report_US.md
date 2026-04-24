# 📋 Network Security Compliance Audit Report — NIST SP 800-53 Rev. 5
### Network Design Main final version — Byte Sized Solutions / BeCode
**Date:** April 23, 2026 | **Classification:** CONFIDENTIAL | **Phase:** 2 — Group Network Audit

---

## 🎯 Report Information

| Field                        | Value                                                        |
| ---------------------------- | ------------------------------------------------------------ |
| **Scope**                    | Simulated Cisco Packet Tracer network infrastructure         |
| **Primary Framework**        | NIST SP 800-53 Rev. 5                                        |
| **Supplementary Frameworks** | CIS Benchmarks v8, ISO/IEC 27001:2022, MITRE ATT&CK v14      |
| **Methodology**              | Documentary review (design audit / paper audit)              |
| **Sources analyzed**         | Extracted .pkt file, PDF security report "Byte Sized Solutions", CLI configurations |
| **Overall compliance score** | **25–30% — INSUFFICIENT**                                    |
| **Overall risk level**       | 🔴 **HIGH**                                                   |

---

## 🔑 Executive Summary

The audited network infrastructure presents an **insufficient security posture** against the requirements of NIST SP 800-53 Rev. 5. Of the 26 findings identified, **4 are CRITICAL severity**, directly compromising the confidentiality, integrity, and availability of the network.

### ⚠️ Critical Point: Gap Between Claims and Reality

Analysis of the actual device configurations reveals **significant discrepancies** between what the PDF report claims to have implemented and what is actually configured on the devices. Two emblematic examples:

- The report states **CoPP is active** → configs show the `service-policy` is **never attached to the control-plane**
- The report states **the Support B ACL is applied on VLAN 40** → the Vlan40 interface has **no `ip access-group`**

> **This drift between documentation and reality is itself a major compliance risk** (NIST CM-7, CM-9, CM-6).

### 🔴 Top 3 Priorities for Management

| #    | Priority                                                     | Business Impact                                      |
| ---- | ------------------------------------------------------------ | ---------------------------------------------------- |
| 1    | **Fix default gateways on 30 PCs** — VLAN segmentation ACLs are entirely bypassed | Unrestricted lateral movement between departments    |
| 2    | **Secure access to network devices** — no authentication on L2 switches | Full infrastructure takeover by an internal attacker |
| 3    | **Actually activate CoPP and fix VLAN 40 ACL** — declared controls do not exist in production | Critical servers (RADIUS, DNS, DHCP) are exposed     |

---

## 🔬 Scope and Methodology

### Audit Scope
- 2 routers (ISP, ROUTER A — ISR4331)
- 1 L3 core switch (3650-24PS)
- 5 L2 switches (2960-24TT: Switch-L2-A/B/C/D, DMZ-Switch)
- 5 servers (DNS, DHCP, RADIUS, FTP, HTTP)
- 44 user endpoints (PC-PT)

### Methodology
Documentary audit (paper audit) in accordance with NIST SP 800-115 recommendations. Cross-analysis between:
1. CLI configurations extracted from the .pkt file
2. The PDF security report produced by the team
3. Reference frameworks: NIST SP 800-53 Rev. 5, CIS Benchmark IOS, MITRE ATT&CK

---

## 🏗️ Network Architecture and Security Zone Analysis

```
Internet
   │
[ISP Router] ──── 192.168.12.0/30
   │
[ROUTER A] ──── 192.168.10.248/30
   │
[Switch-L3-CORE]
   ├── VLAN 10 (Mgmt/Study)   192.168.10.0/27
   ├── VLAN 20 (Production)   192.168.10.32/27
   ├── VLAN 30 (Support A)    192.168.10.64/27
   ├── VLAN 40 (Support B)    192.168.10.96/27  ← ACL not applied!
   ├── VLAN 50 (Printers)     192.168.11.0/27   ← No ACL!
   ├── VLAN 60 (DMZ)          192.168.10.192/27
   ├── VLAN 70 (Int. Servers) 192.168.10.128/27
   ├── VLAN 80 (FTP/iSCSI)    192.168.10.224/29
   └── VLAN 99 (Network Mgmt) 192.168.10.160/27 ← No ACL!
```

---

## 📋 Findings — 26 Items

---

### 🔴 CRITICAL

---

#### F-01 — SSH Version 1 on ROUTER A
**Severity:** CRITICAL (Score: 9.1)

**Description:** The main router uses SSH version 1, a cryptographically obsolete protocol affected by vulnerabilities enabling session interception and injection.

**Evidence:**
```
ip ssh version 1    ← ROUTER A config
```

**NIST SP 800-53 Rev. 5:**
- **IA-2** — Identification and Authentication (Organizational Users)
- **SC-8** — Transmission Confidentiality and Integrity
- **SC-23** — Session Authenticity

**CIS Benchmark IOS:** 1.1.4 — Ensure SSH version 2 is used
**MITRE ATT&CK:** T1557 (Adversary-in-the-Middle), T1040 (Network Sniffing)

**Remediation:**
```
no ip ssh version 1
ip ssh version 2
ip ssh time-out 60
ip ssh authentication-retries 3
```

---

#### F-02 — No VTY Authentication on L2 Switches
**Severity:** CRITICAL (Score: 9.8)

**Description:** All 4 L2 switches and the DMZ switch have their VTY lines configured with `login` but no password defined. Cisco IOS allows unauthenticated access in this state — anyone on the network can connect with no credentials whatsoever.

**Evidence:**
```
! Switch-L2-A, L2-B, L2-C, L2-D, DMZ-Switch:
line vty 0 4
 login        ← no password, no local database reference
line vty 5 15
 login
```

**NIST SP 800-53 Rev. 5:**
- **IA-2** — Identification and Authentication
- **AC-3** — Access Enforcement
- **AC-17** — Remote Access

**MITRE ATT&CK:** T1078 (Valid Accounts), T1110 (Brute Force)

**Remediation:**
```
username admin privilege 15 secret <strong_password>
line vty 0 15
 login local
 transport input ssh
 exec-timeout 5 0
```

---

#### F-03 — Trivial Plaintext Enable Password
**Severity:** CRITICAL (Score: 9.6)

**Description:** Switch-L3-CORE and DMZ-Switch use `enable password 0000` — a trivial password stored in plaintext with no `enable secret`.

**Evidence:**
```
! Switch-L3-CORE:
enable password 0000    ← plaintext, trivial, no "enable secret"
! DMZ-Switch:
enable password 0000
```

**NIST SP 800-53 Rev. 5:**
- **IA-5** — Authenticator Management — IA-5(1) Password-Based Authentication
- **CM-6** — Configuration Settings

**CIS Benchmark IOS:** 1.1.2 — Enable Secret
**MITRE ATT&CK:** T1078.001 (Default Accounts)

**Remediation:**
```
no enable password
enable secret <complex_password_16+_chars>
service password-encryption
```

---

#### F-04 — Complete VLAN Segmentation Bypass via Incorrect Default Gateways
**Severity:** CRITICAL (Score: 9.5)

**Description:** 30 PCs across VLANs 20, 30, and 40 are configured with gateway `192.168.10.1` (VLAN 10 SVI) instead of their own VLAN gateway. All their traffic is routed through the VLAN 10 interface, **completely bypassing** `ACL_PRODUCTION_IN`, `ACL_SUPPORT_A_IN`, and `ACL_SUPPORT_B_IN`. Network segmentation is theoretically present but practically non-existent.

**Evidence:**
```
! PROD-PC-1 (should be on VLAN 20, correct GW = 192.168.10.33)
# Default GW: 192.168.10.1  ← WRONG

! SUPA-PC-1 (should be on VLAN 30, correct GW = 192.168.10.65)
# Default GW: 192.168.10.1  ← WRONG

! SUPB-PC-1 (should be on VLAN 40, correct GW = 192.168.10.97)
# Default GW: 192.168.10.1  ← WRONG
```

| VLAN      | Subnet           | Correct GW    | Configured GW  | ACL Bypassed      |
| --------- | ---------------- | ------------- | -------------- | ----------------- |
| 20 (PROD) | 192.168.10.32/27 | 192.168.10.33 | ❌ 192.168.10.1 | ACL_PRODUCTION_IN |
| 30 (SUPA) | 192.168.10.64/27 | 192.168.10.65 | ❌ 192.168.10.1 | ACL_SUPPORT_A_IN  |
| 40 (SUPB) | 192.168.10.96/27 | 192.168.10.97 | ❌ 192.168.10.1 | ACL_SUPPORT_B_IN  |

**NIST SP 800-53 Rev. 5:**
- **SC-7** — Boundary Protection
- **AC-4** — Information Flow Enforcement
- **CM-6** — Configuration Settings

**MITRE ATT&CK:** T1599 (Network Boundary Bridging)

**Remediation:** Correct the IP configurations on all affected PCs and enforce DHCP with the correct VLAN pools (already configured server-side on the DHCP server).

---

### 🟠 HIGH

---

#### F-05 — CoPP Declared Active but Not Attached to Control-Plane
**Severity:** HIGH (Score: 8.2)

**Description:** The PDF report claims CoPP is configured and active. The actual configs show that `policy-map COPP_POLICY` is defined but the `control-plane / service-policy input COPP_POLICY` command is **absent** from both devices. The protection simply does not exist.

**Evidence:**
```
! ROUTER A — policy-map defined BUT:
policy-map COPP_POLICY
 class OSPF_CLASS
 class EIGRP_CLASS
 ...
! NO "control-plane" + "service-policy input" anywhere in the full config
! Same finding on Switch-L3-CORE
```

**NIST SP 800-53 Rev. 5:**
- **SC-5** — Denial of Service Protection
- **CM-7** — Least Functionality
- **CA-7** — Continuous Monitoring

**Documentation vs. Reality Gap:** The PDF (pages 21–23) presents verification screenshots — these do not match the configurations extracted from the PKT file.

**Remediation:**
```
control-plane
 service-policy input COPP_POLICY
```

---

#### F-06 — ACL_SUPPORT_B_IN Defined but Not Applied on VLAN 40
**Severity:** HIGH (Score: 8.5)

**Description:** The ACL intended to control Support B department traffic is fully defined but the Vlan40 interface has no `ip access-group`. Combined with F-04, VLAN 40 has **zero active access control**.

**Evidence:**
```
interface Vlan40
 ip address 192.168.10.97 255.255.255.224
 ip helper-address 192.168.10.131
 ← NO ip access-group ACL_SUPPORT_B_IN in
```

**NIST SP 800-53 Rev. 5:**
- **AC-3** — Access Enforcement
- **AC-4** — Information Flow Enforcement
- **SC-7** — Boundary Protection

---

#### F-07 — No ACL on VLAN 99 (Management)
**Severity:** HIGH (Score: 9.0)

**Description:** The network management VLAN has no access control whatsoever. Any internal host can reach the administration infrastructure without restriction.

**Evidence:**
```
interface Vlan99
 ip address 192.168.10.161 255.255.255.224
 ← NO ip access-group
```

**NIST SP 800-53 Rev. 5:**
- **AC-3** — Access Enforcement
- **MA-4** — Nonlocal Maintenance
- **SC-7(5)** — Deny by Default / Allow by Exception

---

#### F-08 — No ACL on VLAN 50 (Printers / MFD)
**Severity:** HIGH (Score: 7.5)

**Description:** Printers and MFDs run rarely-updated firmware and are frequently vulnerable. VLAN 50 has no ACL — these devices can communicate freely with any network segment.

**NIST SP 800-53 Rev. 5:**
- **SC-7** — Boundary Protection
- **CM-7** — Least Functionality
- **SI-3** — Malicious Code Protection

**MITRE ATT&CK:** T1584.005 (Compromise Infrastructure via IoT/printers)

---

#### F-09 — Conflicting Dual Default Routes on L3-Core
**Severity:** HIGH (Score: 7.5)

**Description:** Two static default routes point to different next hops, one of which does not exist in the topology, creating routing instability and potential packet loss.

**Evidence:**
```
ip route 0.0.0.0 0.0.0.0 192.168.11.249   ← unknown/non-existent next-hop
ip route 0.0.0.0 0.0.0.0 192.168.10.250   ← ROUTER A (correct)
```

**NIST SP 800-53 Rev. 5:**
- **SC-7** — Boundary Protection
- **CP-10** — System Recovery and Reconstitution
- **SI-2** — Flaw Remediation

---

#### F-10 — DMZ ACL: Incorrect Rule Order (Established Before Deny)
**Severity:** HIGH (Score: 8.0)

**Description:** The first rule of `ACL_DMZ_IN` permits established connections from the DMZ to the internal network **before** the specific deny rules below it. A packet matching the `established` condition will be permitted and will never reach the deny rules, allowing DMZ hosts to reach internal subnets via established sessions.

**Evidence:**
```
! Rule 1 — PERMITS before DENYING:
permit tcp 192.168.10.192 0.0.0.31 192.168.10.0 0.0.0.127 established
! Following rules — intended to deny:
deny ip 192.168.10.192 0.0.0.31 192.168.10.0 0.0.0.31
```

**NIST SP 800-53 Rev. 5:**
- **AC-4** — Information Flow Enforcement
- **SC-7** — Boundary Protection

**MITRE ATT&CK:** T1021 (Remote Services via established sessions)

---

#### F-11 — UDP Any/Any Rule in ACL_PRODUCTION_IN
**Severity:** HIGH (Score: 7.8)

**Description:** The very first rule of the Production ACL permits UDP port 66 from any source to any destination, placed before all deny rules. This undermines the entire ACL's intent.

**Evidence:**
```
ip access-list extended ACL_PRODUCTION_IN
 permit udp any any eq 66    ← first rule, before all denys!
```

**NIST SP 800-53 Rev. 5:**
- **AC-4** — Information Flow Enforcement
- **CM-7** — Least Functionality

**MITRE ATT&CK:** T1570 (Lateral Tool Transfer)

---

#### F-12 — Overly Broad DNS Rule in ACL_INTERNET_IN
**Severity:** HIGH (Score: 7.2)

**Description:** The DNS rule in the perimeter ACL allows any internet source to send DNS queries to any internal destination, enabling potential DNS-based C2 or data exfiltration channels.

**Evidence:**
```
permit udp any any eq domain
! Should be:
permit udp any host 192.168.10.130 eq domain
```

**NIST SP 800-53 Rev. 5:**
- **SC-7** — Boundary Protection
- **SC-20** — Secure Name/Address Resolution Service

**MITRE ATT&CK:** T1071.004 (DNS C2), T1568 (Dynamic Resolution)

---

#### F-13 — Trivial RADIUS Shared Secret
**Severity:** HIGH (Score: 7.8)

**Description:** The RADIUS shared secret is `cisco123`, a widely known default value that is trivially guessable via dictionary attack.

**Evidence:**
```
radius server 192.168.10.132
 key cisco123
```

**NIST SP 800-53 Rev. 5:**
- **IA-3** — Device Identification and Authentication
- **IA-5** — Authenticator Management
- **SC-8** — Transmission Confidentiality and Integrity

---

#### F-14 — RSA Keys Potentially Missing on L3-Core (SSH Non-Functional)
**Severity:** HIGH (Score: 7.0)

**Description:** Although `ip domain-name` is configured on L3-Core, no `crypto key generate rsa` or `ip ssh version 2` command appears in the configuration. SSH may not be operational despite `transport input ssh` being declared on VTY lines.

**NIST SP 800-53 Rev. 5:**
- **IA-2** — Identification and Authentication
- **SC-8** — Transmission Confidentiality and Integrity

---

### 🟡 MEDIUM

---

#### F-15 — IP Address Conflict on VLAN 99: Switch-L2-C and L2-D
**Severity:** MEDIUM (Score: 6.5)

**Evidence:**
```
Switch-L2-C: interface Vlan99 → ip address 192.168.10.165
Switch-L2-D: interface Vlan99 → ip address 192.168.10.165  ← identical!
```

**NIST:** CM-6 (Configuration Settings), CM-8 (System Component Inventory)

---

#### F-16 — Empty RIP Process Running on ROUTER A
**Severity:** MEDIUM (Score: 5.0)

**Description:** An empty `router rip` block is configured with no `network` statements — an unnecessary attack surface that could be exploited to inject false routing information.

**Evidence:**
```
router rip
 ← empty, no network statements
```

**NIST:** CM-7 (Least Functionality), SC-7

---

#### F-17 — Orphan ACL `sl_def_acl` Never Applied
**Severity:** MEDIUM (Score: 4.5)

**Description:** An ACL containing contradictory rules (deny SSH followed by permit SSH — the permit is unreachable) is defined on ROUTER A but applied to no interface.

**Evidence:**
```
ip access-list extended sl_def_acl
 deny tcp any any eq 22      ← denies SSH
 permit tcp any any eq 22    ← unreachable (shadowed by deny above)
```

**NIST:** CM-6, AC-3

---

#### F-18 — ROUTER A Console Without Local Authentication
**Severity:** MEDIUM (Score: 5.5)

**Evidence:**
```
line con 0
 exec-timeout 5 0
 login       ← should be "login local"
```

**NIST:** IA-2, AC-17

---

#### F-19 — Port Security Absent on Switch-L2-D
**Severity:** MEDIUM (Score: 5.0)

**Description:** All 10 SUPB access ports on Switch-L2-D have neither port-security nor portfast configured, unlike the other three L2 switches.

**NIST:** SC-7, CM-6

---

#### F-20 — Overly Permissive Catch-All Rules at End of Internal ACLs
**Severity:** MEDIUM (Score: 5.5)

**Description:** All user-facing ACLs end with `permit ip [subnet] any`, partially negating the effect of the preceding deny rules and granting unrestricted outbound access.

**NIST:** AC-4, SC-7(5) (Deny by Default)

---

#### F-21 — NAT ACL Covers a /24 Instead of Specific /27 Subnets
**Severity:** MEDIUM (Score: 4.5)

**Evidence:**
```
access-list 1 permit 192.168.10.0 0.0.0.255    ← covers entire /24
```

**NIST:** SC-7, AC-4

---

#### F-22 — DMZ-Switch Syslog Points to Wrong IP
**Severity:** MEDIUM (Score: 4.5)

**Evidence:**
```
! DMZ-Switch:
logging 192.168.10.13     ← WRONG, should be .130
logging 192.168.10.130    ← correct
```

**NIST:** AU-9 (Protection of Audit Information), AU-12 (Audit Record Generation)

---

#### F-23 — External DNS (8.8.8.8) Configured on Most PCs
**Severity:** MEDIUM (Score: 4.0)

**Description:** Most PCs use Google DNS instead of the internal DNS server, making internal DNS records (`estatekamkar.be`, `mfd.lan`, `sharedstorage.lan`) unreachable for those endpoints.

**NIST:** SC-20, CM-6

---

#### F-24 — STP Priority Configured Only for VLAN 10
**Severity:** MEDIUM (Score: 4.0)

**Evidence:**
```
spanning-tree vlan 10 priority 24576    ← VLAN 10 only
! No STP priority for VLANs 20, 30, 40, 50, 60, 70, 80, 99
```

**NIST:** SC-5, CP-10
**MITRE ATT&CK:** T1557 (ARP/STP Spoofing)

---

### 🟢 LOW

---

#### F-25 — Unidentified DHCP Devices (Test PC, PC1)
**Severity:** LOW (Score: 3.0)

**Description:** Two endpoints configured with DHCP have no documentation, no clear identification, and no apparent business justification on the network.

**NIST:** CM-8 (System Component Inventory), IA-3 (Device Identification)

---

#### F-26 — Missing `log` Keyword on ACL Deny Rules
**Severity:** LOW (Score: 2.5)

**Description:** Most `deny` rules lack the `log` keyword, preventing the generation of syslog entries for blocked traffic and hindering forensic analysis.

**NIST:** AU-2 (Event Logging), AU-12 (Audit Record Generation)

---

## 📊 Risk Summary Table

| ID   | Title                             | Severity   | Score | NIST Control        | MITRE ATT&CK |
| ---- | --------------------------------- | ---------- | ----- | ------------------- | ------------ |
| F-01 | SSH Version 1                     | 🔴 CRITICAL | 9.1   | IA-2, SC-8, SC-23   | T1557, T1040 |
| F-02 | No VTY Auth on L2 Switches        | 🔴 CRITICAL | 9.8   | IA-2, AC-3, AC-17   | T1078, T1110 |
| F-03 | Trivial Enable Password           | 🔴 CRITICAL | 9.6   | IA-5, CM-6          | T1078.001    |
| F-04 | Incorrect Default Gateways        | 🔴 CRITICAL | 9.5   | SC-7, AC-4, CM-6    | T1599        |
| F-05 | CoPP Not Attached                 | 🟠 HIGH     | 8.2   | SC-5, CM-7, CA-7    | T1498        |
| F-06 | VLAN 40 ACL Not Applied           | 🟠 HIGH     | 8.5   | AC-3, AC-4, SC-7    | T1599        |
| F-07 | No ACL on VLAN 99                 | 🟠 HIGH     | 9.0   | AC-3, MA-4, SC-7(5) | T1078        |
| F-08 | No ACL on VLAN 50                 | 🟠 HIGH     | 7.5   | SC-7, CM-7, SI-3    | T1584        |
| F-09 | Dual Conflicting Default Routes   | 🟠 HIGH     | 7.5   | SC-7, CP-10, SI-2   | —            |
| F-10 | DMZ ACL Rule Order                | 🟠 HIGH     | 8.0   | AC-4, SC-7          | T1021        |
| F-11 | UDP Any/Any in PROD ACL           | 🟠 HIGH     | 7.8   | AC-4, CM-7          | T1570        |
| F-12 | DNS Rule Too Broad                | 🟠 HIGH     | 7.2   | SC-7, SC-20         | T1071.004    |
| F-13 | Trivial RADIUS Key                | 🟠 HIGH     | 7.8   | IA-3, IA-5, SC-8    | T1550        |
| F-14 | SSH Potentially Broken on L3-Core | 🟠 HIGH     | 7.0   | IA-2, SC-8          | T1040        |
| F-15 | IP Conflict VLAN 99               | 🟡 MEDIUM   | 6.5   | CM-6, CM-8          | —            |
| F-16 | Empty RIP Process                 | 🟡 MEDIUM   | 5.0   | CM-7, SC-7          | —            |
| F-17 | Orphan ACL Never Applied          | 🟡 MEDIUM   | 4.5   | CM-6, AC-3          | —            |
| F-18 | Console Without Local Auth        | 🟡 MEDIUM   | 5.5   | IA-2, AC-17         | T1078        |
| F-19 | Port Security Missing on L2-D     | 🟡 MEDIUM   | 5.0   | SC-7, CM-6          | T1557        |
| F-20 | Catch-All Permit in ACLs          | 🟡 MEDIUM   | 5.5   | AC-4, SC-7(5)       | —            |
| F-21 | NAT ACL Too Broad                 | 🟡 MEDIUM   | 4.5   | SC-7, AC-4          | —            |
| F-22 | Syslog Wrong IP on DMZ Switch     | 🟡 MEDIUM   | 4.5   | AU-9, AU-12         | —            |
| F-23 | External DNS on Most PCs          | 🟡 MEDIUM   | 4.0   | SC-20, CM-6         | T1568        |
| F-24 | Partial STP Priority              | 🟡 MEDIUM   | 4.0   | SC-5, CP-10         | T1557        |
| F-25 | Unidentified DHCP Devices         | 🟢 LOW      | 3.0   | CM-8, IA-3          | —            |
| F-26 | Missing `log` on Deny Rules       | 🟢 LOW      | 2.5   | AU-2, AU-12         | —            |

---

## 📐 NIST SP 800-53 Compliance Analysis by Control Family

| Family                             | Controls Concerned             | Status          | Level    |
| ---------------------------------- | ------------------------------ | --------------- | -------- |
| **AC** — Access Control            | AC-3, AC-4, AC-17              | ❌ Non-compliant | CRITICAL |
| **AU** — Audit & Accountability    | AU-2, AU-9, AU-12              | ⚠️ Partial       | MEDIUM   |
| **CM** — Configuration Mgmt        | CM-6, CM-7, CM-8, CM-9         | ❌ Non-compliant | HIGH     |
| **CP** — Contingency Planning      | CP-10                          | ⚠️ Partial       | MEDIUM   |
| **IA** — Identification & Auth     | IA-2, IA-3, IA-5               | ❌ Non-compliant | CRITICAL |
| **MA** — Maintenance               | MA-4                           | ❌ Non-compliant | HIGH     |
| **SC** — System & Comms Protection | SC-5, SC-7, SC-8, SC-20, SC-23 | ⚠️ Partial       | HIGH     |
| **SI** — System & Info Integrity   | SI-2, SI-3                     | ⚠️ Partial       | MEDIUM   |
| **CA** — Assessment & Monitoring   | CA-7                           | ❌ Non-compliant | HIGH     |
| **IR** — Incident Response         | IR-*                           | ❌ Not defined   | N/A      |

**Estimated overall compliance score: 25–30%**

---

## ✅ Positive Findings

Despite the numerous issues, several controls deserve recognition:

| Positive Finding                                             | NIST Control |
| ------------------------------------------------------------ | ------------ |
| ✅ Port-security active on L2-A/B/C (violation restrict mode) | SC-7         |
| ✅ STP PortFast on user access ports                          | SC-5         |
| ✅ Logically sound VLAN architecture (9 zones)                | SC-7         |
| ✅ Perimeter ACL applied on ROUTER A (Gi0/0/0 inbound)        | SC-7         |
| ✅ enable secret + service password-encryption on ROUTER A    | IA-5         |
| ✅ login block-for configured on ROUTER A and L3-Core         | AC-7         |
| ✅ exec-timeout 5 0 on ROUTER A and L3-Core                   | AC-11        |
| ✅ AAA new-model + local authentication on L3-Core            | IA-2         |
| ✅ NAT/PAT correctly configured                               | SC-32        |
| ✅ MOTD banner present on all devices                         | AC-8         |
| ✅ Centralized Syslog partially operational                   | AU-12        |
| ✅ NTP configured (single source)                             | AU-8         |
| ✅ DMZ isolated in a dedicated VLAN 60                        | SC-7         |
| ✅ switchport nonegotiate on L2-B (DTP attack prevention)     | CM-7         |

---

## 🏁 Conclusion — Overall Compliance Posture

### Verdict: ❌ NON-COMPLIANT — HIGH Risk

The infrastructure presents an **insufficient security posture** for an enterprise environment. The most concerning finding is the **systematic gap between controls declared in documentation and actual running configurations** — which is itself a violation of NIST **CM-9 (Configuration Management Plan)** and **CA-7 (Continuous Monitoring)**.

### Expected Risk Reduction After Remediation

| Phase                     | Actions                                 | Estimated Score |
| ------------------------- | --------------------------------------- | --------------- |
| **Immediate (48h)**       | Fix F-02, F-03, F-04, F-06              | 7.8 → 4.5       |
| **Short-term (2 weeks)**  | Fix F-01, F-05, F-07, F-08, F-09, F-10  | 4.5 → 3.0       |
| **Medium-term (1 month)** | Fix F-11 through F-24                   | 3.0 → 2.0       |
| **Long-term**             | Fix F-25, F-26 + continuous improvement | 2.0 → ~1.5      |

> **With full implementation of all recommendations, the risk score would decrease from 7.8/10 to approximately 2.0/10**, corresponding to an acceptable security posture for an organization of this size, compliant with the NIST SP 800-53 Moderate baseline.

