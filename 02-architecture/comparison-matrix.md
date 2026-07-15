# Comparison Matrix

This document compares identity, collaboration, device, and access architecture options for a Microsoft 365 GCC High merger and acquisition coexistence scenario.

The goal is to evaluate the architecture patterns that support secure Microsoft 365 collaboration, user migration, device compliance, and access to retained on-premises engineering resources while keeping both environments logically separate during the coexistence phase.

## Scenario Summary

Two organizations need to collaborate after a merger or acquisition while keeping their environments separate during the initial phase.

- **Tenant A**: Primary Microsoft 365 GCC High tenant. This tenant is expected to become the main tenant for users, email, Microsoft Teams, SharePoint Online, OneDrive for Business, Microsoft 365 Apps, and back-office collaboration.
- **Tenant B**: Acquired Microsoft 365 GCC High tenant with hybrid identity. This tenant maintains on-premises Active Directory synchronization and retains engineering and DevOps workloads.
- **Tenant B** continues to host engineering and DevOps resources such as engineering applications, DevOps-related systems, OpenShift or private cloud resources, virtual machines, GPU-based compute resources, and large on-premises engineering datasets.
- Back-office users are expected to move toward Tenant A for Microsoft 365 services.
- Selected users from Tenant A may need access to Tenant B engineering resources.
- Tenant B engineering workloads are not targeted for immediate migration to Azure because of cost, GPU requirements, operational dependencies, and large data volumes.
- Remote access to Tenant B engineering resources should use the customer’s existing or planned VPN / Zero Trust remote access approach.
- Microsoft 365 cloud collaboration and on-premises engineering access must be treated as separate architecture areas.

The architecture must support two primary access surfaces:

1. **Microsoft 365 cloud collaboration access**
2. **On-premises engineering / DevOps resource access**

---

## Cloud Resource Access Options

Cloud resources include Microsoft Teams, SharePoint Online, OneDrive for Business, Exchange Online, Microsoft 365 Apps, and other Microsoft 365 collaboration experiences.

| Option | Commercial Tenant Pattern | GCC High Pattern | When to Use | Key Considerations |
|---|---|---|---|---|
| B2B Guest Collaboration | Invite external users as guests into the resource tenant for access to Teams, SharePoint, OneDrive, or apps. | Valid collaboration pattern to evaluate in GCC High. Tenant external collaboration settings must be reviewed and controlled. | Small groups, targeted collaboration, temporary access, or scenarios where manual guest access is acceptable. | Simple to start, but lifecycle management can become difficult at scale. Existing guest accounts should be reviewed before adding new patterns. |
| B2B Direct Connect | Enable cross-tenant Teams shared channel collaboration without requiring traditional guest account switching. | Evaluate for GCC High same-cloud collaboration. Confirm availability, tenant settings, and policy requirements before using as a design dependency. | Teams shared channel scenarios where users need collaboration across tenants with lower friction. | Useful for Teams collaboration, but does not replace governance, Conditional Access, or on-premises access design. |
| Cross-Tenant Access Settings | Configure inbound and outbound trust between tenants. Define whether MFA, compliant device, or hybrid-joined device claims are trusted from the other tenant. | Important pattern for GCC High tenant-to-tenant control. Must validate exact settings and behavior before implementation. | When Conditional Access, MFA trust, device compliance, and tenant isolation need to be controlled across tenants. | Critical for secure cross-tenant access. Incorrect configuration can either block required access or allow more access than intended. |
| Cross-Tenant Synchronization | Automatically provision users from one tenant into another tenant as B2B users for lifecycle management. | Evaluate for same-cloud GCC High scenarios. Licensing, scope, and exact feature behavior must be validated before implementation. | M&A coexistence scenarios where many users require repeatable and governed access between tenants. | Helps automate lifecycle management. Does not solve on-premises Active Directory, LDAP, SSH, RDP, or engineering resource access by itself. |
| Multi-Tenant Organization | Provides a more connected Microsoft 365 experience across tenants for organizations operating multiple tenants. | Evaluate only if long-term coexistence is expected and GCC High feature availability is confirmed. | Long-term multi-tenant coexistence where user experience across tenants matters. | Helpful for collaboration experience, but not a replacement for security governance, access reviews, device strategy, or application access design. |
| Entitlement Management / Access Packages | Package access to groups, Teams, SharePoint sites, and apps with approval and review workflows. | Validate licensing and GCC High availability before including in the final architecture. | Governed onboarding, temporary access, role-based access, and recurring access reviews. | Useful for access governance. Should be used where supported and operationally sustainable. |

---

## On-Premises Engineering Access Options

On-premises engineering resources in this scenario are not generic file shares or internal web applications by default. Based on the customer-provided scenario, the known scope includes engineering applications, DevOps-related systems, OpenShift or private cloud resources, virtual machines, GPU-based compute resources, large on-premises datasets, SSH/RDP access, and remaining LDAP-dependent services.

The customer already has a remote access model today and is planning to use an always-on VPN / Zero Trust remote access approach for remote users. Therefore, the architecture should not assume a new Microsoft-native app publishing model as the primary design. The access pattern should build around the approved remote access approach and validate each engineering workload.

| Option | Commercial Tenant Pattern | GCC High Pattern | When to Use | Key Considerations |
|---|---|---|---|---|
| Approved VPN / Zero Trust Remote Access Platform | Provide controlled remote access to private resources using VPN, ZTNA, device posture, identity, and policy controls. | Primary on-premises engineering access pattern for this scenario because the customer already uses or plans this model. Validate compliance and security requirements for GCC High / regulated workloads. | Access to Tenant B engineering resources such as SSH, RDP, VMs, DevOps systems, OpenShift/private cloud resources, GPU workloads, and large on-premises datasets. | Should be scoped to approved users and resources. Avoid broad access where resource-level access is sufficient. Must include MFA, device posture, monitoring, and clear support ownership. |
| SSH / RDP Access Through Approved Remote Access Path | Users connect to engineering systems using standard remote protocols through the approved access path. | Applicable to GCC High scenario because SSH and RDP were identified as current access methods. | Engineering users or approved Tenant A users who need access to Tenant B systems. | Requires access control, logging, approval, segmentation, and device compliance controls. Should not be treated as Microsoft 365 collaboration access. |
| LDAP-Dependent Service Handling | Existing services that still depend on LDAP may require special identity handling. | Applicable because some services remain LDAP-dependent. Exact handling must be determined through discovery. | Remaining services not fully moved to SSO or modern authentication. | Microsoft Entra cross-tenant collaboration alone does not satisfy LDAP dependency. Must validate how the service authenticates and how user lifecycle will be managed. |
| Application-by-Application Discovery | Inventory each engineering and DevOps workload and classify access requirements. | Required before final GCC High architecture approval. | Engineering applications, DevOps systems, OpenShift/private cloud, VMs, GPU workloads, LDAP-dependent services, SSH/RDP endpoints. | Prevents wrong assumptions. Determines whether access is identity-based, network-based, protocol-based, or dependent on local AD. |
| Microsoft Entra Application Proxy | Publish internal HTTP/HTTPS web applications through Microsoft Entra. | Not a primary pattern for this scenario unless internal web applications are later confirmed. GCC High availability and fit must be validated before use. | Only if customer confirms internal HTTP/HTTPS web applications that need publishing. | Do not position as a confirmed requirement. It does not replace access to SSH, RDP, OpenShift/private cloud, GPU systems, or non-web engineering workloads. |
| Microsoft Entra Private Access / Global Secure Access | Modern private access / ZTNA-style access to private applications in commercial environments. | Do not assume availability or parity in GCC High. Validate before positioning. | Future option only if available, approved, and aligned with customer security requirements. | Not the current primary design because the customer already has a VPN / Zero Trust access approach. |
| Shadow / Separate AD Accounts | Create or maintain accounts in the resource organization’s AD for services that require local AD authentication. | Possible fallback only if specific LDAP or AD-dependent services require it. | Legacy services that cannot use cross-tenant identity or the approved remote access identity model. | Not a default recommendation. Adds lifecycle, governance, security, and operational overhead. Use only if confirmed by application discovery. |
| Application Modernization | Modernize services to use modern authentication or reduce dependency on legacy access models. | Long-term option if business owners approve and GCC High capabilities support it. | Future roadmap where legacy dependencies create operational or security risk. | Not part of the immediate coexistence design unless customer creates a modernization workstream. |

---

## Commercial vs GCC High Summary

| Area | Commercial Microsoft 365 | Microsoft 365 GCC High |
|---|---|---|
| Microsoft 365 cloud collaboration | Broad feature availability and faster rollout. | Supported collaboration patterns may exist, but feature availability, configuration behavior, and limitations must be validated. |
| Cross-tenant access | Mature model with B2B Collaboration, B2B Direct Connect, Cross-Tenant Access Settings, and Cross-Tenant Synchronization. | Same design concepts may apply, but GCC High availability and behavior must be confirmed before implementation. |
| Teams collaboration | Commercial usually receives features first. | Teams features may have different rollout timing or limitations in GCC High. |
| Device compliance trust | Cross-tenant trust can support smoother Conditional Access experiences when configured correctly. | Must validate trust of MFA, compliant device, and hybrid-joined device claims between GCC High tenants. |
| Microsoft 365 identity lifecycle | Cross-tenant synchronization can support scalable provisioning in commercial scenarios. | Evaluate for scoped GCC High provisioning and verify licensing, limits, and lifecycle behavior. |
| Third-party integrations | Broader availability and faster integration support. | More restricted. Any third-party or non-Microsoft remote access platform requires compliance and data handling validation. |
| On-premises engineering access | May use VPN, ZTNA, app publishing, proxy, or modern private access models. | In this scenario, use the customer’s approved VPN / Zero Trust remote access model as the primary path, then validate each engineering workload. |
| Legacy authentication dependencies | Legacy dependencies may require AD-based solutions or modernization. | Remaining LDAP-dependent services require discovery and controlled handling. Do not assume cloud identity alone solves them. |
| Compliance | Depends on organization and industry. | GCC High boundary, CMMC alignment, CUI handling, auditability, and cross-tenant governance must be considered. |
| Architecture approach | Optimize for usability, lifecycle, and security. | Optimize for security, compliance, validated feature availability, tenant separation, and operational continuity. |

---

## Recommended Architecture Pattern

The recommended approach separates the problem into two workstreams.

### 1. Microsoft 365 Cloud Collaboration Workstream

Use Microsoft identity and collaboration capabilities for Microsoft 365 cloud resources.

Recommended pattern:

1. Configure Cross-Tenant Access Settings between the two GCC High tenants.
2. Define approved inbound and outbound trust settings.
3. Evaluate whether MFA claims, compliant device claims, or hybrid-joined device claims should be trusted across tenants.
4. Use B2B Collaboration for targeted collaboration scenarios.
5. Evaluate B2B Direct Connect for Teams shared channel collaboration.
6. Evaluate Cross-Tenant Synchronization for scoped user provisioning and lifecycle management.
7. Scope access to specific users, groups, Teams, SharePoint sites, OneDrive content, and Microsoft 365 workloads.
8. Apply Conditional Access policies.
9. Use access reviews or entitlement management where available and licensed.
10. Monitor sign-ins, guest activity, cross-tenant access, and access review results.

This workstream handles access to:

- Exchange Online
- Microsoft Teams
- SharePoint Online
- OneDrive for Business
- Microsoft 365 Apps
- Microsoft 365 collaboration services

### 2. On-Premises Engineering Access Workstream

Use the customer’s approved VPN / Zero Trust remote access approach for engineering and DevOps resources that remain in the acquired environment.

Recommended pattern:

1. Keep the engineering and DevOps environment in Tenant B / acquired environment during coexistence.
2. Use the approved VPN / Zero Trust remote access path for remote access.
3. Validate that selected users from Tenant A can be authorized through that remote access path.
4. Limit access to approved engineering resources only.
5. Support SSH and RDP access patterns where required.
6. Account for remaining LDAP-dependent services.
7. Perform workload-level discovery before finalizing access.
8. Keep Microsoft 365 collaboration access separate from on-premises engineering access.
9. Avoid broad network exposure where scoped access is sufficient.
10. Do not position immediate Azure migration for GPU-heavy, data-heavy, or engineering-specific workloads unless a separate business case is created.

This workstream handles access to:

- Engineering applications
- DevOps-related systems
- OpenShift or private cloud resources
- Virtual machines
- GPU-based compute resources
- Large on-premises engineering datasets
- SSH access
- RDP access
- Remaining LDAP-dependent services

---

## Resource Type Decision 
