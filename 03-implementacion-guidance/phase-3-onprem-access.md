# Phase 3 - On-Premises Access and Resource Connectivity Design

## Objective

The objective of Phase 3 is to define the secure access model that allows approved users from the primary tenant to access resources in the resource tenant without moving engineering, DevOps, virtual machine, GPU, or large data workloads to Azure.

This phase is focused on access design, not workload migration.

The primary tenant should become the main environment for users, email, and Microsoft 365 services, while the resource tenant continues to host engineering, DevOps, virtual machines, and specialized technical workloads.

---

## Customer-Specific Context

Based on the customer discussion:

- The primary tenant is expected to become the main tenant for users, email, and Microsoft 365 services.
- Selected back-office users are planned to move into the primary tenant.
- Engineering, DevOps, virtual machines, and specialized technical workloads should remain in the resource tenant.
- No remaining on-premises file services were identified because file services have already been migrated to SharePoint.
- Resource access uses standard protocols such as SSH and RDP.
- Remote access to resource tenant workloads uses the customer’s existing Zero Trust VPN model.
- The Zero Trust VPN model is expected to be extended for approved users from the primary tenant.
- On-premises and specialized workloads should not be moved to Azure in this phase because of technical and cost constraints, including large-scale data and GPU-based workloads.

---

## Recommended Design

The recommended design is to keep the technical workloads in the resource tenant and provide controlled access from approved users in the primary tenant through the customer’s existing Zero Trust VPN model.

This phase should not introduce Azure VPN, ExpressRoute, or a new Microsoft network connectivity dependency unless the customer later introduces Azure-hosted workloads, private Azure connectivity, or a dedicated hybrid network requirement.

The optimized recommendation is:

1. Keep engineering, DevOps, virtual machines, GPU workloads, and large data workloads in the resource tenant.
2. Use the primary tenant as the main user, email, and Microsoft 365 tenant.
3. Use the existing Zero Trust VPN as the secure access path for approved users who need resource access.
4. Keep Conditional Access and compliant-device requirements active in both environments.
5. Restrict resource access to approved users only.
6. Validate access through a small pilot before expanding to more users.

---

## Recommended Access Flow

Use this simplified access flow:

1. User signs in from a managed or approved device.
2. Device compliance and identity requirements are validated.
3. User connects to the customer’s Zero Trust VPN.
4. Zero Trust VPN provides secure access into the resource network.
5. User accesses approved resources only.
6. Resource-side permissions still control what the user can access.

### Access Flow Summary

```text
Approved User
   |
   v
Managed or Compliant Device
   |
   v
Identity + Conditional Access Validation
   |
   v
Customer Zero Trust VPN
   |
   v
Resource Network
   |
   v
Approved Resource Tenant Workloads
