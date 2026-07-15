# Phase 2: Cloud Access

## Purpose

This document defines the Phase 2 cloud access implementation guidance for a Microsoft 365 GCC High cross-tenant coexistence architecture.

Phase 2 focuses on enabling secure Microsoft 365 cloud collaboration between two separate GCC High tenants after a merger or acquisition.

The goal is to allow approved users to collaborate across tenants using Microsoft 365 services while preserving tenant separation, Conditional Access controls, compliance boundaries, and least-privilege access.

Phase 2 does not address access to on-premises engineering workloads. On-premises engineering and DevOps access is handled separately in Phase 3.

---

## Phase 2 Scope

Phase 2 focuses on Microsoft 365 cloud resource access.

In scope:

- Microsoft Teams collaboration.
- SharePoint Online access.
- OneDrive for Business sharing.
- Exchange Online and calendar collaboration.
- Microsoft 365 Apps access.
- B2B Collaboration.
- B2B Direct Connect evaluation for Teams shared channels.
- Cross-Tenant Access Settings.
- Cross-Tenant Synchronization evaluation.
- Conditional Access enforcement.
- MFA and device trust validation.
- Guest and external user governance.
- Access reviews and lifecycle management.

Out of scope:

- On-premises engineering application access.
- SSH or RDP access.
- OpenShift or private cloud access.
- GPU compute access.
- Large engineering dataset access.
- LDAP-dependent service access.
- VPN or Zero Trust remote access implementation.
- Migration of engineering workloads to Azure.
- Full tenant consolidation.

---

## Scenario Context

The reference architecture assumes two Microsoft 365 GCC High tenants.

| Area | Tenant A | Tenant B |
|---|---|---|
| Role | Primary Microsoft 365 tenant | Acquired / engineering tenant |
| Cloud | Microsoft 365 GCC High | Microsoft 365 GCC High |
| Identity model | Cloud-focused | Hybrid identity |
| Strategic direction | Main tenant for users, email, Teams, SharePoint, OneDrive, Microsoft 365 Apps, and back-office collaboration | Retained for engineering and DevOps workloads |
| Collaboration need | Users need controlled collaboration across tenant boundary | Tenant remains operational during coexistence |
| Device requirement | Managed or compliant devices expected for full Microsoft 365 client access | Engineering devices may remain aligned to acquired tenant |

Tenant A is the strategic Microsoft 365 tenant for users, email, collaboration, and back-office productivity services.

Tenant B remains operational for engineering and DevOps workloads. However, Phase 2 focuses only on Microsoft 365 cloud collaboration between the tenants.

---

## Phase 2 Objectives

The objectives of Phase 2 are:

1. Enable secure Microsoft 365 cloud collaboration between tenants.
2. Preserve tenant separation during the coexistence phase.
3. Scope access to approved users, groups, teams, sites, and applications.
4. Avoid broad or unmanaged external access.
5. Validate existing B2B collaboration and free/busy configuration.
6. Evaluate B2B Direct Connect for Teams shared channel collaboration.
7. Evaluate Cross-Tenant Synchronization for governed user provisioning.
8. Enforce Conditional Access for cross-tenant cloud access.
9. Validate MFA and device trust behavior across tenants.
10. Implement access governance, monitoring, and access reviews.

---

## Cloud Access Principles

### 1. Cloud Access Must Be Explicit

Cross-tenant access should not be assumed or broadly enabled.

Access should be granted only to approved users, approved groups, approved applications, and approved Microsoft 365 resources.

### 2. Use Microsoft-Native Cross-Tenant Controls

Cloud collaboration should use Microsoft Entra and Microsoft 365 cross-tenant capabilities first.

Relevant capabilities include:

- B2B Collaboration.
- B2B Direct Connect.
- Cross-Tenant Access Settings.
- Cross-Tenant Synchronization.
- Conditional Access.
- Access reviews and governance capabilities where available.

### 3. Conditional Access Remains Enforced

External and cross-tenant users should be subject to Conditional Access policies.

If Conditional Access policies target all users, B2B guests are included. If policies target only selected internal groups, guest and external users must be intentionally included through guest/external user targeting or group assignment.

### 4. Device Trust Requires Design Approval

Trusting device claims from another tenant is a security decision.

The architecture must document whether the resource tenant trusts:

- MFA claims from the partner tenant.
- Compliant device claims from the partner tenant.
- Hybrid joined device claims from the partner tenant.

### 5. Cloud Collaboration Is Not On-Premises Access

Microsoft 365 cross-tenant collaboration does not automatically provide access to retained engineering systems, SSH/RDP endpoints, private cloud resources, GPU workloads, or LDAP-dependent services.

Those access requirements are addressed separately in Phase 3.

---

## Cloud Access Workstreams

## Workstream 1: Microsoft 365 Resource Scope

Identify which Microsoft 365 resources require cross-tenant access.

Common resources include:

- Microsoft Teams.
- Teams shared channels.
- SharePoint Online sites.
- OneDrive for Business content.
- Exchange Online calendars.
- Microsoft 365 Apps.
- Microsoft cloud-hosted collaboration workloads.

For each resource, document:

| Resource | Resource Tenant | User Tenant | Access Type | Owner | Approval Required |
|---|---|---|---|---|---|
| Team or shared channel | Tenant A or Tenant B | Tenant A or Tenant B | Collaboration | Resource owner | Yes |
| SharePoint site | Tenant A or Tenant B | Tenant A or Tenant B | Read, contribute, owner | Site owner | Yes |
| OneDrive content | User-owned | External user | File or folder sharing | Content owner | Yes |
| Calendar availability | Tenant A and Tenant B | Tenant A and Tenant B | Free/busy lookup | Messaging admin | Yes |
| Microsoft 365 Apps | Tenant A | Approved users | Productivity access | Tenant owner | Yes |

Deliverables:

- Microsoft 365 resource inventory.
- Resource owners identified.
- Access approval process defined.
- Access scope documented.

---

## Workstream 2: B2B Collaboration

B2B Collaboration is used when external users need guest-based access to Microsoft 365 resources.

Use cases:

- Targeted SharePoint Online access.
- Teams collaboration where guest access is acceptable.
- OneDrive for Business sharing.
- Application access where guest representation is appropriate.
- Limited collaboration with approved users.

Design guidance:

- Use groups where possible instead of individual assignments.
- Avoid broad guest access.
- Require MFA through Conditional Access.
- Include B2B guests in access reviews.
- Define an internal sponsor or owner for guest access.
- Review existing guest accounts before creating new ones.
- Remove guest access when no longer required.

Important Conditional Access note:

- A policy scoped to `All users` includes B2B guests.
- A policy scoped only to selected internal users or groups does not automatically cover guests unless those guests are included intentionally.
- Guest and external users should be included intentionally in the security design.

Deliverables:

- B2B Collaboration use cases.
- Guest access model.
- Guest lifecycle process.
- Access review process.
- Conditional Access guest scope validation.

---

## Workstream 3: B2B Direct Connect for Teams Shared Channels

B2B Direct Connect should be evaluated for Teams shared channel collaboration.

This pattern can improve collaboration because users can participate in shared channels across organizations without the same guest switching experience used in traditional B2B Collaboration.

Use cases:

- Ongoing collaboration between users in both tenants.
- Teams shared channel scenarios.
- Cross-tenant collaboration where users should remain in their home tenant experience.
- Group-based collaboration between approved user populations.

Design guidance:

- Validate GCC High support and behavior before final design.
- Use partner-specific Cross-Tenant Access Settings.
- Scope access to approved users and groups.
- Avoid enabling broad tenant-wide access.
- Define inbound and outbound access settings.
- Review whether existing B2B guest accounts should remain, be cleaned up, or coexist.
- Apply Conditional Access policies consistently.
- Validate user experience with pilot groups.

Deliverables:

- B2B Direct Connect decision.
- Teams shared channel pilot.
- Approved groups and users.
- Inbound and outbound settings.
- Conditional Access validation.
- User experience findings.

---

## Workstream 4: Cross-Tenant Access Settings

Cross-Tenant Access Settings define how users from one Microsoft Entra tenant access resources in another tenant.

For this architecture, settings should be configured intentionally between Tenant A and Tenant B.

Configuration areas:

- Tenant A inbound access from Tenant B.
- Tenant A outbound access to Tenant B.
- Tenant B inbound access from Tenant A.
- Tenant B outbound access to Tenant A.
- Users and groups allowed.
- Applications allowed.
- B2B Collaboration behavior.
- B2B Direct Connect behavior.
- MFA trust settings.
- Compliant device trust settings.
- Hybrid joined device trust settings.

Recommended approach:

- Create partner-specific settings between Tenant A and Tenant B.
- Avoid relying only on default settings.
- Start with pilot groups.
- Allow only required users and applications.
- Review inbound and outbound access independently.
- Document every trust decision.
- Validate with sign-in logs before broad rollout.

Deliverables:

- Cross-tenant access design.
- Inbound and outbound access rules.
- Allowed user and group scope.
- Allowed application scope.
- MFA and device trust decisions.
- Pilot validation results.

---

## Workstream 5: Cross-Tenant Synchronization

Cross-Tenant Synchronization may be evaluated for scalable user provisioning and lifecycle management between tenants.

Use cases:

- Provisioning approved users from one tenant into the other tenant.
- Reducing manual guest account management.
- Supporting structured M&A coexistence.
- Managing lifecycle for users who need repeatable access.
- Supporting back-office migration planning.

Design guidance:

- Validate GCC High feature availability and behavior.
- Confirm licensing.
- Define source tenant and target tenant.
- Scope synchronization using groups.
- Do not synchronize all users unless there is a clear business requirement.
- Define attribute mapping.
- Define lifecycle behavior for users leaving or changing roles.
- Confirm whether synchronized users should be represented as guests or members based on supported configuration and intended use case.
- Test with a small pilot group first.

Deliverables:

- Cross-Tenant Synchronization decision.
- Source and target tenant roles.
- Pilot group scope.
- Attribute mapping plan.
- Lifecycle behavior.
- Rollback plan.

---

## Workstream 6: SharePoint Online and OneDrive Access

SharePoint Online and OneDrive for Business are common collaboration workloads during coexistence.

Design guidance:

- Keep sharing scoped to approved users, groups, sites, or files.
- Avoid organization-wide anonymous sharing.
- Use site-level ownership and approval.
- Use sensitivity labels and information protection where applicable.
- Use access reviews for shared sites and groups.
- Validate guest user experience.
- Review external sharing settings before enabling broad access.
- Align sharing policies with compliance requirements.

Recommended controls:

- Restrict sharing to approved domains or tenants where required.
- Require authenticated users.
- Require MFA through Conditional Access.
- Use specific site permissions instead of broad tenant-level access.
- Monitor sharing activity.
- Review inactive guest users.

Deliverables:

- SharePoint external collaboration model.
- OneDrive external sharing model.
- Approved sharing boundaries.
- Site owner responsibilities.
- Access review schedule.

---

## Workstream 7: Microsoft Teams Access

Microsoft Teams can support either B2B guest collaboration or B2B Direct Connect shared channels.

Design guidance:

- Use B2B guest access for simple collaboration scenarios.
- Use B2B Direct Connect for shared channel scenarios where appropriate.
- Define which teams or channels are approved for cross-tenant collaboration.
- Avoid broad external access across all teams.
- Define owners for shared teams and channels.
- Use naming conventions for external or shared collaboration spaces.
- Apply information governance and retention policies where applicable.
- Validate Teams desktop and web access with Conditional Access.

Deliverables:

- Teams collaboration model.
- Teams shared channel decision.
- Approved teams and channels.
- Owner and sponsor model.
- Teams user experience validation.

---

## Workstream 8: Exchange Online and Calendar Collaboration

Exchange Online and calendar collaboration may be required during coexistence.

Use cases:

- Free/busy lookup.
- Meeting scheduling across tenants.
- Mailbox migration readiness.
- Back-office user transition toward Tenant A.

Design guidance:

- Review existing free/busy configuration.
- Confirm whether calendar availability sharing is already configured.
- Validate user experience across tenants.
- Confirm whether external users require mailbox access or only scheduling visibility.
- Align Exchange and identity planning with user migration waves.
- Avoid granting mailbox access broadly unless explicitly required.

Deliverables:

- Calendar availability model.
- Exchange coexistence notes.
- Mailbox access assumptions.
- Scheduling experience validation.

---

## Workstream 9: Conditional Access for Cloud Access

Conditional Access policies must be validated for cross-tenant cloud collaboration.

Key questions:

- Are B2B guests included in existing Conditional Access policies?
- Are external users targeted intentionally?
- Are policies scoped to all users or selected groups?
- Is MFA required for external access?
- Are compliant devices required?
- Are hybrid joined devices required?
- Should MFA claims from the partner tenant be trusted?
- Should compliant device claims from the partner tenant be trusted?
- Should hybrid joined device claims from the partner tenant be trusted?
- Are policies tested in report-only mode before enforcement?

Recommended baseline:

- Require MFA for cross-tenant access.
- Require compliant device or trusted device state where appropriate.
- Restrict unmanaged devices for sensitive resources.
- Protect Microsoft 365 desktop client access with device compliance policy.
- Protect administrators separately.
- Use named pilot groups first.
- Review sign-in logs and Conditional Access results before rollout.

Deliverables:

- Conditional Access policy review.
- Guest and external user policy scope confirmation.
- MFA trust decision.
- Device trust decision.
- Report-only test results.
- Enforcement plan.

---

## Workstream 10: Monitoring and Governance

Cloud collaboration must be monitored and reviewed over time.

Monitoring areas:

- Sign-in logs.
- Conditional Access results.
- Guest user activity.
- Cross-tenant access failures.
- SharePoint and OneDrive sharing activity.
- Teams external collaboration.
- Access review results.
- Administrative changes.
- External user lifecycle events.

Governance controls:

- Access reviews.
- Group-based access.
- Resource owner approval.
- Guest sponsor model.
- Recurring review of external users.
- Recurring review of cross-tenant access settings.
- Documented support escalation path.
- Documented exception process.

Deliverables:

- Monitoring plan.
- Access review plan.
- Resource owner matrix.
- Support escalation model.
- Exception process.

---

## Phase 2 Implementation Steps

### Step 1: Confirm Cloud Collaboration Use Cases

Document which Microsoft 365 workloads require cross-tenant access.

Examples:

- Teams collaboration.
- Teams shared channels.
- SharePoint Online sites.
- OneDrive for Business content.
- Calendar availability.
- Microsoft 365 Apps access.

Output:

```text
Cloud collaboration use cases documented.
