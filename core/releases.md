# Releases

These rules apply to repos that tag releases. A repo that deploys every merge and is never
released keeps no version and no changelog ([0012](../decisions/0012-no-changelog-without-releases.md)).

- Versions are changed only by the release tooling: `mason bump <minor|patch>` opens a PR with the version and the changelog section; `mason release tag` tags it. Never by hand in a feature PR.
- Changelog: one fragment file per change in `changelog.d/` (or `changes/`), collected into `CHANGELOG.md` at release.
- Where each repo keeps its version and changelog is set in `mason.toml` (`version = ...`, `changelog = [...]`).
- After a release from `release/X.Y`, merge it back into `main`.
- Release notes come from the changelog section; the release workflow attaches signed artefacts (plugin zips, agent packages).

> **Avoiding conflicts:** fragments instead of one shared changelog; versions and lockfiles only touched by tooling.
