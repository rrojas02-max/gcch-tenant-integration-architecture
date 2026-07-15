
# Conditional Access Baseline

## Purpose

This document defines a Conditional Access baseline for a Microsoft 365 GCC High cross-tenant coexistence scenario.

The purpose is to provide a practical security baseline for two organizations operating in separate Microsoft 365 GCC High tenants after a merger or acquisition. The baseline focuses on Microsoft 365 cloud collaboration, device trust, MFA enforcement, and controlled cross-tenant access while preserving tenant separation.

This document is not intended to be a click-by-click configuration guide. It is an architecture implementation guide that explains what should be reviewed, validated, and configured before enabling broader cross-tenant access.

---

## Scenario Context

The reference architecture assumes two Microsoft 365 GCC High tenants:

| Area | Tenant A | Tenant B |
|---|---|---|
| Role | Primary Microsoft 365 tenant | Acquired / engineering tenant |
| Cloud | Microsoft 365 GCC High | Microsoft 365 GCC High |
| Identity model | Cloud-focused | Hybrid identity |
| Strategic direction | Main tenant for users, email, Teams, SharePoint, OneDrive, and Microsoft 365 Apps | Retained for engineering and DevOps workloads |
| Device model | Managed devices for users moving into the primary tenant | Engineering devices may remain aligned to the acquired environment |
| Access requirement | Users need Microsoft 365 collaboration and selected access to engineering resources | Environment must remain operational during coexistence |

The customer scenario includes the following key access requirements:

- Tenant A becomes the strategic Microsoft 365 tenant.
- Tenant B remains operational for engineering and DevOps workloads.
- Back-office users move toward Tenant A for Microsoft 365 services.
- Engineering users may continue using devices associated with Tenant B.
- Both tenants may enforce Conditional Access policies that require compliant devices for full Microsoft 365 desktop client access.
- Cross-tenant access must be controlled, auditable, and scoped to approved users and resources.
- On-premises engineering access is handled separately from Microsoft 365 cloud collaboration access.

---

## Conditional Access Design Goals

The Conditional Access baseline should support the following goals:

1. Enforce strong authentication for cross-tenant access.
2. Require MFA for users accessing Microsoft 365 cloud resources.
3. Preserve tenant separation during coexistence.
4. Support compliant device requirements for Microsoft 365 desktop clients.
5. Evaluate whether MFA and device claims from the partner tenant can be trusted.
6. Avoid weakening existing Conditional Access policies to improve user experience.
7. Support a phased rollout using pilot groups before broad enforcement.
8. Keep administrative access separate from user collaboration access.
9. Ensure external and cross-tenant users receive only approved access.
10. Maintain auditability through sign-in logs, access reviews, and policy monitoring.

---

## Baseline Design Principles

### 1. Do not start with broad access

Cross-tenant access should not be enabled broadly by default.

Access should be scoped by:

- Approved users.
- Approved groups.
- Approved Microsoft 365 workloads.
- Approved Teams, SharePoint sites, and OneDrive resources.
- Approved business use cases.
- Defined tenant relationships.

### 2. Separate Microsoft 365 access from engineering access

Microsoft 365 cloud access and engineering access are different access surfaces.

Microsoft 365 Conditional Access controls should protect:

- Exchange Online.
- Microsoft Teams.
- SharePoint Online.
- OneDrive for Business.
- Microsoft 365 Apps.
- Microsoft 365 web and desktop clients.

Engineering access should be treated separately and controlled through the approved remote access path, workload-level permissions, device posture, and access governance.

### 3. Use partner-specific cross-tenant settings

Default cross-tenant settings should remain conservative.

Partner-specific settings should be created for the trusted GCC High tenant relationship. This allows the organization to define specific inbound and outbound rules instead of relying only on tenant-wide defaults.

### 4. Trust claims only when justified

Trusting MFA, compliant device, or hybrid joined device claims from another tenant can improve user experience, but it also makes the partner tenant part of the trust chain.

Trust decisions should be documented and approved before enforcement.

### 5. Validate before enforcing

Conditional Access changes should be tested using:

- Report-only mode where available.
- Pilot users.
- Test devices.
- Test cross-tenant access scenarios.
- Sign-in logs.
- Conditional Access insights.
- Access review results.

---

## Recommended Conditional Access Baseline

The following baseline is recommended for the reference architecture.

---

## Policy 1: Require MFA for Cross-Tenant Access

### Objective

Require MFA for users accessing Microsoft 365 resources across tenants.

### Applies To

- External users accessing Tenant A Microsoft 365 resources.
- External users accessing Tenant B Microsoft 365 resources.
- Scoped pilot users during initial rollout.
- Approved cross-tenant collaboration groups.

### Target Resources

- Microsoft Teams.
- SharePoint Online.
- OneDrive for Business.
- Exchange Online.
- Microsoft 365 Apps.
- Other approved Microsoft 365 cloud applications.

### Access Control

Require multifactor authentication.

### Implementation Notes

Both tenants should evaluate whether MFA claims from the partner tenant should be trusted through Cross-Tenant Access Settings.

If MFA trust is enabled, the resource tenant may accept MFA already performed in the user’s home tenant. If MFA trust is not enabled, users may be prompted again by the resource tenant.

### Design Considerations

- Trust MFA only for the approved partner tenant.
- Avoid trusting MFA from all external tenants by default.
- Document the business reason for trusting partner MFA.
- Confirm both tenants have acceptable MFA standards.
- Test user experience before broad rollout.

---

## Policy 2: Require Compliant or Hybrid Joined Device for Full Microsoft 365 Client Access

### Objective

Preserve existing security requirements that allow full Microsoft 365 desktop client access only from managed or trusted devices.

### Applies To

- Users accessing Microsoft 365 desktop clients.
- Users accessing Outlook desktop.
- Users accessing Teams desktop.
- Users accessing SharePoint or OneDrive through synced or desktop experiences.
- Engineering users who retain devices managed in Tenant B but access Microsoft 365 services in Tenant A.

### Target Resources

- Exchange Online.
- Microsoft Teams.
- SharePoint Online.
- OneDrive for Business.
- Microsoft 365 Apps.

### Access Control

Require one of the following, depending on tenant policy design:

- Compliant device.
- Microsoft Entra hybrid joined device.
- Approved device state trusted from the partner tenant.

### Implementation Notes

For the cross-tenant coexistence scenario, the key design question is whether a device managed in Tenant B can satisfy Tenant A Conditional Access requirements when the user accesses Tenant A Microsoft 365 services.

This should be evaluated using Cross-Tenant Access Settings trust configuration.

### Design Considerations

- Tenant A may trust compliant device claims from Tenant B for approved users.
- Tenant A may trust hybrid joined device claims from Tenant B for approved users.
- Tenant B may also define reciprocal trust if Tenant A users need access to Tenant B Microsoft 365 resources.
- Device trust should be scoped to the partner tenant, not broadly applied to all external organizations.
- Device trust should be validated with pilot users and known test devices.

---

## Policy 3: Restrict Access From Unmanaged or Unknown Devices

### Objective

Reduce risk from unmanaged, unknown, or untrusted devices.

### Applies To

- External users.
- Cross-tenant users.
- Users accessing sensitive Microsoft 365 workloads.
- Users accessing regulated or sensitive collaboration spaces.

### Target Resources

- Microsoft Teams.
- SharePoint Online.
- OneDrive for Business.
- Exchange Online.
- Microsoft 365 Apps.
- Sensitive collaboration sites or teams.

### Access Control Options

Depending on business requirements, apply one or more of the following:

- Block unmanaged devices for sensitive resources.
- Allow browser-only access with restrictions where supported.
- Require MFA.
- Require compliant device.
- Require hybrid joined device.
- Require approved client application.
- Use session controls where supported.

### Design Considerations

- Avoid blanket access from unmanaged devices to sensitive resources.
- Define different access levels for managed users, external users, and browser-only access.
- Validate impact on Teams, SharePoint, OneDrive, and Outlook user experience.
- Use a pilot before broad enforcement.

---

## Policy 4: Protect Administrative Access

### Objective

Protect administrative portals and privileged operations.

### Applies To

- Global administrators.
- Security administrators.
- Conditional Access administrators.
- Exchange administrators.
- SharePoint administrators.
- Teams administrators.
- Compliance administrators.
- Privileged roles in either tenant.

### Target Resources

- Microsoft admin portals.
- Microsoft Entra admin center.
- Microsoft 365 admin center.
- Exchange admin center.
- SharePoint admin center.
- Teams admin center.
- Security and compliance portals.
- Azure management portals where applicable.

### Access Control

Require stronger controls for administrative access, such as:

- MFA.
- Compliant device.
- Privileged access workflow where available.
- Strong authentication methods where supported.
- Separate administrative accounts where required by policy.

### Design Considerations

- Administrative access should not be mixed with user collaboration access.
- Partner or external users should not receive administrative access unless explicitly approved.
- Emergency access accounts should be excluded from Conditional Access lockout scenarios and monitored.
- Administrative access should be reviewed on a recurring basis.

---

## Policy 5: Define Cross-Tenant Inbound and Outbound Rules

### Objective

Control which users can access resources across the tenant boundary.

### Applies To

- Tenant A users accessing Tenant B resources.
- Tenant B users accessing Tenant A resources.
- Approved cross-tenant collaboration groups.
- Approved Microsoft 365 workloads.

### Target Resources

- Microsoft 365 cloud resources.
- Teams shared channels where B2B Direct Connect is used.
- SharePoint Online sites.
- OneDrive content.
- Exchange Online where applicable.
- Approved enterprise applications.

### Access Control

Use Cross-Tenant Access Settings to define:

- Inbound access.
- Outbound access.
- Allowed users and groups.
- Allowed applications.
- MFA trust.
- Compliant device trust.
- Hybrid joined device trust.

### Design Considerations

- Avoid using only default cross-tenant 
