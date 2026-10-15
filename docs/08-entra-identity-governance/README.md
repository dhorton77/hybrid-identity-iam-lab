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

All **42 evidence screenshots are stored separately** in the [evidence folder](./evidence/). Click a numbered link to open its original PNG; no screenshots are embedded in this README.

[Full evidence index](./EVIDENCE-INDEX.md) · [Browse evidence folder](./evidence/)

### Governance and access package setup

- [01 — Pim Governance Overview](./evidence/01-pim-governance-overview.png)
- [02 — Access Packages Overview](./evidence/02-access-packages-overview.png)
- [03 — New Package Basics](./evidence/03-new-package-basics.png)
- [04 — Package Catalog Selection](./evidence/04-package-catalog-selection.png)
- [05 — Package Resource Role Selection](./evidence/05-package-resource-role-selection.png)
- [06 — Select Finance Security Group](./evidence/06-select-finance-security-group.png)
- [07 — Resource Role Confirmation](./evidence/07-resource-role-confirmation.png)
- [08 — Resource Group Selection](./evidence/08-resource-group-selection.png)
- [09 — Finance Group Resource Selection](./evidence/09-finance-group-resource-selection.png)
- [10 — Package Basic Settings](./evidence/10-package-basic-settings.png)
- [11 — Finance Governance Group Overview](./evidence/11-finance-governance-group-overview.png)
- [12 — Finance Group Properties](./evidence/12-finance-group-properties.png)
- [13 — Finance Group Membership](./evidence/13-finance-group-membership.png)
- [14 — Package Resource Roles](./evidence/14-package-resource-roles.png)

### Request and approval policy

- [15 — Package Requests Configuration](./evidence/15-package-requests-configuration.png)
- [16 — Request Approval Configuration](./evidence/16-request-approval-configuration.png)
- [17 — Request Eligibility Configuration](./evidence/17-request-eligibility-configuration.png)
- [18 — Request Approver Selection](./evidence/18-request-approver-selection.png)
- [19 — Approval Justification Settings](./evidence/19-approval-justification-settings.png)
- [20 — Email Notifications](./evidence/20-email-notifications.png)
- [21 — Requestor Information](./evidence/21-requestor-information.png)

### Lifecycle, access reviews and creation

- [22 — Lifecycle Expiration](./evidence/22-lifecycle-expiration.png)
- [23 — Lifecycle Review Settings](./evidence/23-lifecycle-review-settings.png)
- [24 — Lifecycle Review Advanced](./evidence/24-lifecycle-review-advanced.png)
- [25 — Lifecycle Review Removal On No Response](./evidence/25-lifecycle-review-removal-on-no-response.png)
- [26 — Custom Extensions](./evidence/26-custom-extensions.png)
- [27 — Package Review Summary Top](./evidence/27-package-review-summary-top.png)
- [28 — Package Review Summary Bottom](./evidence/28-package-review-summary-bottom.png)
- [29 — Finance Package Created](./evidence/29-finance-package-created.png)

### Request, approval and assignment validation

- [30 — Frank Myaccess Home](./evidence/30-frank-myaccess-home.png)
- [31 — Frank Access Package Discovery](./evidence/31-frank-access-package-discovery.png)
- [32 — Frank Request History](./evidence/32-frank-request-history.png)
- [33 — Approver Request Details](./evidence/33-approver-request-details.png)
- [34 — Request Approved](./evidence/34-request-approved.png)
- [35 — Finance Package Overview](./evidence/35-finance-package-overview.png)
- [36 — Frank Group Membership Granted](./evidence/36-frank-group-membership-granted.png)
- [37 — Frank Assignment Delivered](./evidence/37-frank-assignment-delivered.png)

### Revocation and final review settings

- [38 — Finance Assignments Revoked](./evidence/38-finance-assignments-revoked.png)
- [39 — Finance Group Membership Removed](./evidence/39-finance-group-membership-removed.png)
- [40 — Group Access Review List](./evidence/40-group-access-review-list.png)
- [41 — Package Policy Summary](./evidence/41-package-policy-summary.png)
- [42 — Package Review Policy Details](./evidence/42-package-review-policy-details.png)

---

## 11. Final Validation

The Finance access package was configured and tested through the access request, approval, assignment and revocation stages.

The lab demonstrated successful entitlement delivery and removal, supported by screenshot evidence.

**Phase 8 status: Complete.**

---

[Return to main repository](../../README.md)
