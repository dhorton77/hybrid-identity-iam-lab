# Phase 3 — Self-Service Password Reset (SSPR) and Password Writeback

## Overview

This phase implements and validates Microsoft Entra Self-Service Password Reset (SSPR) in a hybrid identity environment.

The objective was to allow a synchronized user to securely reset their password through Microsoft Entra ID using Microsoft Authenticator, while ensuring the new password was written back to the on-premises Active Directory environment.

The implementation was deliberately scoped to a pilot security group before wider deployment.

## Architecture

```text
Frank Castle
     |
     v
Microsoft Entra SSPR
     |
     | Identity verification
     v
Microsoft Authenticator
     |
     | New password
     v
Microsoft Entra ID
     |
     | Password Writeback
     v
Microsoft Entra Connect Sync
     |
     v
On-Premises Active Directory
     |
     v
SC300-PC01 Domain Authentication
```

## Objectives

- Configure SSPR for a controlled pilot group.
- Use Microsoft Authenticator as the verification method.
- Enable password writeback through Microsoft Entra Connect Sync.
- Allow a synchronized hybrid identity to reset its password from the cloud.
- Verify that the password change reaches on-premises Active Directory.
- Prove that the new password can authenticate against the on-premises domain.

## Pilot Deployment

A dedicated security group was created:

`SG-SSPR-Pilot`

The synchronized test identity:

`frank.castle@corp.davidboydltd.co.uk`

was added to the group.

SSPR was configured using **Selected** scope rather than enabling the feature for the entire tenant.

This demonstrates a controlled rollout approach where identity features can be tested with a limited population before broader deployment.

## Authentication Method

Microsoft Authenticator was used to verify the user's identity during the password reset process.

The test user had previously registered Microsoft Authenticator and was targeted by the tenant's Authenticator pilot policy.

During SSPR, Microsoft Entra required the user to approve a notification in the registered Authenticator application before allowing a new password to be created.

## Initial Hybrid Limitation

Before password writeback was configured, Microsoft Entra reported that no agent capable of performing password writeback had been detected.

This provided a useful troubleshooting scenario and demonstrated an important distinction:

**SSPR enables the user reset experience, while password writeback enables a cloud-initiated password change to be written back to on-premises Active Directory.**

## Password Writeback Configuration

Microsoft Entra Connect Sync was reconfigured on:

`SC300-SYNC01`

The existing configuration was retained, including:

- Password Hash Synchronization
- Selected OU synchronization
- Hybrid identity synchronization

The **Password writeback** optional feature was then enabled.

After configuration completed successfully, Microsoft Entra detected the on-premises writeback client and reported:

`Your on-premises writeback client is up and running.`

## End-to-End SSPR Test

The synchronized test user initiated account recovery using the Microsoft work or school account recovery process.

The workflow was:

```text
Identify account
      ↓
Verify identity with Microsoft Authenticator
      ↓
Choose a new password
      ↓
Microsoft Entra accepts password reset
      ↓
Password writeback sends change on-premises
      ↓
Active Directory password is updated
```

Microsoft Entra confirmed:

`Your password has been reset`

## On-Premises Validation

The reset was not considered complete based only on the Microsoft Entra success message.

The on-premises Active Directory account was checked directly on `SC300-DC01`.

The `PasswordLastSet` value for `frank.castle` showed the password had been changed at the time of the SSPR operation.

This provided direct evidence that password writeback had successfully reached Active Directory.

## Authentication Validation

A second validation was performed from domain-joined workstation:

`SC300-PC01`

Using the new password created through SSPR, the test identity successfully authenticated against the domain using:

`SC300LAB\frank.castle`

Access to the domain controller's `NETLOGON` share was also successfully performed under the user's security context.

This demonstrated that the password reset had propagated through the complete hybrid identity path rather than existing only in Microsoft Entra ID.

## Result

The final identity flow was successfully validated:

```text
Microsoft Entra SSPR
        ↓
Microsoft Authenticator verification
        ↓
Password reset
        ↓
Password Writeback
        ↓
Microsoft Entra Connect Sync
        ↓
On-Premises Active Directory
        ↓
Successful domain authentication
```

## Security and IAM Principles Demonstrated

This phase demonstrates several practical IAM concepts:

- Self-Service Password Reset
- Hybrid identity
- Password writeback
- Password Hash Synchronization
- Authentication method registration
- Microsoft Authenticator
- Pilot-group deployment
- Least-privilege rollout strategy
- Identity verification
- Hybrid authentication troubleshooting
- End-to-end technical validation

## Engineering Approach

The implementation followed a controlled engineering process:

**Design → Configure → Test → Troubleshoot → Validate → Document**

Rather than relying solely on portal success messages, the implementation was validated at multiple layers:

**Cloud configuration → Entra Connect → Active Directory → Domain authentication**

This provides evidence that the complete hybrid identity workflow operated successfully.

## Evidence

The `evidence` directory contains screenshots covering the implementation from initial configuration through final validation.

Evidence includes:

- SSPR pilot group targeting
- SSPR policy configuration
- Authentication method policy
- Registration settings
- Initial password writeback detection failure
- Microsoft Entra Connect optional features
- Password writeback selection
- Successful Entra Connect configuration
- Writeback client detected and running
- SSPR account recovery
- Microsoft Authenticator verification
- Password reset workflow
- Successful SSPR completion
- On-premises `PasswordLastSet` validation
- Domain authentication using the new password
- NETLOGON access under the synchronized user's identity

See the [evidence directory](./evidence/) for the complete implementation record.

## Key Learning

This lab demonstrates that SSPR in a hybrid environment is more than enabling a cloud setting.

A successful implementation requires the identity, authentication method, SSPR policy, synchronization infrastructure, password writeback capability, and on-premises Active Directory environment to work together.

The most important validation was therefore not the green success message in Microsoft Entra.

It was proving that a password created through cloud SSPR could ultimately authenticate the same synchronized identity against the on-premises Active Directory domain.