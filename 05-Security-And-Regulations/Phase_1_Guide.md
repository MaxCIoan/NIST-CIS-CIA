# Security and Regulations Project Guide

This file is now the index for the split project guides.

## Main guides

- [Phase 1 AD Audit Guide](./Phase_1_AD_Audit_Guide.md)
- [Phase 2 Network Audit Guide](./Phase_2_Network_Audit_Guide.md)

## Supporting notes

- [NIST CSF Framework to Follow](./NIST_CSF_Framework_To_Follow.md)
- [NIST SP 800-207 - Zero Trust Notes](./NIST_SP_800_207_Zero_Trust_Notes.md)
- [NIST CSWP 29 - CSF 2.0 Notes](./NIST_CSWP_29_CSF_2_0_Notes.md)
- [CIS Controls v8.1 Notes](./CIS_Controls_v8_1_Notes.md)
- [Resource Passports](./RESOURCE_PASSPORTS.md)

## Core module files

- [Security Concepts](./01_SecurityConcepts.md)
- [Regulations](./02_Regulations.md)
- [Final Project Brief](./03_FinalProject.md)

## Progress Alignment Table

This table matches what you have already read, checked, observed, or verified so far.

Status legend:

- 🟢 `Verified`
- 🟡 `Reviewed`
- 🔴 `Needs checking`

| Area | What you have done / reviewed | Evidence or source | Current status | What it shows |
|---|---|---|---|---|
| Security concepts | Read the module material | [01_SecurityConcepts.md](./01_SecurityConcepts.md) | 🟡 Reviewed | You have the theory for CIA, risk, defense in depth, least privilege, and Zero Trust |
| Regulations | Read the regulation module material | [02_Regulations.md](./02_Regulations.md) | 🟡 Reviewed | You have the main compliance references: GDPR, NIS2, CyFun, ISO 27001 |
| Final project brief | Read the project requirements | [03_FinalProject.md](./03_FinalProject.md) | 🟡 Reviewed | You understand the deliverables for Phase 1 and Phase 2 |
| Phase 1 scope | Identified the AD/DC lab as the audit target | [Phase 1 AD Audit Guide](./Phase_1_AD_Audit_Guide.md) | 🟢 Verified | Your Phase 1 scope is your Azure AD/DC environment |
| Phase 2 scope | Identified the Packet Tracer submission and report | [Phase 2 Network Audit Guide](./Phase_2_Network_Audit_Guide.md) | 🟢 Verified | Your Phase 2 scope is the `.pkt` file plus the implementation report |
| Domain and host basics | Collected host, domain, IP, DNS, gateway, Kerberos, FSMO data | [Phase 1 AD Audit Guide](./Phase_1_AD_Audit_Guide.md) | 🟢 Verified | `becode.corp.lab`, `max-dc01`, `10.0.0.4`, Kerberos working, single DC |
| DNS resolution | Ran `nslookup` for domain and DC | PowerShell output recorded in [Phase 1 AD Audit Guide](./Phase_1_AD_Audit_Guide.md) | 🟢 Verified | Forward DNS resolution is working correctly |
| FSMO roles | Ran `netdom query fsmo` | PowerShell output recorded in [Phase 1 AD Audit Guide](./Phase_1_AD_Audit_Guide.md) | 🟢 Verified | All FSMO roles are on `max-dc01`, confirming a single-DC setup |
| Kerberos | Ran `klist` | PowerShell output recorded in [Phase 1 AD Audit Guide](./Phase_1_AD_Audit_Guide.md) | 🟢 Verified | Kerberos ticketing works and uses AES-256 |
| ADUC review | Checked users, groups, OUs in `dsa.msc` | screenshots and notes in [Phase 1 AD Audit Guide](./Phase_1_AD_Audit_Guide.md) | 🟢 Verified | You verified the AD structure and user/group placement |
| Domain Admins | Checked `Domain Admins` membership | `dsa.msc` screenshot and notes in [Phase 1 AD Audit Guide](./Phase_1_AD_Audit_Guide.md) | 🟢 Verified | `Alice Sysadmin`, `Claire Admin`, and `azureadmin` are privileged accounts |
| Service accounts | Viewed `corp/ServiceAccounts` OU | `dsa.msc` screenshot and notes in [Phase 1 AD Audit Guide](./Phase_1_AD_Audit_Guide.md) | 🟢 Verified | Service accounts are separated into a dedicated OU |
| Normal vs admin accounts | Compared `Claire Dupont` and `Claire Admin` | `dsa.msc` screenshots and notes in [Phase 1 AD Audit Guide](./Phase_1_AD_Audit_Guide.md) | 🟡 Reviewed | There is some separation of standard and admin-style accounts |
| Default containers | Observed `azureadmin` in `CN=Users` | `dsa.msc` screenshot and notes in [Phase 1 AD Audit Guide](./Phase_1_AD_Audit_Guide.md) | 🟢 Verified | At least one privileged account is still in the default container |
| Password policy | Verified password baseline | GPO values in [Phase 1 AD Audit Guide](./Phase_1_AD_Audit_Guide.md) | 🟢 Verified | 12 characters, complexity enabled, 90-day age, history of 10 |
| Lockout policy | Verified lockout baseline | GPO values in [Phase 1 AD Audit Guide](./Phase_1_AD_Audit_Guide.md) | 🟢 Verified | Lockout after 5 attempts for 15 minutes |
| GPO objects | Viewed GPOs in `gpmc.msc` | screenshot and notes in [Phase 1 AD Audit Guide](./Phase_1_AD_Audit_Guide.md) | 🟢 Verified | `Default Domain Policy`, `Default Domain Controllers Policy`, and `Security-Monitoring` exist |
| Audit policy | Ran `auditpol /get /category:*` | command output in [Phase 1 AD Audit Guide](./Phase_1_AD_Audit_Guide.md) | 🟢 Verified | Logon, lockout, process creation, account mgmt, and Kerberos auditing are enabled |
| Event Viewer review | Viewed active logs in `eventvwr.msc` | screenshot and notes in [Phase 1 AD Audit Guide](./Phase_1_AD_Audit_Guide.md) | 🟢 Verified | Security, System, and Windows PowerShell logs are active |
| Native process logging | Queried Event ID `4688` | `Get-WinEvent` results in [Phase 1 AD Audit Guide](./Phase_1_AD_Audit_Guide.md) | 🟢 Verified | Native process creation auditing is working |
| PowerShell logging | Queried Event ID `4104` | `Get-WinEvent` results in [Phase 1 AD Audit Guide](./Phase_1_AD_Audit_Guide.md) | 🟢 Verified | ScriptBlock logging is working |
| Sysmon process telemetry | Queried Sysmon Event ID `1` | `Get-WinEvent` results in [Phase 1 AD Audit Guide](./Phase_1_AD_Audit_Guide.md) | 🟢 Verified | Sysmon process creation logging is working |
| Sysmon network / DNS telemetry | Queried Sysmon Event IDs `3` and `22` | `Get-WinEvent` results in [Phase 1 AD Audit Guide](./Phase_1_AD_Audit_Guide.md) | 🔴 Needs checking | No matching events were returned in that sample window |
| DHCP / DNS dependency review | Identified that AD DS, DNS, and DHCP are on the same host | Server Manager and notes in [Phase 1 AD Audit Guide](./Phase_1_AD_Audit_Guide.md) | 🟢 Verified | This is a single point of failure in production terms |
| Zero Trust study | Reviewed NIST SP 800-207 notes | [NIST_SP_800_207_Zero_Trust_Notes.md](./NIST_SP_800_207_Zero_Trust_Notes.md) | 🟡 Reviewed | You have a Zero Trust reference for least privilege, segmentation, and continuous verification |
| CSF 2.0 study | Reviewed NIST CSF 2.0 notes | [NIST_CSWP_29_CSF_2_0_Notes.md](./NIST_CSWP_29_CSF_2_0_Notes.md) | 🟡 Reviewed | You have a framework for gap analysis, current vs target state, and governance |
| CIS study | Reviewed CIS Controls v8.1 notes | [CIS_Controls_v8_1_Notes.md](./CIS_Controls_v8_1_Notes.md) | 🟡 Reviewed | You have a practical implementation reference for technical safeguards |
| Source profiling | Built source passports | [RESOURCE_PASSPORTS.md](./RESOURCE_PASSPORTS.md) | 🟢 Verified | You can explain what each source is, how strong it is, and how to use it |

## Remaining checks

These are the main things still worth confirming directly:

| Item | Why it matters | Suggested check |
|---|---|---|
| 🔴 `PasswordNeverExpires` on service accounts | Confirms whether non-expiring credentials are a formal finding | open each service account in `dsa.msc` → `Properties` → `Account` |
| 🔴 Any objects left in `CN=Computers` | Confirms whether joined machines are unmanaged by OU-targeted GPOs | open `CN=Computers` in `dsa.msc` |
| 🔴 Sysmon Event ID `3` and `22` | Confirms network and DNS telemetry, not only process telemetry | generate a DNS/web request, then rerun the `Get-WinEvent` queries |
| 🔴 DHCP scope details | Needed for stronger DNS/DHCP design analysis | verify scope range, exclusions, options, and DDNS in DHCP Manager |
