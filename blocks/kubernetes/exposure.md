# Exposure, DNS and TLS

Part of [Kubernetes and GitOps](../kubernetes.md).

## Two ingress controllers

- `traefik-public` serves the internet; `traefik-internal` serves the LAN and the mesh VPN.
  Both live in `ingress`, each with a CRD `labelSelector` (`traefik-instance=public|internal`)
  so neither picks up the other's routes, middlewares or TLS stores.
- No IngressClass is the default. Every route says which controller it is for.

## Enforcement, three layers

| Layer | Check |
| --- | --- |
| Kyverno (admission) | `require-traefik-instance-label-on-ingressroute` (Enforce), `require-ingressclass-on-ingress` (Enforce), `forbid-default-ingressclass` (Enforce), `require-namespace-labels` (Enforce), `warn-untagged-vpn-service` (Audit) |
| CI (every MR touching `kubernetes/**/*.yaml`) | A script fails on an IngressRoute without `traefik-instance` or an Ingress without a valid `ingressClassName` |
| NetworkPolicies | On high-value targets; additive, no cluster-wide default deny |

Because both controllers share a namespace, a cross-namespace NetworkPolicy rule always pairs
the `namespaceSelector` with a `podSelector` on `app.kubernetes.io/instance`. A bare namespace
selector would admit the public controller too.

## DNS names

Internal names are hierarchical; the access model is the outermost part:

```
<service>[-<env>].<cluster>.<access>.example.com
  access:  example.com (public) · lan.example.com · vpn.example.com
  cluster: k3s, or the next cluster's name
  env:     a suffix on the service (app-dev), never its own label
```

A wildcard record per cluster and access model (`*.k3s.vpn.example.com`) means services need
no DNS record of their own. Public names are set per app and listed in the cluster's domain map.

## Certificates

- One cert-manager `Certificate` per host, in the app's namespace. No shared wildcard secret,
  no reflected secrets, no default certificate on the internal controller.
- The issuer solves DNS-01, so VPN-only hosts get real certificates without being reachable.
- A route that names a missing secret gets Traefik's self-signed certificate: a browser trust
  error, not a Flux error. Check the served certificate after every new host.

Templates: [certificate](../../templates/kubernetes/app/base/certificate.yaml),
[internal route](../../templates/kubernetes/app/base/ingressroute-internal.yaml),
[NetworkPolicy](../../templates/kubernetes/app/base/networkpolicy.yaml).

> **Avoiding conflicts:** a route, its certificate and its policy live in the app's folder; no
> shared ingress file to edit.
