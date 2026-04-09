# Namespaces

This directory defines the **Kubernetes namespaces** used by the platform and application teams.

## Purpose

- Enforce a clear separation between **platform** components and **application** workloads.
- Support RBAC, NetworkPolicy and quota boundaries per team/domain.
- Provide a consistent naming scheme across all clusters and environments.

## Typical namespaces

- `platform` – ingress, cert-manager, monitoring, logging, policy controllers.
- `frontend` – UI/front-end applications.
- `bff` – Backend-for-frontend services.
- `backend` – core business microservices.
- `gateways` – API gateways (client, service, partner).
- `databases` – stateful data services where they are run inside the cluster.
- `partners` – partner-facing integrations with stricter policies.

These namespace manifests are applied early in cluster bootstrap and are referenced by RBAC, NetworkPolicies and application manifests in the rest of the repository.