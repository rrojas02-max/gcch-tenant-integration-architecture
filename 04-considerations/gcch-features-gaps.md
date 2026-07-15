# GCC High Feature Gaps and Limitations

## Purpose

This document captures known GCC High gaps, limitations, and validation points that may affect a multi-tenant collaboration and access design.

The goal is to identify what may not behave the same as Microsoft Commercial cloud and what should be validated before implementation.

## Summary of Key Gaps

| Area | Gap / Limitation | Impact |
|---|---|---|
| Cross-cloud B2B collaboration | B2B collaboration is not automatically enabled across Microsoft cloud boundaries | Additional configuration is required if tenants are not in the same cloud boundary |
| Government cloud B2B support | B2B collaboration only works when both government tenants support B2B collaboration | Invitations may fail or users may be unable to redeem access if the tenant does not support B2B |
| Cross-tenant access settings | Inbound and outbound settings must be configured intentionally | Access may be blocked if trust settings are not configured correctly |
| B2B Direct Connect | B2B Direct Connect is primarily for Teams shared channels | It should not be treated as a general-purpose access method |
| B2B Direct Connect across clouds | B2B Direct Connect is not supported for collaboration with Microsoft Entra tenants in a different Microsoft cloud | Not a valid option for cross-cloud collaboration scenarios |
| MFA trust | MFA claims from another tenant are not automatically trusted in every scenario | Users may receive additional MFA challenges or be blocked |
| Device compliance trust | Compliant device claims from another tenant must be intentionally trusted | Full client access may fail if device trust is not configured correctly |
| Cross-tenant synchronization | Cross-tenant synchronization is for B2B user lifecycle automation, not full tenant migration | It should only be used if automated provisioning and deprovisioning is required |
| Cross-tenant synchronization licensing | Cross-tenant synchronization requires licensing validation | Implementation may be delayed if the required licensing is not available |
| Guest lifecycle management | Guest accounts may require governance and cleanup processes | Stale guest access can remain if lifecycle is not automated or reviewed |
| On-premises resource access | B2B collaboration does not provide network access to on-premises resources | VPN, application proxy, or another approved access method is still required |
| Legacy protocols | Apps using LDAP, Kerberos, NTLM, SMB, SSH, or RDP may not work through cloud collaboration alone | These workloads may require VPN, resource-side accounts, or application-specific access |
| Commercial feature parity | Some features available in Commercial may be limited, delayed, or unavailable in GCC High | Each feature must be validated before it becomes a design dependency |
| Automatic redemption | Automatic redemption behavior may not work the same in all cloud or tenant scenarios | User onboarding experience may require additional validation |
| Teams collaboration | Teams shared channels require B2B Direct Connect configuration | Teams collaboration design must be validated separately from standard B2B guest access |
| Conditional Access for external users | External user access depends on the collaboration method, cross-tenant settings, and Conditional Access policies in both tenants | Users may be blocked if policies are not aligned |
