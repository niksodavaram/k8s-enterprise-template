# Manifests

This directory is the **top-level entry point for Kubernetes manifests** in this repository.

It ties together:

- Namespaces (`../namespaces/`)
- Platform services (`../platform/`)
- Application workloads (`../apps/`)
- Databases (`../databases/`)
- Gateways (`../gateways/`)
- Policies (`../policies/`)

Using Kustomize or Helm, you can build and apply complete configurations for a given environment (dev/test/pilot/prod) from here.

Example:

```bash
kubectl apply -k manifests/environments/dev/
kubectl apply -k manifests/environments/prod/
```

Each environment overlay selects the appropriate mix of namespaces, platform components, apps, gateways and database/config overlays for that environment.