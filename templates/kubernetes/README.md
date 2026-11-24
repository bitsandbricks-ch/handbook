# Kubernetes templates

Starting points for the [Kubernetes spec](../../blocks/kubernetes.md). Replace every `<...>`;
cluster-specific values stay `${VARS}` from `cluster-vars`.

| Path | For |
| --- | --- |
| `namespaces/` | A layer namespace and its `LimitRange` (bootstrap tier) |
| `flux/` | A tier Kustomization and the explicit cluster `kustomization.yaml` |
| `app/` | An app: namespace, Deployment, Service, internal route, certificate, NetworkPolicy, prod overlay |
