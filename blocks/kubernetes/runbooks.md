# Kubernetes runbooks

Part of [Kubernetes and GitOps](../kubernetes.md). Every runbook ends in a reviewed MR; Flux
applies after merge.

## Add an app

1. Copy [templates/kubernetes/app/](../../templates/kubernetes/app/) to `kubernetes/apps/<app>/`
   (base plus `overlays/<env>/`, or flat for one environment).
2. Set the namespace `<app>` or `<app>-<env>` with the three required labels.
3. Point backing services at `platform` (`<service>.platform.svc.cluster.local`); don't run
   a private copy.
4. Add the app folder to the `apps` tier.
5. Add the namespace to the cluster's namespace index.
6. `flux build kustomization apps ...` renders it; CI passes.

## Add an internal service

1. In the app folder: `certificate.yaml` for `<service>.<cluster>.vpn.example.com` and
   `ingressroute-internal.yaml` with `traefik-instance: internal` and the same `secretName`.
2. List both in the folder's `kustomization.yaml`.
3. The owning tier depends on `security` (the issuer must exist first).
4. After apply, open the host from the VPN and confirm the certificate is the real one.

## Add a tool to a layer

1. `infrastructure/<layer>/<tool>/` with HelmRepository, HelmRelease (namespace = layer,
   versions pinned, requests set) and `kustomization.yaml`.
2. Reference it from the right tier: CRD and operator in `infra-controllers`, its instances
   in the next tier.
3. Update the index's tools column.

## Add a layer

An [architecture decision](../../decisions/) first. Then, in one MR: the namespace file and
`LimitRange` in `infrastructure/namespaces/`, the allowed value in `require-namespace-labels`,
the layer table in [layers](layers.md) and the cluster's index.

## Change production

- The MR says what changes, the backup taken and how to roll back.
- Public production (anything on a public domain) waits for Mike's go-ahead after review.
- A hotfix is still a commit. Never `kubectl edit`; Flux would undo it anyway.
