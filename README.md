# Hybrid Identity \& IAM Engineering Lab

## Overview

This repository documents the design, deployment, security, troubleshooting, and validation of a hands-on hybrid identity environment built around Windows Server Active Directory and Microsoft Entra ID.

The project demonstrates practical Identity and Access Management (IAM) engineering skills, progressing from an on-premises Active Directory foundation into hybrid identity, identity lifecycle management, Conditional Access, privileged access management, automation, and Zero Trust security controls.

Rather than documenting configuration alone, each stage includes testing, validation, security rationale, evidence, and troubleshooting to demonstrate how the environment behaves in practice.

## Lab Architecture

The lab uses a segmented virtual network protected by IPFire and provides an on-premises Active Directory environment that will be extended into Microsoft Entra ID.

* **Firewall / Router:** IPFire
* **Lab Network:** 192.168.1.0/24
* **IPFire GREEN Interface:** 192.168.1.1
* **Domain Controller:** SC300-DC01
* **Domain Controller IP:** 192.168.1.2
* **Active Directory Domain:** ad.sc300lab.test
* **Client 1:** SC300-PC01
* **Client 2:** SC300-PC02
* **Client OS:** Windows 11 Enterprise
* **Server OS:** Windows Server 2025
* **Internal DNS:** Active Directory-integrated DNS on SC300-DC01
* **External DNS Forwarding:** Cloudflare 1.1.1.1

Domain clients use SC300-DC01 for DNS and Active Directory service discovery. External DNS queries are forwarded by the domain controller rather than configuring public DNS directly on domain clients.

## Completed Lab Work

### 01 - On-Premises Active Directory Foundation

Built and validated the on-premises identity foundation for the hybrid IAM environment.

Key work completed:

* Deployed Windows Server 2025 Active Directory Domain Services.
* Created the ad.sc300lab.test Active Directory forest.
* Configured AD-integrated DNS and external DNS forwarding.
* Validated DNS and LDAP service discovery.
* Verified SYSVOL and NETLOGON availability.
* Verified all five FSMO roles.
* Identified and remediated an incorrect domain controller hostname using a supported domain controller rename process.
* Validated SPNs following the rename and checked for duplicate SPNs.
* Joined two Windows 11 Enterprise clients to the domain.
* Verified domain authentication and domain controller discovery.
* Validated the Active Directory secure channel on both clients.

[View the full On-Premises AD Foundation lab](docs/01-on-prem-ad-foundation/README.md)

## Project Roadmap

The lab will progressively extend the on-premises identity foundation into a hybrid Microsoft Entra ID environment.

Planned areas include:

* Hybrid identity using Microsoft Entra Connect.
* Joiner-Mover-Leaver identity lifecycle management.
* Dynamic users and group membership.
* Role-Based Access Control (RBAC) and least privilege.
* Multi-Factor Authentication (MFA) and authentication methods.
* Self-Service Password Reset (SSPR).
* Conditional Access and Zero Trust policy design.
* Microsoft Entra ID Protection and identity risk.
* Privileged Identity Management (PIM) and Just-In-Time privileged access.
* Entitlement Management and access packages.
* Access Reviews.
* Application registrations, OAuth 2.0, and OpenID Connect.
* Workload identities and managed identities.
* Microsoft Graph and PowerShell automation.
* Privileged Access Management (PAM) scenarios.
* Identity security monitoring, investigation, and remediation.

Each stage will include configuration, testing, evidence, security rationale, and troubleshooting rather than configuration alone.

## Transferable Enterprise IT Experience



This project builds on 14+ years of hands-on enterprise IT experience. My previous roles have provided practical experience with Active Directory, endpoint deployment, user administration, troubleshooting, access permissions, and enterprise support processes that transfer directly into Identity and Access Management.



\### Mobile / Field Engineer

\- Hardware installation, replacement, and break/fix support.

\- Desktop and laptop builds and enterprise deployments.

\- Server and Cisco hardware replacement.

\- On-site troubleshooting and incident resolution.

\- Worked within controlled enterprise environments and change processes.



\### Deployment Engineer

\- Enterprise Windows deployment using SCCM.

\- Active Directory and domain-joined endpoint administration.

\- Hardware refresh and large-scale rollout projects.

\- Pre-deployment testing and validation before production rollout.

\- Troubleshooting failed builds, applications, and device configurations.



\### 1st Line Support

\- Troubleshooting hardware, software, authentication, password, and connectivity issues.

\- Password resets and user access support.

\- Incident and service request management.

\- Detailed ticket documentation and issue tracking.

\- Escalation of security and technical incidents when required.



\### 2nd Line Support

\- Advanced troubleshooting across Windows, macOS, networking, applications, and endpoints.

\- Active Directory user and computer administration.

\- User account, group membership, and security permission management.

\- Troubleshooting authentication and access issues.

\- Ownership of escalated incidents through to resolution.

\- Root-cause investigation and technical problem solving.

\- Collaboration with 1st line, infrastructure, security, 3rd line teams, and external vendors.



\### How This Experience Transfers to IAM



\- \*\*Active Directory administration\*\* → Identity, groups, authentication, and access management.

\- \*\*Password and account support\*\* → SSPR, MFA, authentication methods, and identity verification.

\- \*\*Security permissions\*\* → RBAC, least privilege, and access governance.

\- \*\*Deployment engineering\*\* → Identity-aware endpoint onboarding and lifecycle management.

\- \*\*User administration\*\* → Joiner-Mover-Leaver identity lifecycle processes.

\- \*\*Technical troubleshooting\*\* → Authentication, provisioning, Conditional Access, and hybrid identity investigation.

\- \*\*Change and ticket management\*\* → Controlled access, approvals, auditing, and governance.

\- \*\*Enterprise support experience\*\* → Understanding the operational impact of identity and security controls on users and business services.



The objective of this lab is to extend this existing enterprise experience into modern Microsoft identity engineering, hybrid identity, Zero Trust, access governance, privileged access, and automation.

