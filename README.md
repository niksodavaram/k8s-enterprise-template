# Kubernetes Platform Blueprint

[![Build](https://img.shields.io/badge/build-passing-brightgreen.svg)]()
[![Kubernetes](https://img.shields.io/badge/kubernetes-platform-blue.svg)]()
[![IaC](https://img.shields.io/badge/IaC-terraform%20%7C%20ansible-orange.svg)]()
[![Security](https://img.shields.io/badge/security-ISM%20%7C%20Essential%208-critical.svg)]()
[![License](https://img.shields.io/badge/license-MIT-lightgrey.svg)]()

> Opinionated, defence‑style Kubernetes platform blueprint for hybrid cloud and edge fleets, aligned with ASD’s Blueprint for Secure Cloud and the Essential Eight. [web:149][web:66]

---

## Overview

This repository is a **reference implementation of an enterprise Kubernetes platform** that can be used as a template for Defence, government and regulated‑industry environments.

It models:

- Multi‑environment clusters (dev, test, pilot, prod) across **on‑prem and cloud** (EKS/AKS). [web:83][web:91]
- A **platform‑engineering** approach: Kubernetes as an internal product consumed by application teams via GitOps and CI/CD. [web:84][web:90]
- **Edge fleets** using MicroShift and GitOps, inspired by ASD’s secure cloud blueprint and ISM/Essential Eight expectations. [web:149][web:161][web:66]

> This is a **blueprint**, not a turnkey product. Most components are provided as well‑structured templates that can be customised and hardened for a specific organisation.

---

## Architecture at a Glance

- **Control planes:**
  - Cloud clusters (AWS EKS, Azure AKS).
  - On‑prem clusters (Kubernetes/OpenShift‑aligned).
  - Edge clusters (MicroShift on RHEL).

- **Fleet management:**
  - Rancher + Fleet concepts for multi‑cluster GitOps. [web:158][web:160]
  - OpenShift GitOps / RHACM patterns for MicroShift edge fleets. [web:161][web:138]

- **Platform services:**
  - Ingress, TLS/cert‑manager, observability (Prometheus/Grafana placeholders), logging.
  - Namespaces, RBAC, NetworkPolicies, Pod Security, policy‑as‑code.

- **Security & compliance:**
  - ISM‑aligned Linux hardening and Kubernetes baselines.
  - ASD Essential Eight‑inspired controls mapped to concrete platform features. [web:66][web:145]
  - Defence‑style incident response, backup and DR runbooks.

High‑level diagrams and detailed explanations live in `docs/architecture/`.

---

## Repository Structure

```text
docs/                  # Architecture, security model, runbooks
clusters/              # Cluster definitions (EKS, AKS, on-prem, edge)
manifests/             # Namespaces, apps, gateways, DBs, policies
infra/                 # Terraform (AWS/Azure/edge) + Ansible (hardening)
ci/                    # GitHub Actions / pipelines, validation & deploy
ops/                   # Backups, upgrades, ops scripts
tools/                 # Local dev helpers (kind/k3d configs)
```

See [`docs/overview.md`](docs/overview.md) for a detailed breakdown of each area.

---

## Key Capabilities

### 1. Multi‑Environment Kubernetes Platform

- Standardised cluster definitions for **dev, test, pilot and prod** across cloud and on‑prem. [web:83][web:91]
- Consistent namespaces for **frontend, BFF, backend, gateways, partner integrations and databases**.
- Environment‑specific overlays (resources, configs, policies) using Kustomize/Helm.

### 2. GitOps & CI/CD

- Git‑centred workflows for both infrastructure and applications.
- Example CI pipelines for:
  - Validating manifests (kustomize + schema checks).
  - Building and scanning container images.
  - Promoting changes across environments via PRs. [web:84][web:87][web:95]
- Designed to integrate with:
  - Rancher Fleet for multi‑cluster GitOps. [web:158][web:160]
  - OpenShift GitOps / Argo CD for MicroShift edge fleets. [web:161]

### 3. Hybrid & Edge Fleet Management

- Cloud clusters (EKS/AKS) model central services and data‑centre workloads.
- Edge clusters (MicroShift on RHEL) represent **remote sites / branches / tactical edge nodes**.
- GitOps‑based rollout of workloads and policies across many clusters:
  - One change in Git → consistent deployment to all targeted clusters/sites. [web:161][web:160]

### 4. Security, ISM & Essential Eight Alignment

This blueprint draws on ASD’s **Blueprint for Secure Cloud** and the **Essential Eight maturity model**. [web:149][web:66]

Examples:

- **Application & OS patching:** Ansible roles and Terraform modules designed to support automated patching and immutable images.  
- **Application control & hardening:** Admission policies, network segmentation, Pod Security and hardened base images. [web:148]
- **Restrict admin privileges:** Clear separation between platform and app‑team RBAC roles; central logging of admin actions. [web:66]
- **Backups & recovery:** Example Velero scripts and DR runbooks under `ops/backup` and `docs/runbooks/`.

These are templates and must be adapted to each organisation’s risk and accreditation requirements.

---

## Getting Started

1. **Clone the repo**

```bash
git clone https://github.com/<your-org>/k8s-platform-blueprint.git
cd k8s-platform-blueprint
```

2. **Review the architecture**

- Start with [`docs/overview.md`](docs/overview.md).  
- Then read `docs/architecture/logical-architecture.md` and `edge-fleet-architecture.md`.

3. **Try a local cluster (kind/k3d)**

- Use configs under `tools/kind/` or `tools/k3d/` to spin up a local cluster.
- Apply a minimal platform baseline:

```bash
kubectl apply -k manifests/base/
kubectl apply -k manifests/platform/ingress/
```

4. **Experiment with an app**

- Deploy the sample `frontend` service from `manifests/apps/frontend/overlays/dev/`.

5. **Extend and harden**

- Adapt Terraform modules under `infra/terraform` to your AWS/Azure accounts.
- Customise Ansible roles to match your Linux and security baselines.

---

## Who Is This For?

- **Platform engineering teams** building internal Kubernetes platforms. [web:61][web:69]  
- **Defence and government** teams needing a starting point aligned with ASD’s Blueprint, ISM, and Essential Eight. [web:149][web:145][web:66]  
- **Edge/IoT programmes** that want to manage thousands of edge sites with GitOps and MicroShift.

---

## Status

This is an **active blueprint**:

- Some components are stubs or templates.
- Not all security controls are fully implemented by default.
- It is meant to be cloned, adapted and hardened, not deployed as‑is into production.

---

## License

MIT – see [`LICENSE`](LICENSE).  
Please assess and adapt security controls before using in any real environment.