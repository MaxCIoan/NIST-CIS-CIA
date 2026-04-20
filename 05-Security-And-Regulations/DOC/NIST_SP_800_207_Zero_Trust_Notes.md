# NIST SP 800-207 - Zero Trust Architecture Notes

Source:

- NIST SP 800-207, *Zero Trust Architecture*
- PDF: https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-207.pdf
- DOI: https://doi.org/10.6028/NIST.SP.800-207

## Why this belongs here

This document fits best in `05-Security-And-Regulations` because Zero Trust is a security architecture and governance topic. It supports:

- [01_SecurityConcepts.md](./01_SecurityConcepts.md)
- [02_Regulations.md](./02_Regulations.md)
- [03_FinalProject.md](./03_FinalProject.md)

It is especially useful for:

- least privilege
- identity-based access control
- monitoring and continuous verification
- limiting lateral movement
- justifying AD and network hardening recommendations

## Short summary

NIST SP 800-207 explains Zero Trust as a security model where trust is never assumed just because a user or device is inside the network. Access decisions should be made per request, with least privilege, based on identity, device state, policy, and context.

The document does not present one product or one fixed deployment design. It presents principles, architecture components, deployment patterns, risks, and a migration path.

For your project, the main value is this:

- do not trust internal network location by default
- verify users, devices, and sessions continuously
- minimize privilege
- segment access to reduce lateral movement
- log and monitor access decisions

## Chapter-by-chapter notes

## 1. Introduction

### Main idea

NIST frames Zero Trust as a response to the weakness of traditional perimeter-based security. The old model assumes internal traffic is more trustworthy than external traffic. That breaks down with:

- remote work
- cloud services
- BYOD
- contractors and partners
- internal attackers or compromised accounts

### Key takeaway

Zero Trust is not a single product. It is a strategy and architecture approach.

### Why it matters for your AD audit

This supports findings such as:

- internal users should not automatically get broad access
- privileged accounts need stricter controls
- being inside the domain or LAN is not enough to justify trust

## 2. Zero Trust Basics

### Main idea

NIST defines Zero Trust as minimizing uncertainty when making least-privilege, per-request access decisions in an environment that must be treated as potentially compromised.

The focus shifts from protecting only the network perimeter to protecting resources directly:

- data
- applications
- services
- devices
- workloads

### Key takeaway

The goal is granular access control and reduced lateral movement.

### Why it matters for your project

This is highly relevant to both phases:

- Phase 1 AD: restrict who can access what in the domain
- Phase 2 network: avoid flat networks and broad inter-VLAN trust

## 2.1 Tenets of Zero Trust

### Main idea

This is one of the most useful sections in the whole paper.

NIST explains that a ZTA should be designed around a set of core tenets. In practical terms, the most important ones are:

- all data sources and computing services are resources
- all communication should be secured, regardless of network location
- access should be granted per session
- access should be based on dynamic policy
- the enterprise should monitor asset integrity and security posture
- authentication and authorization should be continuous where possible

### Practical reading for your project

Map these tenets directly to findings:

- weak password or admin sprawl -> breaks least privilege
- admin account in default container -> weak privileged identity governance
- no MFA recommendation for admins -> weak identity assurance
- flat network or broad ACLs -> too much implicit trust
- missing logs -> poor continuous verification

## 2.2 A Zero Trust View of a Network

### Main idea

NIST makes it clear that network location should not be treated as proof of trust. Internal networks must be treated as potentially hostile too.

### Key takeaway

The model assumes:

- traffic may be monitored or modified
- every access request should be authenticated and authorized
- remote and internal access should be governed consistently

### Why it matters for your Packet Tracer audit

This is a direct argument for:

- VLAN segmentation
- restricted management access
- DMZ placement
- ACL review
- avoiding broad “inside can talk to everything” rules

## 3. Logical Components of Zero Trust Architecture

### Main idea

NIST breaks ZTA into logical building blocks.

Important components:

- `Policy Engine (PE)`
  - decides whether access is allowed, denied, or revoked
- `Policy Administrator (PA)`
  - executes the PE decision and sets up or tears down the session
- `Policy Enforcement Point (PEP)`
  - the point where access is actually enforced

The PE uses inputs such as:

- enterprise policies
- threat intelligence
- compliance information
- asset/device state
- logs and telemetry

### Key takeaway

Zero Trust depends on policy + identity + device context + logging.

### Why it matters for your AD audit

This supports recommendations such as:

- stronger GPO-based controls
- better privileged account governance
- logging and monitoring as part of access control
- verifying service accounts and admin accounts more strictly

## 3.1 Variations of Zero Trust Architecture Approaches

### Main idea

NIST explains that ZTA can be implemented in different ways, including:

- enhanced identity governance
- micro-segmentation
- network infrastructure / software-defined perimeter style controls

### Key takeaway

You do not “buy Zero Trust.” You assemble it through identity, segmentation, policy, and enforcement.

### Why it matters for your project

You can use this section to justify:

- strong identity controls in AD
- separate OUs and admin accounts
- segmented network design in Packet Tracer
- limiting access per role and per service

## 3.2 Deployed Variations of the Abstract Architecture

### Main idea

NIST gives several deployment models:

- device agent or gateway based
- enclave based
- resource portal based
- application sandboxing

### Key takeaway

The exact implementation can vary depending on the enterprise environment.

### Why it matters for your project

For your bootcamp work, the exact model matters less than the principle:

- put the control point as close as possible to the resource
- do not rely only on a big outer firewall

## 3.3 Trust Algorithm

### Main idea

The trust algorithm is the logic used by the policy engine to decide whether access should be granted.

It can consider:

- user identity
- device posture
- role
- behavior history
- threat intelligence
- resource sensitivity
- time and context

### Key takeaway

Access is a decision process, not a static yes/no based only on login success.

### Why it matters for your audit

This is useful when writing recommendations such as:

- privileged accounts should have stricter conditions
- service accounts should be tightly scoped
- access decisions should be supported by logs and review

## 3.4 Network and Environment Components

### Main idea

NIST notes that the architecture requires strong supporting components:

- identity services
- asset management
- logging
- policy infrastructure
- secure communications

### Key takeaway

Zero Trust fails if the inventory, identity, or monitoring layers are weak.

### Why it matters for your AD audit

This supports using findings about:

- undocumented service accounts
- incomplete asset visibility
- limited monitoring
- privileged accounts without stricter control

## 4. Deployment Scenarios / Use Cases

### Main idea

NIST gives practical situations where ZTA improves security, including:

- enterprises with satellite offices
- multi-cloud organizations
- contractor / nonemployee access
- cross-enterprise collaboration
- public-facing services

### Key takeaway

ZTA is especially relevant when users, devices, and resources are distributed.

### Why it matters for your project

This is a strong reference when discussing:

- remote admin access to your Azure DC
- third-party or customer-facing services in network designs
- why a traditional internal/external split is no longer enough

## 5. Threats Associated with Zero Trust Architecture

### Main idea

NIST is clear that ZTA reduces risk but does not remove it. It creates its own important failure points.

Threats discussed include:

- subversion of the decision process
- denial of service or network disruption
- stolen credentials / insider abuse
- lack of visibility
- sensitive logging and system information storage
- reliance on proprietary formats or solutions
- risks around non-person entities such as service accounts

### Key takeaway

Zero Trust still depends on:

- secure administration
- resilient control points
- strong logging
- good operational discipline

### Why it matters for your AD audit

This section is directly useful for findings about:

- service accounts
- Domain Admins
- single points of failure
- incomplete monitoring

## 6. Interaction with Existing Guidance

### Main idea

NIST explains how ZTA fits with other federal guidance and frameworks rather than replacing them.

It aligns with:

- risk management
- privacy
- identity and access management
- trusted connections
- continuous diagnostics and monitoring

### Key takeaway

Zero Trust complements existing governance and compliance work.

### Why it matters for your project

This is useful for your module because it connects well with:

- NIS2
- CyFun
- ISO 27001
- GDPR

It helps argue that Zero Trust is not just a technical idea; it supports compliance goals too.

## 7. Migrating to a Zero Trust Architecture

### Main idea

NIST does not recommend a “big bang” replacement. It recommends gradual migration.

NIST explicitly says hybrid environments are normal for a long time.

### 7.1 Pure ZTA

This is the ideal state, but not realistic for many organizations in one step.

### 7.2 Hybrid ZTA and Perimeter-Based Architecture

This is the practical reality for most enterprises: some Zero Trust controls, some legacy perimeter controls.

### 7.3 Steps to Introduce ZTA

This is one of the most useful implementation sections.

NIST says you need:

- a detailed inventory of assets
- a detailed inventory of subjects
- knowledge of user privileges
- business process understanding
- workflow and data flow awareness
- awareness of shadow IT

### Key takeaway

You cannot implement Zero Trust without knowing:

- who your users are
- what your devices are
- what talks to what
- which privileges exist
- which business processes must keep working

### Why it matters for your project

This section maps almost perfectly to your audit tasks:

- AD inventory
- Domain Admin review
- service account review
- DNS/DHCP dependency review
- VLAN and ACL review
- topology and trust-boundary mapping

## Best sections for your final project

If you do not want to read the whole paper deeply, focus on:

1. Section 2 - Zero Trust Basics
2. Section 2.1 - Tenets of Zero Trust
3. Section 3 - Logical Components
4. Section 5 - Threats
5. Section 7 - Migration steps

These five sections are the most useful for writing security recommendations.

## How to use this paper in your report

Good ways to cite it:

- to justify least privilege
- to justify stronger controls on privileged identities
- to justify segmentation
- to justify continuous verification and logging
- to justify treating the internal network as potentially hostile

### Example mappings

- `azureadmin` in `Domain Admins` and left in `CN=Users`
  - supports a recommendation for tighter privileged identity governance
- non-expiring service account passwords
  - supports stricter control of non-person entities
- missing or partial monitoring
  - supports continuous verification and logging recommendations
- broad network trust in Packet Tracer
  - supports segmentation and per-resource access controls

## Quick takeaways for your notes

- Zero Trust is about protecting resources, not just the perimeter
- network location should not create implicit trust
- access should be least privilege and per session
- identity, device posture, policy, and telemetry should all influence access
- logging is part of access control, not just incident response
- migration is gradual and inventory-driven

## One-line version

NIST SP 800-207 says organizations should stop trusting users and devices just because they are “inside,” and should instead make granular, policy-based, continuously informed access decisions for every important resource.
