# NIST CSF 2.0 Framework to Follow

Use this as the main framework for your Phase 1 Active Directory audit.

Primary framework:

- `NIST Cybersecurity Framework 2.0`

Supporting references:

- `CIS Controls v8.1` for practical technical safeguards
- `CyFun / NIS2` for Belgian and EU compliance relevance
- `GDPR` only where personal data or privacy risk is involved

## 1. Why NIST CSF 2.0 fits this project

NIST CSF 2.0 is the best main framework for your situation because your task is a **gap analysis**, not a full ISO certification audit.

It helps you organize:

- what exists now
- what is missing
- what risk that creates
- what should be improved first

The six NIST CSF 2.0 functions are:

- `Govern`
- `Identify`
- `Protect`
- `Detect`
- `Respond`
- `Recover`

For your AD audit, use those six functions as the main sections of your findings.

## 2. How to use this framework

For every finding, follow this structure:

```markdown
### Finding title

NIST CSF function:

Current state:

Risk:

Evidence:

Recommended target state:

Recommended action:

Priority:
```

This keeps every finding clear and defensible.

## 3. Govern

Govern is about who owns cybersecurity risk, how decisions are made, and whether policies and responsibilities are clear.

### What to check

- Who has Domain Admin rights?
- Are admin accounts separated from normal accounts?
- Are service accounts documented?
- Are known risks written down?
- Is there a clear policy for privileged access?
- Is there a documented reason for exceptions?

### Evidence you already have

- `Domain Admins` contains `Alice Sysadmin`, `Claire Admin`, and `azureadmin`
- `azureadmin` is stored in `CN=Users`
- service accounts are stored in `corp/ServiceAccounts`
- `Claire Dupont` and `Claire Admin` show partial separation of normal and admin-style identities

### Possible findings

| Finding | Risk | Priority |
|---|---|---|
| Privileged account `azureadmin` remains in default `CN=Users` container | weaker governance and policy targeting for privileged identity | High |
| Multiple Domain Admin accounts exist | larger privileged attack surface | High |
| Service account ownership / rotation not fully confirmed | unclear accountability for non-human identities | Medium |

### Recommended actions

- Create a dedicated admin OU
- Move privileged accounts into that OU
- Document each privileged account owner and purpose
- Review Domain Admin membership regularly
- Apply stricter controls to privileged accounts
- Require MFA where possible for privileged access

## 4. Identify

Identify is about knowing what assets, identities, systems, dependencies, and risks exist.

### What to check

- Domain name
- DC hostname and IP
- DNS configuration
- DHCP configuration
- OUs
- users
- groups
- service accounts
- privileged accounts
- critical dependencies

### Evidence you already have

- Domain: `becode.corp.lab`
- DC: `max-dc01.becode.corp.lab`
- IP: `10.0.0.4`
- DNS resolution works for `becode.corp.lab` and `max-dc01.becode.corp.lab`
- all FSMO roles are on `max-dc01`
- AD DS, DNS, and DHCP are on the same server

### Possible findings

| Finding | Risk | Priority |
|---|---|---|
| Single DC holds all FSMO roles and core services | authentication, DNS, and DHCP depend on one server | High |
| DHCP scope details still need verification | incomplete network service inventory | Medium |
| `CN=Computers` still needs review | possible unmanaged computer objects | Medium |

### Recommended actions

- Maintain a full asset and identity inventory
- Document DC, DNS, DHCP, and FSMO dependencies
- Review `CN=Users` and `CN=Computers`
- Document DHCP scope and DNS options
- In production, deploy a second DC

## 5. Protect

Protect is about safeguards that reduce the likelihood or impact of a security incident.

### What to check

- Password policy
- Account lockout policy
- Least privilege
- OU structure
- Service account separation
- GPO security settings
- RDP exposure
- Privileged account handling

### Evidence you already have

Password policy:

- minimum password length: `12`
- complexity: `Enabled`
- maximum age: `90 days`
- minimum age: `1 day`
- history: `10 passwords`

Lockout policy:

- threshold: `5 invalid attempts`
- duration: `15 minutes`

OU and identity controls:

- service accounts are in `corp/ServiceAccounts`
- admin-style accounts are in `corp/IT`
- standard users are in department OUs

### Possible findings

| Finding | Risk | Priority |
|---|---|---|
| Domain Admin group contains multiple accounts | increased damage if one admin account is compromised | High |
| Service account password expiry still needs verification | possible persistent credential risk | Medium |
| RDP exposure needs review | possible remote attack path | High |

### Recommended actions

- Keep only necessary users in `Domain Admins`
- Use separate standard and admin accounts
- Confirm `PasswordNeverExpires` on service accounts
- Rotate service account credentials or use managed service accounts
- Restrict RDP access by source IP and role
- Apply stricter GPOs to admin accounts

## 6. Detect

Detect is about identifying suspicious activity quickly.

### What to check

- Event ID `4688`
- PowerShell Event ID `4104`
- Sysmon Event ID `1`
- Sysmon Event ID `3`
- Sysmon Event ID `22`
- Account management auditing
- Kerberos auditing
- Log retention

### Evidence you already have

Confirmed:

- Security Event ID `4688` works
- PowerShell Event ID `4104` works
- Sysmon Event ID `1` works
- Logon auditing is enabled
- Account lockout auditing is enabled
- User and group management auditing is enabled
- Kerberos auditing is enabled

Not confirmed yet:

- Sysmon Event ID `3`
- Sysmon Event ID `22`

### Possible findings

| Finding | Risk | Priority |
|---|---|---|
| Sysmon network and DNS events not confirmed | limited visibility into outbound network activity and DNS lookups | Medium |
| Many audit categories are set to `No Auditing` | reduced visibility into object access, policy change, DS changes | Medium |

### Recommended actions

- Generate DNS and web activity, then retest Sysmon IDs `3` and `22`
- Review Sysmon configuration
- Consider enabling selected policy change and directory service auditing
- Forward logs to a central SIEM in production
- Document which logs are collected and why

## 7. Respond

Respond is about what the organization does when suspicious activity or an incident is found.

### What to check

- Is there a written incident response process?
- Who investigates failed logons or lockouts?
- Who responds to suspicious PowerShell?
- Who can disable a compromised account?
- Are escalation steps documented?

### Evidence you have so far

- logging is partially validated
- no incident response workflow has been confirmed yet

### Possible finding

| Finding | Risk | Priority |
|---|---|---|
| No incident response process documented | alerts or suspicious events may not be handled consistently | Medium |

### Recommended actions

- Write a basic incident response procedure
- Define who reviews logs
- Define when to disable accounts
- Define escalation steps
- Define evidence preservation steps

## 8. Recover

Recover is about restoring systems and services after failure or compromise.

### What to check

- Is there a backup?
- Can the DC be restored?
- Is there a second DC?
- Is the DSRM password documented securely?
- What happens if `max-dc01` fails?
- What happens if DNS or DHCP fails?

### Evidence you already have

- all FSMO roles are on `max-dc01`
- AD DS, DNS, and DHCP are on one host
- no second DC has been confirmed
- no backup or recovery test has been confirmed

### Possible findings

| Finding | Risk | Priority |
|---|---|---|
| Single DC with AD DS, DNS, and DHCP | outage could break authentication and network services | High |
| Backup / recovery process not confirmed | restoration ability is uncertain | High |

### Recommended actions

- Document DSRM password storage
- Configure regular system state backup
- Test restore process in a lab
- Add a second DC in production
- Separate or make resilient critical services where possible

## 9. Recommended report structure

```markdown
# Active Directory Compliance Audit

## Executive Summary

## Scope

## Methodology

## Framework Used
Primary framework: NIST CSF 2.0
Supporting controls: CIS Controls v8.1
Compliance support: NIS2, CyFun, GDPR where relevant

## Findings by NIST CSF Function

### Govern

### Identify

### Protect

### Detect

### Respond

### Recover

## Risk Summary Table

## Remediation Roadmap

## Conclusion
```

## 10. Finding template

```markdown
### Finding: Privileged account in default Users container

NIST CSF function:
Govern / Protect

Current state:
The `azureadmin` account is a member of `Domain Admins` and is stored in `CN=Users`.

Risk:
Privileged accounts in default containers are harder to manage with OU-targeted policies and reduce privileged identity governance clarity.

Evidence:
Observed in `dsa.msc` and Domain Admins membership screenshot.

Recommended target state:
All privileged accounts are stored in a dedicated administrative OU with stricter controls.

Recommended action:
Move `azureadmin` or equivalent privileged accounts into a dedicated admin OU, review Domain Admin membership, and apply stricter privileged access controls.

Priority:
High
```

## 11. Quick mapping table

| Your evidence | NIST CSF function | Possible finding |
|---|---|---|
| `Domain Admins` contains three accounts | Govern / Protect | privileged account sprawl |
| `azureadmin` in `CN=Users` | Govern / Protect | privileged account not placed in managed admin OU |
| all FSMO roles on `max-dc01` | Identify / Recover | single point of failure |
| AD DS, DNS, DHCP on one host | Identify / Recover | critical service concentration |
| password policy configured | Protect | positive control |
| lockout policy configured | Protect | positive control |
| 4688 and 4104 working | Detect | positive monitoring control |
| Sysmon ID 3 and 22 not confirmed | Detect | monitoring gap |
| no response procedure confirmed | Respond | missing response process |
| no backup test confirmed | Recover | uncertain recovery capability |

## 12. Wording to use in the report

```markdown
This audit uses the NIST Cybersecurity Framework 2.0 as the primary assessment structure. NIST CSF 2.0 is appropriate because the project is a cybersecurity gap analysis focused on current state, target state, risk, and prioritized remediation. CIS Controls v8.1 are used as supporting practical safeguards, while NIS2, CyFun, and GDPR are used where regulatory or Belgian/EU compliance context is relevant.
```
