# Phase 1 AD Audit Guide

This guide covers **Phase 1: solo compliance audit of your Azure Active Directory / Domain Controller lab**.

It is based on:

- [01_SecurityConcepts.md](./01_SecurityConcepts.md)
- [02_Regulations.md](./02_Regulations.md)
- [03_FinalProject.md](./03_FinalProject.md)
- [AD Day 1](../02-Windows/02-ActiveDirectory/Day1-DC_Setup/README-FIRST.md)
- [AD Day 2](../02-Windows/02-ActiveDirectory/Day2-Identity/README.md)
- [AD Day 3](../02-Windows/02-ActiveDirectory/Day3-Monitoring/README.md)

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

Example:

`Default Domain Policy uses weak password settings -> easier password spraying and brute force -> NIS2, CyFun, ISO 27001 access control expectations -> increase minimum length, complexity, lockout, and review MFA for privileged accounts`

## 2. Core concepts to apply while auditing

From [01_SecurityConcepts.md](./01_SecurityConcepts.md), these are the concepts you should actively use in your report:

- `CIA triad`
  - Confidentiality: who can access data and AD objects?
  - Integrity: who can modify GPOs, groups, or records?
  - Availability: what happens if the only DC or the only firewall goes down?
- `Attack surface`
  - AD users, service accounts, Domain Admins, DNS, DHCP, GPOs, RDP, internet exposure, VLANs, firewall rules
- `Threat, vulnerability, risk`
  - Threat: attacker, insider, ransomware group
  - Vulnerability: weak password policy, over-privileged account, flat network
  - Risk: how likely and how damaging
- `Defense in depth`
  - Password policy alone is not enough; combine logging, segmentation, least privilege, backups, monitoring
- `Least privilege`
  - Especially important for `Domain Admins`, service accounts, helpdesk delegation, firewall admin access
- `Zero Trust mindset`
  - Do not assume "inside the network" means trusted

## 3. Regulations and frameworks to map findings to

From [02_Regulations.md](./02_Regulations.md), the most useful references for this phase are:

- `GDPR`
  - Use when personal data is involved, user accounts exist, or breach reporting / data protection is relevant
- `NIS2` and the Belgian NIS2 law
  - Use for risk management, incident reporting, access control, logging, business continuity, supply chain, board accountability
- `CyFun`
  - Best practical Belgian framework for turning NIS2 into concrete controls
- `ISO 27001:2022`
  - Use for security governance, access control, logging, asset management, risk treatment

Useful mappings:

- Weak password or lockout policy -> NIS2, CyFun, ISO 27001
- Missing logging or monitoring -> NIS2, CyFun, ISO 27001
- Excessive privileges / Domain Admin misuse -> NIS2, CyFun, ISO 27001
- Missing backups / single point of failure -> NIS2, CyFun, ISO 27001
- Personal data handled without safeguards -> GDPR

## 4. Goal

Audit your own Azure Windows Server / AD environment and produce a professional compliance-style report.

## 5. Current environment notes

These notes come from your live AD/DC environment and can be reused in your report as initial evidence.

### Evidence sources provided

The following evidence was provided from the live environment and should be treated as part of the audit record:

- `dsa.msc` output / screenshots
- `dnsmgmt.msc` output / screenshots
- `eventvwr.msc` output / screenshots
- `gpmc.msc` output / screenshots
- PowerShell command output for:
  - `whoami`
  - `hostname`
  - `ipconfig /all`
  - `nslookup becode.corp.lab`
  - `nslookup max-dc01.becode.corp.lab`
  - `klist`
  - `netdom query fsmo`

This is important for the final report because it means your findings can cite both:

- direct command-line evidence
- GUI-based administrative evidence from AD, DNS, Event Viewer, and Group Policy

### Domain and host identification

- Domain: `becode.corp.lab`
- Hostname: `max-dc01`
- FQDN: `max-dc01.becode.corp.lab`
- IPv4 address: `10.0.0.4`
- Subnet mask: `255.255.255.0`
- Default gateway: `10.0.0.1`
- DNS servers: `::1`, `127.0.0.1`
- DHCP server shown by `ipconfig /all`: `168.63.129.16`
- Primary DNS suffix: `becode.corp.lab`

### `nslookup` notes

Observed commands and results:

```powershell
nslookup becode.corp.lab
```

Result:

- server queried: `UnKnown`
- resolver address: `::1`
- name resolved: `becode.corp.lab`
- returned IP: `10.0.0.4`

```powershell
nslookup max-dc01.becode.corp.lab
```

Result:

- server queried: `UnKnown`
- resolver address: `::1`
- name resolved: `max-dc01.becode.corp.lab`
- returned IP: `10.0.0.4`

Interpretation:

- forward DNS resolution for both the domain and the DC is working
- the DC is resolving through its local DNS service

### FSMO role ownership

Observed command:

```powershell
netdom query fsmo
```

Result:

- Schema master: `max-dc01.becode.corp.lab`
- Domain naming master: `max-dc01.becode.corp.lab`
- PDC: `max-dc01.becode.corp.lab`
- RID pool manager: `max-dc01.becode.corp.lab`
- Infrastructure master: `max-dc01.becode.corp.lab`

Interpretation:

- all five FSMO roles are on the same server
- this confirms a single-DC environment
- this is normal for the lab, but a production availability risk

### Kerberos ticket notes

Observed command:

```powershell
klist
```

Key output:

- Client: `azureadmin @ BECODE.CORP.LAB`
- Server: `krbtgt/BECODE.CORP.LAB @ BECODE.CORP.LAB`
- Encryption type: `AES-256-CTS-HMAC-SHA1-96`
- Start time: `4/20/2026 12:07:46`
- End time: `4/20/2026 22:07:46`
- Renew time: `4/27/2026 12:07:46`
- KDC called: `max-dc01`

Interpretation:

- Kerberos is functioning correctly
- the TGT lifetime shown is `10 hours`
- renewable lifetime shown is `7 days`
- strong Kerberos encryption is in use

### AD/GPO/Server Manager visual notes

From the screenshots you provided:

- Server Manager shows these installed roles:
  - `AD DS`
  - `DHCP`
  - `DNS`
  - `File and Storage Services`
- Group Policy Management shows:
  - `Default Domain Policy` linked at `becode.corp.lab`
  - `Security-Monitoring` present in the domain
- Active Directory Users and Computers shows:
  - `ServiceAccounts` OU under `corp`
  - `Backup Service`
  - `FTP Service`
  - `Monitoring Service`
- Event Viewer overview shows active logs including:
  - `Security`
  - `System`
  - `Windows PowerShell`

### Audit/report notes you can reuse directly

- `DNS resolution for becode.corp.lab and max-dc01.becode.corp.lab successfully returned 10.0.0.4.`
- `All five FSMO roles are hosted on max-dc01.becode.corp.lab, confirming a single domain controller deployment.`
- `Kerberos ticketing is operational and using AES-256 encryption.`
- `The environment currently concentrates AD DS, DNS, and DHCP on the same host, which simplifies the lab but creates a single point of failure in a production context.`

## 6. Best order to work

1. `Scope the environment`
2. `Collect evidence`
3. `Identify gaps`
4. `Map each gap to references`
5. `Write prioritized remediation`

Do not start by writing the report. Start by gathering proof.

## 7. Scope checklist

Audit these areas:

- Domain basics
  - domain name, NetBIOS, DC hostname, private IP
- AD structure
  - OUs, containers, user accounts, groups, nesting
- Privileged access
  - `Domain Admins`, delegated rights, service accounts
- GPO security
  - password policy, lockout policy, PowerShell logging, process creation auditing
- Core services
  - DNS, DHCP, Dynamic DNS
- Monitoring
  - Event ID `4688`, `4104`, Sysmon `1`, `3`, `22`
- Exposure and resilience
  - RDP exposure, single DC, no redundancy, backup position

## 8. Evidence collection workflow

### Step 1: Verify domain and AD basics

Use:

- `dsa.msc`
- `dnsmgmt.msc`
- `eventvwr.msc`
- `gpmc.msc`

Commands:

```powershell
whoami
hostname
ipconfig /all
nslookup becode.corp.lab
nslookup max-dc01.becode.corp.lab
klist
netdom query fsmo
```

What to note:

- domain name
- DC private IP
- SRV records exist
- TGT exists
- single DC holds all FSMO roles

### Step 2: Review identities and privilege

Confirmed from the evidence you provided:

- Members of `Domain Admins`:
  - `Alice Sysadmin`
  - `azureadmin`
  - `Claire Admin`
- Service accounts are separated into their own OU:
  - `corp/ServiceAccounts`
  - `Backup Service`
  - `FTP Service`
  - `Monitoring Service`
- Normal and admin-style users are at least partly separated:
  - standard user: `Claire Dupont` in `corp/Management`
  - admin-style account: `Claire Admin` in `corp/IT`
- `GRP-Management` exists in the `Management` OU with:
  - `Claire Dupont`
  - `Marc Leroy`
- At least one privileged account is left in the default container:
  - `azureadmin` is in `CN=Users`

What this means:

- `Alice Sysadmin` and `Claire Admin` are confirmed privileged accounts, not just admin-style names
- `azureadmin` is also a confirmed privileged account and is stored in the default `Users` container
- service accounts are being managed more cleanly than ordinary default-container accounts because they are placed in a dedicated OU
- the structure shows some identity separation, but not full privileged-account hygiene

What still needs confirmation if you want to go further:

- whether `svc_backup`, `svc_ftp`, and `svc_monitor` all have `PasswordNeverExpires` enabled in their account properties
- whether any computer objects are still sitting in `CN=Computers`
- whether any other hidden or stale accounts remain in `CN=Users`

Audit notes you can reuse:

- `The Domain Admins group contains Alice Sysadmin, Claire Admin, and azureadmin.`
- `Administrative-style accounts are partly separated from standard departmental users through OU placement.`
- `Service accounts are stored in a dedicated ServiceAccounts OU, which is a positive management control.`
- `azureadmin remains in the default Users container while also holding Domain Admin privileges, which reduces policy-targeting clarity for a privileged identity.`

Quick evidence table:

| Check | Result | Evidence |
|---|---|---|
| Who is in `Domain Admins` | `Alice Sysadmin`, `azureadmin`, `Claire Admin` | `dsa.msc` → `Domain Admins` → `Members` |
| Service accounts separated into own OU | Yes | `corp/ServiceAccounts` screenshot |
| Normal and admin users separated | Yes, partially | `Claire Dupont` in `Management`, `Claire Admin` in `IT` |
| Objects left in `CN=Users` | Yes | `azureadmin` shown in `becode.corp.lab/Users` |
| `PasswordNeverExpires` on service accounts | Likely, but verify directly | account properties in `dsa.msc` |

### Step 3: Review GPO settings

Confirmed from the evidence you provided:

GPO objects visible in `gpmc.msc`:

- `Default Domain Controllers Policy`
- `Default Domain Policy`
- `Security-Monitoring`

All three are shown as:

- `Enabled`
- WMI Filter: `None`
- Owner: `Domain Admins (BECODE\Domain Admins)`

Password and lockout baseline confirmed:

- Minimum password length: `12 characters`
- Password must meet complexity requirements: `Enabled`
- Maximum password age: `90 days`
- Minimum password age: `1 day`
- Enforce password history: `10 passwords remembered`
- Account lockout threshold: `5 invalid logon attempts`
- Account lockout duration: `15 minutes`

Advanced audit policy confirmed from `auditpol /get /category:*`:

- `Logon`: `Success and Failure`
- `Logoff`: `Success`
- `Account Lockout`: `Success`
- `Process Creation`: `Success`
- `Security Group Management`: `Success and Failure`
- `User Account Management`: `Success and Failure`
- `Kerberos Service Ticket Operations`: `Success and Failure`
- `Kerberos Authentication Service`: `Success and Failure`

Everything else in the provided output is currently:

- `No Auditing`

What this means:

- the password policy is aligned with the course baseline and is materially stronger than default weak-password setups
- lockout controls are present, which helps against brute-force and password-spraying attempts
- `Security-Monitoring` exists as a separate GPO, which is good operational practice because monitoring settings are separated from the default password baseline
- key identity and authentication audit events are enabled
- process creation auditing is enabled, which supports Event ID `4688`
- many other audit categories remain disabled, which reduces noise but also leaves gaps in visibility for object access, policy change, privilege use, and directory service auditing

Audit notes you can reuse:

- `The Default Domain Policy enforces a 12-character minimum password length, complexity, 90-day maximum age, and account lockout after 5 invalid logon attempts.`
- `A separate Security-Monitoring GPO exists and supports a cleaner separation between baseline security configuration and monitoring controls.`
- `Advanced audit policy confirms logging for logon events, account lockout, process creation, user account management, security group management, and Kerberos events.`
- `Several audit categories remain disabled, so the environment has targeted logging rather than full audit coverage.`

Quick evidence table:

| Check | Result | Evidence |
|---|---|---|
| `Default Domain Policy` exists | Yes | `gpmc.msc` screenshot |
| `Security-Monitoring` GPO exists | Yes | `gpmc.msc` screenshot |
| Password policy baseline | Confirmed | provided policy values |
| Lockout baseline | Confirmed | provided policy values |
| Process creation auditing | Enabled | `auditpol /get /category:*` |
| Kerberos auditing | Enabled | `auditpol /get /category:*` |
| User and group management auditing | Enabled | `auditpol /get /category:*` |
| Broad object access / DS auditing | Not enabled | `auditpol /get /category:*` |

Verification commands:

```powershell
gpupdate /force
auditpol /get /category:*
```

### Step 4: Review monitoring evidence

Confirmed from the evidence you provided:

Monitored log locations:

- `Security`
- `Microsoft-Windows-PowerShell/Operational`
- `Microsoft-Windows-Sysmon/Operational`
- `Directory Service`

Observed command results:

```powershell
Get-WinEvent -FilterHashtable @{LogName="Security"; Id=4688} -MaxEvents 5
```

Result:

- Event ID `4688` returned recent events successfully
- timestamps observed around `4/20/2026 12:48 PM`
- confirms native process creation logging is working

```powershell
Get-WinEvent -FilterHashtable @{LogName="Microsoft-Windows-PowerShell/Operational"; Id=4104} -MaxEvents 5
```

Result:

- Event ID `4104` returned recent events successfully
- entries appear as `Creating Scriptblock text`
- confirms PowerShell ScriptBlock logging is working

```powershell
Get-WinEvent -FilterHashtable @{LogName="Microsoft-Windows-Sysmon/Operational"; Id=1} -MaxEvents 5
```

Result:

- Sysmon Event ID `1` returned recent events successfully
- entries appear as `Process Create`
- confirms Sysmon process creation telemetry is working

```powershell
Get-WinEvent -FilterHashtable @{LogName="Microsoft-Windows-Sysmon/Operational"; Id=3} -MaxEvents 5
```

Result:

- no matching events found during this query

```powershell
Get-WinEvent -FilterHashtable @{LogName="Microsoft-Windows-Sysmon/Operational"; Id=22} -MaxEvents 5
```

Result:

- no matching events found during this query

What this means:

- native process auditing is operational through Event ID `4688`
- PowerShell command logging is operational through Event ID `4104`
- Sysmon is installed and collecting at least process creation events through Event ID `1`
- in this test window, no recent Sysmon network connection events (`3`) or DNS query events (`22`) were returned
- this does not automatically mean the feature is broken; it may simply mean no matching activity occurred recently, or that the Sysmon configuration filters suppressed those events

Audit notes you can reuse:

- `Event ID 4688 is being generated, confirming native process creation auditing is active.`
- `Event ID 4104 is being generated, confirming PowerShell ScriptBlock logging is active.`
- `Sysmon Event ID 1 is present, confirming Sysmon process creation logging is operational.`
- `No recent Sysmon Event ID 3 or 22 entries were returned during the query window, so network and DNS telemetry could not be confirmed from this specific sample alone.`

Quick evidence table:

| Check | Result | Evidence |
|---|---|---|
| Security Event ID `4688` | Working | `Get-WinEvent` returned recent events |
| PowerShell Event ID `4104` | Working | `Get-WinEvent` returned recent events |
| Sysmon Event ID `1` | Working | `Get-WinEvent` returned recent events |
| Sysmon Event ID `3` | Not observed in sample | no matching events found |
| Sysmon Event ID `22` | Not observed in sample | no matching events found |

Useful test commands:

```powershell
whoami
ipconfig /all
Get-WinEvent -FilterHashtable @{LogName="Security"; Id=4688} -MaxEvents 5
Get-WinEvent -FilterHashtable @{LogName="Microsoft-Windows-PowerShell/Operational"; Id=4104} -MaxEvents 5
Get-WinEvent -FilterHashtable @{LogName="Microsoft-Windows-Sysmon/Operational"; Id=1} -MaxEvents 5
Get-WinEvent -FilterHashtable @{LogName="Microsoft-Windows-Sysmon/Operational"; Id=3} -MaxEvents 5
Get-WinEvent -FilterHashtable @{LogName="Microsoft-Windows-Sysmon/Operational"; Id=22} -MaxEvents 5
```

### Step 5: Review DHCP and DNS design

Check:

- DHCP scope range
- excluded DC IP
- DNS option points to DC
- Dynamic DNS updates enabled

Why this matters:

- bad DNS settings break domain join and authentication
- bad DHCP scope design can cause addressing issues
- putting DHCP, DNS, and AD on one machine creates a single point of failure

## 9. High-value findings you should actively check

- Over-privileged accounts in `Domain Admins`
- Non-expiring service account passwords
- Accounts or computers left in default containers
- Single DC with no redundancy
- RDP exposed to the internet without additional restrictions
- No MFA for privileged access
- Logging not enabled everywhere needed
- No documented backup / recovery process
- DHCP, DNS, and AD concentrated on one host

## 10. What to change or improve in AD

If you are allowed to improve the environment before documenting it, prioritize these changes:

1. Keep only strictly necessary accounts in `Domain Admins`
2. Separate admin and standard accounts for every admin user
3. Move all managed objects into proper OUs
4. Keep the `Security-Monitoring` GPO active and verified
5. Enable and verify Sysmon
6. Disable unnecessary exposure like broad RDP access where possible
7. Document service account purpose, owner, and rotation process
8. Add a risk note for the single DC and lack of redundancy
9. Add a recommendation for MFA on privileged accounts
10. Add a recommendation for a second DC / backup strategy in production

## 11. How to write each finding

Use this structure:

```markdown
### Finding: Non-expiring service account passwords

Description:
Three service accounts (`svc_backup`, `svc_ftp`, `svc_monitor`) are configured with `PasswordNeverExpires`.

Evidence:
Observed in Active Directory Users and Computers under `OU=ServiceAccounts`.

Risk:
If one of these credentials is stolen, access can remain persistent for a long period without forced rotation.

Reference:
NIS2 risk management and access control expectations; CyFun identity and access controls; ISO 27001 access control and credential management.

Severity:
Medium

Recommendation:
Document service owners, rotate credentials on a schedule, deny interactive logon where possible, and replace with managed service accounts where supported.
```

## 12. Suggested report structure

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
| F1 | Over-privileged admin accounts | High | NIS2 / CyFun / ISO 27001 | Immediate |

### Conclusion

- overall posture
- quick wins
- strategic improvements

## 13. Useful course links

### Local repo links

- [Security Concepts](./01_SecurityConcepts.md)
- [Regulations](./02_Regulations.md)
- [Final Project Brief](./03_FinalProject.md)
- [AD Day 1 README](../02-Windows/02-ActiveDirectory/Day1-DC_Setup/README-FIRST.md)
- [AD Day 1 Verify Domain](../02-Windows/02-ActiveDirectory/Day1-DC_Setup/exercises/03-verify_domain.md)
- [AD Day 2 README](../02-Windows/02-ActiveDirectory/Day2-Identity/README.md)
- [AD Day 2 OUs and Users](../02-Windows/02-ActiveDirectory/Day2-Identity/exercises/01-ous_and_users.md)
- [AD Day 2 GPO](../02-Windows/02-ActiveDirectory/Day2-Identity/exercises/02-gpo.md)
- [AD Day 2 DHCP](../02-Windows/02-ActiveDirectory/Day2-Identity/exercises/03-dhcp.md)
- [AD Day 3 README](../02-Windows/02-ActiveDirectory/Day3-Monitoring/README.md)
- [AD Day 3 Process Auditing](../02-Windows/02-ActiveDirectory/Day3-Monitoring/exercises/01-process_auditing.md)
- [AD Day 3 Sysmon](../02-Windows/02-ActiveDirectory/Day3-Monitoring/exercises/02-sysmon.md)
- [AD Day 3 Final Report](../02-Windows/02-ActiveDirectory/Day3-Monitoring/exercises/05-final_report.md)

### GitHub links

- [Repo root](https://github.com/becodeorg/BXL-Kamkar-5)
- [Security Concepts on GitHub](https://github.com/becodeorg/BXL-Kamkar-5/blob/main/01-Fundamentals/05-Security-And-Regulations/01_SecurityConcepts.md)
- [Regulations on GitHub](https://github.com/becodeorg/BXL-Kamkar-5/blob/main/01-Fundamentals/05-Security-And-Regulations/02_Regulations.md)
- [Final Project on GitHub](https://github.com/becodeorg/BXL-Kamkar-5/blob/main/01-Fundamentals/05-Security-And-Regulations/03_FinalProject.md)
- [Active Directory module on GitHub](https://github.com/becodeorg/BXL-Kamkar-5/tree/main/01-Fundamentals/02-Windows/02-ActiveDirectory)

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
- [Microsoft AD DS overview](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/active-directory-domain-services)
- [Microsoft Group Policy overview](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/manage/group-policy/group-policy-overview)
- [Microsoft Sysmon](https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon)
- [SwiftOnSecurity Sysmon config](https://github.com/SwiftOnSecurity/sysmon-config)
- [PingCastle](https://www.pingcastle.com/)
- [BloodHound Community Edition](https://github.com/SpecterOps/BloodHound)

## 14. Final advice

Think like an auditor reviewing identity, privilege, policy, and monitoring.

If you are unsure whether something belongs in the report, ask:

- Does it increase risk?
- Can I prove it?
- Can I map it to a framework?
- Can I propose a realistic fix?

If the answer is yes to all four, include it.
