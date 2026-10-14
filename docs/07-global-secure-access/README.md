\# Phase 7 — Global Secure Access \& Hybrid Microsoft Entra Join



\*\*Lab dates:\*\* 13–14 October 2026  

\*\*Status:\*\* Partially validated — Global Secure Access authentication testing ongoing  

\*\*Evidence:\*\* 30 screenshots



\## 1. Objective



Extend the existing hybrid identity environment by configuring Microsoft Entra Hybrid Join for Windows 11 devices and deploying the Microsoft Global Secure Access client.



The objective is to demonstrate device identity management, hybrid synchronization, endpoint authentication and practical troubleshooting within a Zero Trust architecture.



\## 2. Lab Environment



| Component | Configuration |

|---|---|

| Domain controller | SC300-DC01 |

| Active Directory domain | ad.sc300lab.test |

| Domain controller IP | 192.168.1.3 |

| Synchronization server | SC300-SYNC01 |

| Endpoint 1 | SC300-PC01 — Windows 11 |

| Endpoint 2 | SC300-PC02 — Windows 11 |

| Firewall | IPFire — 192.168.1.2 |

| Cloud identity | Microsoft Entra ID |

| Endpoint connectivity | Global Secure Access Windows client |



\## 3. Hybrid Microsoft Entra Join



Configured Microsoft Entra Connect Sync to support hybrid device registration.



Engineering activities included:



\- Reviewing synchronization scope and organizational unit filtering.

\- Configuring Hybrid Microsoft Entra Join through the Entra Connect wizard.

\- Configuring the Active Directory Service Connection Point (SCP).

\- Synchronizing the Windows computer objects.

\- Running the Windows Automatic Device Join scheduled task.

\- Validating device registration in Microsoft Entra ID.



Both Windows 11 endpoints were successfully registered as hybrid-joined devices.



\## 4. Active Directory Secure Channel Troubleshooting



During configuration, both Windows 11 devices experienced Active Directory secure channel problems.



The issue was investigated and corrected using PowerShell.



```powershell

Test-ComputerSecureChannel -Verbose

```



The secure channels were repaired using domain administrator credentials and the domain controller.



Following remediation, both devices successfully validated their secure channel connections.



\*\*Engineering outcome:\*\* Restored trusted communication between the domain-joined Windows endpoints and Active Directory.



\## 5. Device Identity Validation



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



They do not independently establish successful user Primary Refresh Token acquisition or Global Secure Access connectivity.



\## 6. Global Secure Access Client Deployment



Installed the Microsoft Global Secure Access Windows client on SC300-PC01.



Deployment activities included:



\- Verifying internet connectivity.

\- Confirming domain controller connectivity.

\- Downloading the Windows client installer.

\- Installing the Global Secure Access client.

\- Restarting the endpoint.

\- Reviewing client authentication status.



The client was installed successfully, but user authentication was not yet validated.



\## 7. Authentication Troubleshooting



Following deployment, the Global Secure Access client displayed a signed-out status when testing with the synchronized domain identity Frank Castle.



Further investigation using `dsregcmd /status` identified:



```text

AzureAdPrt : NO

```



This indicated that a Microsoft Entra Primary Refresh Token was not present in the tested user session.



The investigation also examined differences between on-premises and cloud credentials and the synchronization of the test identity.



\*\*Outstanding validation:\*\* Confirm successful user authentication, PRT acquisition and Global Secure Access traffic forwarding.



\## 8. Security Principles



\### Zero Trust



Device registration is one component of a Zero Trust strategy. Access should be evaluated using relevant identity, device and policy signals rather than assuming trust from network location.



\### Defence in Depth



The lab combines Active Directory, Microsoft Entra ID, hybrid device registration, endpoint configuration and IPFire network segmentation.



\### Least Privilege



Administrative credentials were used for required configuration and repair operations rather than granting unnecessary privileges to standard test identities.



\### Validate Rather Than Assume



The successful installation of a client does not prove that authentication, access policy enforcement or private application connectivity is operational.



\## 9. Validation Summary



| Test | Outcome |

|---|---|

| Active Directory secure channel repair | Passed |

| Entra Connect hybrid device configuration | Completed |

| PC01 Hybrid Microsoft Entra Join | Passed |

| PC02 Hybrid Microsoft Entra Join | Passed |

| Device authentication status | Passed |

| TPM protection status | Confirmed |

| Global Secure Access client installation | Passed |

| Global Secure Access user authentication | Not yet validated |

| User Primary Refresh Token | Not acquired in tested session |

| Private Access connectivity | Not yet validated |



\## 10. Evidence



Thirty screenshots document the configuration, troubleshooting and validation activities.



Evidence files are stored alongside this README as `phase7\_01.png` through `phase7\_30.png`.



The screenshots demonstrate the engineering process, including configuration changes, errors, troubleshooting and successful hybrid device registration.



\## 11. Engineering Outcome



Phase 7 successfully established Microsoft Entra Hybrid Join across two domain-joined Windows 11 endpoints and demonstrated practical troubleshooting of Active Directory trust relationships and device synchronization.



The Global Secure Access client was deployed successfully. Authentication and Private Access testing remain outstanding.



\*\*Next engineering areas:\*\* Complete Global Secure Access validation, continue Microsoft Entra ID Governance, and implement Privileged Identity Management (PIM).

