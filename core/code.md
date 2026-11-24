# Code

| Language | Format and lint | Types |
| --- | --- | --- |
| Python 3.12 | `ruff format`, `ruff check` | Pyright or mypy on new code |
| TypeScript | eslint, prettier (or the repo's existing formatter) | `strict` |
| Go | `gofmt`, `go vet`, staticcheck | |
| Shell | shellcheck | |

- Comments explain **why**, not what. Short.
- Names: say what a thing is in the words users use; no abbreviations a newcomer can't guess.
- **Dependencies:** add one only when it clearly pays for itself; say why in the PR. Prefer the standard library and what the stack standard already includes. Check the licence is compatible with the app's.
- Dependency updates come from Renovate ([decision 0005](../decisions/0005-renovate.md)); lockfiles change only in its PRs or release PRs.
- Generated files are marked in `.gitattributes` (`linguist-generated`) and never hand-edited.
- Config comes from environment variables (or `*_FILE` for secrets), read through one settings object (Pydantic in Python). Settings admins change at runtime may live in the app's database, with the env variable as default ([0008](../decisions/0008-runtime-settings-in-the-app.md)).

> **Avoiding conflicts:** formatters run in CI and in a pre-commit hook, so whitespace and import order never conflict.
