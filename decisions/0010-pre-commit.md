# 0010 pre-commit for local hooks

**Context:** Hubra already runs the `pre-commit` framework; the other repos rely on CI alone, so
formatting and secrets are caught only after a push.

**Decision:** every repo has a `.pre-commit-config.yaml` started from
[the template](../templates/pre-commit-config.yaml), installed for `pre-commit`, `commit-msg` and
`pre-push`. Hooks are fast and match CI: formatters, linters, secret scan, commit message.
Slow checks (full tests, builds) stay in CI. CI runs `pre-commit run --all-files` as well, so a
skipped hook can't land. Renovate keeps the hook versions current.

**Consequences:** one-time `pre-commit install` per clone (the README says so). Agents run
`pre-commit run --all-files` before they push. Lefthook and Husky are not used.
