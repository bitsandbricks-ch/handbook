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
- Generated files are marked in `.gitattributes` (`linguist-generated`) and never hand-edited.
- Config comes from environment variables (or `*_FILE` for secrets), read through one settings object (Pydantic in Python).

> **Avoiding conflicts:** formatters run in CI and in a pre-commit hook, so whitespace and import order never conflict.
