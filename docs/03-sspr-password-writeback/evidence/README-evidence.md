# Phase 3 — SSPR and Password Writeback Evidence

This evidence set documents a controlled SSPR pilot for a synced hybrid identity: scoped rollout, Authenticator verification, Entra Connect password writeback, on-premises AD confirmation, and domain-resource authentication.

## Evidence sequence
1. SSPR pilot group selected.
2. SSPR policy saved.
3. Authentication methods policy reviewed.
4. SSPR registration settings reviewed.
5. Before state: no writeback-capable agent detected.
6. Entra Connect optional features before writeback.
7. Password writeback selected.
8. Ready-to-configure summary.
9. Entra Connect configuration succeeded.
10. Entra reports the on-premises writeback client is running.
11–16. End-user SSPR flow through successful reset.
17. On-premises AD `PasswordLastSet` confirms the change.
18–19. New credentials successfully access `\\SC300-DC01\NETLOGON`.

## Security notes
- SSPR is scoped to a dedicated pilot group.
- No passwords, MFA QR codes, recovery secrets, or authentication tokens are shown.
- The test user was not granted unnecessary RDP or DC logon rights for validation.
