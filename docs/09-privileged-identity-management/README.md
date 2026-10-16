# Phase 9 — Privileged Identity Management (PIM)

**Lab date:** 16 October 2026  
**Status:** Complete — eligible assignment, approval, activation, auditing and manual deactivation validated  
**Test identity:** Frank Castle  
**Role:** Microsoft Entra User Administrator  
**Scope:** Default Directory

## 1. Objective

Demonstrate Just-in-Time (JIT) privileged access using Microsoft Entra Privileged Identity Management (PIM), with controlled elevation, approval, authentication requirements, a time limit, and traceable audit events.

## 2. Configuration

| Control | Lab setting |
|---|---|
| Role | User Administrator |
| Eligible principal | Frank Castle |
| Eligible assignment | Permanent eligibility (not standing privilege) |
| Maximum activation | 2 hours |
| Activation authentication | Azure MFA |
| Justification | Required |
| Approval | Required |
| Named approver | David Horton |
| Request reference | LAB-PIM-009 |
| Ticket system | SC300-IAM-LAB |

The test identity was made **eligible**, not permanently active. PIM policy changes were saved before testing the activation flow.

## 3. Implementation and Validation

1. Inspected existing active role assignments and the User Administrator role settings.
2. Changed maximum activation duration from eight to two hours.
3. Required MFA, a written justification and approval, and selected a named approver.
4. Added Frank Castle as an eligible User Administrator.
5. Submitted an activation request with a lab ticket reference and business justification.
6. Confirmed that the request entered the approval workflow.
7. Approved the request using the designated approver account.
8. Verified that the User Administrator role appeared as **Activated** under Frank's active assignments.
9. Inspected the PIM resource audit: eligible assignment, settings update, request, approval and activation completed successfully.
10. Reviewed a related Entra interactive sign-in event and its MFA requirement.
11. Manually deactivated the role before the two-hour activation period elapsed.
12. Verified successful deactivation and that the User Administrator role returned to **Eligible assignments** with an **Activate** action.

## 4. MFA Validation Boundary

The sign-in record displayed **Authentication requirement: Multifactor authentication**, **Status: Success**, and **MFA requirement satisfied by claim in the token**. This supports MFA satisfaction for the observed sign-in, but does **not** prove a fresh MFA prompt was issued at the moment of PIM activation.

## 5. Security Engineering Outcomes

- **Least privilege:** Elevated permissions were assigned only to the scoped User Administrator role.
- **Just-in-Time access:** Eligibility was retained without leaving the role continuously active.
- **Separation of duties:** A named approver authorised elevation.
- **Time-bound elevation:** Activation was capped at two hours.
- **Accountability:** Request justification, ticket reference and audit events were retained.
- **Revocation:** Manual early deactivation was completed and verified.

**Important distinction:** Permanent *eligibility* does not mean permanent *active privilege*. The role remains available for a future approved activation.

## 6. Evidence

All screenshots are linked individually in the [evidence index](./EVIDENCE-INDEX.md) and stored in the [evidence folder](./evidence/). Original sequence is preserved in chronological filename order.

## 7. Final Result

**Phase 9 completed.** The eligible → requested → approved → activated → audited → deactivated → eligible lifecycle was demonstrated.

The screenshots do not independently demonstrate the automatic two-hour expiry mechanism or a new MFA challenge at elevation time; these were not claimed as validated.

---

[Return to main repository](../../README.md)
