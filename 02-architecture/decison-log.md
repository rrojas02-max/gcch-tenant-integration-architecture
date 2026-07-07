# Architecture Decision Log

This document captures the key Architecture Decision Records (ADRs) for the GCC High cross-tenant integration reference architecture.

ADRs are used to explain the reasoning behind important architecture choices, including alternatives considered, trade-offs, risks, and implementation considerations.

---

## ADR-001: Keep Both Tenants Separate During Initial Integration

### Status

Under review

### Context

The scenario involves two organizations after a merger or acquisition.

- Tenant A is a cloud-only Microsoft 365 GCC High environment.
- Tenant B is a Microsoft 365 GCC High environment with hybrid identity.
- Tenant B continues to rely on on-premises Active Directory, synchronized users, domain-joined devices, and legacy applications.
- Immediate tenant consolidation is not part of the initial architecture.

The business requires selected users from Tenant A to access resources in Tenant B without disrupting Tenant B’s existing identity, device, application, and compliance model.

### Decision

Keep both tenants separate during the initial integration phase.

The architecture will enable controlled cross-tenant collaboration instead of performing a full tenant-to-tenant migration or identity consolidation.

### Rationale

Keeping the tenants separate reduces operational risk during the acquisition integration period. It allows Tenant B to continue operating its existing hybrid identity environment while enabling controlled access for approved users from Tenant A.

This approach is especially important in regulated environments where identity boundaries, compliance controls, and data access patterns must be carefully reviewed before consolidation.

### Alternatives Considered

#### Full tenant consolidation

Move all users, data, devices, and applications into a single tenant.

Rejected for the initial phase because it would introduce higher migration complexity, greater operational disruption, and potential compliance risk.

#### Create separate duplicate accounts for all users

Create full user accounts in both tenants for all users that need access.

Rejected as the default model because it increases identity management overhead and creates lifecycle management risk.

#### Use only external sharing

Use basic external sharing without formal cross-tenant governance.

Rejected because it may not provide enough control, monitoring, lifecycle management, and policy alignment for a regulated environment.

### Consequences

Positive outcomes:

- Reduces immediate migration risk.
- Preserves Tenant B’s existing hybrid identity model.
- Enables phased integration.
- Supports compliance-driven architecture planning.
- Allows collaboration to start without forcing full consolidation.

Trade-offs:

- Requires cross-tenant governance.
- Requires clear ownership of access lifecycle.
- User experience may be less seamless than a fully consolidated tenant.
- Some scenarios may require duplicate or shadow identities for legacy on-premises applications.

---

## ADR-002: Separate Cloud Collaboration From On-Premises Access

### Status

Under review

### Context

Tenant A users need access to two different types of resources in Tenant B:

1. Cloud resources, such as Teams, SharePoint Online, OneDrive, Microsoft 365 applications, and cloud-hosted applications.
2. On-premises resources, such as file shares, internal web applications, legacy applications, and servers joined to Tenant B’s Active Directory domain.

These two access patterns have different technical requirements.

Cloud collaboration can often be handled through Microsoft Entra External ID, B2B collaboration, cross-tenant access settings, user synchronization, Conditional Access, and governance.

On-premises access may depend on Active Directory, Kerberos, NTLM, SMB, RDP, LDAP, or internal network connectivity.

### Decision

Treat cloud collaboration and on-premises access as separate architecture workstreams.

The architecture will not assume that one identity pattern solves both cloud and on-premises access.

### Rationale

Cloud resource access and on-premises resource access are fundamentally different problems.

Cloud applications can usually rely on modern identity and authorization patterns. On-premises applications may require network access, legacy authentication, an Active Directory user object, group membership, or Kerberos/NTLM support.

By separating the workstreams, the architecture can apply the right pattern to each type of resource.

### Alternatives Considered

#### Use one access model for everything

Apply the same B2B or cross-tenant model to both cloud and on-premises resources.

Rejected because B2B collaboration alone does not solve legacy on-premises authentication requirements.

#### Use VPN for all access

Provide network-level access for all cross-tenant users.

Rejected as the default pattern because it can expose more network access than needed and does not follow a least-privilege application access model.

#### Create local accounts for all resources

Create on-premises accounts for every external user, regardless of resource type.

Rejected because it increases operational overhead and should only be used when legacy application requirements justify it.

### Consequences

Positive outcomes:

- Enables cleaner architecture decisions.
- Avoids overusing VPN.
- Allows modern cloud collaboration patterns where appropriate.
- Allows legacy application access to be evaluated case by case.
- Improves security and governance clarity.

Trade-offs:

- Requires application discovery.
- Requires multiple access patterns.
- May require different support models for cloud and on-premises resources.
- Some users may experience different access flows depending on the application.

---

## ADR-003: Use Cross-Tenant Collaboration Controls for Cloud Resources

### Status

Under review

### Context

Selected users from Tenant A need access to cloud resources in Tenant B.

Examples include:

- Microsoft Teams
- SharePoint Online
- OneDrive
- Microsoft 365 applications
- Cloud-hosted business applications

Both environments are assumed to be Microsoft 365 GCC High tenants.

The design must support secure collaboration while maintaining tenant separation and least-privilege access.

### Decision

Use Microsoft Entra cross-tenant collaboration controls for cloud resource access.

The preferred pattern includes:

- B2B collaboration for external identity representation.
- Cross-tenant access settings for inbound and outbound trust controls.
- Cross-tenant synchronization where scalable lifecycle management is required.
- Conditional Access policies to enforce identity and device requirements.
- Access reviews or entitlement management where available and licensed.

### Rationale

This approach uses Microsoft-native identity and collaboration capabilities to provide controlled access between tenants.

It avoids unnecessary tenant consolidation and supports a phased M&A integration model.

It also provides a better governance foundation than unmanaged external sharing, especially when users need recurring access to resources.

### Alternatives Considered

#### Manual sharing only

Manually share Teams, SharePoint sites, or files with external users as needed.

Rejected as the primary model because it does not scale well and may create inconsistent access governance.

#### Full user account creation in Tenant B

Create full member accounts in Tenant B for Tenant A users.

Rejected as the default model because it creates duplicate identities and increases lifecycle management overhead.

#### Full tenant migration

Move all users and resources into one tenant.

Rejected for the initial phase because it is not aligned with the requirement to keep environments separate.

### Consequences

Positive outcomes:

- Supports secure tenant-to-tenant collaboration.
- Keeps environments logically separate.
- Enables Conditional Access enforcement.
- Improves lifecycle management when synchronization is used.
- Provides a scalable pattern for cloud resource access.

Trade-offs:

- Requires careful configuration of inbound and outbound access policies.
- Requires clear process for user onboarding and offboarding.
- Feature availability and behavior must be validated in GCC High before implementation.
- Cross-tenant access does not automatically solve on-premises application access.

---

## ADR-004: Use Microsoft Entra Application Proxy for Internal Web Applications Where Supported

### Status

Under review

### Context

Tenant A users may need access to Tenant B internal web applications hosted on-premises.

These applications may be reachable only inside Tenant B’s network and may not be directly exposed to the internet.

The architecture should avoid broad network-level access when application-specific access is sufficient.

### Decision

Use Microsoft Entra Application Proxy as the preferred pattern for internal web applications when supported by the application and available in the target environment.

Application Proxy should be considered for HTTP and HTTPS-based internal applications.

### Rationale

Application Proxy provides an application publishing pattern that avoids exposing internal applications directly to the internet and can integrate with Microsoft Entra authentication and Conditional Access.

It is a better fit than VPN when users only need access to specific internal web applications.

### Alternatives Considered

#### Traditional VPN

Provide users with VPN access into Tenant B’s internal network.

Rejected as the preferred model for web applications because it may provide broader network access than required.

#### Publicly expose the internal application

Make the internal application reachable from the internet.

Rejected due to increased security risk and reduced control.

#### Create shadow AD accounts only

Create local Active Directory accounts for external users and require internal network access.

Rejected as the default model for web applications because it may introduce unnecessary identity and operational overhead when Application Proxy can meet the requirement.

### Consequences

Positive outcomes:

- Reduces need for VPN.
- Supports application-specific access.
- Allows Conditional Access to protect access.
- Improves security posture compared to broad network exposure.
- Provides a cleaner pattern for internal web applications.

Trade-offs:

- Applies primarily to web applications.
- May not support every authentication model without additional configuration.
- Does not solve file shares, SMB, RDP, or all legacy protocol requirements.
- GCC High availability and tenant configuration must be validated before implementation.

---

## ADR-005: Use Shadow Accounts Only for Legacy On-Premises Applications That Require Traditional Active Directory Authentication

### Status

Under review

### Context

Some Tenant B on-premises resources may require traditional Active Directory authentication.

Examples include:

- SMB file shares
- Kerberos-based applications
- NTLM-based applications
- LDAP-dependent applications
- RDP access to internal servers
- Legacy line-of-business applications

Tenant A cloud-only users do not naturally exist as users in Tenant B’s on-premises Active Directory forest.

### Decision

Use shadow accounts in Tenant B’s on-premises Active Directory only when required by legacy application authentication or authorization requirements.

Shadow accounts should not be the default model for all cross-tenant access.

### Rationale

Some legacy applications require an Active Directory user object, group membership, Kerberos tickets, NTLM authentication, or domain-based authorization.

For these scenarios, modern cross-tenant cloud identity controls may not be enough.

Shadow accounts provide a practical fallback for legacy resource access, but they must be tightly governed.

### Alternatives Considered

#### Force all users through B2B only

Use only B2B identities for all cloud and on-premises access.

Rejected because B2B identities do not automatically satisfy all legacy Active Directory authentication requirements.

#### Use VPN without shadow accounts

Provide VPN access without creating identities in Tenant B’s Active Directory.

Rejected because network access alone does not provide the required user identity for many Kerberos, NTLM, SMB, or domain-based applications.

#### Modernize all applications first

Modernize every legacy app before enabling access.

Rejected as the only approach because modernization may be a long-term roadmap item and may not meet immediate business needs.

### Consequences

Positive outcomes:

- Supports legacy applications that require AD-based authentication.
- Provides a realistic path for file shares and Kerberos/NTLM scenarios.
- Allows business continuity while modernization is planned.

Trade-offs:

- Creates identity lifecycle management overhead.
- Requires strong governance for account creation, password policy, disabling, and removal.
- Requires access reviews.
- Introduces potential confusion for users with more than one identity.
- Should be limited to applications that truly require it.

---

## ADR-006: Treat VPN as a Controlled Exception, Not the Default Access Pattern

### Status

Under review

### Context

Some on-premises resources may require network-level access.

Examples include:

- Legacy applications that cannot be published through Application Proxy.
- Administrative tools.
- Protocols not supported by application publishing methods.
- Applications requiring direct network connectivity.

Traditional VPN can provide this type of access, but it may also expose users to broader internal network reachability than necessary.

### Decision

Use VPN only as a controlled exception when application-specific publishing, modernization, or more granular access patterns cannot meet the requirement.

VPN access must be limited, monitored, and justified by application requirements.

### Rationale

A default VPN-first approach may conflict with least-privilege and Zero Trust principles.

For this scenario, access should be scoped to the specific business resource whenever possible. VPN should only be used when the application’s technical requirements make it necessary.

### Alternatives Considered

#### VPN for all on-premises access

Provide all Tenant A users with VPN access into Tenant B’s network.

Rejected because it creates broader access than needed and increases security risk.

#### Block all on-premises access

Allow Tenant A users to access only cloud resources.

Rejected because the business requirement includes access to certain on-premises resources.

#### Require full tenant or directory consolidation first

Delay on-premises access until identities and environments are consolidated.

Rejected because the business may require access before consolidation is possible.

### Consequences

Positive outcomes:

- Supports least-privilege design.
- Reduces unnecessary network exposure.
- Forces application owners to justify access requirements.
- Encourages application-by-application discovery.
- Keeps VPN as a fallback rather than the primary model.

Trade-offs:

- Some applications may still require VPN.
- Requires network segmentation and monitoring.
- Requires device compliance and strong authentication controls.
- Requires clear operational ownership.

---

## ADR-007: Validate GCC High Feature Availability Before Final Design

### Status

Under review

### Context

Microsoft 365 GCC High environments do not always have the same feature availability, rollout timing, or integration options as commercial Microsoft 365 tenants.

Architecture patterns that are valid in commercial environments may require validation before being included in a GCC High implementation plan.

This is especially important for:

- Cross-tenant collaboration capabilities.
- Microsoft Teams collaboration experiences.
- Microsoft Entra governance features.
- Application publishing features.
- Private access and Zero Trust Network Access capabilities.
- Third-party integrations.
- Compliance-related controls.

### Decision

Before final implementation, validate feature availability and supportability in the target GCC High tenants.

Do not assume commercial Microsoft 365 feature parity.

### Rationale

Government cloud environments have additional compliance, data boundary, and feature rollout considerations.

Validating availability reduces the risk of designing an architecture that depends on a feature that is not available, not generally available, or behaves differently in GCC High.

### Alternatives Considered

#### Design using commercial cloud assumptions

Use the commercial Microsoft 365 feature set as the baseline.

Rejected because this can lead to unsupported or unavailable design choices in GCC High.

#### Avoid advanced Microsoft Entra features entirely

Use only basic sharing, VPN, and local accounts.

Rejected because it may miss secure and scalable Microsoft-native options that are available or supported.

#### Wait for full feature parity

Delay all architecture decisions until GCC High reaches commercial feature parity.

Rejected because the business needs a practical architecture now.

### Consequences

Positive outcomes:

- Reduces implementation risk.
- Improves stakeholder trust.
- Supports accurate customer guidance.
- Encourages consulting-led architecture validation.
- Helps separate recommended patterns from assumptions.

Trade-offs:

- Requires additional validation effort.
- May require revisiting architecture decisions as Microsoft roadmap evolves.
- Some design decisions may need temporary fallback options.
- Implementation sequencing may depend on tenant licensing and feature availability.

---

## ADR-008: Use a Phased Implementation Model

### Status

Under review

### Context

The integration touches multiple areas:

- Identity
- Tenant-to-tenant collaboration
- Conditional Access
- Teams and SharePoint governance
- On-premises application access
- Legacy authentication
- Compliance controls
- User lifecycle management

Implementing everything at once would increase risk and make troubleshooting difficult.

### Decision

Use a phased implementation approach.

Recommended phases:

1. Identity and tenant relationship foundation.
2. Cloud resource access pilot.
3. On-premises application discovery.
4. Internal web application publishing.
5. Legacy access handling with shadow accounts or controlled VPN.
6. Governance, monitoring, and access review maturity.
7. Long-term modernization or tenant consolidation planning.

### Rationale

A phased approach allows the organization to validate assumptions, reduce risk, and gain user feedback before scaling access.

It also supports regulated environments where security and compliance teams may need to approve each access pattern.

### Alternatives Considered

#### Big bang integration

Enable all access patterns at once.

Rejected because it increases risk, reduces visibility, and makes troubleshooting difficult.

#### Cloud-only integration

Only enable cloud resource access and ignore on-premises resources.

Rejected because the business requirement includes on-premises resource access.

#### Wait for full consolidation

Delay collaboration until a future tenant or directory consolidation project.

Rejected because the business needs collaboration before consolidation.

### Consequences

Positive outcomes:

- Lower implementation risk.
- Easier troubleshooting.
- Cleaner stakeholder alignment.
- Better security review process.
- Faster path to initial collaboration.
- More accurate long-term roadmap.

Trade-offs:

- Some users may not get access to all resources immediately.
- Requires roadmap communication.
- Requires tracking dependencies between phases.
- Requires ongoing governance.

---

## Summary of Key Decisions

| ADR | Decision | Outcome |
|---|---|---|
| ADR-001 | Keep both tenants separate during initial integration. | Reduces migration risk and preserves existing hybrid environment. |
| ADR-002 | Separate cloud collaboration from on-premises access. | Prevents one-size-fits-all architecture mistakes. |
| ADR-003 | Use cross-tenant collaboration controls for cloud resources. | Enables governed Microsoft 365 collaboration. |
| ADR-004 | Use Application Proxy for internal web apps where supported. | Reduces need for VPN for web-based apps. |
| ADR-005 | Use shadow accounts only where legacy AD authentication requires it. | Supports legacy workloads while limiting identity sprawl. |
| ADR-006 | Treat VPN as a controlled exception. | Reduces broad network exposure. |
| ADR-007 | Validate GCC High feature availability before final design. | Avoids commercial cloud assumption risk. |
| ADR-008 | Use phased implementation model. | Reduces complexity and improves governance. |

---

## Architecture Principle

The architecture prioritizes secure collaboration, tenant separation, least privilege, and compliance-aware design.

Cloud access should use Microsoft Entra collaboration and governance capabilities where available.

On-premises access should be evaluated application by application, based on authentication type, protocol requirements, sensitivity, and GCC High feature availability.
