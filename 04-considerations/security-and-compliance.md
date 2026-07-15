# Security and Compliance Considerations

## Purpose

This document summarizes the main security and compliance considerations for a GCC High multi-tenant integration where one tenant becomes the primary Microsoft 365 tenant and another tenant continues to host technical workloads.

The goal is to define the main security controls that should be validated before implementation.

## Security Design Principles

The architecture should follow Zero Trust principles:

- Verify explicitly
- Use least privilege
- Assume breach

This means access should not be granted only because a user belongs to the organization or connects from a known network. Access should be based on identity, device state, policy, resource sensitivity, and business need.

## Recommended Security Model

The recommended model uses layered controls:

| Layer | Purpose |
|---|---|
| Identity | Authenticate users and control tenant access |
| Conditional Access | Enforce MFA, device compliance, and access policies |
| Device Compliance | Validate that approved devices meet security requirements |
| Cross-Tenant Access Settings | Control inbound and outbound collaboration between tenants |
| Zero Trust VPN | Provide secure access to technical resources in the resource environment |
| Resource Authorization | Control what users can access after connecting |
| Monitoring and Audit | Track sign-ins, access attempts, policy blocks, and administrative activity |

## Key Security Considerations

### 1. Identity and Access Control

Users should access resources using named accounts and approved authentication methods.

Recommended controls:

- Require MFA for sensitive access.
- Use Conditional Access for Microsoft 365 and cloud applications.
- Use cross-tenant access settings for collaboration between tenants.
- Limit access based on user groups and business requirements.
- Avoid shared administrative accounts.
- Separate standard user access from administrative access.

## 2. Device Compliance

Device compliance is important because access to full Microsoft 365 clients may depend on whether the device is considered compliant.

Recommended controls:

- Require compliant or managed devices for sensitive access.
- Validate whether devices managed in one tenant can satisfy access policies in the other tenant.
- Avoid broad device compliance exceptions.
- Test device compliance behavior during pilot.
- Document which users require corporate-managed devices.

## 3. Cross-Tenant Collaboration Security

Cross-tenant collaboration should be controlled intentionally.

Recommended controls:

- Configure inbound and outbound cross-tenant access settings.
- Allow only required users, groups, and applications.
- Validate MFA trust settings.
- Validate device compliance trust settings.
- Use B2B collaboration for Microsoft 365 collaboration.
- Use cross-tenant synchronization only if automated lifecycle management is required.

## 4. Technical Resource Access

Tenant collaboration alone does not provide access to technical or on-premises resources.

Users who need access to engineering systems, DevOps tools, virtual machines, or legacy applications should use the approved Zero Trust VPN access model.

Recommended controls:

- Limit VPN access to approved users.
- Require MFA and device posture checks where supported.
- Restrict SSH and RDP to approved technical users.
- Avoid broad network-level access when application-specific access is possible.
- Keep resource-side authorization in place.
- Monitor VPN and administrative access activity.

## 5. Resource-Side Authorization

VPN access should not automatically grant application or server access.

Recommended controls:

- Maintain application-level permissions.
- Maintain server-level permissions.
- Use role-based access where available.
- Review privileged access regularly.
- Validate access to DevOps tools, engineering applications, and virtual machines separately.

## 6. Data Protection

Sensitive data should remain protected across both tenants.

Recommended controls:

- Validate sensitivity labels if regulated data is shared.
- Validate Data Loss Prevention policies where required.
- Review external sharing settings.
- Avoid broad anonymous sharing.
- Apply retention policies where required.
- Validate eDiscovery and audit requirements.

## 7. Monitoring and Audit

Security teams should have visibility across identity, device, VPN, and resource access.

Recommended log sources:

- Microsoft Entra sign-in logs
- Conditional Access reports
- Audit logs
- VPN access logs
- Endpoint security alerts
- Application logs
- Administrative activity logs

Recommended monitoring activities:

- Monitor failed sign-ins.
- Monitor Conditional Access blocks.
- Monitor unexpected VPN access.
- Monitor privileged access activity.
- Review guest or external user activity.
- Review access changes periodically.

## 8. Compliance Considerations

Using GCC High does not automatically make the full environment compliant.

The platform provides a government cloud boundary, but the customer is still responsible for configuring and operating the required controls.

Areas to validate:

- Identity policies
- Device compliance
- Cross-tenant access settings
- External collaboration controls
- Data protection policies
- Audit and retention
- Access reviews
- Privileged access management
- Incident response ownership

## Recommended Controls Summary

| Control Area | Recommendation |
|---|---|
| MFA | Require MFA for sensitive and administrative access |
| Conditional Access | Apply policies based on user, device, application, and risk |
| Device Compliance | Require compliant devices for sensitive access |
| Cross-Tenant Access | Configure inbound and outbound settings intentionally |
| VPN Access | Limit to approved users and approved devices |
| SSH / RDP | Restrict to approved technical users |
| Resource Authorization | Keep application and server permissions in place |
| Data Protection | Use labels, DLP, retention, and audit where required |
| Monitoring | Review identity, VPN, endpoint, and application logs |
| Access Reviews | Review technical and guest access periodically |

## Final Recommendation

The security model should remain simple and layered:

- Use Microsoft Entra for identity and cross-tenant collaboration.
- Use Conditional Access for cloud application access.
- Use device compliance to validate trusted endpoints.
- Use Zero Trust VPN for technical resource access.
- Use resource-side permissions to control what users can access after connecting.
- Use monitoring, audit, and access reviews to support compliance.

This approach supports secure collaboration while keeping technical workloads protected in the resource environment.
