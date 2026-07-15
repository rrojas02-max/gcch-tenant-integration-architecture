# Architecture Decision Log

This document captures the key Architecture Decision Records (ADRs) for the GCC High cross-tenant integration reference architecture.

ADRs explain the reasoning behind important architecture choices, including alternatives considered, trade-offs, risks, and implementation considerations.

This document is fully anonymized. Tenant names, organization names, domains, user names, meeting details, and customer-identifying information are intentionally replaced with generic placeholders.

---

## ADR-001: Keep Both Tenants Separate During Initial Integration

### Status

Accepted

### Context

The scenario involves two organizations after a merger or acquisition.

- Tenant A is the primary Microsoft 365 GCC High tenant.
- Tenant A is expected to become the main tenant for users, email, Microsoft Teams, SharePoint Online, OneDrive for Business, Microsoft 365 Apps, and back-office collaboration.
- Tenant B is a Microsoft 365 GCC High tenant with hybrid identity.
- Tenant B maintains on-premises Active Directory synchronization and continues to operate engineering and DevOps workloads.
- Tenant B hosts engineering applications, DevOps-related systems, OpenShift or private cloud resources, virtual machines, GPU-based compute resources, large on-premises engineering datasets, and remaining LDAP-dependent services.
- Immediate tenant consolidation is not part of the initial architecture.
- Immediate migration of Tenant B engineering workloads to Azure is not part of the initial architecture.

The business direction is to move Microsoft 365 productivity and back-office collaboration toward Tenant A while preserving Tenant B’s engineering and DevOps environment.

### Decision

Keep both tenants separate during the initial integration phase.

The architecture will enable controlled coexistence and cross-tenant collaboration instead of performing immediate full tenant consolidation.

### Rationale

Keeping the tenants separate reduces operational risk during the acquisition integration period.

This approach allows Tenant A to become the strategic Microsoft 365 tenant while Tenant B continues operating its engineering and DevOps environment without disruption.

This is important because Tenant B’s engineering workloads have operational, GPU, cost, and data-volume dependencies that make immediate cloud migration inappropriate for the initial design.

In a regulated GCC High scenario, tenant separation also helps preserve compliance boundaries while architecture, identity, device, application, and access patterns are evaluated.

### Alternatives Considered

#### Full tenant consolidation

Move all users, data, devices, applications, and workloads into a single tenant.

Rejected for the initial phase because it would introduce higher migration complexity, greater operational disruption, and potential compliance risk.

#### Immediate migration of engineering workloads to Azure

Move Tenant B engineering and DevOps workloads into Azure as part of the M&A integration.

Rejected for the initial phase because the engineering workloads are GPU-heavy, data-heavy, operationally sensitive, and not positioned for immediate cloud migration.

#### Create duplicate accounts for all users

Create full user accounts in both tenants for all users that need access.

Rejected as the default model because it increases identity management overhead and creates lifecycle management risk. Separate accounts should only be considered if a specific workload dependency requires them.

#### Use only unmanaged external sharing

Use basic external sharing without formal cross-tenant governance.

Rejected because it may not provide enough control, monitoring, lifecycle management, and policy alignment for a regulated GCC High environment.

### Consequences

Positive outcomes:

- Reduces immediate migration risk.
- Preserves Tenant B’s hybrid engineering environment.
- Supports phased Microsoft 365 migration toward Tenant A.
- Supports compliance-driven architecture planning.
- Allows collaboration to start without forcing full consolidation.
- Avoids disruption to engineering and DevOps workloads.

Trade-offs:

- Requires cross-tenant governance.
- Requires clear ownership of access lifecycle.
- Requires separate support models for Microsoft 365 collaboration and engineering access.
- User experience may be less seamless than a fully consolidated environment.
- Some legacy or LDAP-dependent services may require special handling after discovery.

---

## ADR-002: Separate Microsoft 365 Cloud Collaboration From On-Premises Engineering Access

### Status

Under review

### Context

The scenario includes two different access surfaces.

1. Microsoft 365 cloud collaboration access.
2. On-premises engineering and DevOps resource access.

Microsoft 365 cloud collaboration includes:

- Exchange Online
- Microsoft Teams
- SharePoint Online
- OneDrive for Business
- Microsoft 365 Apps
- Back-office collaboration workloads

On-premises engineering access includes:

- Engineering applications
- DevOps-related systems
- OpenShift or private cloud resources
- Virtual machines
- GPU-based compute resources
- Large on-premises engineering datasets
- SSH access
- RDP access
- Remaining LDAP-dependent services

These access patterns have different technical requirements and should not be treated as the same problem.

### Decision

Treat Microsoft 365 cloud collaboration and on-premises engineering access as separate architecture workstreams.

The architecture will not assume that one identity pattern solves both Microsoft 365 collaboration and retained engineering workload access.

### Rationale

Microsoft 365 cloud collaboration can be handled using Microsoft Entra and Microsoft 365 cross-tenant collaboration capabilities.

On-premises engineering access depends on the customer’s approved remote access path, workload requirements, device posture, protocol requirements, and remaining identity dependencies such as LDAP.

Separating the workstreams avoids overextending Microsoft 365 B2B or cross-tenant synchronization into scenarios they do not solve by themselves.

### Alternatives Considered

#### Use one access model for everything

Apply the same B2B or cross-tenant model to both Microsoft 365 and on-premises engineering resources.

Rejected because Microsoft 365 cross-tenant collaboration does not automatically provide access to engineering systems, SSH/RDP endpoints, private cloud resources, GPU workloads, or LDAP-dependent services.

#### Treat all engineering access as Microsoft 365 collaboration

Assume engineering resource access can be solved through Teams, SharePoint, or guest collaboration.

Rejected because engineering access includes on-premises systems and protocols outside standard Microsoft 365 collaboration.

#### Consolidate the engineering environment first

Delay access until Tenant B workloads are migrated or consolidated.

Rejected because the business needs coexistence while Tenant B engineering and DevOps workloads remain operational.

### Consequences

Positive outcomes:

- Creates cleaner architecture boundaries.
- Avoids one-size-fits-all access design.
- Allows Microsoft 365 collaboration to move forward independently.
- Allows engineering workload access to be evaluated with the correct access model.
- Improves security, support, and governance clarity.

Trade-offs:

- Requires multiple access patterns.
- Requires workload-level discovery for engineering systems.
- Requires separate ownership for Microsoft 365 collaboration and engineering access.
- Some users may have different access flows depending on the resource.

---

## ADR-003: Use Microsoft Entra Cross-Tenant Collaboration Controls for Microsoft 365 Cloud Resources

### Status

Accepted

### Context

Back-office users are expected to move toward Tenant A for Microsoft 365 services.

During coexistence, users may require tenant-to-tenant collaboration across Microsoft 365 workloads such as Teams, SharePoint Online, OneDrive for Business, Exchange Online, and Microsoft 365 Apps.

Both tenants are Microsoft 365 GCC High tenants.

The design must support secure collaboration while maintaining tenant separation, Conditional Access enforcement, and least-privilege access.

### Decision

Use Microsoft Entra cross-tenant collaboration controls for Microsoft 365 cloud resource access.

The preferred cloud collaboration pattern includes:

- B2B Collaboration for targeted external collaboration.
- B2B Direct Connect for Teams shared channel scenarios where appropriate.
- Cross-Tenant Access Settings for inbound and outbound trust controls.
- Cross-Tenant Synchronization where scoped user provisioning and lifecycle management are required.
- Conditional Access policies to enforce identity and device requirements.
- Access reviews or entitlement management where available, licensed, and supported.

### Rationale

This approach uses Microsoft-native identity and collaboration capabilities to provide controlled access between tenants.

It avoids immediate tenant consolidation and supports a phased M&A coexistence model.

It also provides a better governance foundation than unmanaged external sharing, especially when users require recurring access to Microsoft 365 resources.

### Alternatives Considered

#### Manual sharing only

Manually share Teams, SharePoint sites, OneDrive content, or files with external users as needed.

Rejected as the primary model because it does not scale well and may create inconsistent governance.

#### Full user account creation in the other tenant

Create full member accounts in the other tenant for users who need collaboration access.

Rejected as the default model because it creates duplicate identities and increases lifecycle management overhead.

#### Full tenant migration first

Move all users and resources into one tenant before enabling collaboration.

Rejected for the initial phase because the architecture must support coexistence before long-term consolidation decisions.

### Consequences

Positive outcomes:

- Supports secure tenant-to-tenant Microsoft 365 collaboration.
- Keeps environments logically separate.
- Enables Conditional Access enforcement.
- Improves lifecycle management where synchronization is used.
- Supports scalable access patterns for Microsoft 365 resources.

Trade-offs:

- Requires careful configuration of inbound and outbound access policies.
- Requires clear onboarding and offboarding processes.
- Requires validation of GCC High feature availability and behavior.
- Does not solve on-premises engineering access by itself.

---

## ADR-004: Use the Approved VPN / Zero Trust Remote Access Path for Engineering Resources

### Status

Accepted

### Context

Tenant B retains engineering and DevOps workloads on-premises.

The known engineering access scope includes:

- Engineering applications
- DevOps-related systems
- OpenShift or private cloud resources
- Virtual machines
- GPU-based compute resources
- Large on-premises engineering datasets
- SSH access
- RDP access
- Remaining LDAP-dependent services

The customer already has remote access for users when they are not on-premises and plans to use an always-on VPN / Zero Trust remote access approach for remote users to access required services.

### Decision

Use the customer’s approved VPN / Zero Trust remote access path as the primary access pattern for retained on-premises engineering and DevOps resources.

The design must validate that selected users from Tenant A can be authorized through this approved access path for approved Tenant B engineering resources.

### Rationale

The customer already has or plans a remote access model for on-premises engineering workloads.

Using the approved access path reduces unnecessary architecture change and aligns with the customer’s current direction.

This is more appropriate for SSH, RDP, VMs, private cloud resources, GPU workloads, and large data sets than trying to force those workloads into Microsoft 365 cloud collaboration patterns.

### Alternatives Considered

#### Use Microsoft 365 B2B for engineering resource access

Use B2B or cross-tenant synchronization as the primary way to access engineering resources.

Rejected because Microsoft 365 cross-tenant collaboration does not automatically provide access to SSH, RDP, private cloud resources, GPU systems, or LDAP-dependent services.

#### Introduce a new Microsoft-native publishing model as the primary path

Use Microsoft Entra Application Proxy or another publishing technology as the default design for on-premises engineering access.

Rejected as the primary model because internal web applications were not confirmed as the main requirement and the known engineering access scope includes SSH/RDP, DevOps systems, private cloud resources, VMs, GPU workloads, and LDAP-dependent services.

#### Migrate engineering workloads to Azure first

Move engineering and DevOps workloads to Azure before enabling access.

Rejected because immediate migration is not aligned with the workload constraints, cost, GPU dependency, and data-volume considerations.

### Consequences

Positive outcomes:

- Aligns with the customer’s existing or planned access model.
- Supports SSH and RDP access patterns.
- Supports retained engineering and DevOps workloads.
- Avoids unnecessary redesign of the engineering environment.
- Keeps engineering access separate from Microsoft 365 collaboration.

Trade-offs:

- Requires careful access scoping.
- Requires integration with identity, MFA, device posture, and monitoring controls.
- Requires support ownership for remote access issues.
- Requires workload-level discovery before broad rollout.
- Does not replace Microsoft 365 cross-tenant governance for cloud collaboration.

---

## ADR-005: Treat LDAP-Dependent Services as Special Cases Requiring Discovery

### Status

Under review

### Context

Tenant B uses SSO for most services, but some services remain LDAP-dependent.

These services may require identity or authorization patterns that are different from Microsoft 365 cloud collaboration.

The architecture should avoid assuming that Microsoft Entra B2B, B2B Direct Connect, or Cross-Tenant Synchronization automatically solves LDAP-dependent access.

### Decision

Treat remaining LDAP-dependent services as special cases that require workload-level discovery.

Do not make shadow accounts, duplicate accounts, or local AD account creation the default model unless discovery confirms that a specific service requires it.

### Rationale

LDAP-dependent services may require special handling, but the exact pattern depends on the service.

The solution must first determine:

- Which services still depend on LDAP.
- Whether LDAP is used for authentication, authorization, group lookup, or another purpose.
- Whether users require an on-premises AD object.
- Whether the existing SSO or remote access model can satisfy the requirement.
- How lifecycle management will work when users leave or change roles.

This avoids over-engineering the design or introducing unnecessary identity sprawl.

### Alternatives Considered

#### Assume all legacy services require shadow accounts

Create local AD accounts for all external users who may need access.

Rejected because this may create unnecessary identity sprawl and lifecycle management overhead before the real application requirements are known.

#### Assume Microsoft Entra cross-tenant identity solves LDAP

Use Microsoft 365 collaboration identity controls only.

Rejected because LDAP-dependent services may still require application-specific or directory-specific handling.

#### Modernize all LDAP-dependent services before access

Require all services to move to modern authentication before enabling access.

Rejected as the only approach because modernization may be a longer-term roadmap item and may not meet immediate coexistence needs.

### Consequences

Positive outcomes:

- Avoids unnecessary shadow-account creation.
- Supports accurate workload-specific design.
- Reduces identity governance risk.
- Keeps the initial architecture flexible.
- Helps prioritize modernization later.

Trade-offs:

- Requires discovery.
- Some services may need temporary legacy access handling.
- User experience may vary by workload.
- Support teams must understand which services require special handling.

---


## ADR-006: Validate GCC High Feature Availability Before Final Design

### Status

Accepted

### Context

Microsoft 365 GCC High environments do not always have the same feature availability, rollout timing, or integration options as commercial Microsoft 365 tenants.

Architecture patterns that are valid in commercial environments may require validation before being included in a GCC High implementation plan.

This is especially important for:

- Cross-tenant collaboration capabilities
- B2B Direct Connect
- Cross-Tenant Access Settings
- Cross-Tenant Synchronization
- Teams collaboration experiences
- Microsoft Entra governance features
- Device compliance trust
- Third-party remote access integrations
- Compliance and audit controls

### Decision

Validate feature availability and supportability in the target GCC High tenants before final implementation.

Do not assume commercial Microsoft 365 feature parity.

### Rationale

Government cloud environments have additional compliance, data boundary, and rollout considerations.

Validating availability reduces the risk of designing an architecture that depends on a feature that is not available, not generally available, or behaves differently in GCC High.

### Alternatives Considered

#### Design using commercial cloud assumptions

Use the commercial Microsoft 365 feature set as the baseline.

Rejected because this can lead to unsupported or unavailable design choices in GCC High.

#### Avoid advanced Microsoft Entra features entirely

Use only basic sharing, local accounts, and remote access.

Rejected because it may miss secure and scalable Microsoft-native options that are available or supported.

#### Wait for full feature parity

Delay all architecture decisions until GCC High reaches full commercial parity.

Rejected because the business needs a practical coexistence architecture now.

### Consequences

Positive outcomes:

- Reduces implementation risk.
- Improves stakeholder trust.
- Supports accurate architecture guidance.
- Separates recommended patterns from assumptions.
- Helps create safer implementation sequencing.

Trade-offs:

- Requires additional validation effort.
- Some design decisions may need fallback patterns.
- Architecture may need to be revisited as GCC High capabilities evolve.
- Implementation sequencing may depend on licensing and feature availability.

---

## ADR-007: Use a Phased Implementation Model

### Status

Under review

### Context

The integration touches multiple architecture domains:

- Identity
- Tenant-to-tenant collaboration
- Cross-Tenant Access Settings
- Conditional Access
- Device compliance
- Teams and SharePoint governance
- Microsoft 365 user migration
- On-premises engineering access
- SSH/RDP access
- LDAP-dependent services
- Remote access platform governance
- Monitoring and access reviews
- Compliance controls

Implementing everything at once would increase risk and make troubleshooting difficult.

### Decision

Use a phased implementation approach.

Recommended phases:

1. Identity and tenant relationship foundation.
2. Microsoft 365 cloud collaboration pilot.
3. Back-office Microsoft 365 migration planning.
4. Device and Conditional Access validation.
5. Engineering access discovery.
6. Approved VPN / Zero Trust remote access validation.
7. SSH/RDP and engineering workload access pilot.
8. LDAP-dependent service handling.
9. Governance, monitoring, and access review maturity.
10. Long-term modernization or consolidation planning.

### Rationale

A phased approach allows the organization to validate assumptions, reduce risk, and collect user feedback before scaling access.

It also supports regulated environments where security and compliance teams may need to approve each access pattern.

This approach keeps Microsoft 365 collaboration moving forward while protecting Tenant B engineering and DevOps operations.

### Alternatives Considered

#### Big bang integration

Enable all access patterns at once.

Rejected because it increases risk, reduces visibility, and makes troubleshooting harder.

#### Cloud-only integration

Only enable Microsoft 365 cloud collaboration and ignore engineering access.

Rejected because selected users may require access to retained Tenant B engineering resources.

#### Wait for full consolidation

Delay collaboration until a future tenant or directory consolidation project.

Rejected because the business needs coexistence before consolidation.

### Consequences

Positive outcomes:

- Lower implementation risk.
- Easier troubleshooting.
- Better stakeholder alignment.
- Faster path to initial Microsoft 365 collaboration.
- Better security review process.
- More accurate long-term roadmap.

Trade-offs:

- Some users may not get access to all resources immediately.
- Requires roadmap communication.
- Requires dependency tracking across phases.
- Requires ongoing governance and access reviews.

---

## ADR-008: Avoid Immediate Azure Migration for Engineering and DevOps Workloads

### Status

Accepted

### Context

Tenant B retains engineering and DevOps workloads that include GPU-based compute resources, large on-premises engineering datasets, private cloud or OpenShift resources, virtual machines, and specialized engineering applications.

The customer direction is not to migrate these workloads to Azure as part of the initial coexistence design.

### Decision

Do not position immediate Azure migration as the default architecture for Tenant B engineering and DevOps workloads.

The initial architecture will focus on secure coexistence and controlled access to retained engineering resources.

### Rationale

The engineering environment has workload-specific constraints such as GPU requirements, large data volumes, operational dependencies, and cost considerations.

Moving these workloads to Azure would require a separate modernization, migration, cost, and performance assessment.

The current architecture goal is to enable Microsoft 365 coexistence and secure access, not to redesign the engineering platform.

### Alternatives Considered

#### Migrate engineering workloads to Azure immediately

Rejected because it is not aligned with the current business direction and may introduce cost, performance, and operational risk.

#### Block access to retained engineering workloads

Rejected because selected users may require access to engineering resources during coexistence.

#### Consolidate engineering workloads into the primary tenant first

Rejected because the acquired engineering environment must remain operational during the initial phase.

### Consequences

Positive outcomes:

- Preserves critical engineering operations.
- Avoids unnecessary migration risk.
- Allows Microsoft 365 collaboration to progress independently.
- Keeps the architecture aligned with customer constraints.
- Supports future evaluation if a separate cloud migration business case is created.

Trade-offs:

- Requires maintaining access to retained on-premises engineering resources.
- Requires remote access governance.
- Requires clear operational ownership.
- Long-term modernization may still be needed.

---

## Summary of Key Decisions

| ADR | Decision | Outcome |
|---|---|---|
| ADR-001 | Keep both tenants separate during initial integration. | Reduces migration risk and preserves the acquired hybrid engineering environment. |
| ADR-002 | Separate Microsoft 365 cloud collaboration from on-premises engineering access. | Prevents one-size-fits-all architecture mistakes. |
| ADR-003 | Use Microsoft Entra cross-tenant collaboration controls for Microsoft 365 cloud resources. | Enables governed Microsoft 365 collaboration while preserving tenant separation. |
| ADR-004 | Use the approved VPN / Zero Trust remote access path for engineering resources. | Aligns on-premises engineering access with the customer’s existing or planned access model. |
| ADR-005 | Treat LDAP-dependent services as special cases requiring discovery. | Avoids unnecessary identity sprawl and supports workload-specific design. |
| ADR-006 | Validate GCC High feature availability before final design. | Avoids commercial cloud assumption risk. |
| ADR-007 | Use a phased implementation model. | Reduces complexity, improves governance, and supports safer rollout. |
| ADR-008 | Avoid immediate Azure migration for engineering and DevOps workloads. | Preserves operational continuity and respects GPU, data, cost, and workload constraints. |

---

## Architecture Principle

The architecture prioritizes secure collaboration, tenant separation, least privilege, compliance-aware design, and operational continuity.

Microsoft 365 cloud access should use Microsoft Entra collaboration and governance capabilities where available and validated for GCC High.

On-premises engineering access should use the approved VPN / Zero Trust remote access path and be evaluated workload by workload based on protocol requirements, identity dependencies, device posture, sensitivity, monitoring, and operational ownership.

The architecture must not assume that Microsoft 365 cross-tenant collaboration automatically solves retained engineering, DevOps, SSH/RDP, GPU, private cloud, or LDAP-dependent access scenarios.
