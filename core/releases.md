# Releases

- Versions are changed only by the release tooling: `mason bump <minor|patch>` opens a PR with the version and the changelog section; `mason release tag` tags it. Never by hand in a feature PR.
- Changelog: one fragment file per change in `changelog.d/` (or `changes/`), collected into `CHANGELOG.md` at release.
- Where each repo keeps its version and changelog is set in `mason.toml` (`version = ...`, `changelog = [...]`).
- After a release from `release/X.Y`, merge it back into `main`.
- Release notes come from the changelog section; the release workflow attaches signed artefacts (plugin zips, agent packages).

## How a release runs

The same in every repo ([0014](../decisions/0014-one-release-flow.md)):

1. The maintainer runs `mason bump <repo> <patch|minor|major>`; it opens the version PR. The maintainer merges it.
2. The maintainer runs `mason release tag <repo>`; it pushes the `vX.Y.Z` tag on `main` (or `release/X.Y`).
3. The tag starts the shared release workflow from `bitsandbricks-ch/.github`. It waits for the `release`
   environment (the maintainer approves), then publishes: images (see [image tags](#image-tags)), signed
   artefacts, npm packages to GitHub Packages.
4. Only after everything is published, the workflow creates the GitHub release with the changelog
   section as notes and the artefacts attached.

- Immutable releases are on in every repo: a published release, its tag and its assets never change.
  A failed or wrong release is fixed with the next patch version, never by moving a tag.
- Nobody creates a release or a `v*` tag in the GitHub web UI.
- A pre-release is a version with a suffix (`1.4.0-rc.1`); it publishes the same way but is never `latest`.

## Image tags

One rule for every repo that builds an image ([0015](../decisions/0015-image-tags.md)):

| Event | Tags | Moves? |
| --- | --- | --- |
| Push to `main` (after CI passed) | `dev`, `sha-<7hex>` | `dev` moves; `sha-*` never |
| Release `vX.Y.Z` | `X.Y.Z`, `X.Y`, `X`, `latest` on the same digest as `sha-<7hex>` (promoted, not rebuilt) | `X.Y`, `X`, `latest` move; `X.Y.Z` never |
| Pre-release `vX.Y.Z-rc.N` | `X.Y.Z-rc.N` only | never |

- Images are multi-arch (`amd64`, `arm64`) and signed; promotion copies the signed digest.
- GHCR cannot lock a tag, so the shared workflow refuses to push a `sha-*` or `X.Y.Z` tag that already exists.
- Deployments pin a digest or `X.Y.Z`, never `latest` or `dev`; Renovate proposes the updates
  ([Kubernetes](../blocks/kubernetes.md)). A dev or staging environment may follow `dev`.
- No other tags (`master-*`, `last-build`, branch names, long shas).

> **Avoiding conflicts:** fragments instead of one shared changelog; versions and lockfiles only touched by tooling.
