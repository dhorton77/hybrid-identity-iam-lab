# Phase 5 — Conditional Access, MFA and SAML SSO

**Lab date:** 10 October 2026

## Objective
Validate a hybrid-synced identity's access to a real SAML enterprise application, evaluate Microsoft Entra Conditional Access controls, and verify emergency administrative access.

## Implementation
- Synchronized test identity **Frank Castle** from on-premises Active Directory using Microsoft Entra Connect.
- Confirmed membership of **SG-Finance-Dynamic**, based on the synchronized `department = Finance` attribute.
- Added **Microsoft Entra SAML Toolkit** as an enterprise application and assigned the Finance dynamic group.
- Configured SAML single sign-on and successfully launched the application as Frank.
- Verified that Frank completed MFA and successfully accessed the application.
- Created and evaluated the following Conditional Access policies:
  - `CA000-Require-MFA-Administrators`
  - `CA001-Finance-Portal-Require-MFA`
  - `CA003-Require-MFA-All-Users`
  - `CA004-Block-Legacy-Authentication`
- Excluded the dedicated emergency administrator from applicable MFA policies and verified successful Azure/Entra administrative sign-in.
- Used **What If**, sign-in logs, and **Report-only** evaluation to troubleshoot policy matching.
- Reviewed Security Defaults in preparation for Conditional Access management.

## Results and evidence boundaries
- **Confirmed:** SAML Toolkit sign-in succeeded for the synchronized Finance user.
- **Confirmed:** MFA was completed in the user test.
- **Confirmed:** The emergency administrator could sign in to Azure/Entra.
- **Confirmed:** The `CA003` policy appeared as **Report-only: User action required** for a Frank sign-in.
- **Not established by the captured logs:** enforced Conditional Access MFA on the SAML Toolkit sign-in. Some successful sign-in events displayed **Conditional Access: Not Applied**. A successful MFA event does not by itself prove that a particular Conditional Access policy enforced it.

## Troubleshooting notes
The original `SC300-Finance-Portal` app registration was not a functioning sign-in application, so a gallery SAML test application was used instead. Separate browser sessions helped avoid cached sign-in state during verification.

## Evidence
Screenshots captured on 10 October 2026 are prepared for review and upload under:

`evidence/conditional-access-2026-10-10/`

**Security note:** Screenshots must be reviewed for tenant identifiers, email addresses, user IDs and other sensitive details before publishing. A header mask alone is not full sanitization.

## Lessons learned
Successful app assignment, SAML SSO, MFA completion, and Conditional Access enforcement are separate claims requiring separate evidence. Use sign-in logs and policy result details to distinguish them.
