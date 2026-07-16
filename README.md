# GCCH Tenant Integration Architecture

Reference architecture for integrating two Microsoft 365 GCC High tenants during a merger and acquisition scenario while keeping both environments separate.

This repository documents an anonymized architecture case study focused on identity, resource access, hybrid integration, GCC High limitations, and compliance-aware design.

> **Disclaimer**  
> This repository is part of my professional architecture portfolio. All company names, tenant names, domains, diagrams, implementation details, and business references have been anonymized. No confidential customer information is included. Content is provided for educational and reference architecture purposes only.

---

## Scenario Overview

This architecture is based on a merger and acquisition scenario where one organization needs to provide selected users access to resources from another organization without performing an immediate tenant consolidation.

The scenario includes two organizations:

| Organization | Environment | Identity Model | Description |
|---|---|---|---|
| Tenant A | Microsoft 365 GCC High | Cloud-only | Primary organization after acquisition. |
| Tenant B | Microsoft 365 GCC High | Hybrid identity | Acquired organization with on-premises Active Directory, synchronized users, devices, legacy apps, and cloud resources. |

The key challenge is that selected users from Tenant A need access to resources in Tenant B, including:

- Microsoft 365 cloud resources
- Teams and SharePoint collaboration spaces
- Cloud applications
- Internal web applications
- File shares
- Legacy on-premises applications
- Resources that depend on Active Directory, Kerberos, NTLM, SMB, LDAP, or RDP

The architecture keeps both tenants operationally separate while enabling controlled collaboration and resource access.

---

## Architecture Goals

The target architecture is designed to:

- Enable secure cross-tenant collaboration.
- Preserve Tenant B’s hybrid identity environment.
- Avoid immediate full tenant consolidation.
- Support Microsoft 365 GCC High constraints.
- Separate cloud resource access from on-premises access.
- Enforce least privilege.
- Support Conditional Access and MFA.
- Provide governance for onboarding and offboarding.
- Reduce unnecessary network exposure.
- Support compliance-aware access patterns for regulated environments.

---

## High-Level Architecture

<img width="2591" height="2268" alt="image" src="https://github.com/user-attachments/assets/6d9ef227-abce-4761-bcb2-f132e662b8e0" />

