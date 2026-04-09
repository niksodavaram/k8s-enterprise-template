# Platform Overview

This repository is a **Kubernetes platform blueprint** for building an enterprise‑grade, security‑focused platform across cloud, on‑prem and edge environments.

It is designed as a **template** that platform teams can clone and adapt for Defence, government and regulated‑industry use cases.

---

## Goals

- Provide a **clear, opinionated structure** for Kubernetes platform code (manifests + IaC).
- Model a **multi‑environment** setup (dev, test, pilot, prod) with consistent patterns.
- Demonstrate **platform engineering** principles: Kubernetes as an internal product for application teams.
- Show how to integrate **hybrid cloud and edge fleets** using GitOps and central control planes.
- Align with **ASD Blueprint for Secure Cloud, ISM and Essential Eight** concepts at a practical level. [web:149][web:66]

---

## Repository Layout (High Level)

- `clusters/` – definitions of Kubernetes clusters and environments (EKS, AKS, on‑prem, edge).
- `manifests/` – namespaces, apps, gateways, databases, policies and platform services.
- `infra/` – Infrastructure as Code for underlying cloud/on‑prem resources (Terraform + Ansible).
- `ci/` – CI/CD workflows and scripts (validation and deployment).
- `ops/` – operational scripts, backup and upgrade helpers.
- `docs/` – this overview, architecture diagrams, security model and runbooks.
- `tools/` – local lab helpers (kind/k3d configs, examples).

See [`docs/architecture/logical-architecture.md`](architecture/logical-architecture.md) for how these pieces fit together logically.

---

## Key Concepts

### Multi‑Environment Platform

The blueprint assumes at least four environments:

- **dev** – fast iteration and experimentation.
- **test** – integration/system testing.
- **pilot** – controlled pre‑production rollout (e.g. subset of branches/sites).
- **prod** – full production, strict controls and change processes.

Each environment has one or more clusters (cloud, on‑prem, edge) that share the same patterns but may differ in size or topology.

### Platform vs Application Responsibilities

- **Platform layer**:
  - Cluster lifecycle, networking, ingress, certs, monitoring, logging, policy controllers.
  - Security baselines, RBAC models, GitOps/CI/CD tooling.
- **Application layer**:
  - Frontend, BFF, backend microservices, gateways, partner integrations, data services.

The repository structure reinforces this separation so application teams can consume the platform safely without managing its internals.

### Hybrid and Edge Fleets

The blueprint supports:

- Central clusters in cloud/on‑prem for core services.
- Edge clusters (e.g. MicroShift on RHEL) for remote/branch/tactical sites.
- GitOps‑driven deployment of workloads and policies across many clusters, using patterns compatible with Rancher Fleet and/or OpenShift GitOps/RHACM. [web:158][web:161]

---

## Security and Compliance

Security is built in from the start:

- Linux hardening and node configuration via Ansible roles.
- Kubernetes hardening with Pod Security, NetworkPolicies, RBAC and policy‑as‑code.
- Documentation and patterns that help implement ASD **Essential Eight** and ISM controls in a Kubernetes context. [web:66][web:145][web:149]

This repo does not claim full compliance out‑of‑the‑box; instead, it provides a structured starting point that can be extended and accredited in a specific organisation.

---

## How to Use This Blueprint

1. **Clone** the repository into your own organisation’s GitHub/GitLab.
2. **Review and adapt** the folder structure to your environments and naming conventions.
3. Start with a **single environment and cluster** (e.g. dev on kind/EKS/AKS) and:
   - Apply base namespaces and platform components.
   - Deploy one or two sample apps and gateways.
4. Incrementally add:
   - Terraform modules for your cloud/on‑prem infra.
   - Ansible roles for your security baselines.
   - CI/CD pipelines and GitOps controllers.

The repository is intentionally modular so you can enable or skip components as needed while keeping a consistent, enterprise‑grade structure.