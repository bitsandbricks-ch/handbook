# Flux layout and reconciliation

Part of [Kubernetes and GitOps](../kubernetes.md).

## Repo layout

```
kubernetes/
├── clusters/<cluster>/          # one folder per cluster: Flux Kustomizations, cluster-vars
│   ├── kustomization.yaml       # explicit list, never auto-generated
│   └── cluster-vars.yaml        # the only place for cluster-specific values
├── infrastructure/
│   ├── namespaces/              # every layer namespace + limitranges.yaml
│   ├── controllers/             # tier file referencing tool folders in place
│   └── <layer>/<tool>/          # helmrepository.yaml, helmrelease.yaml, kustomization.yaml
├── apps/<app>/                  # base/ + overlays/<env>/, or flat for one environment
└── secrets/                     # SOPS-encrypted, outside clusters/<cluster>/
```

A tool folder holds a HelmRepository, a HelmRelease and a `kustomization.yaml`, nothing else.

## Tiers

One Flux Kustomization per **tier**: what can be applied at the same moment. Not one per tool
(long `dependsOn` chains where one broken unit stalls everything) and not one bundle (CRDs
mixed with their instances, one bad Deployment holding the whole cluster).

```
bootstrap                 namespaces, limit ranges, SOPS secrets
  └─ infra-controllers    CRDs, operators, ingress controllers   (wait, 15m)
       ├─ security        issuers, external-dns
       ├─ system          policies, DNS, operators that ship CRs
       │    └─ platform   postgres, cache, queue
       │         └─ middleware   SSO, VPN   (also → security)
       └─ operations      dashboards, monitoring   (→ security)
  apps                    application workloads   (→ security)
```

Rules:

- **No Kustomization holds a CRD and an instance of it.** A CRD installed by a HelmRelease
  is not in the kustomize build, so Flux can't order it against a CR in the same unit.
- A folder's **layer** is where it sits on disk; its **tier** is which Kustomization applies
  it. The tier file references folders in place by relative path.
- `dependsOn` names a tier, never a tool, and only for a real apply-time need. Referencing a
  Middleware is not a reason to depend on the SSO being healthy.
- `wait: true` only where something downstream needs readiness. Leaf tiers (`operations`,
  `apps`) use `wait: false`; add targeted `healthChecks` instead of a blanket gate.
- Always `prune: true`. Defaults: `interval: 1h`, `retryInterval: 1m`, `timeout: 5m`.
- `clusters/<cluster>/kustomization.yaml` is explicit. Without it Flux sweeps every YAML in
  the folder, including ones it can't decrypt.

Template: [templates/kubernetes/flux/](../../templates/kubernetes/flux/).

## Per-cluster values

`infrastructure/` and `apps/` hold no cluster-specific values. Domains, issuer names, the ACME
contact and storage classes come from the `cluster-vars` ConfigMap through
`postBuild.substituteFrom`. A second cluster is a new `clusters/<name>/` folder, not a fork.

- Validate with `flux build kustomization <name> --path <dir> --kustomization-file <file>`;
  `kubectl kustomize` leaves `${VAR}` unresolved.
- A literal `$` that isn't a variable is doubled: `$${1}` yields `${1}`. This bites in Traefik
  regex captures.

## Secrets

- SOPS with age. Encrypted Secrets live in `kubernetes/secrets/`, decrypted by Flux at apply time.
- Only the Kustomization that applies that folder has a `decryption:` block.
- A plain-text secret found in the repo is rotated, not just deleted.

## Resources

- Explicit requests wherever a chart exposes them. Every namespace has a `LimitRange` with a
  `defaultRequest` floor as the backstop for sidecars; it must land with the namespace, since it
  only applies to pods admitted after it.
- No CPU limit on request-path and database workloads: a CFS quota turns a spike into timeouts.
- BestEffort pods show up as "the cluster is slow" while nodes look idle. Check QoS first.

> **Avoiding conflicts:** a new tool is a new folder plus one line in its tier file; values go
> in `cluster-vars`, never inline.
