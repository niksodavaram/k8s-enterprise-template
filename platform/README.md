# Platform

The `platform` directory contains **cluster-wide services** that make Kubernetes usable as an internal platform.

## Typical components

- **Ingress** – NGINX or similar ingress controller plus shared ingress routes.
- **Cert management** – cert-manager, issuers, certificate resources.
- **Monitoring** – Prometheus, Grafana (or placeholders).
- **Logging** – Fluent Bit / Fluentd and log sinks.
- **Policy controllers** – Gatekeeper/Kyverno, admission webhooks, Pod Security configs.
- **Service mesh / internal gateways** (optional).

## Structure

```text
platform/
  ingress/
    nginx-controller/
    ingress-routes/
  cert-manager/
  monitoring/
    prometheus/
    grafana/
  logging/
    fluent-bit/
```

These components are deployed into the `platform` namespace and are usually installed early in the cluster lifecycle so that application teams can consume them as part of the shared platform.