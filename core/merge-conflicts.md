# Avoiding merge conflicts

Most conflicts come from files everyone edits. Each chapter has a short box with its rule; this is the full list.

| Area | Conflict source | Rule |
| --- | --- | --- |
| Changelog | one shared `CHANGELOG.md` | One fragment file per change in `changelog.d/`, collected at release |
| Migrations | numbered files two branches both take | Timestamped Alembic revisions; CI fails on two heads |
| Nav, routes, settings, permissions | one central list | Each module registers its own |
| Translations | one big file per language | One file per module and language |
| Versions, lockfiles | hand edits in feature PRs | Only release tooling and the dependency bot touch them; lockfiles marked generated |
| Formatting | whitespace, import order | Formatters in CI and a pre-commit hook |
| CSS | one global stylesheet | Tokens shared; styles live with their component |
| Branches | long-lived branches | Small PRs, one topic, merge `main` in daily, never rebase a shared branch |
| Shared repo files | the same file edited per repo | Owned by this handbook, synced by `bnb sync` |
