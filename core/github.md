# GitHub

## Where a repo lives

| Kind | Org | Examples |
| --- | --- | --- |
| A product with its own plugins | its own org | `hubra-app/*`, `labdex-app/*` |
| Shared tools, small apps, the website | `bitsandbricks-ch` | handbook, Mason, Plumb, Floorbit, Seatbit, bitsandbricks.ch |
| The tools published under mortar.ch and their landing page | `mortar-ch` ([0013](../decisions/0013-mortar-ch-org.md)) | mortar.ch |
| Private experiments | the personal account | |

A product org holds the core, its CLI, plugins, the plugin template and the marketplace index together. Anything public moves to an org.

## Repo settings

Enforced by `mason doctor` from `mason.toml` in [`.mason`](https://github.com/bitsandbricks-ch/.mason):

- Auto-merge allowed, delete branch on merge, wiki and projects off unless used.
- Secret scanning and push protection on, private vulnerability reporting on, Dependabot security alerts on; version updates come from Renovate.
- Default branch `main`; squash merge only.
- Description, homepage and topics filled in.

## Branch and tag rules

Kept as JSON in `.mason/rulesets/` and applied by `mason doctor --fix`.

| Target | Rule |
| --- | --- |
| `main` | Changes only through a PR; the required check passes; no force-push; no deletion; conversations resolved |
| `main`, reviews | One approval for apps with more than one maintainer; in solo repos the required check is the gate and the maintainer merges |
| `release/X.Y` | Same as `main`. Fixes land on `main` first, then are picked over. Merged back after each release |
| Tags `v*` | Created only by the release workflow or the maintainer; never deleted or moved |

## Issues and labels

How work is tracked, the label set, forms and boards, on GitHub and GitLab: [issues and tracking](issues.md).

## CI/CD

- Reusable workflows live in `bitsandbricks-ch/.github`: lint, test, build, image, release, sign, CodeQL, dependency review. Repos call them instead of copying.
- Pin every action to a full commit sha with the version as a comment: `uses: actions/checkout@<sha> # v4.2.2`.
- `permissions:` minimal, set per job. Never put `${{ github.event.* }}` text straight into a `run:` script; pass it through `env:`.
- One required check per repo, `CI passed`, that waits on the others. Rulesets never need editing when jobs change.
- Images and releases: see [image tags](releases.md#image-tags) and [how a release runs](releases.md#how-a-release-runs).
- Production deploys go through a GitHub environment with the maintainer as required reviewer. Public production is never deployed without Mike's go-ahead.
- Jobs that hold a signing key pin the app to a release tag, never `main`.

> **Avoiding conflicts:** shared files (issue forms, PR template, dependabot, `.gitattributes`, rulesets) are owned here and synced by `mason sync`. Edit them here, never in the app repo.
