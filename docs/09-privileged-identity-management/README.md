# Phase 9 — Privileged Identity Management (PIM)

**Lab date:** 16 October 2026  
**Status:** Complete — eligibility, approval, activation, auditing and manual deactivation validated  
**Evidence:** 27 screenshots

## 1. Objective

Implement and test Microsoft Entra Privileged Identity Management (PIM) to provide Just-in-Time (JIT) access to an administrative role without maintaining continuous elevated permissions.

This phase demonstrates approval-based privilege elevation, scoped administrative access, time-limited activation, accountability through justification and ticket references, and an auditable return to eligible status.

## 2. Lab Environment

| Component | Configuration |
|---|---|
| Identity platform | Microsoft Entra ID |
| Privileged access solution | Microsoft Entra Privileged Identity Management (PIM) |
| Directory scope | Default Directory |
| Privileged role | User Administrator |
| Test identity | Frank Castle |
| Named approver | David Horton |
| Assignment type | Permanently eligible; not permanently active |
| Maximum activation duration | 2 hours |
| Activation authentication requirement | Azure MFA |
| Justification | Required |
| Approval | Required |
| Request reference | LAB-PIM-009 |
| Ticket system | SC300-IAM-LAB |

## 3. Privileged Role Policy Configuration

Reviewed existing role assignments and the User Administrator role settings before changing the activation policy.

Engineering activities included:

- Reduced the maximum activation duration from eight hours to two hours.
- Required MFA to satisfy the authentication requirement for activation.
- Required a written business justification.
- Enabled approval before privileged activation.
- Selected a named approver and saved the policy changes.

**Result:** The User Administrator activation policy was configured for approval-based, time-limited elevation.

## 4. Eligible Role Assignment

Added Frank Castle to the User Administrator role as an **eligible** principal rather than granting an active standing assignment.

Validation included:

- Reviewing the role's existing active assignments.
- Configuring and saving the eligible assignment.
- Confirming the role was available to Frank for activation.

**Result:** Frank could request elevated access when required, without holding continuously active User Administrator privileges.

## 5. Activation Request and Approval Workflow

Submitted a privileged activation request using Frank's eligible role assignment.

The request included a business justification and the lab ticket details: `LAB-PIM-009` in `SC300-IAM-LAB`.

The designated approver reviewed and approved the request.

**Result:** The request followed the configured approval workflow before activation.

## 6. Privileged Access Activation Validation

Following approval, verified that the User Administrator role appeared as **Activated** under Frank's active assignments.

Validation included:

- Confirming the request and approval states.
- Reviewing the active role assignment.
- Checking PIM audit events for the policy change, eligibility, request, approval and activation.

**Result:** The privileged role became active after the approved activation request, with associated audit evidence.

## 7. Authentication and MFA Evidence

Reviewed a related Microsoft Entra interactive sign-in record.

| Sign-in field | Observed result |
|---|---|
| Authentication requirement | Multifactor authentication |
| Status | Success |
| MFA detail | MFA requirement satisfied by claim in the token |

**Validation boundary:** This confirms the observed sign-in satisfied its MFA requirement. It does **not** establish that a new MFA prompt occurred specifically during PIM activation.

## 8. Manual Deactivation and Return to Eligibility

Manually deactivated the User Administrator role before the configured two-hour maximum elapsed.

Validation included:

- Initiating early deactivation.
- Reviewing the resulting PIM activity.
- Confirming the active assignment ended.
- Verifying the role remained under **Eligible assignments**, with an **Activate** action available for future requests.

**Result:** Active elevated access was removed while the eligible assignment remained intact.

**Validation boundary:** The automatic two-hour expiry was configured but not independently tested; this phase demonstrates manual early deactivation.

## 9. Security Engineering Outcomes

This phase demonstrates:

- Least-privilege assignment of a scoped administrative role.
- Separation of **eligibility** from **active** privileged access.
- Approval-based Just-in-Time elevation.
- MFA policy requirements and evidence-aware sign-in analysis.
- Two-hour maximum activation policy.
- Business justification and ticket-based accountability.
- PIM audit logging and traceability.
- Manual deactivation and return to eligible status.

**Important distinction:** Permanent eligibility is not permanent active privilege.

## 10. Screenshot Evidence

All **27 evidence screenshots are stored separately** in the [evidence folder](./evidence/). Click a numbered link below to open the original PNG; no screenshots are embedded in this README.

[Full evidence index](./EVIDENCE-INDEX.md) · [Browse evidence folder](./evidence/)

### Role policy and approval configuration

- [01 — PIM role assignments overview](./evidence/01-pim-081724.png)
- [02 — User Administrator role settings](./evidence/02-pim-081855.png)
- [03 — Activation policy overview](./evidence/03-pim-081951.png)
- [04 — Activation policy editing](./evidence/04-pim-082118.png)
- [05 — Approval configuration](./evidence/05-pim-082605.png)
- [06 — Named approver selection](./evidence/06-pim-082628.png)
- [07 — Activation policy review](./evidence/07-pim-082956.png)
- [08 — Eligible role assignment setup](./evidence/08-pim-092046.png)

### Eligible assignment and activation request

- [09 — Eligibility configuration review](./evidence/09-pim-092106.png)
- [10 — PIM activation settings](./evidence/10-pim-092618.png)
- [11 — Activation policy validation](./evidence/11-pim-092947.png)
- [12 — Eligible assignment confirmation](./evidence/12-pim-093801.png)
- [13 — Frank's eligible role](./evidence/13-pim-094607.png)
- [14 — Activation request preparation](./evidence/14-pim-095618.png)
- [15 — Activation request submitted](./evidence/15-pim-100940.png)

### Approval, activation and audit validation

- [16 — Approval workflow status](./evidence/16-pim-101920.png)
- [17 — Approver request details](./evidence/17-pim-103501.png)
- [18 — Approval and activation audit](./evidence/18-pim-103749.png)
- [19 — Role activation verification](./evidence/19-pim-104805.png)
- [20 — PIM audit activity](./evidence/20-pim-105054.png)
- [21 — Audit event details](./evidence/21-pim-110103.png)
- [22 — Sign-in and MFA verification](./evidence/22-pim-111446.png)
- [23 — Privileged assignment review](./evidence/23-pim-112728.png)

### Deactivation and return to eligible status

- [24 — Role deactivation initiated](./evidence/24-pim-113231.png)
- [25 — Deactivation progress](./evidence/25-pim-113353.png)
- [26 — Deactivation completed](./evidence/26-pim-113421.png)
- [27 — Eligible assignment restored](./evidence/27-pim-113521.png)


---

## 11. Final Validation

The PIM workflow was tested through eligible assignment, activation request, approval, successful role activation, audit review, manual deactivation and return to eligible status.

**Validated:** Approval workflow, role activation, audit trail, manual deactivation and continued eligibility.

**Not independently validated:** Automatic expiry at the two-hour limit or a fresh MFA challenge at the moment of activation.

**Phase 9 status: Complete.**

---

[Return to main repository](../../README.md)
