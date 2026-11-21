# ADR 0001: Cloud-neutral Kubernetes platform baseline

- **Status:** Accepted for this reference project
- **Date:** 2026-11-21

## Context

The portfolio needs to demonstrate enterprise Kubernetes design while remaining portable across managed Kubernetes offerings.

## Decision

Use Kubernetes-native Deployments, Services, Ingress, HPA, PDB, NetworkPolicy, Kustomize, and Argo CD as the common workload and delivery baseline. Keep cloud-specific infrastructure and identity configuration separate from application manifests.

## Consequences

### Positive

- Declarative and reviewable workload configuration.
- Reusable manifests across managed Kubernetes services.
- GitOps reconciliation and drift correction.
- Explicit security, availability, and resource controls.

### Trade-offs

- Ingress, storage, identity, networking, and load-balancing integrations differ by cloud provider.
- NetworkPolicy requires a compatible CNI.
- HPA depends on a functioning metrics pipeline.
- Argo CD must be installed and configured separately.
- The reference manifests need environment-specific overlays before production use.

## Follow-up

Create and test Terraform modules for EKS, AKS, GKE, and OKE; integrate policy-as-code, image signing, external secrets, and observability.
