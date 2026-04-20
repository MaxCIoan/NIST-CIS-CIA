# Resource Passports

This file is a compact profile of the reference sources used in this module.

Use it to answer questions like:

- Who published this?
- Is it law, framework, standard, or guidance?
- Which country or institution does it come from?
- How strong is it as a source in your report?
- What is the best way to use it?

## 1. NIST SP 800-207 - Zero Trust Architecture

### Identity

- Title: `Zero Trust Architecture`
- Reference: `NIST SP 800-207`
- Publisher: `National Institute of Standards and Technology (NIST)`
- Country: `United States`
- Institution type: `U.S. government standards and guidance body`
- Parent institution: `U.S. Department of Commerce`
- Headquarters context: `Gaithersburg, Maryland`
- Publication date: `August 2020`
- DOI: `https://doi.org/10.6028/NIST.SP.800-207`
- PDF: `https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-207.pdf`

### Source type

- Type: `government guidance / architecture publication`
- Legal status: `not Belgian law, not EU law, not mandatory by itself`
- Authority level: `high`

### What it is good for

- explaining Zero Trust
- justifying least privilege
- justifying segmentation
- justifying continuous verification and logging
- supporting recommendations for privileged access hardening

### Best use in your report

Use it as an **architecture and security-principles** reference.

### Limits

- not a regulation
- not Belgium-specific
- does not tell you exact Windows or Cisco settings

### One-line passport

An authoritative U.S. government architecture guide from NIST, part of the Department of Commerce in Gaithersburg, Maryland, useful for explaining Zero Trust principles and access-control design.

## 2. NIST CSWP 29 - Cybersecurity Framework 2.0

### Identity

- Title: `The NIST Cybersecurity Framework (CSF) 2.0`
- Reference: `NIST CSWP 29`
- Publisher: `National Institute of Standards and Technology (NIST)`
- Country: `United States`
- Institution type: `U.S. government standards and guidance body`
- Parent institution: `U.S. Department of Commerce`
- Headquarters context: `Gaithersburg, Maryland`
- Publication date: `February 26, 2024`
- DOI: `https://doi.org/10.6028/NIST.CSWP.29`
- PDF: `https://nvlpubs.nist.gov/nistpubs/CSWP/NIST.CSWP.29.pdf`

### Source type

- Type: `government framework / risk management guidance`
- Legal status: `not Belgian law, not EU law, not mandatory by itself`
- Authority level: `high`

### What it is good for

- structuring gap analysis
- describing current vs target state
- categorizing findings
- prioritizing remediation
- communicating risk to technical and business audiences

### Best use in your report

Use it as a **framework for assessment and communication**.

### Limits

- not a regulation
- does not replace NIS2, GDPR, or CyFun
- outcome-based, not configuration-specific

### One-line passport

An authoritative U.S. government cybersecurity framework from NIST, part of the Department of Commerce in Gaithersburg, Maryland, useful for structuring findings, maturity, and remediation priorities.

## 3. How to talk about NIST correctly

If a coach asks whether NIST is “American,” “Maryland-based,” or “Commerce-related,” this is the safe answer:

```markdown
NIST is the U.S. National Institute of Standards and Technology. It is a U.S. government agency that is part of the Department of Commerce, and its headquarters are in Gaithersburg, Maryland. Its publications are authoritative guidance and framework sources, not Belgian law or EU law.
```

## 4. How to rank sources in your final project

A practical order of strength for your report:

1. `Law / regulation`
   - GDPR, NIS2, Belgian NIS2 Act
2. `National framework linked to regulation`
   - CyFun, CCB guidance
3. `International standards / government frameworks`
   - ISO 27001, NIST CSF, NIST SP 800-207
4. `Vendor or community guidance`
   - Microsoft hardening docs, CIS, SwiftOnSecurity, etc.

This means:

- use EU/Belgian law to justify obligations
- use CyFun / ISO / NIST to justify structure, controls, and best practice

## 3. CIS Controls v8.1

### Identity

- Title: `CIS Critical Security Controls Version 8.1`
- Common name: `CIS Controls v8.1`
- Publisher: `Center for Internet Security (CIS)`
- Country context: `United States`
- Institution type: `independent nonprofit organization`
- Government status: `not a government agency`
- Official page: `https://www.cisecurity.org/controls/v8-1`
- Download page: `https://learn.cisecurity.org/cis-controls-download`
- Overview page: `https://www.cisecurity.org/insights/white-papers/cis-critical-security-controls-v8-1`
- Release context: `v8.1 public update in 2024`

### Source type

- Type: `best-practice control framework`
- Legal status: `not law, not regulation, not mandatory by itself`
- Authority level: `medium-high`

### What it is good for

- concrete hardening recommendations
- prioritized safeguards
- technical remediation guidance
- strengthening implementation sections of a report

### Best use in your report

Use it as a **practical control and implementation reference**.

### Limits

- not Belgian law
- not EU law
- not a formal public regulator
- should not be used alone to claim legal obligation

### One-line passport

An influential nonprofit best-practice framework from CIS, useful for turning audit findings into concrete technical safeguards, but not a legal or regulatory authority.
