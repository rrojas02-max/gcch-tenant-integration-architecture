# Target State Architecture

## Purpose

This document describes the target-state reference architecture for integrating two Microsoft 365 GCC High tenants after a merger or acquisition while keeping both environments separate.

The goal is to enable selected users from one organization to securely access cloud and on-premises resources in another organization without performing a full tenant consolidation during the initial integration phase.

---

## Scenario Summary

The architecture is based on an anonymized merger and acquisition scenario involving two organizations.

| Organization | Environment Type | Identity Model | Key Characteristics |
|---|---|---|---|
| Tenant A | Microsoft 365 GCC High | Cloud-only | Primary organization after acquisition. Users and devices are managed in cloud identity. |
| Tenant B | Microsoft 365 GCC High | Hybrid identity | Acquired organization. Uses on-premises Active Directory synchronized to Microsoft Entra ID. Maintains legacy apps, file shares, servers, and cloud collaboration resources. |

Selected users from Tenant A need access to resources owned by Tenant B.

Tenant B must remain operationally intact and continue using its existing hybrid identity environment.

---

## Target Architecture Goals

The target architecture is designed to meet the following goals:

- Keep both Microsoft 365 GCC High tenants separate.
- Enable secure collaboration between approved users.
- Support access to Tenant B cloud resources.
- Support controlled access to Tenant B on-premises resources.
- Preserve Tenant B hybrid identity and on-premises Active Directory dependencies.
- Avoid full tenant consolidation during the initial phase.
- Enforce least privilege access.
- Support Conditional Access and MFA requirements.
- Provide governance for onboarding, offboarding, and access review.
- Minimize broad network exposure.
- Validate all capabilities against GCC High feature availability before implementation.

---

## High-Level Architecture

```text
+----------------------------------------------------+
| Tenant A                                           |
| Microsoft 365 GCC High                             |
| Cloud-only identity                                |
|                                                    |
| Users                                              |
| Devices                                            |
| Microsoft Entra ID                                 |
+---------------------------+------------------------+
                            |
                            |
                            | Cross-tenant access
                            | B2B collaboration
                            | Cross-tenant settings
                            | Conditional Access
                            | Optional user sync
                            |
+---------------------------v------------------------+
| Tenant B                                           |
| Microsoft 365 GCC High                             |
| Hybrid identity                                    |
|                                                    |
| Microsoft Entra ID                                 |
| Microsoft 365 Cloud Resources                      |
| Teams / SharePoint / OneDrive / Cloud Apps         |
+---------------------------+------------------------+
                            |
                            | Existing directory sync
                            |
+---------------------------v------------------------+
| Tenant B On-Premises Environment                   |
|                                                    |
| Active Directory Domain Services                   |
| File Shares                                        |
| Internal Web Applications                          |
| Legacy Line-of-Business Applications               |
| Domain-Joined Servers                              |
+----------------------------------------------------+
