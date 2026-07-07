# Comparison Matrix

This document compares common identity and access architecture options for a cross-tenant Microsoft 365 integration scenario.

The goal is to evaluate what would normally be available in a commercial Microsoft 365 environment and then identify what must be validated or adjusted for Microsoft 365 GCC High.

## Scenario Summary

Two organizations need to collaborate after a merger or acquisition while keeping their environments separate.

- **Tenant A**: Cloud-only Microsoft 365 environment.
- **Tenant B**: Microsoft 365 GCC High environment with hybrid identity.
- **Tenant B** maintains on-premises Active Directory, synced users, domain-joined devices, and legacy applications.
- Selected users from Tenant A need access to Tenant B resources.

The architecture must support two types of access:

1. Access to cloud resources.
2. Access to on-premises resources.

---

## Cloud Resource Access Options

Cloud resources include Microsoft Teams, SharePoint Online, OneDrive, Microsoft 365 applications, and cloud-hosted business applications.

| Option | Commercial Tenant Pattern | GCC High Pattern | When to Use | Key Considerations |
|---|---|---|---|---|
| B2B Guest Collaboration | Invite external users as guests into the resource tenant. | Expected baseline pattern for cross-tenant collaboration. Must validate tenant external collaboration settings. | Small groups, simple collaboration, manual access scenarios. | Simple to start, but manual lifecycle management can become difficult at scale. |
| Cross-Tenant Access Settings | Configure inbound and outbound access rules between tenants. Trust MFA and device claims from another tenant when appropriate. | Available conceptually for managing collaboration boundaries, but settings must be validated in GCC High. | When organizations need controlled cross-tenant access with Conditional Access alignment. | Requires careful security design. Incorrect defaults can allow too much or block required access. |
| Cross-Tenant Synchronization | Automatically create, update, and remove B2B users in the target tenant. | Supported for same-cloud Azure Government scenarios, but licensing and exact feature behavior must be validated. | M&A scenarios where many users need repeatable access into another tenant. | Improves lifecycle management. Does not automatically solve on-premises Active Directory authentication needs. |
| Multi-Tenant Organization | Provides a more seamless collaboration experience across tenants, especially for Microsoft Teams and Microsoft 365 collaboration. | Must validate current GCC High availability and feature parity before using as a design dependency. | Organizations that plan to operate multiple tenants for an extended period. | Helpful for user experience, but not a replacement for security governance or application access design. |
| Entitlement Management / Access Packages | Package access to apps, groups, and sites with approval and review workflows. | Should be validated based on licensing and GCC High feature availability. | Access governance, temporary access, role-based collaboration groups. | Useful for access reviews and controlled onboarding/offboarding. |

---

## On-Premises Resource Access Options

On-premises resources include file shares, internal web applications, legacy line-of-business applications, RDP, LDAP, Kerberos, NTLM, and domain-joined server resources.

| Option | Commercial Tenant Pattern | GCC High Pattern | When to Use | Key Considerations |
|---|---|---|---|---|
| Microsoft Entra Application Proxy | Publish internal web applications securely through Microsoft Entra without requiring inbound firewall exposure. | Strong candidate for internal web applications in GCC High, but exact tenant capability must be validated. | Internal HTTP/HTTPS web applications. | Good fit for web apps. Not a full replacement for VPN or private network access to all protocols. |
| Microsoft Entra Private Access / Global Secure Access | Modern Zero Trust Network Access approach for private applications and non-web protocols. | Must validate GCC High availability before proposing. Do not assume commercial parity. | Modern replacement for VPN-style access to private apps. | Strong modern pattern, but GCC High feature availability and licensing must be verified. |
| Shadow Accounts in On-Premises AD | Create or maintain accounts in the resource organization’s AD for external users that need legacy access. | Practical fallback for legacy Kerberos/NTLM applications in GCC High. | Legacy apps, file shares, SMB, RDP, Kerberos-dependent applications. | Operational overhead. Requires lifecycle management, password/account governance, and security review. |
| Traditional VPN | Provide network-level access into the resource environment. | Possible but should be treated carefully due to broad network exposure. | When applications require full network connectivity and cannot be modernized quickly. | Higher risk. Must be paired with least privilege, device compliance, MFA, segmentation, and monitoring. |
| Application Modernization | Modernize legacy apps to support modern authentication and cloud-native access patterns. | Long-term recommended direction when feasible. | Strategic modernization roadmap. | May require application refactoring, budget, and business owner alignment. |

---

## Commercial vs GCC High Summary

| Area | Commercial Microsoft 365 | Microsoft 365 GCC High |
|---|---|---|
| Cloud collaboration | Broad feature availability and faster rollout. | Supported collaboration patterns exist, but feature availability and behavior must be validated. |
| Cross-tenant access | Mature model with B2B, cross-tenant access settings, and synchronization options. | Same concepts may apply, but government cloud availability and limitations must be confirmed. |
| Teams collaboration | Commercial usually receives features first. | Some Teams features may lag or behave differently due to government cloud restrictions. |
| Third-party integrations | Broader availability. | More restricted. Third-party tools require additional compliance and data handling review. |
| On-premises web app access | Application Proxy is a common pattern. | Application Proxy is a strong candidate, but implementation must be validated in the tenant. |
| Private network access | Entra Private Access / Global Secure Access may be considered. | Must validate availability before including in final architecture. |
| Legacy app access | Shadow AD accounts or VPN may still be used where needed. | Often more relevant because modern access options may be limited or require validation. |
| Compliance | Depends on organization and industry. | CMMC, CUI, ITAR, FedRAMP, and government data boundary considerations may apply. |
| Architecture approach | Optimize for usability, lifecycle, and security. | Optimize for security, compliance, tenant isolation, and validated feature availability. |

---

## Recommended Architecture Pattern

The recommended approach separates the problem into two workstreams.

### 1. Cloud Resource Access

Use Microsoft identity collaboration capabilities for cloud resources.

Recommended pattern:

1. Configure cross-tenant access settings between tenants.
2. Use B2B collaboration or cross-tenant synchronization for approved users.
3. Scope access to specific users, groups, Teams, SharePoint sites, and applications.
4. Apply Conditional Access policies.
5. Use access reviews or entitlement management where available.
6. Monitor sign-ins and guest activity.

This handles access to:

- Microsoft Teams
- SharePoint Online
- OneDrive
- Microsoft 365 applications
- Cloud-hosted applications

### 2. On-Premises Resource Access

Use an application-by-application access model.

Recommended pattern:

| Resource Type | Preferred Pattern | Fallback Pattern |
|---|---|---|
| Internal web application | Microsoft Entra Application Proxy | VPN |
| File share / SMB | Shadow AD account with controlled access | VPN with segmentation |
| Kerberos / NTLM application | Shadow AD account | Application modernization |
| RDP / server access | Privileged access workflow and segmentation | VPN with strict controls |
| Modernized application | Cloud-native authentication | Application Proxy if still hosted internally |

---

## Design Decision Guidance

Use this decision guidance during architecture workshops.

### If the resource is cloud-based

Use cross-tenant identity collaboration patterns.

Start with:

- B2B collaboration
- Cross-tenant access settings
- Cross-tenant synchronization
- Conditional Access
- Access governance

### If the resource is an internal web application

Evaluate Microsoft Entra Application Proxy.

Good fit when:

- The application is HTTP or HTTPS.
- The application can be published through a connector.
- The organization wants to avoid broad VPN access.
- Conditional Access should protect access.

### If the resource requires Kerberos or NTLM

Evaluate shadow accounts or modernization.

Important questions:

- Does the application require an AD user object?
- Does it use Kerberos constrained delegation?
- Does it require group membership in on-premises AD?
- Can the application be modernized to support SAML, OAuth, or OpenID Connect?

### If the resource requires network-level access

Treat VPN as a controlled exception.

Use only when:

- Application Proxy does not support the scenario.
- Private Access is not available or not approved.
- The application cannot be modernized in the short term.
- Network segmentation and monitoring are in place.

---

## Key Architecture Principle

Cloud collaboration and on-premises access should not be treated as the same problem.

Cloud collaboration can usually be solved with Microsoft Entra B2B, cross-tenant access settings, cross-tenant synchronization, Conditional Access, and governance.

On-premises access depends heavily on the application authentication model, network requirements, Active Directory dependencies, and GCC High feature availability.

---

## Final Recommendation

For this scenario, the recommended design is a phased architecture:

1. Establish secure tenant-to-tenant collaboration for cloud resources.
2. Validate cross-tenant access settings and user lifecycle requirements.
3. Use B2B or cross-tenant synchronization for approved external users.
4. Apply Conditional Access and least privilege access controls.
5. Inventory on-premises applications.
6. Classify on-premises applications by authentication type.
7. Use Application Proxy for internal web applications where supported.
8. Use shadow AD accounts only for legacy applications that require traditional AD authentication.
9. Avoid broad VPN access unless required and justified.
10. Revisit architecture as GCC High feature availability evolves.

This approach allows collaboration to move forward while preserving security, compliance boundaries, and operational control.
