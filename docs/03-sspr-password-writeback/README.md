\# Phase 3 — Self-Service Password Reset (SSPR) and Password Writeback



\## Objective



Implement and validate Self-Service Password Reset (SSPR) for a controlled group of hybrid users, including password writeback from Microsoft Entra ID to the on-premises Active Directory environment.



The goal was to demonstrate an end-to-end hybrid identity recovery workflow:



\*\*User → Microsoft Entra SSPR → Microsoft Authenticator verification → Password reset → Microsoft Entra Connect Sync → Password writeback → On-premises Active Directory\*\*



The implementation was deliberately scoped to a pilot group before wider deployment.



\---



\## Environment



| Component | Configuration |

|---|---|

| On-premises AD DS | `ad.sc300lab.test` |

| Domain Controller | `SC300-DC01` |

| Entra Connect Server | `SC300-SYNC01` |

| Domain Client | `SC300-PC01` |

| Hybrid test identity | `frank.castle@corp.davidboydltd.co.uk` |

| SSPR scope | `SG-SSPR-Pilot` |

| Authentication method | Microsoft Authenticator |

| Synchronization | Microsoft Entra Connect Sync |

| Authentication sync | Password Hash Synchronization |

| Password recovery | SSPR with Password Writeback |



\---



\## Design



SSPR was not enabled tenant-wide during initial testing.



A dedicated security group was created:



`SG-SSPR-Pilot`



The hybrid test account \*\*Frank Castle\*\* was added to this group.



This provided a controlled deployment model where SSPR functionality could be configured and validated with a limited scope before considering broader rollout.



This approach reflects the principle of:



\*\*Pilot → Validate → Monitor → Expand\*\*



\---



\## 1. Configure SSPR Pilot Scope



Self-Service Password Reset was configured for \*\*Selected\*\* users rather than the entire tenant.



The selected group was:



`SG-SSPR-Pilot`



This allowed SSPR testing without affecting every user in the environment.



\### Evidence



!\[SSPR pilot group selected](evidence/01-sspr-pilot-group-selected.png)



!\[SSPR policy saved](evidence/02-sspr-policy-saved.png)



\---



\## 2. Review Authentication Methods



The Microsoft Entra unified Authentication Methods policy was reviewed before testing SSPR.



Microsoft Authenticator was already enabled for a controlled authentication-method pilot group containing the test identity.



The test user had previously registered Microsoft Authenticator successfully.



\### Evidence



!\[Authentication methods policy](evidence/03-authentication-methods-policy.png)



\---



\## 3. Review SSPR Registration Policy



The SSPR registration configuration was reviewed.



The environment was configured to require users to register authentication information when signing in.



Authentication information was configured for periodic reconfirmation.



\### Evidence



!\[SSPR registration settings](evidence/04-sspr-registration-settings.png)



\---



\## 4. Identify Missing Password Writeback Capability



Before enabling password writeback, the Microsoft Entra Password Reset \*\*On-premises integration\*\* page reported that no writeback-capable agent was detected.



This provided a useful baseline and demonstrated that enabling SSPR alone does not provide hybrid password writeback.



\### Evidence



!\[Password writeback not detected](evidence/05-password-writeback-not-detected-before.png)



\---



\## 5. Enable Password Writeback in Microsoft Entra Connect Sync



Microsoft Entra Connect Sync was opened on `SC300-SYNC01`.



The existing synchronization configuration was retained, including:



\- Password Hash Synchronization

\- Existing Active Directory connector

\- Existing Microsoft Entra connector

\- Selected OU synchronization scope



Under \*\*Optional features\*\*, \*\*Password writeback\*\* was enabled.



\### Before



!\[Optional features before password writeback](evidence/06-connect-optional-features-before.png)



\### Password Writeback Selected



!\[Password writeback selected](evidence/07-password-writeback-selected.png)



\### Configuration Review



Before committing the change, the wizard confirmed that it would:



\- Enable Password Writeback

\- Configure synchronization services

\- Start synchronization after configuration



!\[Ready to enable password writeback](evidence/08-ready-to-enable-password-writeback.png)



\### Configuration Complete



Microsoft Entra Connect Sync completed the configuration successfully.



!\[Microsoft Entra Connect configuration complete](evidence/09-connect-configuration-complete.png)



\---



\## 6. Validate On-Premises Integration



After enabling Password Writeback, the Microsoft Entra Password Reset portal was checked again.



The previous warning was replaced with:



\*\*Your on-premises writeback client is up and running.\*\*



Microsoft Entra Connect Sync showed:



\*\*Status: Set up complete\*\*



Password writeback for synchronized users was enabled.



\### Evidence



!\[Password writeback client running](evidence/10-password-writeback-client-running.png)



This confirmed that Microsoft Entra could detect the on-premises password writeback capability.



\---



\## 7. Perform End-to-End SSPR Test



A fresh browser session was used to test the user recovery experience.



The recovery process was started using Microsoft's account recovery workflow.



\### Start SSPR



!\[SSPR sign-in start](evidence/11-sspr-signin-start.png)



\### Select Work or School Account



!\[Work or school account recovery](evidence/12-work-school-account-recovery.png)



\### Identify Hybrid User



The synchronized hybrid identity was supplied to the SSPR service.



!\[SSPR account identification](evidence/13-sspr-account-identification.png)



\---



\## 8. Verify Identity with Microsoft Authenticator



Microsoft Entra identified Microsoft Authenticator as an available verification method for the test user.



The user approved the authentication request using the registered Microsoft Authenticator device.



\### Evidence



!\[Authenticator verification method](evidence/14-authenticator-verification-method.png)



Successful verification allowed the user to proceed to the password reset stage.



!\[Choose new password](evidence/15-sspr-new-password-screen.png)



No passwords, QR codes, MFA secrets, or authentication tokens are stored in this repository.



\---



\## 9. Successful Password Reset



A new password meeting the Active Directory password requirements was entered through the Microsoft SSPR portal.



Microsoft reported:



\*\*Your password has been reset\*\*



\### Evidence



!\[SSPR password reset successful](evidence/16-sspr-password-reset-success.png)



At this stage the cloud workflow had succeeded, but additional validation was performed to confirm that the change had actually reached the on-premises Active Directory environment.



\---



\## 10. Validate Password Writeback in Active Directory



The test user's Active Directory object was queried directly on `SC300-DC01`.



The `PasswordLastSet` property showed a new timestamp corresponding with the SSPR operation.



\### Evidence



!\[On-premises PasswordLastSet proof](evidence/17-onprem-passwordlastset-proof.png)



This demonstrated that the password change had reached the on-premises Active Directory account.



\---



\## 11. Validate New Credentials Against an On-Premises Resource



An additional authentication test was performed from `SC300-PC01`.



A network-only credential context was created for:



`SC300LAB\\frank.castle`



\### Evidence



!\[Frank network authentication](evidence/18-frank-netonly-authentication.png)



The credential context was then used to access the domain controller's `NETLOGON` share:



`\\\\SC300-DC01\\NETLOGON`



The directory was successfully accessed.



\### Evidence



!\[NETLOGON access proof](evidence/19-netlogon-access-proof.png)



This provided end-to-end validation that the new credentials could authenticate against an on-premises domain resource.



\---



\## End-to-End Result



The completed workflow was:



```text

Hybrid User

&#x20;   |

&#x20;   v

Microsoft Entra SSPR

&#x20;   |

&#x20;   v

Microsoft Authenticator

&#x20;   |

&#x20;   v

Identity Verification

&#x20;   |

&#x20;   v

New Password

&#x20;   |

&#x20;   v

Microsoft Entra ID

&#x20;   |

&#x20;   v

Password Writeback

&#x20;   |

&#x20;   v

Microsoft Entra Connect Sync

&#x20;   |

&#x20;   v

On-Premises Active Directory

&#x20;   |

&#x20;   v

SC300-DC01

&#x20;   |

&#x20;   v

Domain Authentication Successful

