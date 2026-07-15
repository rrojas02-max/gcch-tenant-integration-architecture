# Licensing Considerations

## Purpose

This document summarizes the licensing considerations for a GCC High multi-tenant integration scenario.

The goal is to identify which license types are likely to fit each user group and which licensing items must be validated before implementation. This document is not a final licensing or contractual determination.

## Recommended Licensing Approach

Use a persona-based licensing model.

Not every user needs the same license. The recommended approach is to assign licenses based on:

- User role
- Device ownership
- Microsoft 365 productivity needs
- Security and compliance requirements
- Access to technical workloads
- Administrative responsibilities
- Cross-tenant collaboration requirements

## License Direction by User Type

| User Type | Recommended License Direction | Notes |
|---|---|---|
| Standard productivity users | Microsoft 365 Government G3 | Good fit for users who need Microsoft 365 apps, email, Teams, SharePoint, and OneDrive |
| Technical users | Microsoft 365 Government G5 | Recommended for users accessing engineering systems, DevOps tools, virtual machines, or sensitive workloads |
| Administrative users | Microsoft 365 Government G5 plus Entra ID P2 if needed | Recommended for users with privileged access, identity administration, security administration, or audit requirements |
| Web-only or limited users | Microsoft 365 Government F3 or equivalent frontline licensing | Good fit only if users do not require full desktop apps or access to technical workloads |
| Limited users requiring more security | F3 plus F5 Security / Compliance add-ons if available | Use only when limited users need additional security or compliance capabilities |
| Users requiring larger OneDrive capacity | Microsoft 365 Government G3 or G5 | Better fit than limited-access licensing when storage requirements are higher |
| Users requiring cross-tenant user synchronization | Microsoft Entra ID P1 or P2 | Required direction for cross-tenant user synchronization |
| Users requiring cross-tenant group synchronization | Microsoft Entra ID Governance or Microsoft Entra Suite | Required direction if group synchronization or stronger lifecycle governance is needed |
| Users accessing technical workloads through VPN | Microsoft 365 license based on persona plus separate Zero Trust VPN licensing | VPN licensing is separate from Microsoft 365 licensing |

## Key License Options

### Microsoft 365 Government G5

Recommended for:

- Technical users
- Engineering users
- DevOps users
- Administrative users
- Users accessing sensitive workloads
- Users requiring advanced security, compliance, audit, and endpoint controls

Microsoft’s government plan comparison lists Microsoft 365 Government G5 as including stronger identity, security, endpoint, and compliance capabilities such as Entra ID Plan 2, Defender for Endpoint Plan 2, Purview Information Protection Plan 2, Audit Premium, and eDiscovery Premium.

### Microsoft 365 Government G3

Recommended for:

- Standard productivity users
- Back-office users
- Users moving into the primary tenant
- Users who need Microsoft 365 desktop apps and core services
- Users who do not need advanced technical or administrative access

Microsoft’s government plan comparison lists Microsoft 365 Government G3 as a government productivity plan with Microsoft 365 apps and services, Entra ID Plan 1, Intune Plan 1, and baseline security/compliance capabilities.

### Microsoft 365 Government F3 or Frontline Equivalent

Recommended for:

- Web-only users
- Limited-access users
- Users without technical workload access
- Users who do not need full desktop applications

This license direction should not be used for users who need broad access to engineering systems, DevOps resources, virtual machines, administrative tools, or sensitive workloads unless add-ons and access requirements are validated.

### Microsoft Entra ID P1

Recommended when the design requires:

- Conditional Access
- Cross-tenant user synchronization
- Standard identity-based access controls

Microsoft documentation states that cross-tenant user synchronization requires Microsoft Entra ID P1 or P2 in the source tenant. 【2-8a20f0】 Microsoft Entra licensing documentation also lists Entra ID P1 as available standalone and included in several enterprise Microsoft 365 offers.

### Microsoft Entra ID P2

Recommended when the design requires:

- Privileged Identity Management
- Identity Protection
- Risk-based identity controls
- Stronger controls for administrators and high-risk users

Microsoft Entra licensing documentation lists Entra ID P2 as a higher identity licensing option available standalone and included with Microsoft 365 E5 and other security bundles. 

### Microsoft Entra ID Governance or Microsoft Entra Suite

Recommended when the design requires:

- Cross-tenant group synchronization
- Access reviews
- Entitlement management
- Stronger identity lifecycle governance

Microsoft documentation states that cross-tenant group synchronization requires Microsoft Entra ID Governance or Microsoft Entra Suite.

### Zero Trust VPN Licensing

The Zero Trust VPN platform should be treated separately from Microsoft 365 licensing.

Validate separately:

- User licensing
- Device licensing
- Device posture requirements
- MFA integration
- Logging and reporting
- Support ownership
- Coverage for the new user population

Microsoft 365 licensing does not automatically cover third-party VPN access.

## Recommended Licensing Model

The recommended model for this scenario is:

1. Use **Microsoft 365 Government G5** for technical users, administrators, and users accessing sensitive resource tenant workloads.
2. Use **Microsoft 365 Government G3** for standard productivity and back-office users.
3. Use **Microsoft 365 Government F3 or equivalent frontline licensing** only for web-only or limited-access users.
4. Use **F5 Security / Compliance add-ons** only when limited users need additional security or compliance controls.
5. Use **Microsoft Entra ID P1** as the minimum direction for Conditional Access and cross-tenant user synchronization.
6. Use **Microsoft Entra ID P2** for privileged access, identity protection, and high-risk users.
7. Use **Microsoft Entra ID Governance or Microsoft Entra Suite** only if access reviews, entitlement management, or cross-tenant group synchronization are required.
8. Treat **Zero Trust VPN licensing** separately from Microsoft 365 licensing.

## What Should Be Validated

Before implementation, validate:

- Current license inventory in both tenants
- Number of users by persona
- Which users have corporate devices
- Which users are web-only
- Which users need access to technical workloads
- Which users require SSH, RDP, DevOps, or virtual machine access
- Which users require administrative access
- Whether Conditional Access and device compliance are required
- Whether cross-tenant user synchronization is required
- Whether cross-tenant group synchronization is required
- Whether access reviews or entitlement management are required
- Whether the existing VPN license covers the new user population
- Whether the customer agreement includes the required government SKUs

## Final Recommendation

For this architecture, keep licensing simple and persona-based:

- **G5** for technical, sensitive, and administrative users.
- **G3** for standard productivity users.
- **F3 or frontline equivalent** for web-only or limited users.
- **Entra ID P1** minimum for Conditional Access and cross-tenant user synchronization.
- **Entra ID P2 or Governance/Suite** only when privileged access, access reviews, or advanced lifecycle governance are required.
- **Separate VPN licensing** for access to resource tenant workloads.

Final licensing must be confirmed with the customer’s licensing team, reseller, or Microsoft licensing specialist.
