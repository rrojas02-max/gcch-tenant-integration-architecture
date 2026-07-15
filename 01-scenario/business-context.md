# Business Context

## Scenario Overview

This repository documents an anonymized reference architecture for a merger and acquisition (M&A) coexistence scenario involving two organizations operating in Microsoft 365 GCC High.

The acquiring organization, referred to in this case study as **Contoso Federal**, is expected to become the primary Microsoft 365 GCC High tenant for users, identity, email, and collaboration services. Contoso Federal is primarily a cloud-focused Microsoft 365 GCC High environment.

The acquired organization, referred to in this case study as **Fabrikam Defense**, also operates in Microsoft 365 GCC High but maintains a hybrid identity and infrastructure model. Fabrikam Defense uses on-premises Active Directory, synchronized identities, and on-premises engineering and DevOps workloads.

The updated business direction is not to immediately consolidate every workload into a single environment. Instead, the direction is to move Microsoft 365 user productivity and back-office collaboration toward the primary tenant while preserving the acquired organization’s on-premises engineering environment.

This creates a coexistence architecture where:

- The primary Microsoft 365 tenant becomes the main tenant for users, email, and Microsoft 365 collaboration services.
- The acquired hybrid tenant remains operational for engineering and DevOps workloads.
- Back-office functions are expected to move toward the primary tenant.
- Engineering and DevOps workloads remain in the acquired environment.
- Selected users require access across tenant boundaries.
- On-premises engineering workloads remain in place because of GPU, data volume, cost, and operational dependencies.
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
| Licensing pattern | G5 for users with devices, F3 + F5 add-on for web-only or outsourced users, and some E3 users for larger OneDrive requirements | G5 for all users |
| Workload direction | Receives back-office functions and Microsoft 365 services | Keeps engineering and DevOps workloads on-premises |
| Device scope | Users with managed devices are expected to access the acquired engineering environment when required | Engineering users may continue using devices associated with the acquired environment |

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

   - Engineering applications such as SolidWorks and MATLAB
   - DevOps-related systems
   - OpenShift or private cloud environment
   - Virtual machines
   - GPU-based compute clusters
   - Large on-premises datasets

3. **Selected users from the primary tenant need access to acquired engineering resources**

   Some users from the primary tenant may need access to engineering resources that remain in the acquired environment, such as:

   - SSH access to engineering systems
   - RDP access to engineering systems
   - Engineering virtual machines
   - DevOps-related systems
   - OpenShift or private cloud environment
   - Remaining LDAP-dependent services

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
- Recognize that some remaining LDAP-dependent services may still require legacy identity considerations.
- Use Microsoft Entra cross-tenant capabilities for cloud collaboration and identity lifecycle management where supported.
- Use the existing or approved remote access platform for on-premises resource access where appropriate.
- Maintain GCC High security and compliance boundary considerations.
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

This creates an important
