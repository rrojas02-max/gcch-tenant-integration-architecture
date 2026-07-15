# Business Context

## Scenario Overview

This repository documents an anonymized reference architecture for a merger and acquisition (M&A) coexistence scenario involving two organizations operating in Microsoft 365 GCC High.

The acquiring organization, referred to in this case study as **Contoso Federal**, is expected to become the primary Microsoft 365 GCC High tenant for users, identity, email, and collaboration services. Contoso Federal is primarily a cloud-focused Microsoft 365 GCC High environment.

The acquired organization, referred to in this case study as **Fabrikam Defense**, also operates in Microsoft 365 GCC High but maintains a hybrid identity and infrastructure model. Fabrikam Defense uses on-premises Active Directory, synchronized identities, managed or hybrid-joined devices, and on-premises engineering and DevOps workloads.

The updated business direction is not to immediately consolidate every workload into a single environment. Instead, the target direction is to move Microsoft 365 user productivity and back-office collaboration toward the primary tenant while preserving the acquired organization’s on-premises engineering environment.

This creates a coexistence architecture where:

- The primary Microsoft 365 tenant becomes the main identity, email, and collaboration tenant.
- The acquired hybrid tenant remains operational for engineering and DevOps workloads.
- Selected users require access across tenant boundaries.
- On-premises engineering workloads remain in place because of GPU, data volume, application, and operational dependencies.
- Device compliance and Conditional Access must continue to protect access to Microsoft 365 applications.
- The architecture must support secure coexistence before any long-term consolidation decisions are made.

---

## Updated Scenario Summary

| Area | Primary Tenant | Acquired / Engineering Tenant |
|---|---|---|
| Cloud environment | Microsoft 365 GCC High | Microsoft 365 GCC High |
| Identity model | Cloud-focused / primary future tenant | Hybrid identity with on-premises Active Directory synchronization |
| Strategic role | Main tenant for Microsoft 365 users, email, collaboration, and back-office functions | Retained tenant for engineering, DevOps, and on-premises infrastructure |
| Approximate user population | About 1,000 users before migration | About 140 users |
| Licensing pattern | G5 for users with devices, F3 + F5 add-on for web-only or outsourced users, some E3 for larger OneDrive requirements | G5 for all users |
| Workload direction | Receives back-office functions and Microsoft 365 services | Keeps engineering and DevOps workloads on-premises |
| Device scope | Only users with managed devices are expected to access engineering resources | Engineering users may keep devices managed or hybrid-joined in the acquired environment |

---

## Business Problem

After the acquisition, the organization needs to transition users and collaboration services into the primary GCC High tenant while keeping the acquired engineering environment operational.

The previous assumption was that selected users from the primary tenant would simply need access to resources in the acquired tenant. The updated scenario is more nuanced:

1. **Back-office users from the acquired organization need to move into the primary tenant**

   These users are expected to consume Microsoft 365 services from the primary tenant, including:

   - Exchange Online
   - Microsoft Teams
   - SharePoint Online
   - OneDrive for Business
   - Microsoft 365 productivity services
   - Common business collaboration workloads

2. **Engineering and DevOps workloads remain in the acquired environment**

   The acquired organization maintains specialized workloads that are not planned for immediate cloud migration, including:

   - Engineering applications
   - DevOps platforms
   - OpenShift or private cloud environments
   - Virtual machines
   - GPU-based compute clusters
   - Large on-premises datasets
   - Internal infrastructure systems

3. **Selected users from the primary tenant need access to acquired engineering resources**

   Some users from the primary tenant may need access to engineering resources that remain in the acquired environment, such as:

   - RDP or SSH access to internal systems
   - Engineering virtual machines
   - DevOps environments
   - Internal web applications
   - Residual LDAP-bound or legacy applications

4. **The organization must avoid unnecessary cloud migration of engineering workloads**

   The engineering environment is not a good candidate for immediate cloud migration because of cost, GPU requirements, large data volumes, and existing operational dependencies.

The architecture must therefore support secure access and coexistence, not a big-bang migration.

---

## Business Goals

The architecture must support the following goals:

- Establish the primary tenant as the main Microsoft 365 GCC High environment for users, email, Teams, SharePoint, OneDrive, and collaboration services.
- Preserve the acquired tenant and on-premises environment for engineering and DevOps workloads.
- Enable secure collaboration between two GCC High tenants during the coexistence period.
- Support the migration or onboarding of acquired back-office users into the primary tenant.
- Allow selected users to access engineering resources that remain in the acquired environment.
- Avoid migrating GPU-heavy, data-heavy, or engineering-specific workloads to Azure unless a separate business case is created.
- Maintain Conditional Access and device compliance requirements for full Microsoft 365 desktop client access.
- Minimize duplicate accounts where possible.
- Recognize that some legacy or LDAP-bound applications may still require traditional identity patterns.
- Use Microsoft Entra cross-tenant capabilities for cloud collaboration and identity lifecycle management where supported.
- Use an approved Zero Trust Network Access or remote access platform for on-premises resource access where appropriate.
- Maintain CMMC, CUI, DFARS, and GCC High security boundary considerations.
- Provide a phased architecture that supports coexistence first and future consolidation later.

---

## Why This Architecture Matters

M&A scenarios in regulated industries are rarely simple tenant migrations. In this case, the business wants to consolidate Microsoft 365 collaboration and back-office functions into the primary tenant, but the engineering environment has strong reasons to remain separate.

The most important architectural distinction is:

> Microsoft 365 collaboration can move toward the primary GCC High tenant, but engineering and DevOps workloads may need to remain in the acquired on-premises environment for operational, performance, cost, and data-volume reasons.

This means the solution should not be designed as a single big-bang migration. Instead, it should be designed as a controlled coexistence model with clearly separated access paths for cloud collaboration and on-premises engineering access.

---

## Updated Key Architecture Considerations

This scenario requires evaluating several identity, device, collaboration, and on-premises access patterns.

### Cloud Collaboration and Identity

The cloud collaboration layer must evaluate:

- Microsoft Entra B2B Collaboration
- Microsoft Entra B2B Direct Connect for Teams shared channel scenarios
- Microsoft Entra Cross-Tenant Access Settings
- Microsoft Entra Cross-Tenant Synchronization
- Multi-Tenant Organization considerations if coexistence is expected to last for an extended period
- Guest account lifecycle management
- Existing B2B guest accounts and whether they should remain, be cleaned up, or coexist with newer cross-tenant patterns

The goal is to support collaboration without over-permissioning users or creating unnecessary account sprawl.

---

### Device and Conditional Access

Both tenants may have Conditional Access policies that restrict full Microsoft 365 desktop client access to compliant devices.

This creates an important design question:

> Can a device managed in one tenant satisfy Conditional Access requirements when accessing Microsoft 365 resources in another tenant?

The architecture must evaluate:

- Which tenant manages each device population
- Whether engineering devices should remain managed or hybrid-joined in the acquired tenant
- Whether back-office users should receive devices managed in the primary tenant
- Whether Cross-Tenant Access Settings should trust MFA, compliant device, or hybrid-joined device claims from the other tenant
- How users will access full Microsoft 365 desktop clients after mailbox and collaboration workloads move to the primary tenant

This is especially important for engineering users who may keep their devices in the acquired environment but need access to Microsoft 365 services in the primary tenant.

---

### On-Premises Engineering Access

The on-premises access layer must be treated separately from Microsoft 365 cloud collaboration.

Engineering and DevOps users may need access to:

- SSH endpoints
- RDP endpoints
- Engineering virtual machines
- DevOps platforms
- OpenShift or private cloud environments
- GPU clusters
- Internal web applications
- LDAP-bound applications
- Legacy systems that depend on the acquired organization’s Active Directory

For this layer, Microsoft Entra cross-tenant collaboration alone is not enough. Access must also consider:

- Network path
- Device posture
- Remote access platform
- Application authentication method
- Legacy Active Directory dependency
- Whether applications can be modernized to SAML or OIDC
- Whether shadow or linked Active Directory accounts are still required

---

### Migration and Coexistence Planning

The solution must clearly separate resources into three categories:

1. **Resources that should move to the primary tenant**

   Examples:

   - Mailboxes
   - Teams collaboration
   - SharePoint Online
   - OneDrive
   - Back-office Microsoft 365 services
   - Business productivity collaboration

2. **Resources that should remain in the acquired environment**

   Examples:

   - Engineering workloads
   - DevOps workloads
   - GPU clusters
   - Large datasets
   - On-premises virtual machines
   - Specialized engineering applications

3. **Resources that require coexistence access**

   Examples:

   - Shared Teams collaboration
   - SharePoint access during transition
   - Remote access to on-premises engineering systems
   - Applications that still depend on the acquired identity environment

This classification helps prevent the architecture from forcing the wrong migration path.

---

## Resources in Scope

### Cloud Resources Moving or Consolidating Toward the Primary Tenant

- Exchange Online mailboxes
- Microsoft Teams
- SharePoint Online
- OneDrive for Business
- Microsoft 365 productivity services
- Back-office collaboration workloads
- Identity and access patterns for migrated users

---

### Cloud Collaboration Resources During Coexistence

- Teams collaboration between users in both tenants
- SharePoint and OneDrive sharing
- Calendar or availability collaboration where required
- Cross-tenant access policies
- Guest and synchronized external identity lifecycle controls
- Teams shared channels if B2B Direct Connect is selected

---

### On-Premises Resources Remaining in the Acquired Environment

- Engineering applications
- DevOps platforms
- OpenShift or private cloud environments
- Virtual machines
- GPU clusters
- Large engineering datasets
- Internal systems reachable through SSH or RDP
- Residual LDAP-bound services
- Print services if still required

---

### Resources Explicitly Not Positioned for Immediate Cloud Migration

- GPU-heavy engineering workloads
- Very large engineering datasets
- On-premises DevOps and engineering systems that are cost-
