
# Constraints

## Environment Constraints

This architecture is designed for a Microsoft 365 GCC High scenario.

The following environment constraints apply:

- Both organizations operate in Microsoft 365 GCC High.
- The acquiring organization is primarily cloud-only.
- The acquired organization uses a hybrid identity model.
- The acquired organization maintains on-premises Active Directory.
- Users and devices in the acquired organization are synchronized from on-premises Active Directory to Microsoft Entra ID.
- The acquired organization’s current environment must remain operational.
- Immediate tenant consolidation is not part of the initial design.
- Both tenants must remain logically and administratively separate.

---

## Identity Constraints

The design must account for the following identity constraints:

- Users from the acquiring organization do not natively exist in the acquired organization’s on-premises Active Directory.
- The acquired organization still depends on on-premises Active Directory for users, devices, and applications.
- Some legacy applications may require traditional Active Directory authentication.
- Some applications may depend on Kerberos, NTLM, LDAP, or domain-joined resources.
- A cloud-only identity may not be sufficient for all on-premises access scenarios.
- Duplicate or shadow accounts may be required for some legacy access patterns.
- Identity lifecycle management must be carefully controlled across both tenants.

---

## GCC High Constraints

GCC High environments can have different feature availability compared to commercial Microsoft 365 environments.

The architecture must account for the following GCC High constraints:

- Some Microsoft Entra features may not be available at the same time as commercial cloud.
- Some cross-tenant features may have limited availability or different behavior.
- Some Microsoft security and identity capabilities may have delayed rollout in GCC High.
- Some preview features in commercial cloud may not be available in GCC High.
- Third-party integrations may be limited or require additional validation.
- Feature availability must be confirmed before implementation.
- Architecture decisions should avoid depending on unsupported or unavailable services.

---

## Collaboration Constraints

The design must support collaboration while preserving security boundaries.

Collaboration constraints include:

- External users should not receive broad access to the acquired organization’s tenant.
- Access should be limited to approved sites, teams, applications, and data.
- External sharing must be governed using approved policies.
- Tenant relationships must be explicitly configured and controlled.
- Guest access must be reviewed on a recurring basis.
- Collaboration must not bypass compliance or security requirements.
- User experience should be improved where possible, but not at the expense of security.

---

## On-Premises Access Constraints

Accessing on-premises resources is the most complex part of the scenario.

The following constraints apply:

- On-premises resources may not support modern authentication.
- File shares may require SMB and Active Directory-based permissions.
- Legacy applications may require Kerberos or NTLM authentication.
- Internal web applications may require application publishing or proxy-based access.
- Some applications may require network-level access.
- Some applications may not be suitable for direct external access.
- Broad VPN access should be avoided unless required by the application.
- Application-by-application discovery is required before finalizing the access model.
- Not all cloud-based identity patterns solve on-premises authentication requirements.

---

## Security Constraints

The design must follow a conservative security model.

Security constraints include:

- Least privilege must be enforced.
- Access must be explicitly granted, not broadly inherited.
- Conditional Access must be used where supported.
- MFA must be required for external access.
- Device trust should be considered when supported.
- Access from unmanaged or unknown devices should be restricted.
- Sensitive data access must be controlled and auditable.
- Administrative access must remain separate from user collaboration access.
- Security monitoring must include both tenants where possible.
- External users should not receive unnecessary network-level access.

---

## Compliance Constraints

The architecture must respect government cloud and regulated data requirements.

Compliance constraints include:

- The design must consider CMMC alignment.
- The design must consider CUI handling requirements.
- Data sharing must be limited to approved business purposes.
- Access to regulated data must be reviewed and justified.
- Audit logs should be retained according to organizational policy.
- Teams, SharePoint, and application access must follow governance rules.
- Cross-tenant access must not weaken compliance boundaries.
- Any implementation must be reviewed against the organization’s security and compliance policies.

---

## Operational Constraints

The design must be practical for IT teams to operate.

Operational constraints include:

- The solution should avoid unnecessary complexity.
- Implementation should be phased.
- A pilot group should be used before broad rollout.
- Support teams must understand which tenant owns each identity and resource.
- User onboarding and offboarding must be documented.
- Access reviews must be operationally sustainable.
- Application owners must validate authentication and access requirements.
- The solution should be flexible enough to adapt as GCC High capabilities mature.

---

## Design Assumptions

The following assumptions are used for this reference architecture:

- The organizations want to remain in separate tenants for the initial phase.
- The acquired organization’s hybrid identity environment will remain intact.
- Only a subset of users requires cross-tenant access.
- Not all acquired organization resources need to be exposed to the acquiring organization.
- Cloud collaboration and on-premises access should be treated as separate design areas.
- Microsoft-native identity and access controls are preferred where available.
- Legacy applications may require separate access patterns.
- Final implementation depends on application discovery, licensing, and GCC High feature availability.
