# Hybrid Identity & IAM Engineering Lab

## Overview

This repository documents the design, deployment, security, troubleshooting, and validation of a hands-on **Hybrid Identity and Identity & Access Management (IAM) lab** built around Windows Server Active Directory and Microsoft Entra ID.

The project is designed to demonstrate practical IAM engineering capability rather than certification theory alone.

It begins with an on-premises Active Directory environment and progressively introduces hybrid identity, authentication, identity lifecycle management, Conditional Access, privileged access, governance, automation, and Zero Trust security controls.

Each phase follows an engineering approach:

**Design → Configure → Test → Validate → Troubleshoot → Document**

Failures and troubleshooting are intentionally retained where they demonstrate useful engineering knowledge.

---

## Current Architecture

```text
                         Microsoft Entra ID
                         /              \
                        /                \
         Password Hash Sync              SSPR
                      /                    \
                     v                      v
                SC300-SYNC01 <------ Password Writeback
             Microsoft Entra Connect
                       │
                       │
                       v
                  SC300-DC01
              Active Directory / DNS
               ad.sc300lab.test
                  192.168.1.2
                    /       \
                   /         \
             SC300-PC01    SC300-PC02
              Windows 11    Windows 11
                   \         /
                    \       /
                     IPFire
                   192.168.1.1
```

### Identity Design

| Component | Configuration |
|---|---|
| Active Directory forest | `ad.sc300lab.test` |
| Domain Controller | `SC300-DC01` |
| Domain Controller IP | `192.168.1.2` |
| Entra Connect Server | `SC300-SYNC01` |
| Windows clients | `SC300-PC01`, `SC300-PC02` |
| Hybrid identity OU | `Hybrid-Users` |
| Verified cloud UPN | `corp.davidboydltd.co.uk` |
| Authentication | Password Hash Synchronization |
| Password recovery | SSPR with Password Writeback |
| MFA | Microsoft Authenticator |
| Firewall / Router | IPFire |
| Lab network | `192.168.1.0/24` |
| External DNS forwarding | Cloudflare `1.1.1.1` |

The internal Active Directory namespace is deliberately separated from the routable cloud identity namespace.

On-premises directory:

`ad.sc300lab.test`

Cloud sign-in suffix:

`corp.davidboydltd.co.uk`

---

# Completed Lab Phases

## Phase 1 — On-Premises Active Directory Foundation ✅

Built and validated the on-premises identity foundation required for the hybrid IAM environment.

### Key Engineering Work

- Deployed Windows Server 2025 Active Directory Domain Services.
- Created the `ad.sc300lab.test` Active Directory forest.
- Configured Active Directory-integrated DNS.
- Configured external DNS forwarding.
- Validated DNS and LDAP service discovery.
- Verified SYSVOL and NETLOGON.
- Verified all five FSMO roles.
- Diagnosed and corrected an incorrect domain controller hostname.
- Validated Service Principal Names following the DC rename.
- Checked the directory for duplicate SPNs.
- Joined two Windows 11 Enterprise clients to the domain.
- Validated domain authentication.
- Verified domain controller discovery.
- Validated Active Directory secure channels.
- Captured implementation and troubleshooting evidence.

[View Phase 1 — On-Premises Active Directory Foundation](docs/01-on-prem-ad-foundation/README.md)

---

## Phase 2 — Hybrid Identity & Microsoft Entra Connect ✅

Extended the on-premises Active Directory environment into Microsoft Entra ID using a dedicated Microsoft Entra Connect synchronization server.

### Key Engineering Work

- Deployed dedicated synchronization server `SC300-SYNC01`.
- Joined the synchronization server to the Active Directory domain.
- Validated AD DNS, LDAP, secure channel, time synchronization, and HTTPS connectivity.
- Verified the custom domain `corp.davidboydltd.co.uk` in Microsoft Entra ID.
- Added the routable UPN suffix to the Active Directory forest.
- Created a dedicated `Hybrid-Users` OU.
- Restricted Microsoft Entra Connect synchronization to the controlled OU.
- Configured `mS-DS-ConsistencyGuid` as the source anchor.
- Enabled Password Hash Synchronization.
- Validated Microsoft Entra Connect scheduler and connectors.
- Validated successful synchronization runs.
- Synchronized an on-premises test identity into Microsoft Entra ID.
- Proved cloud authentication using the synchronized Active Directory password.
- Investigated MFA registration through Microsoft Entra sign-in diagnostics.
- Created a controlled Microsoft Authenticator pilot group.
- Targeted the Authenticator authentication method to the pilot population.
- Diagnosed failed Authenticator enrollment.
- Successfully registered Microsoft Authenticator after troubleshooting.
- Validated the final authentication-method state.

### Hybrid Authentication Flow

```text
On-Premises AD DS
       │
       ▼
Microsoft Entra Connect
       │
       ▼
Password Hash Synchronization
       │
       ▼
Microsoft Entra ID
       │
       ▼
Microsoft Authenticator / MFA
```

[View Phase 2 — Hybrid Identity & Microsoft Entra Connect](docs/02-hybrid-identity-entra-connect/README.md)

---

## Phase 3 — Self-Service Password Reset & Password Writeback ✅

Implemented and validated Self-Service Password Reset (SSPR) for a synchronized hybrid identity, including secure password writeback to on-premises Active Directory.

### Key Engineering Work

- Created dedicated `SG-SSPR-Pilot` security group.
- Scoped SSPR to a controlled pilot population.
- Validated Microsoft Authenticator as the user's verification method.
- Reviewed SSPR registration settings.
- Identified the initial absence of a password-writeback-capable agent.
- Reconfigured Microsoft Entra Connect Sync to enable Password Writeback.
- Preserved the existing Password Hash Synchronization and OU filtering configuration.
- Validated that Microsoft Entra detected the on-premises writeback client.
- Performed an end-to-end SSPR password reset.
- Verified the password change directly in on-premises Active Directory using `PasswordLastSet`.
- Validated the new credentials against the `SC300-DC01` NETLOGON share.
- Avoided granting unnecessary Remote Desktop or domain-controller logon rights solely for testing.
- Captured implementation, troubleshooting, and validation evidence.

### Hybrid Password Recovery Flow

```text
Microsoft Entra SSPR
        │
        ▼
Microsoft Authenticator
        │
        ▼
Identity Verification
        │
        ▼
Password Reset
        │
        ▼
Password Writeback
        │
        ▼
Microsoft Entra Connect Sync
        │
        ▼
On-Premises Active Directory
        │
        ▼
Domain Authentication
```

[View Phase 3 — SSPR & Password Writeback](docs/03-sspr-password-writeback/README.md)

---

# Engineering Approach

The project follows several security principles throughout the environment.

### Zero Trust

**Verify explicitly, use least privilege, and assume breach.**

Authentication and authorization decisions should be based on identity, device, location, risk, application, and other available signals rather than implicit trust.

### Least Privilege

Users, administrators, applications, and workloads should receive only the access required to perform their function.

The SSPR validation deliberately avoided granting the test identity additional interactive or Remote Desktop logon rights purely to make a test succeed.

### Defence in Depth

Identity controls are combined with network, endpoint, authentication, monitoring, and governance controls.

### Default Deny

Access is not assumed simply because an identity or resource exists.

### Controlled Deployment

Changes are introduced to limited scopes where possible before wider deployment.

Examples include:

- Microsoft Authenticator targeted to a dedicated pilot security group.
- SSPR targeted to the dedicated `SG-SSPR-Pilot` group.
- Microsoft Entra Connect synchronization restricted to the `Hybrid-Users` OU.

### Validate Rather Than Assume

Configuration is independently tested.

Examples include:

- DNS resolution
- LDAP service discovery
- Active Directory secure channels
- Microsoft Entra Connect scheduler
- Synchronization connectors
- Synchronization run history
- Password Hash Synchronization
- Cloud authentication
- MFA registration
- SSPR identity verification
- Password Writeback
- Active Directory `PasswordLastSet`
- Authentication against an on-premises domain resource

### Troubleshooting Is Evidence

Failures are not automatically removed from the project history.

A failed configuration can demonstrate more engineering capability than a successful wizard if the failure is systematically investigated, understood, remediated, and validated.

Examples captured in the lab include:

- Active Directory DNS and domain-controller configuration.
- Microsoft Authenticator registration failure and remediation.
- Password Writeback initially not being detected.
- Domain-controller availability affecting Microsoft Entra Connect.
- Distinguishing authentication failure from Windows logon-right authorization.

---

# Transferable Enterprise IT Experience

This project builds on **14+ years of hands-on enterprise IT experience** across field engineering, deployment, service desk, endpoint support, Active Directory administration, troubleshooting, and controlled enterprise environments.

## Mobile / Field Engineering

- Hardware installation, replacement, and break/fix support.
- Desktop and laptop deployment.
- Server and Cisco hardware replacement.
- On-site troubleshooting and incident resolution.
- Work within controlled enterprise change processes.

## Deployment Engineering

- Enterprise Windows deployment using SCCM.
- Active Directory and domain-joined endpoint administration.
- Hardware refresh and large-scale rollout projects.
- Pre-deployment testing and validation.
- Troubleshooting failed builds, applications, and device configurations.

## 1st and 2nd Line Support

- Windows and macOS troubleshooting.
- Active Directory user and computer administration.
- Authentication and password troubleshooting.
- Group membership and security permission management.
- Incident and service request management.
- Root-cause investigation.
- Escalation and collaboration with infrastructure, security, third-line teams, and external vendors.

## How This Transfers to IAM

| Enterprise Experience | IAM Application |
|---|---|
| Active Directory administration | Identity, groups and authentication |
| Password/account support | SSPR, MFA and authentication methods |
| Security permissions | RBAC and least privilege |
| User administration | Joiner-Mover-Leaver lifecycle |
| Deployment engineering | Identity-aware endpoint lifecycle |
| Troubleshooting | Authentication, provisioning and Conditional Access investigation |
| Change management | Access approvals, auditing and governance |
| Enterprise support | Understanding operational impact of security controls |

The objective is to build on existing enterprise engineering experience and apply it to modern **Identity, IAM, PAM and Zero Trust architecture**.

---

# Project Roadmap

### Phase 1 — On-Premises Active Directory Foundation
**Status: Complete ✅**

### Phase 2 — Hybrid Identity & Microsoft Entra Connect
**Status: Complete ✅**

### Phase 3 — Self-Service Password Reset & Password Writeback
**Status: Complete ✅**

### Upcoming Engineering Areas

- Joiner-Mover-Leaver identity lifecycle management
- Dynamic users and groups
- Role-Based Access Control (RBAC)
- Authentication methods and passwordless authentication
- Conditional Access
- Microsoft Entra ID Protection
- User risk and sign-in risk investigation
- Privileged Identity Management (PIM)
- Just-In-Time privileged access
- Entitlement Management
- Access Packages
- Access Reviews
- OAuth 2.0 and OpenID Connect
- Application registrations
- Workload identities
- Managed identities
- Microsoft Graph automation
- PowerShell identity automation
- Privileged Access Management (PAM)
- Identity security monitoring and remediation

---

# Current Status

The lab now has a functioning **bidirectional hybrid identity workflow** between Windows Server Active Directory and Microsoft Entra ID.

The environment has progressed from:

```text
Standalone Active Directory
```

to:

```text
Active Directory
      ↓
Microsoft Entra Connect
      ↓
Password Hash Synchronization
      ↓
Microsoft Entra ID
      ↓
Cloud Authentication
      ↓
Microsoft Authenticator / MFA
```

and now also supports the reverse password recovery path:

```text
Microsoft Entra SSPR
      ↓
Microsoft Authenticator Verification
      ↓
Password Reset
      ↓
Password Writeback
      ↓
Microsoft Entra Connect Sync
      ↓
On-Premises Active Directory
      ↓
Domain Authentication
```

The first three phases establish a working hybrid identity foundation with synchronized identities, cloud authentication, MFA, SSPR, and on-premises password writeback.

The next phases will build identity lifecycle, access control, risk-based security, governance, privileged access, application identity, and automation on top of this foundation — progressing from **hybrid identity implementation** toward broader **IAM and PAM engineering**.