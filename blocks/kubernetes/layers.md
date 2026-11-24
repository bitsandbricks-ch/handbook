# Layers, namespaces and labels

Part of [Kubernetes and GitOps](../kubernetes.md).

## Layer model

One namespace per layer. The namespace name is the layer name, except for `apps` (one
namespace per app) and `gitops` (vendor-named). A new tool joins the namespace of its layer.

| Layer / namespace | Holds | Typical tools |
| --- | --- | --- |
| `storage` | Storage backends and volume provisioners | Longhorn, NFS provisioner |
| `security` | Certificates, issuers, DNS automation | cert-manager, external-dns |
| `system` | Operators and controllers the cluster depends on | CloudNativePG, Kyverno, CrowdSec, RabbitMQ operator, VPN operator |
| `ingress` | Ingress controllers (kept apart: they hold cluster-wide watch rights) | Traefik public, Traefik internal |
| `platform` | Backing services shared by several apps | Postgres, Valkey, RabbitMQ cluster |
| `middleware` | Request-path services between ingress and apps | Authentik |
| `operations` | Monitoring, dashboards, ops tools | Headlamp, Uptime Kuma, metrics and dashboards |
| `apps` | Application workloads, one namespace each | `<app>` or `<app>-<env>` |
| `gitops` | Agents installed by outside systems | `gitlab-agent-*` |

A tool may write an object into another layer's namespace (a bouncer `Middleware` in
`ingress`); the folder it lives in decides its layer.

## Naming

- Infrastructure namespaces are the bare layer name. Charts that want their own namespace
  (`cert-manager`, `longhorn-system`) are pinned to the layer's.
- Apps use `<app>` (one environment) or `<app>-<env>` (`hubra-dev`, `hubra-prod`). Pick one
  form per app. Per-app namespaces are the ResourceQuota and NetworkPolicy boundary, so the
  apps layer is never merged.
- Adding a namespace outside `apps` means adding a layer: an [architecture decision](../../decisions/),
  plus the layer table, the Kyverno policy and the cluster's index in the same MR.

## Declaring namespaces

- Every infrastructure namespace is one file in `infrastructure/namespaces/`, applied by the
  `bootstrap` tier together with its `LimitRange` ([template](../../templates/kubernetes/namespaces/)).
  Tool folders hold no `namespace.yaml`.
- App namespaces live in the app's own folder.
- Never `createNamespace: true` on a HelmRelease.

## Required labels

```yaml
metadata:
  labels:
    app.kubernetes.io/managed-by: flux
    cluster.local/layer: <storage|security|system|ingress|platform|middleware|operations|apps|gitops>
    cluster.local/owner: <team-or-person-slug>
```

Kyverno's `require-namespace-labels` policy rejects a namespace without them. When the allowed
values change, the policy must reconcile before any namespace that uses a new value.

Find things by label, not by a directory:

```sh
kubectl get ns -l cluster.local/layer=apps
kubectl get ns -l app.kubernetes.io/managed-by=flux
```

## Namespace index

Each cluster repo keeps a namespace index (namespace, layer, tools, Flux tier) in its
`CONVENTIONS.md`. It is the one shared table, edited in the same MR that adds or renames a
namespace.

> **Renames:** once a namespace holds real data, renaming it recreates its PVCs. That needs a
> backup and restore plan, not a rename.

> **Avoiding conflicts:** one file per namespace, one folder per tool; only the index is shared.
