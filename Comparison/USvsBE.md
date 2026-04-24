# Detailed Comparison: US Audit Report vs. BE Audit Report

While both reports analyze the exact same network infrastructure (the Byte Sized Solutions / BeCode project) and identify the same fundamental technical flaws, they differ significantly in their evaluation lens, their respective regulatory frameworks, and how they articulate business impact.

## 1. Regulatory Frameworks and Reference Standards

- **US Report**: This report is built around the American federal standard **NIST SP 800-53 Rev. 5**. It relies on verifying specific control families (e.g., IA-2 for Identification and Authentication, SC-8 for System and Communications Protection). It highlights the gap between provided documentation and technical reality as a major compliance violation (NIST CM-7, CM-9).
- **BE Report**: The document is strictly aligned with European legal obligations, specifically the **NIS2 Directive** (transposed by the Belgian Law of April 26, 2024), as well as the **CyFun (CyberFundamentals)** evaluation framework created by the CCB (Centre for Cybersecurity Belgium).

## 2. Scoring Systems and Overall Evaluation

- **US Report**: The security posture is evaluated via a global compliance score estimated between **25% and 30%** (deemed INSUFFICIENT). The report identifies a total of 26 vulnerabilities, including **4 of CRITICAL severity**. The focus is on a granular, technical risk score.
- **BE Report**: The evaluation relies on the CyFun maturity scale. The organization scores **1.2 out of 5 (Level 1 - Initial)**. The report breaks down scores by cybersecurity function: Identify (1.0/5), Protect (1.5/5), Detect (0.5/5), Respond (0.5/5), and Recover (1.0/5). Out of the 10 mandatory measures of NIS2 Article 21(2), the score is **0/10**. Furthermore, this report classifies **5 vulnerabilities as CRITICAL**.

## 3. Consequences, Risks, and Business Impact

- **US Report**: The document focuses heavily on immediate **operational and tactical risks**. It highlights a "Top 3 priorities for management," focusing on the threat of unrestricted lateral movement, the potential for an internal attacker to take full control of the infrastructure, and the exposure of critical servers.
- **BE Report**: The impact presented here is primarily **financial and legal**. The report classifies the company as an "Important Entity" (IE) under NIS2 and warns that non-compliance exposes it to **administrative fines of up to €7 million** (or 1.4% of global turnover). It also emphasizes the risk of **personal liability for management board members** (Article 20 of the directive) and the legal obligation to register with the CCB.

## 4. Presentation and Remediation of Technical Findings

- **US Report**: Vulnerabilities are listed with a precise severity score (e.g., Score 9.1). The first critical flaw highlighted (F-01) is cryptographic in nature: the use of legacy SSH Version 1 on the core router. The goal is to patch vulnerabilities to block attackers.
- **BE Report**: Every technical flaw is formally tied to a legal requirement (e.g., Belgian Law, NIS2 Art. 21) and a required CyFun function. The first critical flaw (F-01) identified is the bypass of VLAN segmentation due to default gateway misconfigurations on 30 client workstations. The goal is to fix processes to prove due diligence to authorities.

------

## 🔍 Deep Dive: Understanding the Frameworks & Context

To better understand *why* these two reports look so different despite analyzing the same network, it helps to understand the philosophy behind the frameworks they use.

### The American Approach: NIST SP 800-53

- **What it is**: Developed by the National Institute of Standards and Technology, it is a comprehensive catalog of privacy and security controls originally meant for U.S. federal information systems.
- **The Philosophy**: It is highly granular, deeply technical, and prescriptive. It focuses on implementing specific controls (over 1,000 are available) to mitigate specific threat vectors. It is often used globally as a "gold standard" blueprint for building secure IT environments from the ground up.

### The European Approach: NIS2 Directive

- **What it is**: The Network and Information Security (NIS2) Directive is EU-wide legislation aimed at achieving a high common level of cybersecurity across Member States.
- **The Philosophy**: NIS2 shifts cybersecurity from being an "IT department problem" to a "Boardroom legal obligation." It doesn't tell you exactly *how* to configure your router; instead, it demands that critical infrastructure and essential businesses have systemic risk management, incident reporting capabilities (within 24 hours), and holds C-level executives personally accountable for negligence.

### The Belgian Bridge: CCB CyFun (CyberFundamentals)

- **What it is**: A framework created by the Centre for Cybersecurity Belgium designed to help organizations, especially SMEs, practically implement the high-level legal requirements of NIS2.
- **The Philosophy**: It borrows the core structure from the NIST Cybersecurity Framework (Identify, Protect, Detect, Respond, Recover) but tailors it to European realities. It uses a "Maturity Model" (scoring from 1 to 5) which is extremely useful for C-level executives to track progress over time, rather than just looking at a list of IT bugs.

**Summary**: A US-style technical audit answers the question: *"Can we be hacked, and how do we stop it?"* A European NIS2/CyFun audit answers the question: *"Are we legally negligent, and will the government fine our executives if we get hacked?"* Both perspectives are crucial for modern enterprise security.