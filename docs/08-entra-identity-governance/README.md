# Phase 8 — Microsoft Entra ID Governance

**Lab date:** 15 October 2026
**Status:** Complete — access package provisioning, approval, assignment and revocation validated
**Evidence:** 42 screenshots

## 1. Objective

Implement Microsoft Entra ID Governance to manage access requests, approvals, entitlement assignments, access expiration and periodic access reviews.

This phase demonstrates how Identity Governance supports least privilege, controlled access provisioning and the Joiner-Mover-Leaver (JML) lifecycle.

## 2. Lab Environment

| Component | Configuration |
|---|---|
| Identity platform | Microsoft Entra ID |
| Governance solution | Microsoft Entra ID Governance |
| Access package | Finance-Access-Package |
| Target security group | SG-Finance-Governance |
| Test identity | Frank Castle |
| Approver | David-User |
| Access duration | 30 days |
| Access review frequency | Monthly |
| Review duration | 7 days |

## 3. Access Package Configuration

Created the Finance-Access-Package using Microsoft Entra Entitlement Management.

Configured the package to provide controlled access to the Finance security group through an access request and approval workflow.

Engineering activities included:

- Reviewing the Entitlement Management interface.
- Selecting the access package catalog.
- Adding the Finance security group as a resource.
- Configuring resource roles.
- Defining request eligibility and approval settings.
- Configuring lifecycle and access review policies.
- Reviewing the configuration before creating the package.

## 4. Approval Workflow

Configured an approval process requiring authorization before access could be granted.

The workflow was tested using the lab identity Frank Castle.

The request was submitted through Microsoft My Access and reviewed by the designated approver.

**Result:** The access request was approved and the entitlement assignment was delivered.

## 5. Access Provisioning Validation

Following approval, the test identity was granted access through the Finance access package.

Validation included:

- Reviewing the access request history.
- Confirming approval status.
- Confirming the entitlement assignment.
- Verifying Finance security group membership.

**Result:** Frank Castle received the expected Finance group membership.

## 6. Lifecycle and Expiration

Configured access package lifecycle settings to limit the duration of assigned access.

**Configured duration:** 30 days.

This supports time-bound access and reduces the risk of unnecessary standing permissions.

## 7. Access Reviews

Configured periodic access reviews for the access package.

| Setting | Configuration |
|---|---|
| Review frequency | Monthly |
| Review duration | 7 days |
| Reviewer | David-User |
| Justification | Required |
| No-response action | Remove access |

These settings provide a mechanism for periodically validating whether access remains appropriate.

**Validation scope:** Review policy configuration was documented. A complete future monthly review cycle was not required to demonstrate the configuration.

## 8. Access Revocation Testing

Tested the removal of access from the Finance access package.

Validation included:

- Reviewing the entitlement assignment.
- Revoking the assigned access.
- Confirming the assignment was removed.
- Verifying the Finance security group membership was removed.

**Result:** The test identity no longer retained the Finance group membership following revocation.

This demonstrates controlled deprovisioning through entitlement management.

## 9. Security Engineering Outcomes

This phase demonstrates:

- Least-privilege access assignment.
- Approval-based access provisioning.
- Entitlement Management configuration.
- Time-bound access.
- Periodic access review configuration.
- Access revocation and deprovisioning.
- Practical JML lifecycle controls.
- Evidence-based validation of IAM security controls.

## 10. Screenshot Evidence

All **42 screenshots are stored as separate PNG files** in the [evidence folder](./evidence/), not embedded in this README.

**[Open the Phase 8 Screenshot Evidence Index — 42 individually linked images](./EVIDENCE-INDEX.md)**

The index groups evidence by numbered capture and provides a direct clickable link to each PNG, covering package configuration, access request, approval, assignment, group membership, review-policy settings and revocation.

---

## 11. Final Validation

The Finance access package was configured and tested through the access request, approval, assignment and revocation stages.

The lab demonstrated successful entitlement delivery and removal, supported by screenshot evidence.

**Phase 8 status: Complete.**

---

[Return to main repository](../../README.md)
