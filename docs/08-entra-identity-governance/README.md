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

## 10. Evidence Gallery

The following screenshots document the configuration and validation activities.

### 01 — pim governance overview

![pim governance overview](evidence/01-pim-governance-overview.png)

### 02 — access packages overview

![access packages overview](evidence/02-access-packages-overview.png)

### 03 — new package basics

![new package basics](evidence/03-new-package-basics.png)

### 04 — package catalog selection

![package catalog selection](evidence/04-package-catalog-selection.png)

### 05 — package resource role selection

![package resource role selection](evidence/05-package-resource-role-selection.png)

### 06 — select finance security group

![select finance security group](evidence/06-select-finance-security-group.png)

### 07 — resource role confirmation

![resource role confirmation](evidence/07-resource-role-confirmation.png)

### 08 — resource group selection

![resource group selection](evidence/08-resource-group-selection.png)

### 09 — finance group resource selection

![finance group resource selection](evidence/09-finance-group-resource-selection.png)

### 10 — package basic settings

![package basic settings](evidence/10-package-basic-settings.png)

### 11 — finance governance group overview

![finance governance group overview](evidence/11-finance-governance-group-overview.png)

### 12 — finance group properties

![finance group properties](evidence/12-finance-group-properties.png)

### 13 — finance group membership

![finance group membership](evidence/13-finance-group-membership.png)

### 14 — package resource roles

![package resource roles](evidence/14-package-resource-roles.png)

### 15 — package requests configuration

![package requests configuration](evidence/15-package-requests-configuration.png)

### 16 — request approval configuration

![request approval configuration](evidence/16-request-approval-configuration.png)

### 17 — request eligibility configuration

![request eligibility configuration](evidence/17-request-eligibility-configuration.png)

### 18 — request approver selection

![request approver selection](evidence/18-request-approver-selection.png)

### 19 — approval justification settings

![approval justification settings](evidence/19-approval-justification-settings.png)

### 20 — email notifications

![email notifications](evidence/20-email-notifications.png)

### 21 — requestor information

![requestor information](evidence/21-requestor-information.png)

### 22 — lifecycle expiration

![lifecycle expiration](evidence/22-lifecycle-expiration.png)

### 23 — lifecycle review settings

![lifecycle review settings](evidence/23-lifecycle-review-settings.png)

### 24 — lifecycle review advanced

![lifecycle review advanced](evidence/24-lifecycle-review-advanced.png)

### 25 — lifecycle review removal on no response

![lifecycle review removal on no response](evidence/25-lifecycle-review-removal-on-no-response.png)

### 26 — custom extensions

![custom extensions](evidence/26-custom-extensions.png)

### 27 — package review summary top

![package review summary top](evidence/27-package-review-summary-top.png)

### 28 — package review summary bottom

![package review summary bottom](evidence/28-package-review-summary-bottom.png)

### 29 — finance package created

![finance package created](evidence/29-finance-package-created.png)

### 30 — frank myaccess home

![frank myaccess home](evidence/30-frank-myaccess-home.png)

### 31 — frank access package discovery

![frank access package discovery](evidence/31-frank-access-package-discovery.png)

### 32 — frank request history

![frank request history](evidence/32-frank-request-history.png)

### 33 — approver request details

![approver request details](evidence/33-approver-request-details.png)

### 34 — request approved

![request approved](evidence/34-request-approved.png)

### 35 — finance package overview

![finance package overview](evidence/35-finance-package-overview.png)

### 36 — frank group membership granted

![frank group membership granted](evidence/36-frank-group-membership-granted.png)

### 37 — frank assignment delivered

![frank assignment delivered](evidence/37-frank-assignment-delivered.png)

### 38 — finance assignments revoked

![finance assignments revoked](evidence/38-finance-assignments-revoked.png)

### 39 — finance group membership removed

![finance group membership removed](evidence/39-finance-group-membership-removed.png)

### 40 — group access review list

![group access review list](evidence/40-group-access-review-list.png)

### 41 — package policy summary

![package policy summary](evidence/41-package-policy-summary.png)

### 42 — package review policy details

![package review policy details](evidence/42-package-review-policy-details.png)

---

## 11. Final Validation

The Finance access package was configured and tested through the access request, approval, assignment and revocation stages.

The lab demonstrated successful entitlement delivery and removal, supported by screenshot evidence.

**Phase 8 status: Complete.**

---

[Return to main repository](../../README.md)
