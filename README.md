# GOAL: Learn How do SCCM to Intune Migration 

## About the Project.

I'm an aspiring sysadmin trying to teach myself how to execute an SCCM-to-Intune migration. I want to learn from full on prem to using co-management as the bridge to full cloud (or not if there are limitations I do not yet know). I want to learn SCCM with a ground up approach to really understand current environmets before going into cloud. I'll build the MECM environment and learn the console hands on(e.g. App deployment, security baselines, compliance policies, software updates). Then figure out how to migrate all of this existing infrastructure to the cloud. I'll explore pain points along the way to gain the knowledge that could be applied at scale for a Small to Medium Business (SMB) environment.


---

## My Virtual Box Lab

```
DC:     Windows Server 2022 — Active Directory, DNS — 10.0.0.1
SCCM   Windows Server 2022 — MECM + SQL Server, Site Code: LAB — 10.0.0.2
Client01 Windows 11 Pro — domain joined, MECM client installed — 10.0.0.20
Client02 Windows 11 Pro — domain joined, MECM client NOT installed — 10.0.0.25
```
All VMs running on a single Mini PC. An Elite desk that I upgraded to have 32 GBs of RAM. Still there were limitations with being able to run all the VMs at once for extended periods of time. 

---
## Table of Contents for My KB Rough Drafts

| Doc | What's in it |
|-----|--------------|
| [SCCM Server Setup](SCCM%20server%20setup/Setting%20up%20SCCM%20personal%20note%20walkthrough.md) | Domain, SQL Server, prerequisites, MECM install |
| [First Managed Device](Configuring%20MECM%20&%20First%20Managed%20Device/Configuring%20MECM%20&%20First%20Managed%20Device.md) | Discovery, boundaries, client push |
| [Managing Devices](Managing%20Device%20with%20MECM/Managing%20Device%20with%20MECM.md) | Collections, harware inventory, app deployment |

## What's Been Built so Far

The MECM foundation is complete:

- AD domain (`lab.local`), extended schema, System Management container, service accounts
- SQL Server 2022 with dedicated volume, service account, and memory tuning
- MECM prerequisites (ADK + WinPE, IIS, WSUS, .NET) and primary site install
- Discovery methods (System, User, Forest), boundaries, and boundary group with DP
- CLIENT01 pushed the MECM client via GPO-managed service account, healthy in console
- Device collections: All Managed Clients, Windows 11 Devices, Pilot Ring
- Silent app deployments: 7-Zip (MSI) and Notepad++ (EXE with registry detection) — both confirmed working
- Hardware inventory running and validated via Resource Explorer

---

## Personal Notes of Mistakes and Lesson Learned the Hard Way

**ODBC version matters.** The MECM installer silently fails at "Setting up SQL Server database accounts" if ODBC 18 is too new. Rolling back from 18.6.1.1 to 18.5.2.1 fixed it. [Relevant docs.](https://learn.microsoft.com/en-us/sql/connect/odbc/windows/release-notes-odbc-sql-server-windows)

**Enable network discovery on the client firewall.** The device will show up in MECM but you won't be able to reach it. The logs will not make this obvious.

**GPO everything from day one.** WMI rules, local admin rights for the service account. Do it via GPO so every future device is ready automatically.

**Check your eval license.** If your DC randomly stops working, run `slmgr /xpr`. Learned this one the hard way.


