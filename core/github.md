# GitHub

## Where a repo lives

| Kind | Org | Examples |
| --- | --- | --- |
| A product with its own plugins | its own org | `hubra-app/*`, `labdex-app/*` |
| Shared tools, small apps, the website | `bitsandbricks-ch` | handbook, Mason, Plumb, Floorbit, Seatbit, bitsandbricks.ch |
| Private experiments | the personal account | |

A product org holds the core, its CLI, plugins, the plugin template and the marketplace index together. Anything public moves to an org.

## Repo settings

Enforced by `bnb doctor` from [`mason.toml`](../mason.toml):

- Auto-merge allowed, delete branch on merge, wiki and projects off unless used.
- Secret scanning and push protection on, private vulnerability reporting on, Dependabot alerts on.
- Default branch `main`; squash merge only.
- Description, homepage and topics filled in.

## Branch and tag rules

Kept as JSON in [`rulesets/`](../rulesets/) and applied by Mason.

| Target | Rule |
| --- | --- |
| `main` | Changes only through a PR; the required check passes; no force-push; no deletion; conversations resolved |
| `main`, reviews | One approval for apps with more than one maintainer; in solo repos the required check is the gate and the maintainer merges |
| `release/X.Y` | Same as `main`. Fixes land on `main` first, then are picked over. Merged back after each release |
| Tags `v*` | Created only by the release workflow or the maintainer; never deleted or moved |

## Issues and labels

- Issue forms: bug, feature, question. Blank issues off. Security reports go through private vulnerability reporting, never an issue.
- One label set everywhere: `type:bug|feature|docs|chore`, `area:<part of the app>`, `prio:high|normal|low`, `good first issue`, `blocked`.
- An agent never closes an issue it did not open; it links the PR that fixes it.

## CI/CD

- Reusable workflows live in `bitsandbricks-ch/.github`: lint, test, build, image, release, sign, CodeQL, dependency review. Repos call them instead of copying.
- Pin every action to a full commit sha with the version as a comment: `uses: actions/checkout@<sha> # v4.2.2`.
- `permissions:` minimal, set per job. Never put `${{ github.event.* }}` text straight into a `run:` script; pass it through `env:`.
- One required check per repo, `CI passed`, that waits on the others. Rulesets never need editing when jobs change.
- Images: `latest` = newest release, `dev` = newest commit on `main`, plus `sha-<7hex>`.
- Production deploys go through a GitHub environment with the maintainer as required reviewer. Public production is never deployed without Mike's go-ahead.
- Jobs that hold a signing key pin the app to a release tag, never `main`.

> **Avoiding conflicts:** shared files (issue forms, PR template, dependabot, `.gitattributes`, rulesets) are owned here and synced by `bnb sync`. Edit them here, never in the app repo.
