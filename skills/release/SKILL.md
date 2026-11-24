---
name: release
description: Prepare a BitsAndBricks release or version bump with Mason. Use when asked to release, bump or tag a version.
---

Follow [core/releases.md](https://github.com/bitsandbricks-ch/handbook/blob/main/core/releases.md).

1. Never change the version, the changelog heading or lockfiles by hand, and never push a tag.
2. Check that the changelog fragments since the last release describe every user-facing change; add missing ones in a normal PR first.
3. The maintainer runs `mason bump <repo> [patch|minor|major]`, which opens the bump PR, and `mason release <repo>` after it merges. Offer the exact commands; run them only when asked.
4. Never create a release or a `v*` tag in the GitHub web UI: the tag comes from Mason, and the release workflow creates the GitHub release after it has published.
5. After a release from `release/X.Y`, open a PR that merges it back into `main`.
