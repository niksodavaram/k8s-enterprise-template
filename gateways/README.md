# Gateways

This directory defines gateway components that front and route traffic to backend services.

## Types of gateways

- **Client gateway** – external edge/API gateway handling inbound client traffic, TLS termination, rate limiting, auth.
- **Service gateway** – internal gateway / API mesh entry point for microservices.
- **Partner gateway** – dedicated gateway for partner integrations with stricter controls (IP allowlists, mTLS, custom headers).

## Structure

```text
gateways/
  client-gateway/
    base/
      deployment.yaml
      service.yaml
      configmap.yaml
    overlays/
      dev/
      test/
      pilot/
      prod/
  service-gateway/
    base/
    overlays/
  partner-gateway/
    base/
    overlays/
```

Gateways typically integrate with the **ingress controller** in `platform/` (e.g. NGINX, HAProxy) and rely on NetworkPolicies and RBAC defined under `manifests/` and `policies/`.