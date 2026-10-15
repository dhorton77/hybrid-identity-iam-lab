# Phase 7 — Global Secure Access & Hybrid Microsoft Entra Join

**Lab dates:** 13–14 October 2026  

**Status:** Complete — hybrid join, PRT acquisition, Global Secure Access connectivity and Private Access tunnelling validated  

**Evidence:** 46 screenshots

## 1. Objective

Extend the existing hybrid identity environment by configuring Microsoft Entra Hybrid Join for Windows 11 devices and deploying the Microsoft Global Secure Access client.

The objective is to demonstrate device identity management, hybrid synchronization, endpoint authentication and practical troubleshooting within a Zero Trust architecture.

## 2. Infrastructure Overview

### Active Directory and Identity

- **Domain Controller:** SC300-DC01
- **Active Directory Domain:** ad.sc300lab.test
- **Domain Controller IP:** 192.168.1.3
- **Microsoft Entra Connect Server:** SC300-SYNC01

### Windows 11 Endpoints

- **SC300-PC01:** Domain-joined Windows 11 workstation
- **SC300-PC02:** Domain-joined Windows 11 workstation
- **Device Identity:** Hybrid Microsoft Entra joined

### Network and Security

- **Firewall:** IPFire
- **Firewall IP:** 192.168.1.2
- **Internal Network:** Private Hyper-V Green Zone
- **Cloud Identity:** Microsoft Entra ID
- **Security Architecture:** Zero Trust and least privilege

## 3. Hybrid Microsoft Entra Join

Configured Microsoft Entra Connect Sync to support hybrid device registration.

Engineering activities included:

- Reviewing synchronization scope and organizational unit filtering.

- Configuring Hybrid Microsoft Entra Join through the Entra Connect wizard.

- Configuring the Active Directory Service Connection Point (SCP).

- Synchronizing the Windows computer objects.

- Running the Windows Automatic Device Join scheduled task.

- Validating device registration in Microsoft Entra ID.

Both Windows 11 endpoints were successfully registered as hybrid-joined devices.

## 4. Active Directory Secure Channel Troubleshooting

During configuration, both Windows 11 devices experienced Active Directory secure channel problems.

The issue was investigated and corrected using PowerShell.

```powershell

Test-ComputerSecureChannel -Verbose

```

The secure channels were repaired using domain administrator credentials and the domain controller.

Following remediation, both devices successfully validated their secure channel connections.

**Engineering outcome:** Restored trusted communication between the domain-joined Windows endpoints and Active Directory.

## 5. Device Identity Validation

Device registration was tested using:

```powershell

dsregcmd /status

```

The following results were confirmed on both endpoints:

| Device property | Result |

|---|---|

| AzureAdJoined | YES |

| DomainJoined | YES |

| DeviceAuthStatus | SUCCESS |

| TpmProtected | YES |

These results confirm successful hybrid device registration and device authentication.

User Primary Refresh Token (PRT) acquisition was subsequently confirmed through PowerShell (`dsregcmd /status`, `AzureAdPrt : YES`). This is distinct from the device registration checks above.

## 6. Global Secure Access Client Deployment

Deployed and tested the Microsoft Global Secure Access Windows client on SC300-PC01 and SC300-PC02.

Deployment activities included:

- Verifying internet connectivity.

- Confirming domain controller connectivity.

- Downloading the Windows client installer.

- Installing the Global Secure Access client.

- Restarting the endpoint.

- Reviewing client authentication status.

**Final result:** Both clients showed **Connected** status, including connected **Entra** and **Private** channels. Earlier client health and sign-in issues are retained below as troubleshooting evidence.

## 7. Authentication Troubleshooting

Following deployment, the Global Secure Access client displayed a signed-out status when testing with the synchronized domain identity Frank Castle.

Further investigation using `dsregcmd /status` identified:

```text

AzureAdPrt : NO

```

This indicated that a Microsoft Entra Primary Refresh Token was not present in the tested user session.

The investigation also examined differences between on-premises and cloud credentials and the synchronization of the test identity.

**Resolution:** After identity and client troubleshooting, the user confirmed `AzureAdPrt : YES` in PowerShell. Both Global Secure Access clients subsequently showed connected Entra and Private channels. The original `AzureAdPrt : NO` output above records an earlier troubleshooting state, not the final outcome.

## 8. Private Access Application and Traffic Validation

To test access to an internal application, IIS was installed on **SC300-SYNC01** and an internal test webpage was made available at `http://192.168.1.4/index.html`.

- Configured the **Quick Access** private application segment for **192.168.1.4**, **TCP port 80**.
- Saved the segment and assigned the test user **Frank Castle** to the Quick Access application.
- Refreshed Global Secure Access policy and checked the client forwarding rules.
- Confirmed a **Tunnel** forwarding rule for `192.168.1.4:80/TCP` on SC300-PC01.
- Validated an active TCP connection to **192.168.1.4:80** in the Global Secure Access traffic diagnostics, with **Action = Tunnel**.

**Engineering result:** Private Access tunnel traffic to the internal IIS application was demonstrated on **SC300-PC01**. The connected client status on PC02 was also observed; a separate PC02 traffic trace is not included in this evidence set.

## 9. Security Principles

### Zero Trust

Device identity, user authentication, application assignment and traffic policy were checked separately. A connected client alone was not treated as proof of private application tunnelling.

### Defence in Depth

The lab combines Active Directory, Microsoft Entra ID, hybrid device registration, Global Secure Access policy, IIS application testing and IPFire network segmentation.

### Least Privilege

Access to the test private application was assigned to a named test identity rather than granting broad access to all users.

### Validate Rather Than Assume

The final traffic diagnostics provide stronger proof of forwarding than installation or connection status alone.

## 10. Validation Summary

| Validation area | Outcome |
|---|---|
| AD secure channel repair | Validated on PC01 and PC02 |
| Hybrid Microsoft Entra Join | Validated on PC01 and PC02 |
| Device authentication and TPM protection | Validated |
| User PRT acquisition | Confirmed through PowerShell (`AzureAdPrt : YES`) |
| Global Secure Access client | Connected on PC01 and PC02 |
| Entra and Private channels | Connected on both clients |
| Quick Access application segment and user assignment | Configured |
| Private Access tunnel to internal IIS app | Validated through PC01 forwarding rule and traffic diagnostics |

## 11. Evidence

The original 30 screenshots document hybrid join, secure-channel remediation and client setup. The 16 additional screenshots cover application configuration, troubleshooting and final Global Secure Access / Private Access validation.

### Original configuration and troubleshooting screenshots

- [Screenshot 01](evidence/phase7_01.png)
- [Screenshot 02](evidence/phase7_02.png)
- [Screenshot 03](evidence/phase7_03.png)
- [Screenshot 04](evidence/phase7_04.png)
- [Screenshot 05](evidence/phase7_05.png)
- [Screenshot 06](evidence/phase7_06.png)
- [Screenshot 07](evidence/phase7_07.png)
- [Screenshot 08](evidence/phase7_08.png)
- [Screenshot 09](evidence/phase7_09.png)
- [Screenshot 10](evidence/phase7_10.png)
- [Screenshot 11](evidence/phase7_11.png)
- [Screenshot 12](evidence/phase7_12.png)
- [Screenshot 13](evidence/phase7_13.png)
- [Screenshot 14](evidence/phase7_14.png)
- [Screenshot 15](evidence/phase7_15.png)
- [Screenshot 16](evidence/phase7_16.png)
- [Screenshot 17](evidence/phase7_17.png)
- [Screenshot 18](evidence/phase7_18.png)
- [Screenshot 19](evidence/phase7_19.png)
- [Screenshot 20](evidence/phase7_20.png)
- [Screenshot 21](evidence/phase7_21.png)
- [Screenshot 22](evidence/phase7_22.png)
- [Screenshot 23](evidence/phase7_23.png)
- [Screenshot 24](evidence/phase7_24.png)
- [Screenshot 25](evidence/phase7_25.png)
- [Screenshot 26](evidence/phase7_26.png)
- [Screenshot 27](evidence/phase7_27.png)
- [Screenshot 28](evidence/phase7_28.png)
- [Screenshot 29](evidence/phase7_29.png)
- [Screenshot 30](evidence/phase7_30.png)

### Additional Private Access and validation screenshots

- [Global Secure Access client connected](evidence/01-gsa-client-connected.png)
- [Client health diagnostics](evidence/02-client-health-check.png)
- [Quick Access configuration](evidence/03-quick-access-configuration.png)
- [IIS installation on internal server](evidence/04-iis-installation.png)
- [Internal IIS test website](evidence/05-iis-local-website.png)
- [Client webpage test before routing](evidence/06-iis-client-webpage-before-routing.png)
- [Private Access application segment](evidence/07-private-access-app-segment.png)
- [Application segment saved](evidence/08-private-access-segment-saved.png)
- [Quick Access assignment state](evidence/09-quick-access-before-assignment.png)
- [Frank Castle assigned to Quick Access](evidence/10-frank-quick-access-assignment.png)
- [Global Secure Access policy refresh](evidence/11-gsa-policy-refresh.png)
- [TCP 80 tunnel forwarding rule](evidence/12-forwarding-rule-tunnel.png)
- [Internal IIS webpage via client](evidence/13-iis-website-via-client.png)
- [Traffic diagnostics showing Tunnel action](evidence/14-traffic-tunnel-evidence.png)
- [Client connectivity recovery](evidence/15-client-recovery.png)
- [Private Access policy settings](evidence/16-private-access-policy-settings.png)

## 12. Engineering Outcome

Phase 7 established Hybrid Microsoft Entra Join on two Windows 11 endpoints, restored their Active Directory secure channels, confirmed user PRT acquisition and validated connected Global Secure Access clients.

A private IIS test application was configured through Quick Access, with named-user assignment and **actual tunneled TCP traffic** demonstrated in the PC01 diagnostics.

**Next engineering areas:** Microsoft Entra ID Governance and Privileged Identity Management (PIM).

---

## Screenshot evidence index

[Browse the numbered screenshot evidence links](./EVIDENCE-INDEX.md) · [Open the separate evidence folder](./evidence/)
