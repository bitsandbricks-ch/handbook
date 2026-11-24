# Kubernetes and GitOps

For cluster repos (profile `infra`). The detailed, cluster-specific rules live in the cluster
repo's own `CONVENTIONS.md`; this chapter is the part every BitsAndBricks cluster shares.

## GitOps

- Flux applies from git. Nobody runs `kubectl apply`, `helm install` or edits live objects by hand; a hotfix is a commit.
- Every change goes through a reviewed merge request. Production changes carry a backup and rollback note; public production is never applied without Mike's go-ahead.
- Validate before merge: `flux build kustomization` (not plain `kubectl kustomize`, which leaves `${VAR}` unresolved), plus the repo's CI checks.
- One Flux Kustomization per bootstrap tier, not per tool and not one bundle. No Kustomization holds both a CRD and an instance of it. `dependsOn` points at tiers, never at single tools. Always `prune: true`.
- Cluster-specific values (domains, issuers, storage classes) come from one per-cluster ConfigMap via `postBuild.substituteFrom`. A new cluster is a new `clusters/<name>/` folder, not a fork.

## Namespaces and labels

- One namespace per layer (`storage`, `security`, `system`, `ingress`, `platform`, `middleware`, `operations`); apps get one namespace each (`<app>` or `<app>-<env>`). A new layer is an architecture decision.
- Namespaces are explicit manifests, never `createNamespace: true`.
- Required labels on every namespace: `app.kubernetes.io/managed-by: flux`, `cluster.local/layer`, `cluster.local/owner`. Kyverno enforces them.
- The namespace index in `CONVENTIONS.md` is updated in the same MR that adds or renames a namespace.

## Workloads

- Explicit resource requests on everything; a `LimitRange` per namespace as the floor. No CPU limit on request-path and database workloads.
- Image tags pinned (digest or exact version); never `latest`. Renovate proposes the updates.
- Health checks wired to the app's `/healthz`; metrics scraped from `/metrics`; alerts route to the alerting bot.
- Shared backing services (Postgres, cache, queue) live in `platform`; apps reference them, they don't run their own copies.

## Exposure and network

- Every ingress route names its controller explicitly (public or internal); no default ingress class. Kyverno and a CI check both enforce it.
- Internal services get their own certificate per host; no shared wildcard secret.
- NetworkPolicies on high-value targets, each cross-namespace rule paired with a pod selector.

## Secrets

- SOPS-encrypted Secrets in one secrets folder, decrypted by Flux at apply time. Never a plain-text token in the repo; one found there is rotated, not just deleted.
- The Flux Kustomization that applies secrets is the only one with a `decryption:` block, and the cluster's root `kustomization.yaml` stays explicit so nothing is swept in by accident.

## What a cluster expects from an app

An app repo that ships to a BitsAndBricks cluster provides:

- a container that runs as non-root, with a read-only root filesystem where possible;
- `/healthz` (and `/readyz` when start-up is slow) and `/metrics`;
- configuration from environment variables and `*_FILE` secrets, no baked-in hosts;
- an image tagged with its release version (`latest`, `dev`, `sha-<7>` as in [releases](../core/releases.md));
- a short `deploy/` note: ports, volumes, backing services, required secrets by name.

> **Avoiding conflicts:** one folder per tool or app; the namespace index is the only shared table and is edited in the same MR as the namespace.
