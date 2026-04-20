# Part 2: Cybersecurity Regulations & Compliance

---

## Table of Contents

1. [Introduction](#introduction)
2. [European Union](#21-european-union)
   - [GDPR](#gdpr--general-data-protection-regulation)
   - [NIS2](#nis2--network-and-information-security-directive-2)
   - [DORA](#dora--digital-operational-resilience-act)
   - [Cyber Resilience Act](#cyber-resilience-act-cra)
   - [eIDAS 2.0](#eidas-20)
   - [AI Act](#ai-act)
3. [Belgium](#22-belgium)
   - [CCB](#ccb--centre-for-cybersecurity-belgium)
   - [Belgian NIS2 Transposition](#belgian-nis2-transposition)
   - [CyFun](#cyfun--cyberfundamentals-framework)
   - [APD / GBA](#apd--gba--autorité-de-protection-des-données)
4. [International Standards](#23-international-standards)
   - [ISO/IEC 27001:2022](#isoiec-270012022)
   - [ISO/IEC 27005:2022](#isoiec-270052022)
   - [SOC 2](#soc-2)
5. [Quick Reference — Comparison Table](#24-quick-reference--comparison-table)
6. [Applying Regulations — Gap Analysis](#25-applying-regulations--gap-analysis)
7. [Official Sources](#official-sources)

---

## Introduction

This document covers the major cybersecurity and data protection regulations in force in **2026** that you will encounter as a cybersecurity professional working in Belgium or with European clients.

> ⚠️ **Important: Security vs. Compliance**
>
> **Compliance** means meeting the minimum requirements of a regulation or standard.
> **Security** means actually protecting against threats.
>
> These are **not the same thing**. An organization can be fully compliant and still be breached. A good security professional aims for both. You will often encounter organizations that treat compliance as a checkbox exercise — your role is to help them understand that compliance is the **floor**, not the ceiling.

A **regulation** is a legally binding rule backed by government authority, with penalties for non-compliance. A **standard or framework** (like ISO 27001) is a voluntary best-practice guide that organizations can choose to adopt and get certified against. Regulations often reference or require alignment with these standards.

---

## 2.1  European Union


### GDPR — General Data Protection Regulation

**Reference:** Regulation (EU) 2016/679 | **In force:** May 2018 | **Region:** EU + extraterritorial

> 💡 **ELI5:** The EU's privacy rulebook. Any company that collects or uses personal data of people in the EU must follow strict rules about how that data is stored, processed, shared, and protected — and give people control over their own data.

**Technical overview:** GDPR regulates the processing of personal data of EU data subjects by any organization, regardless of where the organization is established. It introduces individual rights (access, erasure/right to be forgotten, portability, rectification, objection, restriction) and obligations for organizations: lawful basis for processing, data minimization, purpose limitation, storage limitation, security, and accountability. A Data Protection Officer (DPO) is mandatory in certain cases. Breaches must be reported to the supervisory authority within 72 hours.

**Applies to:** Any organization anywhere in the world that processes personal data of EU residents — including companies based outside the EU.

**Key requirements:**
- Establish a lawful basis for every data processing activity
- Appoint a Data Protection Officer (DPO) where required
- Conduct Data Protection Impact Assessments (DPIA) for high-risk processing
- Implement "privacy by design and by default"
- Report data breaches to the supervisory authority within 72 hours
- Honor data subject rights: access, erasure, portability, rectification, objection
- Maintain records of processing activities (ROPA — Article 30)

**Non-compliance penalties:** Up to €20 million or 4% of global annual turnover (whichever is higher).

*Notable fines: Meta €1.2B (2023), Amazon €746M (2021)*

📎 [Official text (EUR-Lex)](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX%3A32016R0679) | [EDPB (European Data Protection Board)](https://edpb.europa.eu) | [APD/GBA — Belgium](https://www.autoriteprotectiondonnees.be)

---

### NIS2 — Network and Information Security Directive 2

**Reference:** Directive (EU) 2022/2555 | **Transposition deadline:** October 2024 | **Region:** EU — all 27 Member States

> NIS2 tells important companies in critical sectors (hospitals, power grids, water, banks, digital infrastructure) that they MUST take cybersecurity seriously, report attacks quickly, and their executives can be held personally liable if they do not. It is a major upgrade from the original NIS Directive (2016).

**Technical overview:** NIS2 replaces the original NIS Directive with much broader scope and stronger enforcement. It covers **essential entities** (11 high-criticality sectors: energy, transport, banking, health, water, digital infrastructure, space, public administration, etc.) and **important entities** (7 other sectors). Incident reporting timelines are strict: 24h early warning, 72h detailed notification, 1-month final report. Executive management can be held personally liable.

**Applies to:** Medium and large enterprises in 18 sectors across the EU, plus all public administration bodies.

**Key requirements:**
- Risk analysis and security policies for all information systems
- Report significant incidents: 24h early warning → 72h full notification → 1 month final report to national CSIRT/authority
- Business continuity and disaster recovery planning
- Supply chain security — assess and manage ICT supplier risks (see dedicated section below)
- Mandatory cybersecurity training for all staff
- Multi-factor authentication (MFA) for all internet-facing systems and privileged accounts
- Cryptography and encryption policies
- Board-level accountability for cybersecurity risk

#### NIS2 and Supply Chain Security

Supply chain attacks — where an attacker compromises a vendor or software provider to reach the real target — have become one of the most significant threats in recent years. SolarWinds (2020), Kaseya (2021), and the XZ Utils backdoor (2024) are all examples of attackers using trusted third parties as an entry point.

NIS2 addresses this directly in **Article 21(2)(d) and (e)**, making supply chain security a mandatory risk management obligation — not a nice-to-have.

**What NIS2 requires from organizations regarding their suppliers:**

- **Map all ICT dependencies** — identify every supplier, service provider, and software vendor that has access to your network or systems, or whose product failure could affect your operations.
- **Assess supplier security practices** — evaluate the quality and resilience of your suppliers' products and cybersecurity practices, including their secure development procedures. You cannot simply assume a supplier is secure because they are well-known.
- **Include security clauses in contracts** — agreements with ICT suppliers must include provisions covering: minimum security standards, incident notification obligations, audit rights, and vulnerability disclosure commitments.
- **Manage concentration risk** — avoid over-dependence on a single ICT supplier for critical functions. NIS2 expects organizations to have considered what happens if a key supplier fails or is compromised.
- **Monitor the supplier lifecycle** — supply chain risk management is ongoing: perform periodic reassessments, particularly when a supplier undergoes major changes (acquisition, restructuring, known breach).

**EU-level coordinated risk assessments (Article 22):**

NIS2 also gives ENISA and national competent authorities the power to conduct **coordinated security risk assessments of critical ICT supply chains** across the EU — essentially auditing entire technology categories that many critical organizations depend on. This already happened for 5G networks (EU Toolbox on 5G Cybersecurity). Cloud infrastructure, managed security services, and semiconductors are expected next.

**What if you are the supplier?**

This is the part most people miss: NIS2's supply chain obligations create a **cascading compliance effect**. If your company provides IT services, software, managed security, cloud hosting, or any technical service to a NIS2 essential or important entity — that entity is legally required to assess your security posture and impose contractual security requirements on you.

In practice this means:

- A NIS2 entity can send you a **security questionnaire** before and during a contract, asking you to demonstrate your security controls.
- They can require you to **meet specific security standards** — often ISO 27001, CyFun, or their own internal baseline — as a condition of the contract.
- They can include **audit rights** in the contract, giving them (or their mandated auditor) the right to assess your security practices.
- They can require you to **notify them within a defined timeframe** if you suffer a security incident that could affect them.
- They can terminate the contract if you cannot demonstrate adequate security.

You do not need to be a NIS2 entity yourself to be affected by NIS2. If any of your clients are, their compliance obligations flow down to you through the contractual relationship. This is already happening in Belgium and across the EU — companies that are suppliers to hospitals, utilities, financial institutions, or public administrations are receiving security requirements from their clients citing NIS2.

> 💡 **Why this matters for you:** As a security professional, you will encounter this from both sides. You may be auditing an organization's supply chain on behalf of a NIS2 entity — or you may be working at a company that receives NIS2-driven security requirements from a client. Understanding the mechanism means you can anticipate what will be asked and help your organization prepare before a client contract forces the issue.

> ⚠️ **A common misconception:** NIS2 does not directly regulate suppliers who are not themselves essential or important entities. The regulation reaches them *indirectly* — through the contractual obligations that NIS2 entities are required to impose. The legal obligation sits with the NIS2 entity, but the practical security demand lands on the supplier.

**Non-compliance penalties:** Essential entities: up to €10M or 2% of global annual turnover. Important entities: up to €7M or 1.4%. Executive management personal liability.

📎 [Official text (EUR-Lex)](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX%3A32022L2555) | [ENISA NIS2 resources](https://www.enisa.europa.eu/topics/cybersecurity-policy/nis-directive-new) | [ENISA — Good practices for supply chain cybersecurity](https://www.enisa.europa.eu/publications/good-practices-for-supply-chain-cybersecurity) | [Belgian NIS2 transposition — CCB](https://ccb.belgium.be/en/nis2)

---

### DORA — Digital Operational Resilience Act

**Reference:** Regulation (EU) 2022/2554 | **Fully applicable:** January 17, 2025 | **Region:** EU — financial sector

> DORA is a law just for the financial sector. Banks, insurers, investment firms — you MUST be able to survive a cyberattack and keep running. And you must verify that your IT suppliers are also secure.

**Technical overview:** DORA establishes a comprehensive ICT risk management framework for financial entities. Five pillars: ICT risk management framework, incident classification and reporting, digital operational resilience testing (including Threat-Led Penetration Testing — TLPT every 3 years for significant entities), ICT third-party risk management (contractual requirements, concentration risk, exit strategies, register of dependencies), and voluntary information sharing.

**Applies to:** Banks, credit institutions, investment firms, insurance companies, crypto-asset service providers (CASPs), payment institutions, and their critical ICT third-party providers.

**Key requirements:**
- ICT risk management framework: policies, procedures, protocols, tools
- Classify and report major ICT incidents to supervisory authority within strict timelines
- Annual digital operational resilience testing — vulnerability assessments and scenario-based testing
- TLPT (Threat-Led Penetration Testing) every 3 years for significant entities
- Register and monitor all ICT third-party dependencies
- ICT business continuity policies and tested disaster recovery plans with defined RTO/RPO
- Board-level accountability: senior management must have ICT risk expertise or adequate training

**Non-compliance penalties:** Ongoing penalties up to 1% of average daily worldwide turnover for 6 consecutive months. Critical ICT third-party providers: up to €5 million.

📎 [Official text (EUR-Lex)](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX%3A32022R2554) | [EBA DORA resources](https://www.eba.europa.eu/regulation-and-policy/operational-resilience/digital-operational-resilience-act-dora)

---

### Cyber Resilience Act (CRA)

**Reference:** Regulation (EU) 2024/2847 | **Adopted:** October 2024 | **Compliance phased through 2027** | **Region:** EU — product manufacturers

> The CRA says every software and hardware product sold in Europe must be secure from the start. No more selling smart home devices with default passwords everyone knows, or software with known unpatched vulnerabilities. Manufacturers must fix security flaws throughout a product's lifetime.

**Technical overview:** The CRA introduces mandatory cybersecurity requirements for all products with digital elements placed on the EU market. Products are classified into **Default** (most products), **Class I** (higher risk: identity management software, browsers, firewalls, routers, smart meters) and **Class II** (critical: industrial control systems, medical devices, critical infrastructure components). Manufacturers must provide security updates for a minimum of 5 years and a Software Bill of Materials (SBOM), and report actively exploited vulnerabilities to ENISA within 24 hours.

**Applies to:** Manufacturers, importers, and distributors of hardware or software products with digital elements sold in the EU.

**Key requirements:**
- Security by design and by default — no known exploitable vulnerabilities at launch
- Secure default configurations — no universal default passwords
- Security updates for at least 5 years, easily applicable by users
- Report actively exploited vulnerabilities to ENISA within 24 hours
- Software Bill of Materials (SBOM) for Class I and II products
- CE marking demonstrating conformity

**Non-compliance penalties:** Class I: up to €10M or 2% of global annual turnover. Class II: up to €15M or 2.5%. False information to authorities: up to €5M.

📎 [Official text (EUR-Lex)](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX%3A32024R2847) | [ENISA CRA resources](https://www.enisa.europa.eu/topics/cybersecurity-policy/cyber-resilience-act)

---

### eIDAS 2.0

**Reference:** Regulation (EU) 2024/1183 | **Builds on eIDAS 1.0 (2014)** | **Region:** EU

> eIDAS 2.0 creates a European Digital Identity Wallet for every EU citizen — like a digital passport on your phone that is trusted in every EU country. It also ensures electronic signatures and certificates are legally valid across all member states.

**Technical overview:** eIDAS 2.0 establishes the framework for European Digital Identity Wallets (EUDIW), which member states must offer by 2026. Large private platforms (banking, telco, large e-commerce) must accept the EUDIW. For security professionals: qualified trust services (certificates, signatures, timestamps, seals), identity proofing assurance levels (Low/Substantial/High), and security standards for wallet providers.

**Applies to:** EU member states (must provide EUDIW), Qualified Trust Service Providers (QTSPs), large private platforms required to accept EUDIW.

**Key requirements:**
- Member states must provide EUDIW to all citizens and residents
- QTSPs must meet strict security requirements and pass regular audits
- Electronic signatures interoperable across all EU member states
- Strong authentication for High assurance level identity verification
- Privacy by design: selective disclosure, unlinkability between transactions

**Non-compliance penalties:** National supervisory bodies can suspend or revoke QTSP status.

📎 [Official text (EUR-Lex)](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX%3A32024R1183) | [EUDIW Architecture Reference Framework](https://github.com/eu-digital-identity-wallet/eudi-doc-architecture-and-reference-framework)

---

### AI Act

**Reference:** Regulation (EU) 2024/1689 | **Phased application 2025–2027** | **Region:** EU

> The world's first comprehensive law on AI. It bans the most dangerous uses of AI, puts strict rules on high-risk AI systems, and requires transparency when you interact with a chatbot or deepfake. Increasingly relevant as AI tools are embedded into security products.

**Technical overview:** The AI Act classifies systems by risk: **Unacceptable** (prohibited — e.g., social scoring, real-time public biometric surveillance in most cases), **High-risk** (strict requirements — AI in critical infrastructure, employment, healthcare, law enforcement), **Limited risk** (transparency obligations — chatbots, deepfakes), **Minimal risk** (no obligations). AI-powered security tools (SIEM AI, automated threat detection) may fall under limited or high-risk categories.

**Applies to:** Providers and deployers of AI systems in the EU, or affecting EU persons, regardless of provider location.

**Key requirements:**
- Risk assessment and classification before deployment
- High-risk AI: conformity assessment, technical documentation, human oversight mechanisms
- Prohibited AI: real-time biometric surveillance in public spaces (limited exceptions), social scoring
- Transparency: inform users when interacting with an AI system
- Register high-risk AI systems in the EU AI database

**Non-compliance penalties:** Prohibited AI uses: up to €35M or 7% of global turnover. High-risk violations: up to €15M or 3%.

📎 [Official text (EUR-Lex)](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX%3A32024R1689) | [EU AI Act implementation tracker](https://artificialintelligenceact.eu/)

---

## 2.2  Belgium

Belgium is where you work. The EU regulations above apply directly, but Belgium also has its own national cybersecurity authority, its own transposition laws, and its own practical framework for organizations operating here.

---

### CCB — Centre for Cybersecurity Belgium

**Website:** [ccb.belgium.be](https://ccb.belgium.be/en) | **Role:** National cybersecurity authority

> The CCB is Belgium's "national cybersecurity agency" — the equivalent of ANSSI in France or BSI in Germany. It is the government body responsible for coordinating national cybersecurity policy, overseeing compliance with NIS2, and running CERT.be (the national CSIRT that handles incident reports).

**Technical overview:** The Centre for Cybersecurity Belgium (CCB) was established by Royal Decree in 2015. It operates under the authority of the Prime Minister. Its responsibilities include: national cybersecurity strategy coordination, implementation and enforcement of the Belgian NIS2 law, operation of CERT.be (the national Computer Security Incident Response Team), awareness campaigns (safeonweb.be), and publication of guidance for businesses and public administrations.

The CCB is the competent authority under the Belgian NIS2 transposition law. Organizations that qualify as essential or important entities under NIS2 must notify the CCB, and the CCB can impose audits and sanctions.

📎 [CCB official website](https://ccb.belgium.be/en) | [CERT.be](https://cert.be) | [Safeonweb.be](https://www.safeonweb.be)

---

### Belgian NIS2 Transposition

**Reference:** Loi du 26 avril 2024 relative à la sécurité des réseaux et des systèmes d'information (NIS2 Act) | **In force:** 18 October 2024 | **Region:** Belgium

> 💡 **ELI5:** Belgium had to turn the EU's NIS2 Directive into a Belgian law. This law — adopted in April 2024 — says which Belgian organizations are covered, what they must do, and what happens if they do not comply. The CCB is the authority that enforces it.

**Technical overview:** The Belgian NIS2 Act transposes EU Directive 2022/2555 into national law. It establishes two tiers of entities: **essential entities** (large organizations in high-criticality sectors) and **important entities** (medium-sized organizations in the same or adjacent sectors). Organizations must self-register with the CCB. Key additions in the Belgian transposition include specific incident reporting workflows through CERT.be, designation of sector-specific competent authorities (e.g., FSMA for financial sector, IBPT for telecoms), and provisions for municipal and regional public administrations.

**Applies to:** Belgian organizations in the 18 sectors defined by NIS2 — energy, transport, banking, financial market infrastructure, health, water, digital infrastructure, ICT service management (B2B), public administration, space, post and couriers, waste management, chemicals, food, manufacturing, digital providers, research.

**Key requirements:**
- Register with the CCB as an essential or important entity (mandatory self-registration)
- Implement the 10 minimum security measures defined by the CCB
- Report significant incidents to CERT.be: 24h early warning → 72h full notification → 1 month final report
- Ensure supply chain security and ICT third-party risk management
- Executive management must approve cybersecurity policies and can be held personally liable
- Undergo CCB-ordered security audits upon request

**Non-compliance penalties:** Essential entities: up to €10M or 2% of global annual turnover. Important entities: up to €7M or 1.4%. CCB can also impose operational restrictions on non-compliant entities.

📎 [Belgian NIS2 Act — Belgian Official Gazette](https://www.ejustice.just.fgov.be/eli/loi/2024/04/26/2024011823/justel) | [CCB NIS2 portal](https://ccb.belgium.be/en/nis2) | [CCB — 10 minimum security measures](https://ccb.belgium.be/en/publication/10-minimum-security-measures-nis2)

---

### CyFun — CyberFundamentals Framework

**Reference:** CCB CyberFundamentals Framework v2.0 | **Publisher:** Centre for Cybersecurity Belgium | **Region:** Belgium

> CyFun is Belgium's practical cybersecurity checklist for businesses. The CCB built it to help Belgian organizations know exactly what security measures to put in place — from "Basic" (small companies) to "Important" and "Essential" (large critical sector organizations). It maps to NIS2 obligations.

**Technical overview:** The CyberFundamentals Framework (CyFun) is the CCB's national cybersecurity framework, inspired by the NIST Cybersecurity Framework but adapted for Belgian and EU regulatory context. It is organized around 5 functions (Identify, Protect, Detect, Respond, Recover) and 4 assurance levels: **Basic**, **Small**, **Important**, and **Essential**. Each level maps to NIS2 obligations and, for the higher levels, to ISO 27001 controls. Organizations can use CyFun to self-assess their security posture, and the CCB formally links NIS2 compliance to achieving the appropriate CyFun assurance level.

The CyFun also integrates with the CCB's online assessment tool available at [atwork.safeonweb.be](https://atwork.safeonweb.be/cyberfundamentals-toolbox).

**Applies to:** All Belgian organizations — it is voluntary for non-NIS2 entities, and de facto mandatory for NIS2 essential and important entities as the primary compliance reference.

**Key requirements (vary by assurance level):**
- Asset inventory and risk management
- Access control and identity management
- Patch and vulnerability management
- Incident detection and response capability
- Business continuity and backup plans
- Security awareness training for staff

📎 [CyFun Framework — CCB](https://ccb.belgium.be/en/cyberfundamentals-framework) | [CyFun online assessment tool](https://atwork.safeonweb.be/cyberfundamentals-toolbox) | [CyFun NIS2 gap analysis](https://advisera.com/tools/nis-2-gap-analysis-tool/)

---

### APD / GBA — Autorité de Protection des Données

**Reference:** Loi du 30 juillet 2018 relative à la protection des personnes physiques à l'égard des traitements de données à caractère personnel | **Region:** Belgium

> The APD/GBA is Belgium's GDPR watchdog. If a Belgian company breaks GDPR rules — leaking data, not responding to access requests, failing to report a breach on time — it is the APD that investigates and can impose fines.

**Technical overview:** The *Autorité de Protection des Données* (APD, in French) / *Gegevensbeschermingsautoriteit* (GBA, in Dutch) is Belgium's independent national data protection supervisory authority, as required by GDPR Article 51. It has full investigative, corrective, and advisory powers. The APD enforces GDPR for all organizations processing personal data of Belgian residents, operates a complaint mechanism for data subjects, and publishes binding decisions and guidance. It coordinates with other EU data protection authorities through the EDPB (European Data Protection Board).

**Applies to:** All organizations processing personal data of persons in Belgium.

**Key enforcement actions:**
- Investigation of data breaches and failure to notify within the 72-hour deadline
- Audits of data processing activities and records (ROPA)
- Enforcement of data subject rights (access, erasure, portability)
- Investigation of consent and lawful basis violations
- Coordination with EDPB for cross-border cases

**Notable Belgian GDPR fines:** APD has issued fines against Belgian companies and public bodies for cookie consent violations, insufficient security measures, and failure to honor data subject rights.

📎 [APD/GBA official website](https://www.autoriteprotectiondonnees.be) | [APD decisions database](https://www.autoriteprotectiondonnees.be/citoyen/themes/internet/decisions-et-avis) | [EDPB — Belgian authority page](https://edpb.europa.eu/about-edpb/about-edpb/members_en)

---

## 2.3  International Standards

These are voluntary frameworks adopted globally. They are not laws — but they are often referenced by regulations (NIS2 and CyFun both map to ISO 27001), required by clients and partners, or used as the benchmark for compliance audits.

---

### ISO/IEC 27001:2022

**Reference:** ISO/IEC 27001:2022 | **Publisher:** ISO / IEC | **Updated:** October 2022

> ISO 27001 is the international gold standard for information security management. A company that is "ISO 27001 certified" has had its entire security management system checked by an independent auditor — and passed. It signals to clients and regulators that you take security seriously.

**Technical overview:** ISO/IEC 27001:2022 specifies requirements for establishing, implementing, maintaining, and continually improving an Information Security Management System (ISMS). The 2022 revision reorganized the controls annex into 4 themes (Organizational, People, Physical, Technological — 93 controls total, down from 114 in 2013) and added 11 new controls covering threat intelligence, cloud security, ICT continuity, data masking, and secure coding.

Certification is issued by accredited third-party certification bodies after a Stage 1 (documentation review) and Stage 2 (on-site audit). Certifications last 3 years with annual surveillance audits.

**Applies to:** Any organization globally, any size, any sector.

**Key requirements:**
- Define ISMS scope and conduct a formal information security risk assessment
- Define risk treatment plan and implement Annex A controls as applicable
- Set measurable security objectives and monitor performance
- Management review and internal audit cycle
- Statement of Applicability (SoA) documenting chosen controls and justifications
- Continual improvement process

📎 [ISO 27001:2022 (ISO.org)](https://www.iso.org/standard/27001) | [ISO 27001 Annex A controls summary (ISMS.online)](https://www.isms.online/iso-27001/annex-a-controls/)

---

### ISO/IEC 27005:2022

**Reference:** ISO/IEC 27005:2022 | **Publisher:** ISO / IEC | **Updated:** October 2022

> If ISO 27001 is the what (what your security management must cover), ISO 27005 is the how for risk management — it gives organizations a detailed methodology for identifying, analyzing, evaluating, and treating information security risks.

**Technical overview:** ISO/IEC 27005:2022 provides guidelines for information security risk management in support of ISO 27001. The 2022 version aligned the risk management process with ISO 31000 principles. It covers the full risk management lifecycle: context establishment, risk identification (assets, threats, vulnerabilities, impacts), risk analysis (likelihood × consequence), risk evaluation (against risk criteria), risk treatment options (modify/avoid/share/retain), and monitoring and review. The standard supports both event-based and asset-based risk assessment approaches.

**Applies to:** Any organization implementing ISO 27001 or wanting a structured approach to information security risk management.

📎 [ISO 27005:2022 (ISO.org)](https://www.iso.org/standard/80585.html)

---

### SOC 2

**Reference:** SOC 2 — AICPA Trust Services Criteria | **Publisher:** AICPA (American Institute of Certified Public Accountants)

> SOC 2 is a security audit report that US-origin SaaS companies often need to show their clients. If you are working with an American cloud provider, they will often hand you their SOC 2 Type II report to prove their infrastructure is secure. It is becoming increasingly expected in B2B relationships globally.

**Technical overview:** SOC 2 is an auditing standard based on the AICPA's Trust Services Criteria across 5 categories: **Security** (mandatory), **Availability**, **Processing Integrity**, **Confidentiality**, and **Privacy**. Type I is a point-in-time assessment; Type II covers a period (typically 6–12 months) and is considered more meaningful. Audits are conducted by licensed CPA firms. SOC 2 does not result in a pass/fail certification — it produces a report with findings, which clients review and evaluate.

**Applies to:** Technology and cloud service providers, primarily those serving US clients or operating in B2B SaaS markets. Increasingly requested by European clients as an alternative or complement to ISO 27001.

**Key trust services criteria (Security):**
- Logical and physical access controls
- System operations and monitoring
- Change management
- Risk mitigation and incident response

📎 [AICPA SOC 2 overview](https://www.aicpa-cima.com/topic/audit-assurance/audit-and-assurance-greater-than-soc-2) | [SOC 2 vs ISO 27001 comparison (ISMS.online)](https://www.isms.online/iso-27001/soc-2-vs-iso-27001/)

---

## 2.4  Quick Reference — Comparison Table

| Regulation / Standard | Type | Scope | Enforced by | Penalties |
|-----------------------|------|-------|-------------|-----------|
| **GDPR** | EU Regulation (binding) | Personal data of EU residents — global reach | APD/GBA (Belgium), EDPB | Up to €20M / 4% turnover |
| **NIS2** | EU Directive (transposed) | Critical and important sectors — EU-wide | CCB (Belgium) | Up to €10M / 2% turnover |
| **DORA** | EU Regulation (binding) | Financial sector — EU-wide | NBB / FSMA (Belgium) | Up to 1% daily turnover |
| **Cyber Resilience Act** | EU Regulation (binding) | Product manufacturers / importers | Market surveillance authorities | Up to €15M / 2.5% turnover |
| **eIDAS 2.0** | EU Regulation (binding) | QTSPs, public administrations, large platforms | National supervisory bodies | QTSP suspension / revocation |
| **AI Act** | EU Regulation (binding) | AI system providers and deployers — EU-wide | National market surveillance authorities | Up to €35M / 7% turnover |
| **Belgian NIS2 Act** | Belgian Law | NIS2 entities in Belgium | CCB | Same as NIS2 |
| **CyFun** | Belgian Framework (voluntary/de facto) | All Belgian organizations | CCB (for NIS2 entities) | Indirect — via NIS2 enforcement |
| **ISO 27001:2022** | International Standard (voluntary) | Any organization | Certification bodies (auditors) | Loss of certification |
| **ISO 27005:2022** | International Standard (voluntary) | Any organization | N/A | N/A |
| **SOC 2** | US Auditing Standard (voluntary) | Technology/cloud providers | AICPA-licensed auditors | N/A (report-based) |

---

## 2.5  Applying Regulations — Gap Analysis

When you audit an organization in Belgium, the starting question is always: **which regulations apply to this organization?**

Use this decision flow:

1. **Does the organization process personal data of EU residents?** → GDPR applies (and APD/GBA has jurisdiction in Belgium)
2. **Is it in one of the 18 NIS2 sectors, medium or large size?** → Belgian NIS2 Act + CCB registration mandatory
3. **Is it a financial entity?** → DORA also applies (in addition to NIS2)
4. **Does it manufacture or sell hardware/software products in the EU?** → Cyber Resilience Act applies
5. **Does it use or deploy AI systems?** → AI Act risk classification required
6. **Has it chosen ISO 27001 certification?** → ISO 27005 for risk management methodology
7. **Does it serve US-based enterprise clients?** → SOC 2 report may be expected

A **gap analysis** compares the organization's current state to the requirements above. For each applicable regulation:

- Identify which controls are **present and adequate**
- Identify which controls are **present but insufficient** (partial — needs improvement)
- Identify which controls are **absent** (missing — immediate action needed)

Rate each gap by risk (likelihood × impact), prioritize, and build a remediation roadmap.

> ⚠️ Regulations often overlap. An organization covered by NIS2 and GDPR will find that many controls satisfy both simultaneously — for example, incident detection and logging, access control, encryption, and staff training. Documenting this overlap prevents double work.

---

## Official Sources

### European Union

| Regulation | Official Text | Guidance |
|-----------|--------------|---------|
| GDPR | [EUR-Lex](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX%3A32016R0679) | [EDPB Guidelines](https://edpb.europa.eu/our-work-tools/general-guidance/guidelines-recommendations-best-practices_en) |
| NIS2 Directive | [EUR-Lex](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX%3A32022L2555) | [ENISA NIS2](https://www.enisa.europa.eu/topics/cybersecurity-policy/nis-directive-new) |
| DORA | [EUR-Lex](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX%3A32022R2554) | [EBA DORA](https://www.eba.europa.eu/regulation-and-policy/operational-resilience/digital-operational-resilience-act-dora) |
| Cyber Resilience Act | [EUR-Lex](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX%3A32024R2847) | [ENISA CRA](https://www.enisa.europa.eu/topics/cybersecurity-policy/cyber-resilience-act) |
| eIDAS 2.0 | [EUR-Lex](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX%3A32024R1183) | [EUDIW ARF](https://github.com/eu-digital-identity-wallet/eudi-doc-architecture-and-reference-framework) |
| AI Act | [EUR-Lex](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX%3A32024R1689) | [AI Act tracker](https://artificialintelligenceact.eu/) |

### Belgium

| Source | URL |
|--------|-----|
| Centre for Cybersecurity Belgium (CCB) | [ccb.belgium.be](https://ccb.belgium.be/en) |
| CERT.be | [cert.be](https://cert.be) |
| Safeonweb.be (CCB awareness platform) | [safeonweb.be](https://www.safeonweb.be) |
| CyFun Framework | [ccb.belgium.be/en/cyberfundamentals-framework](https://ccb.belgium.be/en/cyberfundamentals-framework) |
| CyFun assessment tool | [atwork.safeonweb.be](https://atwork.safeonweb.be/cyberfundamentals-toolbox) |
| Belgian NIS2 Act (official gazette) | [ejustice.just.fgov.be](https://www.ejustice.just.fgov.be/eli/loi/2024/04/26/2024011823/justel) |
| APD / GBA (Belgian GDPR authority) | [autoriteprotectiondonnees.be](https://www.autoriteprotectiondonnees.be) |
| NBB — National Bank of Belgium (DORA oversight) | [nbb.be](https://www.nbb.be/en) |
| FSMA (Financial sector oversight) | [fsma.be](https://www.fsma.be/en) |

### International Standards

| Standard | URL |
|----------|-----|
| ISO/IEC 27001:2022 | [iso.org/standard/27001](https://www.iso.org/standard/27001) |
| ISO/IEC 27005:2022 | [iso.org/standard/80585.html](https://www.iso.org/standard/80585.html) |
| SOC 2 (AICPA) | [aicpa-cima.com/topic/audit-assurance/audit-and-assurance-greater-than-soc-2](https://www.aicpa-cima.com/topic/audit-assurance/audit-and-assurance-greater-than-soc-2) |
| ENISA (EU Agency for Cybersecurity) | [enisa.europa.eu](https://www.enisa.europa.eu) |
| EDPB (European Data Protection Board) | [edpb.europa.eu](https://edpb.europa.eu) |
