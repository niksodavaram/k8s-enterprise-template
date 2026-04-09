# Logical Architecture

This document describes the **logical architecture** of the Kubernetes Platform Blueprint:  
how applications, gateways, databases, platform services and edge nodes fit together across environments.

---

## 1. High-Level View

At a high level, the platform consists of:

- **Kubernetes clusters** in multiple environments (dev, test, pilot, prod) running on:
  - Cloud: AWS EKS, Azure AKS.
  - On‑prem: Kubernetes/OpenShift‑aligned clusters.
  - Edge: MicroShift on RHEL for remote/branch/tactical sites.
- **Platform services** that make Kubernetes consumable as an internal platform (ingress, cert management, monitoring, logging, policy controllers).
- **Application domains**:
  - Frontend (UI)
  - BFF (Backend-for-Frontend)
  - Backend microservices
  - Gateways (client, service, partner)
  - Databases and caches
- **Fleet management & GitOps**:
  - Central Git repositories (this blueprint).
  - CI/CD pipelines.
  - Rancher/Fleet and/or OpenShift GitOps/RHACM patterns for multi‑cluster and edge fleets.

The focus is on **platform engineering**: treating Kubernetes and its supporting services as a product for engineering teams.

---

## 2. Environments and Clusters

Environments are modelled explicitly:

- **Development (dev)** – rapid iteration, relaxed policies, feature branches.
- **Test** – integration and system testing, closer to production config.
- **Pilot** – limited‑scope production trial (e.g. a small set of sites).
- **Production (prod)** – full production load, strict controls and change management.

Each environment is backed by one or more clusters:

- `eks/dev`, `eks/test`, `eks/pilot`, `eks/prod`
- `aks/dev`, `aks/prod`
- `onprem/dc1`, `onprem/dc2`
- `edge/microshift/sites/*` (branch/tactical sites)

Clusters share common patterns (namespaces, platform components, policies) but can differ in size, topology and enabled features.

---

## 3. Namespaces and Application Domains

Namespaces provide isolation and clear ownership:

- `platform` – shared services (ingress, cert-manager, monitoring, logging, policy controllers).
- `frontend` – web/UI applications.
- `bff` – backend-for-frontend services that aggregate backend APIs.
- `backend` – core business microservices and domain services.
- `gateways` – API gateways (client, service, partner).
- `databases` – in-cluster stateful components (where required).
- `partners` – partner-facing integrations with stricter policies.

Each application or gateway is deployed as a microservice in its domain namespace, with environment-specific configuration overlaid via Kustomize/Helm.

---

## 4. Traffic Flow and Gateways

Typical request path:

1. **External client** → DNS → Ingress / Load Balancer (e.g. NGINX ingress in `platform`).
2. Ingress routes traffic to the **Client Gateway** in `gateways` (edge/API gateway).
3. The Client Gateway authenticates, authorises and routes to:
   - **BFF** services for UI APIs, or
   - **Service Gateway** for internal service-to-service APIs.
4. The BFF/Service Gateway calls **Backend** microservices.
5. Backend services access **databases/caches** via internal services or cloud DB endpoints.

**Partner Gateway** handles partner‑facing APIs separately, with additional controls (IP allowlists, mTLS, stricter rate limits).

---

## 5. Data and Databases

Data services can be:

- **Managed cloud DBs** (e.g. AWS RDS, Azure Database) defined in `infra/terraform`.
- **In-cluster DBs** (Postgres, Redis) defined under `databases/` when local state is required.

Key aspects:

- Each service uses per‑environment connection configuration.
- NetworkPolicies restrict which namespaces can talk to DB services.
- DB provisioning and hardening are automated via Terraform + Ansible where possible.

---

## 6. Platform Services

The `platform` components provide shared capabilities:

- **Ingress** – NGINX or equivalent ingress controller and ingress rules.
- **Cert Management** – cert-manager and issuers for TLS certificates.
- **Monitoring** – Prometheus + Grafana (or equivalent) for metrics and dashboards.
- **Logging** – Fluent Bit/Fluentd shipping logs to a central store.
- **Policy Controllers** – Gatekeeper/Kyverno for policy-as-code:
  - Enforcing image sources, privilege restrictions, required labels, etc.
- Optional: service mesh or internal gateways for advanced traffic management.

These run primarily in the `platform` namespace and are installed early in cluster bootstrap.

---

## 7. Hybrid and Edge Fleet Architecture

For hybrid and edge scenarios:

- Central clusters (EKS/AKS/on‑prem) host core services, control planes and observability.
- Edge clusters (MicroShift on RHEL) run at remote/branch/tactical sites with:
  - Local workloads that can operate **offline‑first**.
  - GitOps agents that reconcile from this repository (or a dedicated edge repo).
- A central control plane (e.g. Rancher/Fleet or RHACM/OpenShift GitOps) manages:
  - Cluster registration and grouping.
  - Policy distribution (security baselines, resource constraints).
  - Application rollout across many sites.

This pattern is designed to scale from a handful to thousands of sites while keeping operations consistent and auditable.

---

## 8. Security and Compliance Overview

Security is treated as a first‑class concern:

- **Linux hardening** via Ansible roles (CIS/ISM-aligned baselines).
- **Kubernetes hardening**:
  - Pod Security, NetworkPolicies, RBAC, restricted use of privileged features.
  - Policy-as-code via Gatekeeper/Kyverno.
- **Essential Eight alignment** (patching, application control, limiting admin privileges, logging, backups) is documented in the security model and reflected in automation where possible.
- **Auditability and GitOps**:
  - Changes to infra and workloads go through Git and CI/CD, providing traceability and review.

This blueprint is intended to be adapted and extended to meet specific ISM, PSPF and organisational accreditation requirements.