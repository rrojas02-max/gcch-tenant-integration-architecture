# Requirements

## Functional Requirements

The architecture must support the following functional requirements:

- Enable secure coexistence between two Microsoft 365 GCC High tenants.
- Establish the acquiring organization tenant as the primary Microsoft 365 tenant for users, email, collaboration, and back-office productivity services.
- Preserve the acquired organization’s hybrid identity and on-premises engineering environment.
- Allow the acquired organization to continue operating its on-premises Active Directory and engineering infrastructure during the coexistence phase.
- Support migration or onboarding of acquired organization back-office users into the primary Microsoft 365 tenant.
- Support secure collaboration between users in both tenants during the transition period.
- Enable controlled access to Microsoft 365 cloud resources, including:
  - Exchange Online
  - Microsoft Teams
  - SharePoint Online
  - OneDrive for Business
  - Microsoft 365 Apps
- Support collaboration without exposing the entire tenant or granting broad access.
- Support access limited to approved users, groups, teams, sites, and resources.
- Provide a scalable model for user onboarding, offboarding, and lifecycle management.
- Support Conditional Access enforcement for cross-tenant access.
- Support strong authentication using MFA where required by policy.
- Support least-privilege access to Microsoft 365 and engineering resources.
- Maintain clear separation between Microsoft 365 cloud collaboration access and on-premises engineering access.
- Preserve access to acquired organization engineering and DevOps workloads that remain on-premises.
- Support access to acquired organization engineering resources, including:
  - Engineering applications
  - DevOps-related systems
  - OpenShift or private cloud environment
  - Virtual machines
  - GPU-based compute resources
  - Large on-premises engineering datasets
  - Systems accessed through SSH or RDP
  - Remaining LDAP-dependent services
- Use the customer’s existing or planned always-on VPN / Zero Trust remote access approach for approved remote access to acquired engineering resources.
- Avoid immediate migration of GPU-heavy, data-heavy, or engineering-specific workloads to Azure unless a separate business case is created.
- Support phased coexistence before any long-term consolidation decision.

---

## Non-Functional Requirements

The architecture must satisfy the following non-functional requirements:

- Maintain tenant separation between organizations during the coexistence phase.
- Avoid immediate full tenant consolidation.
- Avoid immediate migration of acquired organization engineering and DevOps workloads to Azure.
- Reduce operational disruption to the acquired organization’s engineering environment.
- Preserve the acquired organization’s operational model for engineering and DevOps teams.
- Minimize duplicate identities where Microsoft-native identity patterns can support the required access.
- Recognize that some remaining LDAP-dependent services may require special handling.
- Support a phased implementation model.
- Provide auditability for user access and authentication events.
- Align with government cloud security expectations.
- Support future expansion as Microsoft 365 GCC High capabilities mature.
- Minimize unnecessary exposure of engineering systems, applications, and data.
- Provide clear ownership boundaries between organizations.
- Support compliance-driven access governance.
- Maintain a practical balance between security, usability, and operational complexity.
- Keep Microsoft 365 collaboration access and on-premises engineering access as separate design areas.
- Support user experience improvements without weakening security controls.

---

## Identity Requirements

The architecture must address the following identity requirements:

- Define how users from one tenant are represented during the coexistence period.
- Support acquired organization back-office users moving toward the primary Microsoft 365 tenant.
- Preserve the acquired organization’s hybrid identity model for engineering and DevOps workloads.
- Evaluate Microsoft Entra B2B Collaboration for targeted external collaboration.
- Evaluate B2B Direct Connect for Teams shared channel scenarios.
- Evaluate Cross-Tenant Access Settings for trust relationships between the two GCC High tenants.
- Evaluate Cross-Tenant Synchronization for scoped user provisioning and lifecycle management.
- Determine whether MFA claims from one tenant can be trusted by the other tenant.
- Determine whether compliant device claims from one tenant can be trusted by the other tenant.
- Determine whether hybrid-joined device claims from one tenant can be trusted by the other tenant.
- Define user lifecycle management across both tenants.
- Define how access is revoked when users leave, move roles, or no longer require access.
- Define ownership of identity governance between organizations.
- Separate identity requirements for back-office users from identity requirements for engineering users.
- Account for remaining LDAP-dependent services in the acquired environment.
- Avoid assuming that cloud identity alone solves all on-premises engineering access requirements.

---

## Cloud Access Requirements

The architecture must support secure Microsoft 365 cloud collaboration between tenants.

Cloud access requirements include:

- Enable controlled access to Microsoft Teams collaboration spaces.
- Enable controlled access to SharePoint Online sites.
- Enable controlled access to OneDrive for Business content where required.
- Support Exchange Online and Microsoft 365 service access for users moving toward the primary tenant.
- Support collaboration without exposing the entire tenant.
- Restrict access based on approved users, groups, resources, and policies.
- Apply Conditional Access policies to cross-tenant access.
- Define approved tenant relationships.
- Establish governance for guest users and synchronized external identities.
- Support collaboration while preserving compliance boundaries.
- Evaluate B2B Direct Connect for Teams shared channel collaboration.
- Evaluate Cross-Tenant Synchronization for automated and scoped user provisioning.
- Evaluate Multi-Tenant Organization only if long-term coexistence between tenants is expected.
- Review existing B2B collaboration before adding additional cross-tenant access patterns.
- Avoid unnecessary guest account sprawl.

---

## On-Premises Engineering Access Requirements

The architecture must address access to on-premises engineering and DevOps resources in the acquired organization’s environment.

Customer-provided information indicates that the acquired organization already uses remote access when users are not on-premises and plans to use an always-on VPN / Zero Trust remote access approach to make required services available remotely.

On-premises engineering access requirements include:

- Support access to engineering and DevOps resources that remain in the acquired organization’s environment.
- Support access to engineering applications used by the acquired organization.
- Support access to DevOps-related systems.
- Support access to an OpenShift or private cloud environment.
- Support access to virtual machines hosted in the acquired organization environment.
- Support access to GPU-based compute resources.
- Support access to large on-premises engineering datasets.
- Support SSH access patterns.
- Support RDP access patterns.
- Account for remaining LDAP-dependent services.
- Use the customer’s existing or planned always-on VPN / Zero Trust remote access approach for approved remote access scenarios.
- Validate that selected users from the primary tenant can be authorized through the approved remote access path for acquired engineering resources.
- Avoid treating Microsoft 365 cross-tenant collaboration as sufficient for on-premises engineering access.
- Keep Microsoft 365 cloud collaboration access and on-premises engineering access as separate design areas.
- Avoid broad access to the acquired environment where scoped access to approved engineering resources is sufficient.
- Perform workload-level discovery before finalizing access for each engineering system.
- Confirm any additional on-premises protocols, dependencies, or access requirements before final design.

---

## Device and Conditional Access Requirements

The architecture must address device compliance and Conditional Access requirements across tenants.

Device and Conditional Access requirements include:

- Define which tenant manages devices for each user group.
- Support engineering users who may need to keep devices aligned with the acquired organization’s environment.
- Support back-office users who may move toward devices and access patterns aligned with the primary tenant.
- Evaluate whether device compliance claims from one tenant can satisfy Conditional Access requirements in the other tenant.
- Evaluate whether MFA claims from one tenant can satisfy Conditional Access requirements in the other tenant.
- Evaluate whether hybrid-joined device claims can be trusted across tenants.
- Preserve full Microsoft 365 desktop client access where allowed by policy.
- Avoid weakening Conditional Access policies to improve user experience.
- Restrict access from unmanaged or unknown devices where required by policy.
- Validate user experience for:
  - Outlook
  - Teams
  - SharePoint Online
  - OneDrive for Business
  - Microsoft 365 Apps
- Define device ownership, management responsibility, and support responsibility for each user group.

---

## Security Requirements

The architecture must satisfy the following security requirements:

- Enforce least privilege.
- Require MFA for cross-tenant or external access where required by policy.
- Apply Conditional Access policies for cross-tenant access.
- Restrict access to approved users only.
- Limit access to approved applications, teams, sites, workloads, and data locations.
- Monitor sign-ins, guest activity, cross-tenant access, and authentication events.
- Maintain clear identity ownership.
- Protect sensitive government-regulated data.
- Reduce lateral movement risk.
- Avoid unnecessary exposure of on-premises engineering systems.
- Support audit and compliance review.
- Keep administrative access separate from user collaboration access.
- Treat Microsoft 365 collaboration access and engineering resource access as separate security models.
- Review existing B2B guest access to avoid unnecessary account sprawl.
- Define inbound and outbound cross-tenant access policies.
- Limit engineering access to users with valid business need.
- Avoid broad network-level access unless a specific workload requires it and it is approved.

---

## Compliance Requirements

The architecture must consider the following compliance requirements:

- Microsoft 365 GCC High environment boundaries.
- Government cloud security expectations.
- CMMC alignment.
- Controlled Unclassified Information handling considerations.
- Identity governance.
- Access reviews.
- Audit logging.
- Conditional Access enforcement.
- External collaboration controls.
- Cross-tenant access governance.
- Data sharing restrictions.
- Protection of regulated engineering data that remains in the acquired environment.
- Security and compliance review before implementation.
- Validation of third-party or non-Microsoft remote access platforms before use with regulated workloads.
- Preservation of compliance boundaries between the primary Microsoft 365 collaboration environment and the acquired engineering environment.

---

## Operational Requirements

The architecture must be practical for IT teams to operate during the coexistence phase.

Operational requirements include:

- Provide a phased rollout plan.
- Start with a limited pilot group before broad rollout.
- Validate Microsoft 365 collaboration access separately from on-premises engineering access.
- Define which tenant owns each identity, device, workload, and resource.
- Document user onboarding and offboarding processes.
- Document access approval processes for Microsoft 365 collaboration resources.
- Document access approval processes for acquired organization engineering resources.
- Validate the approved always-on VPN / Zero Trust remote access path for engineering resources.
- Define support ownership for:
  - Cross-tenant access issues
  - Conditional Access failures
  - Device compliance issues
  - Remote access issues
  - SSH access issues
  - RDP access issues
  - LDAP-dependent service issues
- Validate Microsoft 365 migration requirements for:
  - Exchange Online
  - Microsoft Teams
  - SharePoint Online
  - OneDrive for Business
  - Microsoft 365 Apps
- Validate engineering access requirements for:
  - Engineering applications
  - DevOps-related systems
  - OpenShift or private cloud resources
  - Virtual machines
  - GPU resources
  - Large engineering datasets
  - LDAP-dependent services
- Define monitoring and escalation procedures.
- Maintain clear architecture decision records.
- Review GCC High feature availability before final implementation.
- Separate Microsoft 365 collaboration support from acquired environment engineering access support.
- Keep the operating model flexible so it can adapt as GCC High capabilities mature.

---

## Out of Scope for Initial Design

The following items are not part of the initial design unless the customer later confirms them as requirements:

- Immediate full tenant consolidation.
- Immediate migration of GPU-heavy engineering workloads to Azure.
- Immediate migration of large engineering datasets to Azure.
- Broad network access from primary tenant users into the acquired environment.
- Exposing all acquired organization resources to primary tenant users.
- Treating traditional file shares as a primary requirement.
- Treating internal web applications as a confirmed requirement.
- Assuming all legacy services can use modern cloud authentication without additional discovery.
- Treating Microsoft 365 cloud collaboration access and on-premises engineering access as the same access pattern.
- Replacing the acquired organization’s hybrid identity environment during the initial coexistence phase.
