# Phase 2 Network Audit Guide

This guide covers **Phase 2: group compliance audit of a Packet Tracer network topology**.

It is based on:

- [01_SecurityConcepts.md](./01_SecurityConcepts.md)
- [02_Regulations.md](./02_Regulations.md)
- [03_FinalProject.md](./03_FinalProject.md)

## 1. What the project is really asking you to do

This is not a pentest and not a build exercise.

You are acting as a junior security auditor. Your job is to:

1. Identify what exists
2. Compare it against security principles and regulations
3. Rate the risk
4. Give clear remediation actions

Your findings must always connect:

- a technical observation
- a business/security risk
- a framework or regulation reference
- a realistic remediation

Use this logic everywhere:

`Observation -> Risk -> Reference -> Recommendation`

## 2. Core concepts to apply while auditing

Use these actively:

- `CIA triad`
- `Attack surface`
- `Threat, vulnerability, risk`
- `Defense in depth`
- `Least privilege`
- `Zero Trust mindset`

In this phase, they apply especially to:

- VLANs
- ACLs
- routing
- management access
- internet exposure
- public-facing services
- network resilience

## 3. Regulations and frameworks to map findings to

Most useful references:

- `GDPR`
- `NIS2` and Belgian NIS2 law
- `CyFun`
- `ISO 27001:2022`

Useful mappings:

- weak segmentation -> NIS2, CyFun, ISO 27001
- weak monitoring -> NIS2, CyFun, ISO 27001
- broad firewall / ACL access -> NIS2, CyFun, ISO 27001
- lack of resilience -> NIS2, CyFun, ISO 27001
- personal data exposure -> GDPR

## 4. Packet Tracer file to use

Use these as the scope for Phase 2:

- `C:\Users\maxco\Desktop\BECODE\shared_Network_Teams(SUBMISSION2).pkt`
- `C:\Users\maxco\Desktop\BECODE\AXMALI_Network_Implementation_Report_Final 7.docx`

Use them together:

- the `.pkt` file for topology, VLANs, device links, IP layout, and visible services
- the `.docx` report for design intent, implementation details, and supporting explanations

This means your Phase 2 audit should be based on both the technical topology and the written implementation report.

## 5. Goal

Audit a network you did not build and judge whether its design is secure and compliant.

This is a `paper audit`, not a live exploitation test.

## 6. Best order to work

1. Read the topology end to end
2. Identify assets and trust boundaries
3. Map VLANs, subnets, gateways, and ACLs
4. Review admin access paths
5. Review internet exposure
6. Review logging, monitoring, and resilience
7. Build the findings list

Before step 1, open the Packet Tracer file and make a quick inventory table with:

- device name
- device type
- IP/subnet
- VLAN
- role
- management access method
- uplinks/downlinks

## 7. What to inspect in Packet Tracer

Check these areas:

- `Segmentation`
  - are Management, Production, Support, IT, Servers separated?
- `Routing and ACLs`
  - which VLANs can talk to which VLANs?
  - are rules too broad?
- `Firewall policy`
  - inbound rules
  - outbound rules
  - any `allow any any` style rule
- `Management plane security`
  - who can access routers, switches, firewall, servers?
  - SSH or Telnet?
  - default passwords?
- `Server placement`
  - public-facing services in a DMZ or mixed with internal servers?
- `DNS / DHCP / AD dependencies`
  - are critical services separated or all on one box?
- `Monitoring`
  - syslog, IDS/IPS, NetFlow, SNMP, logging destinations
- `Resilience`
  - single firewall, single switch, single server, no backup links

## 8. Questions to ask while reviewing the topology

- Can a compromised user workstation reach critical servers directly?
- Can Support or Study reach management interfaces they do not need?
- Is the server network isolated from user VLANs?
- Are admin protocols exposed to large parts of the network?
- Is internet-facing traffic clearly separated from internal traffic?
- Is there any evidence of least privilege in ACLs?
- Is there a logging path for incidents?
- What fails if one device goes down?

## 9. Typical Packet Tracer findings

Strong candidate findings:

- Flat network or weak VLAN separation
- Overly permissive inter-VLAN ACLs
- Telnet enabled instead of SSH
- Shared admin credentials
- No DMZ for exposed services
- No central logging or monitoring
- No documented backup path or device redundancy
- Sensitive servers accessible from too many VLANs

## 10. How to divide team work

Use a simple split:

- Person 1: topology and segmentation review
- Person 2: regulations/framework mapping and risk rating
- Person 3: report assembly and presentation preparation

Then do one final joint review so everyone can defend the findings.

## 11. Suggested report structure

### Executive Summary

- overall risk level
- top 3 issues
- what should be fixed first

### Scope and Methodology

- what environment you reviewed
- what evidence you used
- whether you had live access or only documentation

### Findings

For each finding include:

- description
- evidence
- risk
- reference
- severity
- recommendation

### Risk Summary Table

| ID | Finding | Severity | Reference | Priority |
|---|---|---|---|---|
| F1 | Overly permissive inter-VLAN ACLs | High | NIS2 / CyFun / ISO 27001 | Immediate |

### Conclusion

- overall posture
- quick wins
- strategic improvements

## 12. Fast checklist before you submit

Before you finish, verify:

- every finding has evidence
- every finding has a reference
- every finding has a remediation
- risks are prioritized consistently
- you separate facts from opinions
- you explain business impact, not just technical detail

## 13. Useful links

### Local repo links

- [Security Concepts](./01_SecurityConcepts.md)
- [Regulations](./02_Regulations.md)
- [Final Project Brief](./03_FinalProject.md)

### GitHub links

- [Repo root](https://github.com/becodeorg/BXL-Kamkar-5)
- [Security Concepts on GitHub](https://github.com/becodeorg/BXL-Kamkar-5/blob/main/01-Fundamentals/05-Security-And-Regulations/01_SecurityConcepts.md)
- [Regulations on GitHub](https://github.com/becodeorg/BXL-Kamkar-5/blob/main/01-Fundamentals/05-Security-And-Regulations/02_Regulations.md)
- [Final Project on GitHub](https://github.com/becodeorg/BXL-Kamkar-5/blob/main/01-Fundamentals/05-Security-And-Regulations/03_FinalProject.md)

### External references

- [NIST Cybersecurity Framework](https://www.nist.gov/cyberframework)
- [NIST SP 800-207 Zero Trust](https://csrc.nist.gov/publications/detail/sp/800-207/final)
- [MITRE ATT&CK](https://attack.mitre.org/)
- [CCB CyberFundamentals Framework](https://ccb.belgium.be/en/cyberfundamentals-framework)
- [CCB NIS2 portal](https://ccb.belgium.be/en/nis2)
- [Belgian NIS2 Act](https://www.ejustice.just.fgov.be/eli/loi/2024/04/26/2024011823/justel)
- [GDPR official text](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX%3A32016R0679)
- [NIS2 official text](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX%3A32022L2555)
- [ISO 27001 overview](https://www.iso.org/standard/27001)

## 14. Final advice

Think like a consultant reviewing trust boundaries, segmentation, exposure, and resilience.

If you are unsure whether something belongs in the report, ask:

- Does it increase risk?
- Can I prove it?
- Can I map it to a framework?
- Can I propose a realistic fix?

If the answer is yes to all four, include it.
