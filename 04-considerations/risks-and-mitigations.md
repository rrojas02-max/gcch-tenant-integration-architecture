# Risks and Mitigations

## Purpose

This document summarizes the main risks for a GCC High multi-tenant integration where one tenant becomes the primary Microsoft 365 tenant and another tenant continues to host technical workloads such as engineering systems, DevOps tools, virtual machines, and specialized applications.

The goal is to identify key risks early and define practical mitigations before implementation.

## Risk Summary

| Risk | Impact | Mitigation |
|---|---|---|
| Too many users receive access to technical workloads | Increased security and compliance exposure | Limit access to approved users using group-based access |
| VPN access is confused with tenant integration | Architecture misunderstanding | Separate identity, VPN access, and resource authorization clearly |
| B2B collaboration is treated as on-premises access | Users may not be able to access technical systems | Use B2B for Microsoft 365 collaboration and VPN for technical resources |
| Conditional Access policies block valid users | User disruption during rollout | Test Conditional Access and device compliance with a small pilot group |
| Device compliance trust is not aligned between tenants | Users may fail to access full Microsoft 365 apps | Validate cross-tenant access settings and device trust behavior |
| Cross-tenant synchronization is enabled without a clear need | Added lifecycle and licensing complexity | Use only if automated B2B provisioning and deprovisioning is required |
| Technical workloads are moved to cloud too early | High cost, performance, or operational risk | Keep engineering, DevOps, VM, GPU, and large data workloads in the resource tenant unless future assessment justifies migration |
| Legacy applications require separate authentication | Users may need additional accounts or permissions | Inventory legacy applications and document LDAP, Kerberos, NTLM, or local authentication dependencies |
| SSH or RDP access is too broad | Increased privileged access risk | Restrict SSH and RDP to approved technical users and monitored devices |
| Licensing is not aligned with the design | Unexpected cost or missing required capabilities | Validate G3, G5, F3, Entra ID P1/P2, Governance, Intune, Defender, Purview, and VPN licensing before rollout |
| GCC High feature availability is assumed | Design may depend on unavailable or limited features | Validate required GCC High features before final architecture approval |
| Operational ownership is unclear | Delays in support, approvals, and troubleshooting | Define owners for identity, devices, VPN, applications, security, and resource access |

## Key Mitigation Strategy

The recommended mitigation strategy is to keep the design simple and controlled:

- Use the primary tenant for Microsoft 365 productivity services.
- Keep technical workloads in the resource tenant.
- Use Microsoft Entra B2B collaboration for Microsoft 365 collaboration.
- Use the existing Zero Trust VPN model for technical resource access.
- Limit technical access to approved users only.
- Validate Conditional Access, device compliance, licensing, and GCC High feature availability before rollout.
- Start with a small pilot group before expanding access.

## Final Recommendation

The main architecture risk is overcomplicating the design.

The best approach is to avoid unnecessary workload migration, avoid broad access, and avoid assuming that tenant collaboration automatically provides access to technical resources.

A controlled pilot with approved users, validated licensing, and clear ownership is the recommended next step.
