# Databases

This directory contains **Kubernetes manifests for in-cluster data services** (e.g. Postgres, Redis) and their environment-specific overlays.

> Note: The underlying cloud databases (e.g. AWS RDS, Azure Database) are defined in `infra/terraform`. This folder models how applications connect to them, or runs stateful workloads inside the cluster when required.

## Structure

```text
databases/
  postgres-expense/
    base/
      statefulset.yaml
      service.yaml
      configmap.yaml
    overlays/
      dev/
      test/
      pilot/
      prod/
  redis-cache/
    base/
    overlays/
```

## Purpose

- Provide reusable patterns for **stateful workloads** (StatefulSets, PVCs) and connection configuration.
- Show how to manage **configuration per environment** (dev/test/pilot/prod) via overlays.
- Demonstrate **network and security controls** between applications and data services (used together with `policies/` and `namespaces/`).