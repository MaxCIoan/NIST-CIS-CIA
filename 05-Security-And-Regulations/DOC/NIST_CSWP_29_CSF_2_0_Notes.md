# NIST CSWP 29 - Cybersecurity Framework 2.0 Notes

Source:

- NIST CSWP 29, *The NIST Cybersecurity Framework (CSF) 2.0*
- PDF: https://nvlpubs.nist.gov/nistpubs/CSWP/NIST.CSWP.29.pdf
- DOI: https://doi.org/10.6028/NIST.CSWP.29
- Publication date: February 26, 2024

## Publisher context

This source is from the **National Institute of Standards and Technology (NIST)**, a U.S. government agency.

Important context for your notes:

- NIST is part of the **U.S. Department of Commerce**
- NIST is headquartered in **Gaithersburg, Maryland**
- NIST’s mission is tied to **U.S. innovation, industrial competitiveness, standards, and technology**

Why that matters:

- this is an **authoritative public framework source**
- it is **American**, not Belgian or EU law
- it is a **framework and guidance source**, not a binding Belgian regulation
- it is still highly useful as a security reference because it is widely adopted internationally

## Why this belongs here

This document belongs in `05-Security-And-Regulations` because it is a cybersecurity risk management framework used to:

- assess current posture
- define target posture
- prioritize remediation
- communicate cyber risk to technical and non-technical audiences

It is highly relevant to your final project because the project asks for:

- gap analysis
- prioritized findings
- risk-based recommendations
- professional reporting

## Short summary

NIST CSF 2.0 is a flexible cybersecurity risk management framework that any organization can use, regardless of size or sector. It does not prescribe exact technical settings. Instead, it gives a structured way to describe cybersecurity outcomes, compare current vs target state, and communicate priorities.

The major update in CSF 2.0 is the addition of the **GOVERN** function, which emphasizes that cybersecurity is not only a technical issue but also a governance, accountability, and risk management issue.

For your project, the main value is this:

- structure your findings using a known framework
- describe current state vs desired state
- prioritize improvements
- connect technical issues to governance and business risk

## Chapter-by-chapter notes

## 1. Cybersecurity Framework Overview

### Main idea

CSF 2.0 has three main components:

- `CSF Core`
- `Organizational Profiles`
- `Tiers`

The framework helps organizations:

- understand risk
- assess risk
- prioritize action
- communicate cyber posture

### Key takeaway

CSF is not a checklist of tools. It is a structured model for discussing cybersecurity outcomes.

### Why it matters for your project

This is directly useful for your report structure:

- current state = what you found
- target state = what should exist
- gap = your findings
- priorities = your remediation roadmap

## 2. Introduction to the CSF Core

### Main idea

The CSF Core is built from:

- `Functions`
- `Categories`
- `Subcategories`

The six functions in CSF 2.0 are:

- `GOVERN`
- `IDENTIFY`
- `PROTECT`
- `DETECT`
- `RESPOND`
- `RECOVER`

### Important update

`GOVERN` is new in CSF 2.0 and is a major change.

It covers:

- risk management strategy
- policy
- expectations
- roles and responsibilities
- oversight
- supply chain risk

### Why it matters for your project

This gives you a very clean way to categorize findings.

Examples:

- weak account governance -> `GOVERN`
- unknown assets or weak inventory -> `IDENTIFY`
- bad password policy -> `PROTECT`
- missing audit logs -> `DETECT`
- no incident plan -> `RESPOND`
- no backup / redundancy -> `RECOVER`

## 2.1 The Six Functions in practical language

### GOVERN

Cybersecurity leadership, accountability, policy, and risk ownership.

Use it when writing about:

- privileged account governance
- who owns security decisions
- whether security is documented and reviewed
- supply chain responsibilities

### IDENTIFY

Know what you have and what matters.

Use it when writing about:

- users and groups in AD
- service accounts
- servers, VLANs, and exposed services
- asset inventory gaps

### PROTECT

Safeguards that reduce likelihood or impact.

Use it when writing about:

- password policy
- account lockout
- least privilege
- segmentation
- secure administration

### DETECT

Find malicious or abnormal activity.

Use it when writing about:

- Event ID `4688`
- PowerShell `4104`
- Sysmon
- logging gaps

### RESPOND

Take action during an incident.

Use it when writing about:

- alert handling
- incident response process
- escalation paths

### RECOVER

Restore operations and resilience.

Use it when writing about:

- backup strategy
- single DC risk
- service restoration
- business continuity

## 3. Introduction to CSF Profiles and Tiers

### 3.1 Profiles

Profiles describe the organization’s cybersecurity posture using the CSF outcomes.

Important types:

- `Current Profile`
  - what the organization currently does
- `Target Profile`
  - what the organization wants or needs to achieve

### Why this matters for your project

This is almost exactly what your audit is doing.

You can think of:

- your screenshots, commands, and `.pkt` review = `Current Profile`
- your recommendations = movement toward the `Target Profile`

### 3.2 Tiers

Tiers describe how mature and rigorous the organization’s cybersecurity risk governance is.

The four tiers are:

- Tier 1: `Partial`
- Tier 2: `Risk Informed`
- Tier 3: `Repeatable`
- Tier 4: `Adaptive`

### Key takeaway

Tiers are not scores or certifications. They are context for maturity.

### Why it matters for your project

You can use this indirectly when describing maturity:

- a small AD lab with partial logging and a single DC is not highly mature
- documented controls, repeatable policy, and monitoring suggest stronger maturity

## 4. Online Resources That Supplement the CSF

### Main idea

The CSF document itself is stable and high-level. NIST keeps additional resources online that can be updated more often.

Important supporting resource types:

- `Informative References`
- `Implementation Examples`
- `Quick Start Guides`

### Key takeaway

CSF tells you the outcomes; supporting resources help with how to reach them.

### Why it matters for your project

This matters because your final report can do the same:

- state the desired security outcome
- then recommend practical ways to get there

## 5. Improving Cybersecurity Risk Communication and Integration

### Main idea

CSF is designed to improve communication between:

- executives
- managers
- practitioners

It also helps integrate cybersecurity with broader enterprise risk management.

### Key takeaway

Cybersecurity is not only a technical operations issue. It must be explained in business language too.

### Why it matters for your project

This supports the report requirement for:

- executive summary
- non-technical explanation of top risks
- prioritized remediation

It also supports the way you should write findings:

- technical observation
- business impact
- framework reference
- recommendation

## Appendix A - CSF Core

### Main idea

This appendix contains the actual outcome structure of the framework.

### Why it matters for your project

This is the most useful part if you want to map a finding to:

- a governance outcome
- an identity or access outcome
- a monitoring outcome
- a recovery outcome

## Best sections for your final project

If you do not want to read the whole document in depth, focus on:

1. Section 1 - Overview
2. Section 2 - Core
3. Section 3 - Profiles and Tiers
4. Section 5 - Risk communication and integration
5. Appendix A - Core outcomes

## How to use CSF 2.0 in your report

Use CSF 2.0 when you want to:

- structure your findings cleanly
- explain current vs target state
- justify remediation priorities
- communicate cyber risk in business language

### Example mappings

- single DC / no redundancy -> `RECOVER`
- weak visibility or missing logs -> `DETECT`
- privileged account sprawl -> `GOVERN` and `PROTECT`
- poor asset understanding -> `IDENTIFY`
- missing policy ownership -> `GOVERN`
- weak segmentation in Packet Tracer -> `PROTECT`

## How to describe this source correctly

You are right that this is an American source.

A good way to describe it in your notes or report:

```markdown
NIST CSF 2.0 is a U.S. cybersecurity risk management framework published by the National Institute of Standards and Technology, an agency of the U.S. Department of Commerce headquartered in Gaithersburg, Maryland. It is not Belgian law, but it is an authoritative and internationally used framework for structuring cyber risk governance, assessment, and improvement.
```

## Quick takeaways for your notes

- CSF 2.0 is a framework, not a law
- it is high-level and outcome-based
- it is useful for gap analysis
- it is useful for current vs target state comparison
- it is useful for executive communication
- the new `GOVERN` function is especially important

## One-line version

NIST CSF 2.0 gives you a structured way to describe what cybersecurity outcomes exist now, what should exist next, and how to communicate the gap clearly.
