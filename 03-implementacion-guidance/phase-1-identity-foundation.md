# Phase 1: Identity Foundation

## Purpose

This document defines the Phase 1 identity foundation for a Microsoft 365 GCC High merger and acquisition coexistence architecture.

The goal of this phase is to establish the identity, tenant relationship, access governance, and Conditional Access foundation required before enabling broader Microsoft 365 collaboration or on-premises engineering access.

This phase does not perform full tenant consolidation. It also does not migrate engineering or DevOps workloads to Azure. Instead, it prepares both tenants for controlled coexistence while preserving security, compliance, and operational boundaries.

---

## Phase 1 Scope

Phase 1 focuses on identity and tenant relationship readiness.

The scope includes:

- Confirming tenant roles and identity ownership.
- Defining the Microsoft 365 primary tenant direction.
- Preserving the acquired tenant hybrid identity model.
- Reviewing existing B2B collaboration configuration.
- Defining cross-tenant access settings.
- Evaluating Cross-Tenant Synchronization for scoped user provisioning.
- Evaluating B2B Direct Connect for Teams shared channel collaboration.
- Reviewing Conditional Access policies in both tenants.
- Defining MFA and device trust decisions across tenants.
- Creating pilot groups for controlled validation.
- Documenting identity lifecycle governance.

Phase 1 does not include:

- Full tenant consolidation.
- Migration of engineering workloads to Azure.
- Migration of GPU-heavy or data-heavy workloads.
- Broad network access into the acquired environment.
- Final engineering access implementation.
- Application modernization.
- Replacement of the acquired tenant hybrid identity environment.

---

## Scenario Context

The reference architecture assumes two anonymized Microsoft 365 GCC High tenants.

| Area | Tenant A | Tenant B |
|---|---|---|
| Role | Primary Microsoft 365 tenant | Acquired / engineering tenant |
| Cloud | Microsoft 365 GCC High | Microsoft 365 GCC High |
| Identity model | Cloud-focused | Hybrid identity |
| Strategic direction | Main tenant for users, email, Teams, SharePoint, OneDrive, Microsoft 365 Apps, and back-office collaboration | Retained tenant for engineering and DevOps workloads |
| Device model | Devices aligned to primary tenant for back-office users | Engineering devices may remain aligned to acquired tenant |
| Workload direction | Receives Microsoft 365 back-office services | Keeps engineering and DevOps workloads on-premises |
| Cross-tenant need | Users need collaboration and selected access across tenant boundary | Tenant remains operational during coexistence |

Tenant A is the strategic Microsoft 365 tenant for users, email, collaboration, and back-office services.

Tenant B remains operational for engineering and DevOps workloads, including engineering applications, DevOps systems, private cloud or OpenShift environments, virtual machines, GPU-based compute resources, large engineering datasets, SSH and RDP access, and remaining LDAP-dependent services.

---

## Phase 1 Objectives

The objectives of Phase 1 are:

1. Establish a clear identity foundation between both tenants.
2. Define which tenant is the strategic home for Microsoft 365 services.
3. Preserve the acquired tenant hybrid identity model during coexistence.
4. Enable controlled cross-tenant collaboration using Microsoft Entra capabilities.
5. Avoid unmanaged guest access or uncontrolled sharing.
6. Prepare Conditional Access policies for cross-tenant access.
7. Evaluate trust of MFA, compliant device, and hybrid joined device claims.
8. Define pilot users, groups, and test scenarios.
9. Document ownership for identity lifecycle, access governance, and support.
10. Validate GCC High feature availability before implementation decisions are finalized.

---

## Identity Foundation Principles

### 1. Tenant Separation Is Preserved

Both tenants remain logically and administratively separate during the initial integration phase.

Tenant A becomes the primary Microsoft 365 tenant for productivity and collaboration.

Tenant B remains active for engineering and DevOps workloads.

### 2. Microsoft 365 Collaboration Is Separate From Engineering Access

Microsoft 365 cloud collaboration is handled through Microsoft Entra and Microsoft 365 cross-tenant capabilities.

Engineering and DevOps access is handled separately through the approved remote access model and workload-specific controls.

Cross-tenant identity controls do not automatically provide access to on-premises engineering systems.

### 3. Cross-Tenant Access Must Be Explicit

Access between tenants should be explicitly configured, scoped, monitored, and reviewed.

The design should avoid broad tenant-wide access when a smaller user, group, or application scope is sufficient.

### 4. Conditional Access Remains Enforced

Conditional Access policies must continue protecting Microsoft 365 resources.

The architecture should evaluate whether MFA, compliant device, or hybrid joined device claims from one tenant can be trusted by the other tenant.

### 5. Lifecycle Management Must Be Defined Early

User onboarding, offboarding, role changes, guest access, synchronized external identities, and access reviews must be understood before broad rollout.

---

## Phase 1 Design Decisions

### Decision 1: Treat Tenant A as the Primary Microsoft 365 Tenant

Tenant A is the target Microsoft 365 tenant for users, email, Microsoft Teams, SharePoint Online, OneDrive for Business, Microsoft 365 Apps, and back-office collaboration.

This supports the business direction of moving Microsoft 365 productivity and back-office services toward the primary tenant.

### Decision 2: Preserve Tenant B Hybrid Identity

Tenant B continues operating as a hybrid identity tenant.

The on-premises Active Directory environment remains in place because engineering and DevOps workloads continue to depend on the acquired environment.

### Decision 3: Use Cross-Tenant Controls Instead of Full Consolidation

Phase 1 uses cross-tenant access patterns instead of full tenant consolidation.

The primary identity options to evaluate are:

- Microsoft Entra B2B Collaboration.
- Microsoft Entra B2B Direct Connect.
- Cross-Tenant Access Settings.
- Cross-Tenant Synchronization.
- Conditional Access.
- Access reviews and governance capabilities where available.

### Decision 4: Use Pilot Groups Before Broad Rollout

Phase 1 should begin with named pilot groups.

Pilot groups help validate identity, access, Conditional Access, device compliance, and user experience before expanding to larger populations.

### Decision 5: Validate GCC High Availability

Feature availability and behavior must be validated in Microsoft 365 GCC High before final implementation.

The architecture should not assume commercial cloud feature parity.

---

## Identity Workstreams

### Workstream 1: Tenant Role Definition

Define the role of each tenant.

| Item | Tenant A | Tenant B |
|---|---|---|
| Microsoft 365 strategic home | Yes | No |
| Back-office collaboration target | Yes | No |
| Engineering and DevOps operational tenant | No | Yes |
| Hybrid identity retained | No | Yes |
| User migration target for back-office users | Yes | No |
| On-premises engineering workload host | No | Yes |

Deliverables:

- Tenant role decision documented.
- Primary Microsoft 365 tenant confirmed.
- Retained engineering tenant confirmed.
- Tenant boundaries documented.

---

### Workstream 2: User Population Mapping

Define the user populations involved in Phase 1.

User groups may include:

- Back-office users moving toward Tenant A.
- Engineering users remaining aligned to Tenant B.
- Users from Tenant A requiring access to Tenant B resources.
- Users from Tenant B requiring access to Tenant A Microsoft 365 services.
- Pilot users for Microsoft 365 collaboration.
- Pilot users for Conditional Access and device compliance validation.
- Administrators and support users.

Document for each population:

| User Population | Home Tenant | Resource Tenant | Primary Access Need | Notes |
|---|---|---|---|---|
| Back-office users | Tenant B initially, moving toward Tenant A | Tenant A | Microsoft 365 services | Migration path required |
| Engineering users | Tenant B | Tenant A and Tenant B | Engineering workloads plus Microsoft 365 collaboration | Device strategy must be validated |
| Tenant A selected users | Tenant A | Tenant B | Approved engineering resource access | Engineering access handled separately |
| Administrators | Respective tenant | Respective tenant | Administration only | Admin access separate from collaboration access |

Deliverables:

- User populations identified.
- Pilot users selected.
- User migration categories defined.
- Access needs documented.

---

### Workstream 3: Existing Collaboration Review

Review existing cross-tenant collaboration configuration.

Items to review:

- Existing B2B collaboration.
- Existing guest users.
- Existing free/busy or calendar sharing configuration.
- Existing SharePoint and Teams external sharing.
- Existing guest invitation policies.
- Existing guest lifecycle process.
- Existing access reviews.
- Existing tenant allow/block lists.
- Existing Conditional Access policies affecting external users.

Deliverables:

- Existing B2B state documented.
- Existing guest users reviewed.
- Existing external sharing reviewed.
- Risks or cleanup items identified.
- Current state preserved before changes.

---

### Workstream 4: Cross-Tenant Access Settings

Define inbound and outbound cross-tenant access settings between Tenant A and Tenant B.

Cross-tenant settings should be partner-specific rather than relying only on default settings.

Configuration areas to decide:

- Inbound access from Tenant B to Tenant A.
- Outbound access from Tenant A to Tenant B.
- Inbound access from Tenant A to Tenant B.
- Outbound access from Tenant B to Tenant A.
- Allowed users and groups.
- Allowed applications.
- MFA trust.
- Compliant device trust.
- Hybrid joined device trust.
- B2B Collaboration settings.
- B2B Direct Connect settings.

Recommended approach:

- Start with pilot groups.
- Do not allow broad access by default.
- Document each trust decision.
- Confirm whether device claims should be trusted.
- Confirm whether MFA claims should be trusted.
- Validate using sign-in logs and Conditional Access results.

Deliverables:

- Tenant relationship documented.
- Inbound and outbound access model drafted.
- Trust decisions documented.
- Pilot configuration identified.
- Security review completed before enforcement.

---

### Workstream 5: B2B Collaboration Baseline

Use B2B Collaboration for targeted external access scenarios where guest representation is appropriate.

B2B Collaboration may be useful for:

- Targeted Microsoft Teams collaboration.
- SharePoint Online site access.
- OneDrive for Business sharing.
- Application access where guest representation is acceptable.
- Users requiring limited cross-tenant collaboration.

Design rules:

- Use approved groups where possible.
- Avoid unmanaged individual sharing at scale.
- Require MFA through Conditional Access.
- Include guests in access reviews.
- Define owner or sponsor for each guest population.
- Remove guest access when no longer required.

Deliverables:

- B2B guest access model defined.
- Guest lifecycle process documented.
- Access review ownership defined.
- Guest access included in governance.

---

### Workstream 6: B2B Direct Connect Evaluation

Evaluate B2B Direct Connect for Microsoft Teams shared channel collaboration.

B2B Direct Connect may be useful when users need ongoing Teams collaboration across tenants without traditional guest account switching.

Design considerations:

- Validate GCC High support and behavior.
- Confirm Teams shared channel requirements.
- Define allowed users and groups.
- Configure inbound and outbound trust intentionally.
- Keep B2B Direct Connect separate from on-premises engineering access.
- Confirm whether existing B2B guest accounts should remain, be cleaned up, or coexist.

Deliverables:

- B2B Direct Connect use cases identified.
- Teams shared channel pilot defined.
- Cross-tenant access settings reviewed.
- User experience tested.
- Governance impact documented.

---

### Workstream 7: Cross-Tenant Synchronization Evaluation

Evaluate Cross-Tenant Synchronization for scoped provisioning and lifecycle management.

Cross-Tenant Synchronization may be useful when the organization needs repeatable user provisioning between tenants.

Potential use cases:

- Provisioning selected users from one tenant into the other tenant.
- Supporting lifecycle management during coexistence.
- Reducing manual guest account management.
- Supporting structured M&A coexistence.

Design considerations:

- Validate GCC High availability
