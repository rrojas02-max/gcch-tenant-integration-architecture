# Target State Architecture

## Purpose

This document describes the target-state reference architecture for integrating two Microsoft 365 GCC High tenants after a merger or acquisition while keeping both environments separate during the initial coexistence phase.

The goal is to enable secure Microsoft 365 collaboration and controlled access to retained engineering resources without performing full tenant consolidation or immediate migration of engineering and DevOps workloads.

The architecture separates two major access areas:

1. Microsoft 365 cloud collaboration access.
2. On-premises engineering and DevOps resource access.

This distinction is important because Microsoft 365 collaboration can be addressed using Microsoft Entra and Microsoft 365 cross-tenant capabilities, while on-premises engineering access depends on remote access, device posture, workload requirements, protocols such as SSH/RDP, and remaining LDAP-dependent services.

---

## Scenario Summary

The architecture is based on an anonymized merger and acquisition scenario involving two organizations operating in Microsoft 365 GCC High.

| Organization | Environment Type | Identity Model | Key Characteristics |
|---|---|---|---|
| Tenant A | Microsoft 365 GCC High | Cloud-focused primary tenant | Strategic Microsoft 365 tenant after acquisition. Expected to become the main tenant for users, email, Teams, SharePoint Online, OneDrive for Business, Microsoft 365 Apps, and back-office collaboration. |
| Tenant B | Microsoft 365 GCC High | Hybrid identity | Acquired organization. Maintains on-premises Active Directory synchronization and retains engineering and DevOps workloads in its environment. |

Tenant A is expected to become the primary Microsoft 365 environment for users and back-office services.

Tenant B remains operational for engineering and DevOps workloads that are not planned for immediate migration to Azure because of GPU requirements, cost, large data volumes, and operational dependencies.

Known Tenant B engineering scope includes:

- Engineering applications.
- DevOps-related systems.
- OpenShift or private cloud environment.
- Virtual machines.
- GPU-based compute resources.
- Large on-premises engineering datasets.
- SSH access.
- RDP access.
- Remaining LDAP-dependent services.
- Existing or planned VPN / Zero Trust remote access path for remote users.

Traditional file services are not treated as a primary design driver in this reference architecture because the customer indicated file services had already moved to SharePoint.

Internal web applications are not listed as confirmed scope. If discovered later, they should be added as a separate application discovery item.

---

## Target Architecture Goals

The target architecture is designed to meet the following goals:

- Keep both Microsoft 365 GCC High tenants separate during the initial coexistence phase.
- Establish Tenant A as the target Microsoft 365 tenant for users, email, Teams, SharePoint Online, OneDrive for Business, Microsoft 365 Apps, and back-office collaboration.
- Preserve Tenant B’s hybrid identity and engineering environment.
- Avoid immediate migration of Tenant B engineering and DevOps workloads to Azure.
- Enable secure collaboration between approved users in both tenants.
- Support controlled access to Microsoft 365 cloud resources.
- Support controlled access to Tenant B on-premises engineering and DevOps resources.
- Use Microsoft Entra cross-tenant capabilities where available and validated for GCC High.
- Use the customer-approved VPN / Zero Trust remote access path for retained engineering resources.
- Enforce least privilege access.
- Support Conditional Access and MFA requirements.
- Evaluate cross-tenant trust for MFA, compliant device, and hybrid-joined device claims.
- Provide governance for onboarding, offboarding, and access review.
- Minimize unnecessary exposure of Tenant B engineering systems.
- Keep Microsoft 365 collaboration access separate from engineering access.
- Validate all capabilities against Microsoft 365 GCC High feature availability before implementation.

---

## High-Level Architecture

```text
+------------------------------------------------------------------+
| Tenant A                                                         |
| Microsoft 365 GCC High                                           |
| Primary Microsoft 365 Tenant                                     |
|                                                                  |
| Users                                                            |
| Devices                                                          |
| Microsoft Entra ID                                               |
| Exchange Online                                                  |
| Microsoft Teams                                                  |
| SharePoint Online                                                |
| OneDrive for Business                                            |
| Microsoft 365 Apps                                               |
| Back-office Collaboration Services                               |
+-------------------------------+----------------------------------+
                                |
                                |
                                | Microsoft 365 Cross-Tenant Access
                                | - B2B Collaboration
                                | - B2B Direct Connect, where appropriate
                                | - Cross-Tenant Access Settings
                                | - Conditional Access
                                | - Optional Cross-Tenant Synchronization
                                | - Access Reviews / Governance
                                |
+-------------------------------v----------------------------------+
| Tenant B                                                         |
| Microsoft 365 GCC High                                           |
| Acquired / Engineering Tenant                                    |
| Hybrid Identity                                                  |
|                                                                  |
| Microsoft Entra ID                                               |
| Synchronized identities                                          |
| Existing Microsoft 365 collaboration resources during coexistence |
+-------------------------------+----------------------------------+
                                |
                                | Directory synchronization
                                |
+-------------------------------v----------------------------------+
| Tenant B On-Premises / Engineering Environment                   |
|                                                                  |
| Active Directory Domain Services                                 |
| Engineering Applications                                         |
| DevOps-related Systems                                           |
| OpenShift / Private Cloud Environment                            |
| Virtual Machines                                                 |
| GPU-based Compute Resources                                      |
| Large Engineering Datasets                                       |
| SSH / RDP Access                                                 |
| Remaining LDAP-dependent Services                                |
+-------------------------------+----------------------------------+
                                ^
                                |
                                | Approved Remote Access Path
                                | - VPN / Zero Trust Remote Access
                                | - Device posture validation
                                | - MFA / Conditional Access where supported
                                | - Scoped access to approved engineering resources
                                |
+-------------------------------+----------------------------------+
| Approved Users from Tenant A                                     |
|                                                                  |
| Access Tenant B engineering resources only when authorized        |
| Access path is separate from Microsoft 365 collaboration access   |
+------------------------------------------------------------------+
