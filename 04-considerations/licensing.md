# Licensing Considerations

## Purpose

This document captures licensing considerations for a GCC High multi-tenant integration scenario where one tenant becomes the primary user and Microsoft 365 tenant, while another tenant continues to host technical resources such as engineering workloads, DevOps systems, virtual machines, GPU workloads, and large datasets.

The purpose of this document is to identify which licensing areas must be validated before implementation. It does not provide final pricing or contractual licensing confirmation.

## Customer Licensing Context

Based on the discovery discussion, the current licensing model includes different user profiles:

| User Type | Current Understanding |
|---|---|
| Users with assigned corporate devices | Licensed with a higher-tier government plan |
| Web-only or limited users | Licensed with a frontline or lower-cost model plus security add-ons |
| Users requiring larger storage | Some users may have a different plan for larger OneDrive capacity |
| Technical users in the resource tenant | Licensed with a higher-tier government plan due to equipment and workload requirements |
| Users expected to access resource tenant workloads | Limited to users with approved devices or higher-tier licensing |

The customer also clarified that web-only users are not expected to access the resource tenant environment. Access to resource tenant workloads should be limited to users with the right license, device, and business requirement. 【1-d5e696】【2-ee6b43】

## Licensing Principles

The recommended licensing approach is to license users based on their actual role, device ownership, security requirements, and access needs.

Do not license all users the same way unless the business requirement applies to all users.

Recommended principles:

1. License by persona.
2. Keep web-only users out of scope for resource tenant access.
3. License technical users based on device, security, and workload requirements.
4. Validate licensing before enabling cross-tenant synchronization.
5. Validate licensing before relying on Conditional Access, device compliance, access reviews, or governance features.
6. Keep Microsoft 365 licensing separate from third-party VPN licensing.
7. Keep resource-side application licensing separate from Microsoft 365 licensing.

## User Persona Licensing Model

| Persona | Expected Access | Licensing Guidance |
|---|---|---|
| Standard productivity users | Email, Teams, SharePoint, OneDrive, Office apps | Assign standard productivity licensing based on user role |
| Web-only users | Browser-based access only | Keep out of scope for resource tenant access unless business need changes |
| Back-office migrated users | Primary tenant productivity services | Validate productivity, security, and device requirements |
| Technical users | Engineering, DevOps, VMs, admin tools, restricted systems | Validate higher-tier security, device compliance, endpoint protection, and resource access needs |
| Administrative users | Tenant administration, identity, security, privileged access | Validate advanced identity governance, privileged access, audit, and compliance capabilities |
| External or synchronized users | Cross-tenant app or resource access | Validate B2B, cross-tenant access, lifecycle, and target resource licensing |

## Feature Licensing Areas to Validate

| Capability | Why It Matters | Licensing Validation Needed |
|---|---|---|
| Conditional Access | Used to enforce access based on user, app, device, location, or risk | Validate Microsoft Entra Conditional Access licensing |
| MFA | Required for strong authentication | Validate included authentication capabilities |
| Device Compliance | Required when access depends on managed or compliant devices | Validate Intune or endpoint management licensing |
| Cross-Tenant Access Settings | Required to control inbound and outbound trust between tenants | Validate Microsoft Entra licensing and configuration rights |
| Trust MFA Claims | Used to reduce repeated MFA prompts across tenants | Validate cross-tenant access configuration and licensing |
| Trust Device Claims | Used when a device managed in one tenant must satisfy access in another tenant | Validate device compliance and cross-tenant trust support |
| Cross-Tenant Synchronization | Automates B2B user provisioning and deprovisioning | Validate Microsoft Entra licensing before enabling |
| Cross-Tenant Group Synchronization | Automates group synchronization scenarios | Validate Microsoft Entra Governance or Microsoft Entra Suite licensing if required |
| Access Reviews | Helps remove stale access | Validate identity governance licensing |
| Privileged Identity Management | Reduces standing admin access | Validate premium identity licensing |
| Defender capabilities | Supports endpoint, identity, email, and cloud app protection | Validate security suite coverage by user group |
| Purview capabilities | Supports labels, DLP, audit, retention, and eDiscovery | Validate compliance suite coverage by user group |
| Zero Trust VPN | Provides secure access to resource tenant workloads | Validate separately with the customer’s VPN provider |
| Resource-side applications | Engineering tools, DevOps systems, VMs, databases, legacy apps | Validate separately from Microsoft 365 licensing |

Microsoft documentation states that Conditional Access is Microsoft’s Zero Trust policy engine and uses signals such as user, device, application, location, and risk to enforce access decisions. 【3-a7585f】 Microsoft’s Conditional Access product page states that Conditional Access capabilities are available with Microsoft Entra ID P1. 【4-92bfe2】

## Cross-Tenant Synchronization Licensing

Cross-Tenant Synchronization should only be used if the customer needs automated lifecycle management for B2B users.

It should not be treated as a full tenant migration tool.

Cross-Tenant Synchronization automates the creation, update, and deletion of B2B collaboration users and groups across tenants. 【5-06554e】 Microsoft documentation states that the source tenant requires Microsoft Entra ID P1 or P2 for cross-tenant user synchronization, and Microsoft Entra ID Governance or Microsoft Entra Suite for cross-tenant group synchronization. 【6-914f1f】

### Recommended Position

Use Cross-Tenant Synchronization only if:

- The customer needs automated B2B user creation.
- The customer needs automated B2B user removal.
- Manual guest invitation is not scalable.
- User lifecycle management is a priority.
- The licensing requirement is confirmed.

Do not enable Cross-Tenant Synchronization only because multiple tenants exist.

## Conditional Access and Device Licensing

Both tenants may use Conditional Access policies that require compliant devices for full Microsoft 365 client access.

This means licensing must be validated for:

- Conditional Access
- Device compliance
- Endpoint management
- MFA
- Cross-tenant access settings
- Trust of MFA claims
- Trust of compliant device claims

If a device remains managed in the resource tenant but the user accesses Microsoft 365 services in the primary tenant, both policy behavior and licensing must be validated.

## Resource Tenant Access Licensing

Access to the resource tenant should not be assigned broadly.

Only approved users should receive:

- Resource tenant access
- VPN access
- SSH or RDP access
- DevOps access
- Engineering application access
- Virtual machine access
- Administrative access

The customer clarified that technical workloads remain in the resource tenant and that remote access uses an existing Zero Trust VPN model with standard protocols such as SSH and RDP. 【1-d5e696】

## Zero Trust VPN Licensing

The existing Zero Trust VPN is part of the access model, but it should be treated separately from Microsoft 365 licensing.

Microsoft 365 licensing helps cover identity, productivity, security, compliance, and device controls.

Zero Trust VPN licensing should be validated separately with the customer’s VPN provider or internal network/security team.

Recommended validation:

- Which users need VPN access?
- Is VPN licensed per user, per device, per application, or another model?
- Are device posture checks included?
- Are MFA integrations included?
- Are access logs and reporting included?
- Are additional licenses required for external or migrated users?

## Licensing Scope Recommendation

The recommended scoped approach is:

| User Group | Licensing Recommendation |
|---|---|
| Web-only users | Keep licensed for web-only productivity needs; do not include in resource tenant access scope |
| Standard productivity users | License for Microsoft 365 productivity and baseline security needs |
| Migrated back-office users | License for full productivity experience in the primary tenant |
| Technical users | Validate higher-tier licensing for security, compliance, device, and endpoint requirements |
| Users accessing resource tenant workloads | Validate Microsoft 365 license, device compliance license, VPN license, and resource-side permissions |
| Administrators | Validate premium identity governance, privileged access, audit, and security requirements |

## What Should Be Validated Before Final Design

Before implementation, validate:

- Current licenses in both tenants.
- Which users are assigned higher-tier government licenses.
- Which users are web-only or limited-access users.
- Which users have corporate devices.
- Which users need resource tenant access.
- Which users require SSH or RDP.
- Which users require DevOps or engineering tool access.
- Which users require admin access.
- Whether Conditional Access licensing is covered.
- Whether device compliance licensing is covered.
- Whether Cross-Tenant Synchronization licensing is covered.
- Whether Cross-Tenant Group Synchronization is required.
- Whether Access Reviews or Identity Governance are required.
- Whether privileged access controls are required.
- Whether Purview or Defender features are required.
- Whether VPN licensing covers the new access population.

## Licensing Risks

| Risk | Impact | Mitigation |
|---|---|---|
| Licensing all users as technical users | Higher cost | License by persona and access need |
| Including web-only users in resource tenant access | Security and cost exposure | Keep web-only users out of scope unless justified |
| Enabling Cross-Tenant Synchronization without license validation | Implementation delay | Confirm Entra licensing before design approval |
| Assuming Microsoft 365 licensing covers VPN access | Cost or ownership confusion | Validate VPN licensing separately |
| Assuming B2B users can access licensed apps without target licensing | Access failures | Validate application licensing in the target tenant |
| Using advanced security features without scoped licensing | Compliance or licensing issue | Map each feature to assigned users |
| Giving technical access without device compliance licensing | Policy failure | Validate endpoint and compliance licensing |
| Granting admin access without governance licensing | Privileged access risk | Validate PIM, access reviews, and audit requirements |

## Recommended Deliverables

The licensing workstream should produce:

- User persona matrix
- Current license inventory
- Target license assignment model
- Feature-to-license mapping
- Access group to license group mapping
- Cross-tenant synchronization licensing decision
- Conditional Access licensing validation
- Device compliance licensing validation
- VPN licensing validation
- Admin and privileged access licensing validation
- Open licensing questions

## Final Recommendation

Use a targeted licensing model.

The best-fit approach is to license users based on actual access requirements:

- Standard users receive standard productivity licensing.
- Web-only users stay limited and should not receive resource tenant access unless required.
- Technical users receive licensing aligned to device, security, and resource access requirements.
- Administrative users receive licensing aligned to privileged access, audit, and governance.
- Cross-Tenant Synchronization is only licensed and enabled if automated lifecycle management is required.
- Zero Trust VPN licensing is validated separately from Microsoft 365 licensing.

This keeps the solution aligned to business need while avoiding unnecessary cost, over-permissioning, and unsupported assumptions.
