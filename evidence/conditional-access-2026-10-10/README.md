# Phase 5 — Conditional Access and SAML evidence

**Evidence captured:** 10 October 2026 · **91 screenshots in the local evidence archive**.

## Verified outcomes
- On-premises AD DS identity synchronized to Microsoft Entra ID.
- Finance dynamic group membership used to assign the Microsoft Entra SAML Toolkit enterprise application.
- Frank Castle successfully authenticated to the SAML Toolkit application.
- MFA was successfully tested for Frank.
- Emergency-access administrator sign-in verified.
- Conditional Access policies CA000, CA001, CA003 and CA004 created and evaluated.

## Important interpretation
At the time of the captured sign-in logs, the policies were **Report-only**. MFA success and SAML SSO success are confirmed, but they do **not** establish that an enabled Conditional Access policy enforced MFA. Some sign-ins show **Not Applied**. Validate enforcement separately before claiming it.

## Screenshot publication
The 91 PNG files are prepared in the evidence archive, but **are not yet committed to this GitHub directory**. The archive has only a standard header mask, which is not sufficient to guarantee removal of all private information. Review/redact every image before uploading; pay particular attention to tokens, recovery details, tenant/user identifiers and emails. Keep images under this directory so the evidence is browsable on GitHub.

See [Phase 5 documentation](../../docs/05-conditional-access-saml/README.md).
