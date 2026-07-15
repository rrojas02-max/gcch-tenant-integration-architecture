# Constraints

## Environment Constraints

This architecture is designed for a Microsoft 365 GCC High coexistence scenario.

The following environment constraints apply:

- Both organizations operate in Microsoft 365 GCC High.
- The acquiring organization is primarily cloud-focused.
- The acquiring organization is expected to become the primary Microsoft 365 tenant for users, email, and Microsoft 365 services.
- The acquired organization uses a hybrid identity model.
- The acquired organization maintains on-premises Active Directory.
- The acquired organization maintains on-premises engineering and DevOps workloads.
- The acquired organization’s current engineering environment must remain operational.
- Immediate tenant consolidation is not part of the initial design.
- Immediate migration of on-premises engineering workloads to Azure is not part of the initial design.
- Both tenants must remain logically and administratively separate during the coexistence phase.
- The architecture must support coexistence while Microsoft 365 back-office services move toward the primary tenant.

---

## Identity Constraints

The design must account for the following identity constraints:

- The acquiring organization is expected to become the primary identity and Microsoft 365 services tenant.
- The acquired organization still depends on its hybrid identity environment.
- The acquired organization uses on-premises Active Directory and synchronized identities.
- The acquired organization’s engineering and DevOps environment currently uses authentication tied to the acquired tenant.
- Most authentication in the acquired environment is based on single sign-on.
- A small number of services still depend on LDAP.
- A cloud-only identity model may not be sufficient for all on-premises engineering access scenarios.
- Identity lifecycle management must be carefully controlled across both tenants.
- Back-office users moving to the primary tenant may need a different identity and access model than engineering users who continue to access acquired environment resources.
- The design must avoid unnecessary duplicate accounts unless a specific application dependency requires a separate legacy access pattern.

---

## GCC High Constraints

GCC High environments can have different feature availability compared to commercial Microsoft 365 environments.

The architecture must account for the following GCC High constraints:

- Some Microsoft Entra features may not be available at the same time as commercial cloud.
- Some cross-tenant features may have limited availability or different behavior in GCC High.
- Some Microsoft security and identity capabilities may have delayed rollout in GCC High.
- Some preview features in commercial cloud may not be available in GCC High.
- Third-party integrations may require additional validation before being used in a regulated environment.
- Feature availability must be confirmed before implementation.
- Architecture decisions should avoid depending on unsupported or unavailable services.
- Any recommendation involving cross-tenant collaboration, cross-tenant synchronization, device trust, or remote access must be validated for GCC High before implementation.

---

## Collaboration Constraints

The design must support collaboration while preserving security boundaries.

Collaboration constraints include:

- External or cross-tenant users should not receive broad access to the other tenant.
- Access should be limited to approved users, groups, teams, sites, and resources.
- Microsoft 365 collaboration should be governed using approved sharing and access policies.
- Tenant relationships must be explicitly configured and controlled.
- Guest access and cross-tenant access must be reviewed on a recurring basis.
- Collaboration must not bypass compliance or security requirements.
- User experience should be improved where possible, but not at the expense of security.
- Existing B2B collaboration should be reviewed before introducing additional cross-tenant patterns.
- B2B Direct Connect should be evaluated for Teams shared channel scenarios if that improves collaboration without creating unnecessary guest account sprawl.
- Cross-Tenant Synchronization should be evaluated only for scoped user provisioning and lifecycle management use cases.
- Multi-Tenant Organization should be considered only if long-term coexistence between both tenants is expected.

---

## On-Premises Engineering Access Constraints

Accessing on-premises engineering resources is the most complex part of the scenario.

The following constraints apply:

- On-premises engineering and DevOps workloads are expected to remain in the acquired environment.
- The acquired environment includes engineering applications such as SolidWorks and MATLAB.
- The acquired environment includes DevOps-related systems.
- The acquired environment includes an OpenShift or private cloud environment.
- The acquired environment includes virtual machines.
- The acquired environment includes GPU-based compute resources.
- The acquired environment includes large on-premises datasets.
- The customer indicated that the on-premises engineering environment is not planned for immediate cloud migration because of cost, GPU requirements, and data volume.
- Access to engineering resources may require SSH.
- Access to engineering resources may require RDP.
- Some remaining services may depend on LDAP.
- Not all cloud-based identity patterns solve on-premises access requirements.
- Application-by-application discovery is required before finalizing the access model.
- Traditional file services are not a primary driver in this scenario because the customer indicated that file services have largely moved to SharePoint.
- On-premises engineering access must be designed separately from Microsoft 365 cloud collaboration access.
- The remote access method for engineering workloads must be validated with the customer’s current access platform and security requirements.
- Broad access to the acquired environment should be avoided unless specifically required by the workload.

---

## Device and Conditional Access Constraints

Device compliance and Conditional Access are key constraints in this scenario.

The following constraints apply:

- Both environments may use Conditional Access policies that require compliant devices for full Microsoft 365 desktop client access.
- Engineering users may need to keep devices aligned with the acquired environment because engineering workloads remain there.
- Back-office users may move toward devices and access patterns aligned with the primary tenant.
- The architecture must evaluate whether device compliance claims from one tenant can be trusted by the other tenant.
- The architecture must evaluate whether MFA claims from one tenant can be trusted by the other tenant.
- The architecture must evaluate whether hybrid-joined or compliant device claims can satisfy Conditional Access policies across tenants.
- The design must avoid breaking access to Outlook, Teams, SharePoint, OneDrive, or Microsoft 365 Apps during user migration.
- The design must avoid weakening Conditional Access requirements to improve user experience.
- Access from unmanaged or unknown devices should be restricted where required by policy.
- Device ownership and management responsibility must be clearly defined for each user group.

---

## Security Constraints

The design must follow a conservative security model.

Security constraints include:

- Least privilege must be enforced.
- Access must be explicitly granted, not broadly inherited.
- Conditional Access must be used where supported.
- MFA must be required for external or cross-tenant access where supported.
- Device trust should be considered where supported.
- Access from unmanaged or unknown devices should be restricted where required.
- Sensitive data access must be controlled and auditable.
- Administrative access must remain separate from user collaboration access.
- Security monitoring should include both tenants where possible.
- Cross-tenant users should not receive unnecessary network-level access.
- Engineering access must be limited to users with a valid business need.
- Back-office collaboration access and engineering resource access should be treated as different security models.
- Existing B2B guest access should be reviewed to avoid unnecessary account sprawl.
- Cross-tenant access policies should define inbound and outbound trust explicitly.

---

## Compliance Constraints

The architecture must respect government cloud and regulated data requirements.

Compliance constraints include:

- The design must consider GCC High security and compliance boundaries.
- The design must consider CMMC alignment where applicable.
- The design must consider CUI handling requirements where applicable.
- Data sharing must be limited to approved business purposes.
- Access to regulated data must be reviewed and justified.
- Audit logs should be retained according to organizational policy.
- Teams, SharePoint, OneDrive, and application access must follow governance rules.
- Cross-tenant access must not weaken compliance boundaries.
- Any implementation must be reviewed against the organization’s security and compliance policies.
- Third-party access platforms or integrations must be validated against the organization’s compliance requirements before being used for regulated workloads.
- Engineering data and workloads that remain in the acquired environment must continue to be protected according to the acquired organization’s compliance requirements.

---

## Operational Constraints

The design must be practical for IT teams to operate.

Operational constraints include:

- The solution should avoid unnecessary complexity.
- Implementation should be phased.
- A pilot group should be used before broad rollout.
- Support teams must understand which tenant owns each identity, device, workload, and resource.
- User onboarding and offboarding must be documented.
- Access reviews must be operationally sustainable.
- Application owners must validate authentication and access requirements.
- Engineering workload owners must validate access requirements for SSH, RDP, DevOps systems, OpenShift or private cloud resources, VMs, GPU resources, and LDAP-dependent services.
- Microsoft 365 workload owners must validate migration requirements for Exchange Online, Teams, SharePoint Online, OneDrive, and Microsoft 365 Apps.
- The solution should be flexible enough to adapt as GCC High capabilities mature.
- The operating model must separate Microsoft 365 collaboration support from acquired environment engineering access support.
- Any access model must include clear ownership for troubleshooting cross-tenant access, Conditional Access failures, device compliance failures, and remote access issues.

---

## Design Assumptions

The following assumptions are used for this reference architecture:

- The organizations want to remain in separate tenants for the initial phase.
- The acquiring organization is expected to become the primary Microsoft 365 tenant.
- The acquired organization’s hybrid identity environment will remain intact during the coexistence phase.
- Back-office Microsoft 365 services are expected to move toward the primary tenant.
- Engineering and DevOps workloads are expected to remain in the acquired environment.
- Only a subset of users requires access to acquired environment engineering resources.
- Not all acquired organization resources need to be exposed to the acquiring organization.
- Cloud collaboration and on-premises engineering access should be treated as separate design areas.
- Microsoft-native identity and access controls are preferred where available and supported in GCC High.
- Existing or approved remote access capabilities may be required for on-premises engineering access.
- Remaining LDAP-dependent services may require separate handling.
- Final implementation depends on application discovery, licensing, device management decisions, compliance review, and GCC High feature availability.

---

## Out of Scope for Initial Design

The following items are not part of the initial design unless the customer later confirms them as requirements:

- Immediate full tenant consolidation.
- Immediate migration of GPU-heavy engineering workloads to Azure.
- Immediate migration of large engineering datasets to Azure.
- Broad network access from all primary tenant users into the acquired environment.
- Exposing all acquired environment resources to the primary tenant.
- Designing access for traditional file shares as a primary requirement, since file services were described as largely moved to SharePoint.
- Treating Microsoft 365 collaboration access and on-premises engineering access as the same access pattern.
- Assuming all legacy services can use modern cloud authentication without further discovery.
