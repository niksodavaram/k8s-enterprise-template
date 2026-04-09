# Clusters

This directory defines the **Kubernetes clusters and environments** for this platform.  
It describes *which* clusters exist (dev, test, pilot, prod, edge) and how they are grouped, independent of how the underlying cloud/on‑prem infrastructure is created.

## Structure

```text
clusters/
  eks/
    dev/
      cluster-config.yaml
      kustomization.yaml
    test/
      cluster-config.yaml
    pilot/
      cluster-config.yaml
    prod/
      cluster-config.yaml
  aks/
    dev/
      cluster-config.yaml
    prod/
      cluster-config.yaml
  onprem/
    dc1/
      cluster-config.yaml
    dc2/
      cluster-config.yaml
  edge/
    microshift/
      site-template/
        cluster-config.yaml
        bootstrap-apps.yaml
      sites/
        branch-001/
          values.yaml
        branch-002/
          values.yaml
```

- `eks/`, `aks/`, `onprem/`: logical definitions for cloud and data‑centre clusters.
- `edge/microshift/`: patterns for edge sites running MicroShift (single‑node K8s).

## Purpose

- Provide a **single source of truth** for all clusters and environments.
- Attach **labels, annotations and cluster groups** used by GitOps tools and fleet managers (e.g. Rancher Fleet, RHACM).
- Enable consistent rollout of platform add‑ons (ingress, monitoring, policies) via Kustomize/Helm overlays.

> The actual creation of clusters (VPC/VNet, control planes, node groups, etc.) is handled in `infra/` using Terraform and Ansible. This directory focuses on the Kubernetes view of the world.