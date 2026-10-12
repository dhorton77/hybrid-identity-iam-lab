# Phase 6 — Azure Key Vault and RBAC

**Lab date:** 12 October 2026

## Objective
Implement Azure Key Vault security using Microsoft Entra identities, Azure RBAC and least-privilege access controls.

## Implementation
- Deployed Azure Key Vault `kv-sc300-frank-2026` in UK South.
- Created resource group `rg-sc300-keyvault`.
- Enabled Azure role-based access control (RBAC).
- Assigned Frank Castle the Key Vault Secrets User role.
- Assigned Frank the Reader role at Key Vault scope.
- Assigned David-User the Key Vault Secrets Officer role.
- Created and enabled the dummy secret `sc300-test-secret`.
- Tested secret retrieval using Frank's account.

## Results
- **PASS:** Frank successfully accessed the Key Vault.
- **PASS:** Frank successfully retrieved the test secret value.
- **PASS:** Secret access was controlled through Azure RBAC.
- **PASS:** Roles were scoped to the individual Key Vault.

## Network Security
Public network access remained enabled to support testing from a dynamic public IP address.

Azure RBAC protects secret access, but the public endpoint remains reachable. A private endpoint or appropriately restricted network access would provide stronger production security.

## Evidence
Screenshots captured on 12 October 2026 are stored in:

`./evidence/`

Screenshots demonstrate vault deployment, RBAC assignments, secret creation and successful access testing.

## Lessons Learned
Azure resource visibility and Key Vault secret permissions are separate.

The Reader role allows Frank to discover and navigate the vault, while Key Vault Secrets User permits reading secret values.

The Application Administrator role does not automatically grant Key Vault secret access.

## Security Note
Evidence must be reviewed for sensitive information before publishing. Never upload actual secret values, passwords, tokens or private keys.
