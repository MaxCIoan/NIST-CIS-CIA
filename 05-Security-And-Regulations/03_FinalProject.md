# Final Project: Compliance Audit

---

## Table of Contents

1. [Project Overview](#project-overview)
2. [Phase 1 — Solo: Active Directory Audit](#phase-1--solo-active-directory-audit)
3. [Phase 2 — Group: Network Topology Compliance Audit](#phase-2--group-network-topology-compliance-audit)
4. [Deliverables](#deliverables)
5. [Grading Criteria](#grading-criteria)
6. [Recommended Tools and References](#recommended-tools-and-references)

---

## Project Overview

This final project is a professional-grade compliance audit exercise. You will work as a security analyst conducting an assessment of a real technical environment and produce a written report with actionable, prioritized recommendations.

The project is split into two phases:

| Phase | Format | Environment |
|-------|--------|-------------|
| Phase 1 | Solo | Your own Active Directory |
| Phase 2 | Group (3 people) | A network topology you did not build |

> ⚠️ **This is not a hacking exercise.** You are not trying to exploit vulnerabilities — you are auditing configurations, identifying gaps between the current state and what regulations and best practices require, and writing actionable recommendations. This is what GRC analysts, security auditors, and compliance consultants do every day.

---

## Phase 1 — Solo: Active Directory Audit

### Scenario

You are a junior security analyst contracted by a mid-size company. They have asked you to audit the Active Directory environment to determine whether it meets current security standards and regulatory requirements. You have been given read access to the environment — not administrative control.

Your goal is to produce a written report documenting what you found, what risks those findings create, and what the company needs to do about it.

### Your Environment

You will audit the Active Directory you built during Module 3. You know how it was built — which means you also know where shortcuts were taken, where default settings remain, and what was never hardened.

You are not being judged on how well you built the AD. You are being judged on how thoroughly and honestly you audit it.

### What You Are Looking For

You will need to research this yourself. The regulations in Part 2 of this module, combined with frameworks like CIS Benchmarks and Microsoft's security baselines, define what a properly configured and compliant AD environment looks like. Your job is to identify the gap between that standard and what actually exists.

Think about: accounts, permissions, policies, logging, network exposure, and organizational controls.

---

## Phase 2 — Group: Network Topology Compliance Audit

### Scenario

You are part of a three-person security consulting team hired to audit a company's network infrastructure. You have been provided with the network topology documentation for an environment you did not build. You have never seen this network before.

Your team must analyze the network design, identify security and compliance gaps, and deliver a professional audit report and a 15-minute presentation to the client (the rest of the class and the coaches).

### Your Environment

You will be assigned a network topology created by another group during the course. You will have access to: the network diagram, device inventory, firewall rule summaries, and VLAN configuration notes.

You will not have shell access to the systems. This is a documentation and architecture review — also called a **design audit** or **paper audit** — a common type of engagement in real consulting work.

### Working as a Team

Divide responsibilities clearly. A real consulting engagement has distinct roles: someone owns the technical analysis, someone owns the report writing, someone owns the client presentation. Your team decides who does what — but all three must contribute visibly.

---

## Deliverables

### Phase 1 — Written Report (individual)

Your report must follow a professional structure. At minimum it must include:

- **Executive Summary** — A non-technical summary of your findings suitable for a business manager to read. State the overall risk level and the top 3 priorities.
- **Scope and Methodology** — What you audited and how you approached it.
- **Findings** — Each finding documented individually with: description, evidence, applicable regulation or framework reference, risk rating (Critical / High / Medium / Low), and remediation recommendation.
- **Risk Summary Table** — A consolidated table of all findings, rated and prioritized.
- **Conclusion** — Overall compliance posture assessment.

> ⚠️ Every finding must be linked to a specific regulation, framework, or security standard. A finding without a reference is an opinion, not an audit result.

### Phase 2 — Written Report (group) + Presentation

The group report follows the same structure as Phase 1 but covers the network topology. In addition, your team will present your findings to the class.

You are expected to defend your findings. Be prepared to explain *why* something is a risk and *which regulation or standard* it relates to.

---

## Recommended Tools and References

These tools and references are not required — they are starting points. Part of this project is learning how to find what you need.

### Auditing and Analysis Tools

| Tool | Purpose | Link |
|------|---------|------|
| **PingCastle** | Active Directory security assessment and scoring | [pingcastle.com](https://www.pingcastle.com/) |
| **BloodHound Community Edition** | AD attack path visualization and privilege analysis | [github.com/SpecterOps/BloodHound](https://github.com/SpecterOps/BloodHound) |
| **Microsoft Security Compliance Toolkit** | Official Microsoft security baselines for AD and Windows | [microsoft.com/en-us/download/details.aspx?id=55319](https://www.microsoft.com/en-us/download/details.aspx?id=55319) |
| **Nmap** | Network scanning and service discovery | [nmap.org](https://nmap.org/) |
| **Wireshark** | Network traffic analysis | [wireshark.org](https://www.wireshark.org/) |

### Frameworks and Benchmarks

| Resource | What it covers | Link |
|----------|---------------|------|
| **CIS Benchmarks** | Detailed hardening checklists for Windows, Linux, network devices, AD | [cisecurity.org/cis-benchmarks](https://www.cisecurity.org/cis-benchmarks/) |
| **NIST SP 800-53** | Security and privacy controls for federal information systems — widely used as a reference baseline | [csrc.nist.gov/publications/detail/sp/800-53/rev-5/final](https://csrc.nist.gov/publications/detail/sp/800-53/rev-5/final) |
| **NIST Cybersecurity Framework 2.0** | Risk-based framework for managing cybersecurity risk | [nist.gov/cyberframework](https://www.nist.gov/cyberframework) |
| **MITRE ATT&CK** | Adversary tactics and techniques — useful for threat-linking findings | [attack.mitre.org](https://attack.mitre.org/) |
| **CCB CyFun Framework** | Belgium's national cybersecurity framework — maps NIS2 requirements to concrete controls | [ccb.belgium.be/en/cyberfundamentals-framework](https://ccb.belgium.be/en/cyberfundamentals-framework) |
| **Microsoft Defender for Identity** | Documentation on AD attack techniques and detections | [learn.microsoft.com/en-us/defender-for-identity](https://learn.microsoft.com/en-us/defender-for-identity/) |

### Regulations to Consider

When mapping findings to regulatory requirements, refer to Part 2 of this module (Regulations & Compliance). The regulations most relevant to this project are:

- **GDPR** — for any personal data stored or processed in the environment
- **NIS2 / Belgian NIS2 Act** — for network and information system security obligations
- **ISO/IEC 27001:2022** — for information security management controls
- **CyFun (CCB)** — Belgium's practical cybersecurity framework, directly linked to NIS2 compliance
- **DORA** — if the scenario involves a financial sector organization

Regulations do not usually specify *exactly* what technical configuration to apply. Your job is to interpret what they require and map it to specific technical findings. That interpretation is the skill you are developing.

---

## Report Writing Guidance

A professional security audit report is not a list of observations. It is a risk communication document. The person reading it — often a CISO, CTO, or board member — must be able to understand the risk, trust your assessment, and act on your recommendations.

Some principles:

- **Be specific.** "Password policy is weak" is not a finding. "The Default Domain Policy enforces a minimum password length of 7 characters; ANSSI guidelines and CIS Benchmark Level 1 both require a minimum of 12 characters" is a finding.
- **Reference your evidence.** What did you observe? Where? What tool or method did you use?
- **Rate risks consistently.** Use a clear, documented scale. Explain your rating criteria once and apply it uniformly.
- **Prioritize.** Not every finding is equally urgent. Help your client understand what to fix first and why.
- **Write for your audience.** The executive summary is for non-technical readers. The findings section is for technical teams. Both need to be clear.

> 📖 **Further reading on audit report writing:** [SANS Reading Room — Writing Penetration Testing Reports](https://www.sans.org/white-papers/33343/) | [NIST SP 800-115 — Technical Guide to Information Security Testing and Assessment](https://csrc.nist.gov/publications/detail/sp/800-115/final)
