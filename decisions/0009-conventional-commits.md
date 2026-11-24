# 0009 Conventional Commits for commits and merges

**Context:** commit and PR titles differ from repo to repo, so changelogs and release notes need
hand sorting, and agents guess the style.

**Decision:** every repo follows [Conventional Commits 1.0.0](https://www.conventionalcommits.org/en/v1.0.0/).
Because `main` is squash-merged, the **PR title** is the commit that lands, so it must follow the
spec; a CI check enforces it. Commits on a branch follow it too, checked by the `commit-msg` hook.
Types: `feat`, `fix`, `docs`, `refactor`, `perf`, `test`, `build`, `ci`, `chore`, `revert`.
Breaking changes use `!` and a `BREAKING CHANGE:` footer.

**Consequences:** `mason bump` can group changelog entries by type and suggest the next version
(`feat` minor, `fix` patch, `!` major); the maintainer still decides. Old history is not rewritten.
