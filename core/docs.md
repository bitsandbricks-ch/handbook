# Docs

- `README.md`: what it is (one paragraph), a screenshot, how to run it, where things are, licence. Nothing more.
- `docs/` with mkdocs for anything longer; `mkdocs build --strict` in CI.
- `CHANGELOG.md` built from fragments in repos with releases ([releases](releases.md), [0012](../decisions/0012-no-changelog-without-releases.md)).
- `SECURITY.md`, `CONTRIBUTING.md`, `AGENTS.md` in every repo (synced templates where possible).
- Screenshots come from a demo instance with example data, never from a real install.

> **Avoiding conflicts:** one page per topic, so two PRs rarely edit the same page.
