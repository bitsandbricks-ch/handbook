# 0012 No changelog for repos that are never released

**Context:** [releases](../core/releases.md) and [docs](../core/docs.md) ask every repo for a
`CHANGELOG.md` built from fragments. Some repos are never released: a static site deploys every
merge to `main` straight to production (bitsandbricks.ch, reyki.me, mortar.ch). Fragments there
would only be collected by a release that never comes.

**Decision:** a repo without versions or releases keeps no `CHANGELOG.md` and no fragments. The
merged pull requests on `main` (one squash commit each) are its history. Once a repo starts
tagging releases, the release rules apply in full.

**Consequences:** [releases](../core/releases.md), [docs](../core/docs.md) and the
[pr-review skill](../skills/pr-review/SKILL.md) say "repos with releases". No repo has to change.
