# Security and Governance Baseline

## Controls represented in this repository

- Restricted Pod Security Admission labels on the demo namespace.
- Non-root container execution.
- RuntimeDefault seccomp profile.
- No privilege escalation.
- Read-only root filesystem.
- Drop all Linux capabilities.
- No automatic service-account token mount for the demo workload.
- CPU and memory requests/limits.
- NetworkPolicy example restricting ingress and egress.
- TLS secret reference in the Ingress example (certificate provisioning is external).

## Production controls to implement

| Domain | Required design |
|---|---|
| Identity | Integrate cloud workload identity; avoid static cloud credentials in Pods |
| RBAC | Define least-privilege service accounts and RoleBindings; restrict cluster-admin |
| Secrets | Use an external secret manager and rotation workflow |
| Supply chain | Pin image digests, scan dependencies/images, sign images, verify provenance |
| Admission | Add policy-as-code (Kyverno or Gatekeeper) and enforce approved registries |
| Network | Validate CNI NetworkPolicy support; restrict egress and east-west traffic |
| Runtime | Add runtime detection and audit logging where required |
| Tenancy | Separate environments and teams with namespaces, quotas, and policies |
| Compliance | Map controls to the organization's regulatory and data-classification requirements |

## Operational cautions

- NetworkPolicy enforcement depends on the cluster CNI. Confirm that the chosen provider/network plugin enforces policies.
- The sample egress policy allows DNS only. Applications requiring other outbound access need explicit, reviewed rules.
- The example Ingress assumes an NGINX ingress controller and an already-created TLS Secret.
- Pod Security Admission labels are enforced only when the cluster supports the feature and the namespace is created with those labels.
- Do not store real credentials or customer data in this repository.

## Security review checklist

- [ ] Cloud workload identity is configured.
- [ ] RBAC is reviewed and least privilege is enforced.
- [ ] External secrets and rotation are operational.
- [ ] Images are scanned, signed, and pinned by digest.
- [ ] Admission policies are enforced.
- [ ] NetworkPolicies are tested with the selected CNI.
- [ ] Audit logs and runtime monitoring are configured.
- [ ] ResourceQuota and LimitRange are applied per namespace.
- [ ] Backup and restore are tested.
