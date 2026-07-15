# Licensing Considerations

## Purpose

This document captures the licensing considerations and recommended license options for a GCC High multi-tenant integration scenario.

The scenario assumes one tenant becomes the primary user and Microsoft 365 productivity tenant, while another tenant continues to host technical resources such as engineering workloads, DevOps systems, virtual machines, GPU workloads, large datasets, and specialized applications.

The goal is to identify which licenses are most appropriate for each user type and which licensing areas must be validated before implementation.

This document is architecture-aligned licensing guidance. It is not a contractual licensing determination.

## Licensing Goals

The licensing model should support the following goals:

- Provide Microsoft 365 productivity services for users moving into the primary tenant.
- Support secure collaboration between tenants.
- Support Conditional Access and device compliance requirements.
- Support users who need access to technical workloads in the resource tenant.
- Avoid licensing all users with the highest tier if only a subset requires advanced capabilities.
- Keep web-only or limited users out of scope for technical resource access unless there is a clear business requirement.
- Validate identity, security, compliance, and governance features before rollout.
- Treat third-party VPN or Zero Trust Network Access licensing separately from Microsoft 365 licensing.

## Current Licensing Context

The current licensing model includes multiple user profiles:

| User Type | Current Understanding |
|---|---|
| Users with assigned corporate devices | Licensed with a higher-tier government plan |
| Web-only or limited users | Licensed with a lower-cost or frontline model plus security add-ons |
| Users requiring larger OneDrive capacity | May require a plan with larger storage capability |
| Technical users in the resource tenant | Licensed with a higher-tier government plan because of device and workload requirements |
| Users expected to access resource tenant workloads | Limited to approved users with the right license, approved device, and business requirement |

Access to technical workloads in the resource tenant should not be granted broadly.

Only approved users should receive access to resource tenant workloads. Web-only users should remain out of scope for technical resource access unless a specific business requirement is approved.

## Recommended License Options

The following license options should be considered for this scenario.

| License Option | Recommended Use |
|---|---|
| Microsoft 365 Government G5 | Best fit for technical users, administrators, sensitive workloads, advanced security, advanced compliance, and users accessing resource tenant workloads |
| Microsoft 365 Government G3 | Good fit for standard full productivity users who need Microsoft 365 apps and services but do not require the full advanced security and compliance stack |
| Office 365 Government G5 | Useful when the customer needs Office 365 services and stronger compliance or security capabilities but does not require the full Microsoft 365 bundle |
| Microsoft 365 Government F3 or frontline equivalent | Good fit for limited-access or web-only users who do not need full desktop apps or technical resource access |
| F5 Security / Compliance add-ons or equivalent frontline add-ons | Useful only where frontline or limited users require additional security or compliance capabilities |
| Microsoft Entra ID P1 | Minimum identity license direction for Conditional Access and cross-tenant user synchronization scenarios |
| Microsoft Entra ID P2 | Recommended for privileged users, identity protection, privileged access, and higher-risk access scenarios |
| Microsoft Entra ID Governance or Microsoft Entra Suite | Required direction when cross-tenant group synchronization, access reviews, entitlement management, or advanced lifecycle governance is required |
| Microsoft Intune Plan 1 | Required direction for endpoint management and device compliance enforcement |
| Microsoft Defender for Endpoint Plan 2 | Recommended for technical users, administrators, and devices accessing sensitive resource tenant workloads |
| Microsoft Purview capabilities | Recommended for users handling sensitive data, audit, eDiscovery, DLP, labels, retention, or compliance workflows |
| Third-party Zero Trust VPN license | Required separately if users access resource tenant workloads through the customer’s existing Zero Trust VPN platform |

## Recommended License Mapping by Persona

| Persona | Recommended License Direction | Reason |
|---|---|---|
| Standard productivity users | Microsoft 365 Government G3 | Provides full productivity baseline for users who need Microsoft 365 apps and services |
| Higher-risk or sensitive users | Microsoft 365 Government G5 | Better fit when advanced security, compliance, audit, endpoint, and identity capabilities are required |
| Technical users accessing resource tenant workloads | Microsoft 365 Government G5 preferred | Technical users need stronger device, identity, endpoint, monitoring, and compliance controls |
| Technical users where cost optimization is required | Microsoft 365 Government G3 plus required security/compliance add-ons | Possible option if the exact required capabilities are added and validated |
| Administrative users | Microsoft 365 Government G5 plus Entra ID P2 or governance capabilities as needed | Admins need stronger privileged access, identity protection, audit, and governance controls |
| Web-only or limited-access users | Microsoft 365 Government F3 or frontline equivalent | Good fit only if users do not need full desktop apps or technical resource access |
| Web-only users with additional security needs | Microsoft 365 Government F3 plus F5 Security / Compliance add-ons or equivalent | Useful if limited users need additional security or compliance controls |
| Users needing larger OneDrive capacity | Microsoft 365 Government G3 or G5 | Better fit than limited-access licensing when storage requirements are higher |
| Users requiring cross-tenant synchronized access | Microsoft Entra ID P1 or P2 in the source tenant | Required for cross-tenant user synchronization |
| Users or groups requiring cross-tenant group synchronization | Microsoft Entra ID Governance or Microsoft Entra Suite | Required for cross-tenant group synchronization |
| Users accessing resource tenant workloads through VPN | Microsoft 365 license based on persona plus separate Zero Trust VPN licensing | VPN licensing is separate from Microsoft 365 licensing |

## Recommended Licensing Approach

## 1. Use Microsoft 365 Government G5 for technical and sensitive users

Microsoft 365 Government G5 is the preferred license direction for users who need stronger security, compliance, identity, endpoint, and audit capabilities.

Recommended for:

- Technical users
- Engineering users
- DevOps users
- Users accessing virtual machines
- Users accessing sensitive resource tenant workloads
- Users with corporate-managed devices
- Users requiring stronger endpoint protection
- Users requiring stronger compliance controls
- Users requiring privileged or elevated access

Why this fits:

These users have a higher risk profile because they access technical systems, administrative tools, engineering workloads, or sensitive environments. They should have stronger identity, endpoint, security, compliance, and auditing capabilities.

## 2. Use Microsoft 365 Government G3 for standard productivity users

Microsoft 365 Government G3 is a good fit for standard users who need full Microsoft 365 productivity capabilities but do not require the full advanced security and compliance stack.

Recommended for:

- Standard business users
- Back-office users
- Users moving into the primary tenant
- Users who need Microsoft 365 desktop apps and services
- Users who do not access resource tenant technical workloads
- Users without privileged access needs

Why this fits:

This provides a strong productivity baseline without automatically assigning the highest-cost license to every user.

## 3. Use Microsoft 365 Government F3 or frontline licensing only for limited-access users

Microsoft 365 Government F3 or equivalent frontline licensing can be used for users with limited productivity needs.

Recommended for:

- Web-only users
- Limited-access users
- Users without corporate devices
- Users who do not need full desktop apps
- Users who do not need access to resource tenant technical workloads

Important:

Do not use frontline licensing for users who need broad access to engineering systems, DevOps resources, virtual machines, administrative tools, or sensitive workloads unless the required security, compliance, device, endpoint, and access controls are added and validated.

## 4. Use F5 Security / Compliance add-ons only where needed

If limited-access or frontline users require stronger security or compliance capabilities, add-on licensing may be considered.

Recommended for:

- Limited-access users requiring stronger security controls
- Frontline users requiring additional compliance controls
- Users who do not need a full G5 license but need selected advanced capabilities

Important:

The exact add-on availability must be validated against the customer agreement and available government SKUs.

## 5. Use Microsoft Entra ID P1 as the minimum for Conditional Access and cross-tenant user synchronization

Microsoft Entra ID P1 should be treated as the minimum identity licensing direction when the design requires Conditional Access and cross-tenant user synchronization.

Recommended for:

- Conditional Access
- Cross-tenant user synchronization
- Standard cross-tenant access controls
- Group-based access assignments
- Identity-based access policy enforcement

Use Entra ID P1 when the scenario needs baseline identity security and cross-tenant synchronization capabilities but does not require advanced identity protection or privileged identity management.

## 6. Use Microsoft Entra ID P2 for admins and high-risk users

Microsoft Entra ID P2 should be considered for users who require stronger identity security and privileged access controls.

Recommended for:

- Privileged administrators
- Identity administrators
- Security administrators
- Users requiring Identity Protection
- Users requiring Privileged Identity Management
- Users requiring risk-based identity controls
- Users requiring stronger governance over privileged access

Use Entra ID P2 when the risk profile justifies stronger controls.

## 7. Use Microsoft Entra ID Governance or Microsoft Entra Suite for lifecycle governance

Microsoft Entra ID Governance or Microsoft Entra Suite should be considered when the customer needs stronger identity lifecycle governance.

Recommended if:

- Cross-tenant group synchronization is required.
- Access reviews are required.
- Entitlement management is required.
- Automated access lifecycle governance is required.
- Guest or external access requires recurring review.
- The organization wants stronger governance over cross-tenant access.

Do not add governance licensing unless the project requires these capabilities.

## 8. Use Microsoft Intune Plan 1 for device compliance

If access policies depend on managed or compliant devices, device management licensing must be validated.

Recommended for:

- Device compliance policies
- Managed corporate devices
- Conditional Access policies based on device state
- Endpoint configuration
- Mobile device management
- Mobile application management

Device compliance is important because the design depends on allowing only approved and compliant devices to access sensitive applications or resource tenant workloads.

## 9. Use Microsoft Defender for Endpoint Plan 2 for technical and sensitive access users

Defender for Endpoint Plan 2 should be considered for devices and users with technical or sensitive access.

Recommended for:

- Technical users
- Engineering users
- DevOps users
- Administrative users
- Users accessing resource tenant workloads
- Devices accessing sensitive systems
- Devices requiring endpoint detection and response

This is especially important where users connect to internal technical resources through VPN, SSH, RDP, or administrative tools.

## 10. Use Microsoft Purview capabilities for compliance-sensitive users

Microsoft Purview capabilities should be considered for users who handle regulated, sensitive, or compliance-relevant information.

Recommended for:

- Data loss prevention
- Sensitivity labels
- Information protection
- Audit
- eDiscovery
- Retention
- Compliance investigations
- Sensitive data protection

Not every user may require the same Purview capabilities. Assign based on user role, data sensitivity, and compliance obligation.

## 11. Treat Zero Trust VPN licensing separately

The customer’s Zero Trust VPN platform is not included in Microsoft 365 licensing.

Validate separately:

- Per-user VPN licensing
- Per-device VPN licensing
- Device posture licensing
- MFA integration
- Logging and reporting
- Admin access controls
- Support ownership
- Whether the existing VPN agreement covers the new user population

The Microsoft 365 license controls identity, productivity, security, compliance, and device management. The VPN license controls the secure access path into the resource environment.

## Feature-to-License Mapping

| Requirement | Recommended License Option |
|---|---|
| Full Microsoft 365 productivity for standard users | Microsoft 365 Government G3 |
| Full Microsoft 365 productivity with advanced security and compliance | Microsoft 365 Government G5 |
| Technical workload access | Microsoft 365 Government G5 preferred |
| Technical workload access with cost optimization | Microsoft 365 Government G3 plus required security/compliance add-ons |
| Web-only productivity | Microsoft 365 Government F3 or frontline equivalent |
| Web-only productivity with extra security/compliance | Microsoft 365 Government F3 plus F5 Security / Compliance add-ons or equivalent |
| Larger OneDrive/storage needs | Microsoft 365 Government G3 or G5 |
| Conditional Access | Microsoft Entra ID P1 minimum |
| Cross-tenant user synchronization | Microsoft Entra ID P1 or P2 |
| Cross-tenant group synchronization | Microsoft Entra ID Governance or Microsoft Entra Suite |
| Privileged Identity Management | Microsoft Entra ID P2 |
| Identity Protection | Microsoft Entra ID P2 |
| Access Reviews | Microsoft Entra ID Governance or Microsoft Entra Suite |
| Entitlement Management | Microsoft Entra ID Governance or Microsoft Entra Suite |
| Device compliance | Microsoft Intune Plan 1 |
| Endpoint detection and response | Microsoft Defender for Endpoint Plan 2 |
| Advanced compliance, audit, eDiscovery, DLP, and labeling | Microsoft Purview capabilities, commonly aligned to G5 or applicable add-ons |
| Resource tenant VPN access | Separate customer Zero Trust VPN licensing |
| Resource-side application access | Application-specific or resource-side licensing must be validated separately |

## Recommended Licensing Model

The recommended model for this scenario is a persona-based licensing model.

## Standard Productivity Users

Recommended license:

- Microsoft 365 Government G3

Use when users need:

- Microsoft 365 apps
- Email
- Teams
- SharePoint
- OneDrive
- Standard productivity workloads
- Baseline identity and device controls

Do not assign resource tenant access unless required.

## Technical Users

Recommended license:

- Microsoft 365 Government G5

Alternative:

- Microsoft 365 Government G3 plus required security and compliance add-ons, if validated

Use when users need:

- Engineering applications
- DevOps systems
- Virtual machine access
- SSH or RDP access
- Sensitive workload access
- Managed devices
- Strong endpoint controls
- Stronger audit and compliance controls

## Administrative Users

Recommended license:

- Microsoft 365 Government G5
- Microsoft Entra ID P2
- Microsoft Entra ID Governance or Microsoft Entra Suite if lifecycle governance or access reviews are required

Use when users need:

- Tenant administration
- Security administration
- Identity administration
- Privileged access
- Access reviews
- Audit
- Stronger identity protection

## Web-Only or Limited Users

Recommended license:

- Microsoft 365 Government F3 or frontline equivalent
- F5 Security / Compliance add-ons or equivalent only if required

Use when users need:

- Browser-based work
- Limited Microsoft 365 access
- No technical resource access
- No unrestricted desktop app requirement
- No privileged access

Important:

Web-only users should not be included in resource tenant access unless a specific requirement is approved and licensing is validated.

## Users Accessing Resource Tenant Workloads

Recommended license:

- Microsoft 365 Government G5 preferred
- Separate Zero Trust VPN licensing
- Resource-side permissions and application licensing validated separately

Use when users need:

- VPN access
- SSH
- RDP
- DevOps access
- Engineering systems
- Virtual machines
- Specialized technical workloads

This group should be limited and access should be approved by the resource owner.

## Licensing Validation Checklist

Before final design approval, validate:

- Current license inventory in both tenants
- User count by persona
- Users with corporate devices
- Users without corporate devices
- Web-only or frontline users
- Users requiring larger OneDrive capacity
- Users requiring technical resource access
- Users requiring SSH or RDP
- Users requiring DevOps access
- Users requiring administrative access
- Users requiring Conditional Access
- Users requiring device compliance
- Users requiring cross-tenant user synchronization
- Users requiring cross-tenant group synchronization
- Users requiring access reviews
- Users requiring Privileged Identity Management
- Users requiring Defender capabilities
- Users requiring Purview capabilities
- Users requiring VPN access
- Existing VPN licensing model
- Customer agreement and available government SKUs

## Licensing Risks and Mitigations

| Risk | Impact | Mitigation |
|---|---|---|
| Licensing all users as technical users | Increased cost | License by persona and access need |
| Granting resource tenant access to web-only users | Security and compliance exposure | Keep web-only users out of resource tenant access scope |
| Enabling cross-tenant synchronization without license validation | Implementation delay | Confirm Entra ID P1 or P2 before design approval |
| Enabling cross-tenant group synchronization without governance licensing | Feature gap or implementation delay | Confirm Entra ID Governance or Entra Suite |
| Assuming Microsoft 365 includes VPN licensing | Cost or ownership confusion | Treat VPN licensing separately |
| Assuming Microsoft 365 licensing grants access to resource applications | Access failures | Validate resource-side permissions and app licensing |
| Assigning G3 where G5 capabilities are required | Security or compliance gap | Map features to required license level |
| Assigning G5 to all users without need | Higher cost | Assign G5 only to users with technical, admin, or sensitive access needs |
| Device requirements not licensed or managed correctly | Conditional Access failures | Validate Intune and device compliance licensing |
| Admin users lack privileged access controls | Security risk | Validate Entra ID P2 and governance requirements |

## Recommended Deliverables

The licensing workstream should produce:

- User persona matrix
- Current license inventory
- Target license assignment model
- Feature-to-license mapping
- Access group to license group mapping
- Cross-tenant synchronization licensing decision
- Cross-tenant group synchronization licensing decision
- Conditional Access licensing validation
- Device compliance licensing validation
- Defender licensing validation
- Purview licensing validation
- VPN licensing validation
- Administrative licensing requirements
- Cost optimization notes
- Open licensing questions

## Final Recommendation

Use a targeted licensing model.

Recommended final position:

1. Use Microsoft 365 Government G5 for technical users, administrators, and users accessing sensitive resource tenant workloads.
2. Use Microsoft 365 Government G3 for standard productivity users.
3. Use Microsoft 365 Government F3 or frontline licensing only for web-only or limited-access users who do not need resource tenant technical access.
4. Use F5 Security / Compliance add-ons or equivalent frontline add-ons only when limited-access users require additional security or compliance capabilities.
5. Use Microsoft Entra ID P1 minimum for Conditional Access and cross-tenant user synchronization.
6. Use Microsoft Entra ID P2 for privileged administrators, identity protection, and privileged access scenarios.
7. Use Microsoft Entra ID Governance or Microsoft Entra Suite when cross-tenant group synchronization, access reviews, entitlement management, or stronger lifecycle governance is required.
8. Use Microsoft Intune Plan 1 when device compliance is required.
9. Use Microsoft Defender for Endpoint Plan 2 for technical users and sensitive access users.
10. Use Microsoft Purview capabilities for users and workloads with compliance, audit, retention, DLP, eDiscovery, or information protection requirements.
11. Treat Zero Trust VPN licensing separately from Microsoft 365 licensing.
12. Validate all license assignments against the customer agreement, available government SKUs, and final user personas before implementation.

## Important Note

This document provides architecture-aligned licensing guidance only.

Final licensing must be validated against:

- Customer agreement
- Available government SKUs
- Tenant cloud boundary
- User personas
- Security requirements
- Compliance requirements
- Device management model
- VPN licensing model
- Microsoft licensing specialist or reseller confirmation
