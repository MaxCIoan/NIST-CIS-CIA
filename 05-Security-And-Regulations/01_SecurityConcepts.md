# Part 1: Security Concepts

---

## Table of Contents

1. [Module Overview](#module-overview)
2. [What is Cybersecurity?](#11-what-is-cybersecurity)
3. [What Do We Protect?](#12-what-do-we-protect--assets-and-attack-surface)
4. [Threats, Vulnerabilities, and Risk](#13-threats-vulnerabilities-and-risk)
5. [Key Security Models and Principles](#14-key-security-models-and-principles)
6. [Sources](#sources)

---

## Module Overview

This is the fifth and final foundational module of the bootcamp. After Networking, Python, Windows/Active Directory, and Linux — it is now time to understand **why** all of that technical work matters from a professional, legal, and organizational standpoint.

This module is split into two parts:

- **Part 1 — Security Concepts** *(this document)*: core principles that define what cybersecurity professionals protect, and how they think about threats and risk.
- **Part 2 — Regulations & Compliance**: the major cybersecurity laws and standards in force in 2026, across Europe, Belgium and internationally.

> **What you will be able to do after this module:**
> - Explain the CIA Triad and why it is the foundation of all security work
> - Identify the assets and data that organizations need to protect
> - Know the key cybersecurity regulations in force in 2026 and what they require
> - Understand the difference between being compliant and being secure
> - Identify which regulations apply to a given organization
> - Conduct a compliance gap analysis on a real network and Active Directory
> - Write a professional security audit report with prioritized recommendations

---

## 1.1  What is Cybersecurity?

Cybersecurity is the practice of protecting systems, networks, applications, and data from digital attacks, unauthorized access, damage, or destruction. It covers both **technical measures** (firewalls, encryption, monitoring) and **human processes** (policies, training, incident response).

Every organization — from small businesses to hospitals to national critical infrastructure — is a potential target. The average cost of a data breach globally now exceeds **$5 million USD**. Regulators across Europe and the world impose legal obligations that directly shape how security teams must work.

### The CIA Triad — Three Fundamental Goals

All cybersecurity work revolves around protecting three core properties, known as the **CIA Triad**:

| Property | What it means | Example of failure |
|----------|--------------|-------------------|
| **Confidentiality** | Only authorized people can access information | A hacker reads private patient files after stealing a nurse's credentials |
| **Integrity** | Data is accurate and has not been tampered with | An attacker modifies a bank transfer amount from €100 to €100,000 |
| **Availability** | Systems and data are accessible when needed | Ransomware shuts down hospital systems for 3 days, delaying surgeries |

A fourth property sometimes added is **Non-repudiation** — the ability to prove that a specific action was taken by a specific person (via digital signatures). This is important in forensics and legal proceedings.

> 📖 **Further reading:** [NIST definition of cybersecurity](https://www.nist.gov/cyberframework) | [ENISA Glossary](https://www.enisa.europa.eu/topics/cybersecurity-education/glossary-and-terminology)

---

## 1.2  What Do We Protect? — Assets and Attack Surface

Before you can defend something, you must know what you have. Understanding an organization's **assets** — its *attack surface* — is the first step in any security engagement, whether you are a defender or a pentester.

| Asset Type | Examples | Why it matters |
|------------|----------|---------------|
| **Data** | Customer PII, financial records, intellectual property, health records, trade secrets | Most targeted asset — valuable for fraud, extortion, espionage, dark market resale |
| **Systems** | Servers, workstations, databases, cloud instances, OT/SCADA systems | Compromise gives an attacker control or disrupts critical operations |
| **Networks** | LAN, WAN, Wi-Fi, VPN, DNS, routing infrastructure | Used for lateral movement and data exfiltration |
| **Applications** | Web apps, APIs, ERP, CRM, SaaS tools | Vulnerable code and misconfigurations are primary entry points |
| **Identities** | User accounts, service accounts, admin credentials, API keys | Stolen credentials provide instant access to protected resources |
| **People** | Employees, contractors, executives, IT teams | Social engineering exploits humans, not just technology |
| **Physical** | Data centers, hardware, access control systems | Physical access can bypass all digital controls entirely |

> 🔗 **Connection to your previous modules:** The network topology you mapped in Module 1, the Active Directory you built in Module 3, and the Linux systems you hardened in Module 4 are all part of an organization's attack surface. Knowing this attack surface is what lets both defenders (monitoring, patching, hardening) and pentesters (target identification, attack path planning) do their jobs effectively.

---

## 1.3  Threats, Vulnerabilities, and Risk

> A threat is someone who wants to break into your house. A vulnerability is the broken lock on your back door. Risk is how likely the burglar will find that broken lock, and how bad things will be if they do.

| Concept | Definition | Example |
|---------|-----------|---------|
| **Threat** | Any potential cause of harm to a system or organization | A ransomware group targeting hospitals in France |
| **Vulnerability** | A weakness that could be exploited | Unpatched Windows Server with a public CVE exposed to the internet |
| **Risk** | Likelihood × impact of a threat exploiting a vulnerability | High: internet-facing server, public exploit exists, contains patient data |
| **Exploit** | The technique or tool used to take advantage of a vulnerability | A script sending a crafted packet to trigger remote code execution |
| **Impact** | The consequences if the risk materializes | Operational disruption, financial loss, regulatory fine, reputational damage |

### The Risk Equation

$$\text{RISK} = \text{Likelihood} \times \text{Impact}$$

Security teams maintain **risk registers** — structured documents listing all identified risks with likelihood and impact scores (typically 1–5), risk ratings, and remediation tracking. All major cybersecurity regulations now require organizations to demonstrate a formal risk management process.

### Threat Actor Landscape

| Actor Type | Motivation | Capability | Real Examples |
|------------|-----------|-----------|--------------|
| **Script Kiddies** | Curiosity, fame, thrill | Low — uses pre-built tools | DDoS with ready-made botnets, basic phishing kits |
| **Cybercriminals** | Financial gain | Medium to high | LockBit, ALPHV/BlackCat, Clop ransomware groups |
| **Hacktivists** | Ideology, political protest | Variable | Anonymous, Killnet (pro-Russia DDoS campaigns) |
| **Insider Threats** | Revenge, greed, coercion | High (has privileged access) | Disgruntled admin exfiltrating client database before resignation |
| **Nation-State APTs** | Espionage, sabotage, strategic disruption | Very high — advanced, stealthy, persistent | APT28 (Russia/GRU), Lazarus Group (North Korea), APT41 (China) |
| **Competitors** | Industrial espionage | Variable | Corporate spy targeting R&D data or M&A plans |

> 📖 **Further reading:** [MITRE ATT&CK — Threat actor groups](https://attack.mitre.org/groups/) | [ENISA Threat Landscape 2024](https://www.enisa.europa.eu/topics/cyber-threats/enisa-threat-landscape)

---

## 1.4  Key Security Models and Principles

### Defense in Depth

> A castle has a moat, then walls, then gate guards, then inner guards, then a vault for the crown jewels. If an attacker gets past one layer, they face another — and another. Defense in Depth works the same way for IT systems.

Defense in Depth is a **layered security strategy** where multiple independent controls protect assets at different levels. The principle: no single control is perfect, so overlapping layers mean that if one fails, others compensate.

Typical layers, from outside to inside:

```
Internet
    ↓
[ Perimeter: Firewall, IPS, WAF ]
    ↓
[ Network: VLANs, DMZ, segmentation ]
    ↓
[ Endpoint: AV, EDR, patch management ]
    ↓
[ Application: secure code, input validation ]
    ↓
[ Data: encryption at rest and in transit, DLP ]
    ↓
[ Identity: MFA, PAM, Zero Trust ]
    ↓
[ Physical: access cards, CCTV, secure rooms ]
```

---

### Zero Trust

> Old security said: "You have a badge and you got in the door — we trust you." Zero Trust says: "Even if you are inside the building, you must prove who you are every single time you want to access something. No free pass, ever."

Zero Trust is a security model built on **"never trust, always verify."** It assumes threats exist both inside and outside the traditional network perimeter.

Three core principles:

1. **Verify explicitly** — Always authenticate and authorize based on all available signals: identity, device health, location, service, data sensitivity.
2. **Use least privilege access** — Users and systems get only what they need, when they need it. Just-in-time (JIT) and just-enough-access (JEA).
3. **Assume breach** — Minimize blast radius. Segment access. Verify encryption end-to-end. Use analytics to detect threats that got through.

Zero Trust is now explicitly or implicitly required by NIS2, CMMC, ANSSI frameworks, and Germany's BSI. It is the dominant architectural model for security projects in 2026.

> 📖 **Further reading:** [NIST SP 800-207 — Zero Trust Architecture](https://csrc.nist.gov/publications/detail/sp/800-207/final) | [ANSSI — Zero Trust](https://www.ssi.gouv.fr/en/guide/zero-trust/)

---

### Principle of Least Privilege (PoLP)

Every user, service, or system must have the **minimum permissions necessary** to do their job — nothing more. This limits the damage an attacker can do after compromising an account.

It is especially critical in Active Directory environments, where misconfigured group memberships and over-privileged accounts are among the **top causes of real-world compromise**.

---

### Security Controls

| Control Type | Purpose | Examples |
|-------------|---------|---------|
| **Preventive** | Stop an attack before it happens | Firewalls, MFA, patch management, encryption, access controls |
| **Detective** | Identify an attack in progress or after the fact | SIEM, IDS/IPS, audit logs, file integrity monitoring, UEBA |
| **Corrective** | Respond to and recover from an attack | Incident response plan, backup restoration, system reimaging |
| **Deterrent** | Discourage attackers from attempting | Legal warning banners, security cameras, published security posture |
| **Compensating** | Alternative when primary controls cannot be applied | Network isolation for an unpatchable legacy industrial system |

---

## Sources

| Topic | Source | URL |
|-------|--------|-----|
| NIST Cybersecurity Framework | NIST | https://www.nist.gov/cyberframework |
| ENISA Threat Landscape 2024 | ENISA | https://www.enisa.europa.eu/topics/cyber-threats/enisa-threat-landscape |
| ENISA Glossary | ENISA | https://www.enisa.europa.eu/topics/cybersecurity-education/glossary-and-terminology |
| MITRE ATT&CK | MITRE | https://attack.mitre.org |
| Zero Trust Architecture (SP 800-207) | NIST | https://csrc.nist.gov/publications/detail/sp/800-207/final |
| ANSSI Zero Trust Guide | ANSSI | https://www.ssi.gouv.fr/en/guide/ |
| CIS Controls | CIS | https://www.cisecurity.org/controls |
| ISO/IEC 27000 (Vocabulary) | ISO | https://www.iso.org/standard/73906.html |
