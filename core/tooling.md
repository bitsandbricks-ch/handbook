# Tooling: format, lint, hooks

One tool per job, the same locally, in the hook and in CI. A repo's `AGENTS.md` lists the
exact commands.

| Language or file | Format | Lint | Types and tests |
| --- | --- | --- | --- |
| Python 3.12 | `ruff format` | `ruff check` (rules `E,F,I,B,UP,S` as the base) | mypy or Pyright on new code; pytest |
| TypeScript, React | Prettier | ESLint with typescript-eslint | `tsc --noEmit` with `strict`; Vitest; Playwright e2e |
| Vue | Prettier | ESLint with eslint-plugin-vue | Vitest |
| Go | `gofmt` | golangci-lint (includes `go vet`, staticcheck) | `go test -race` |
| Shell | shfmt | shellcheck | |
| YAML | | yamllint | |
| Markdown | | markdownlint | `mkdocs build --strict` for docs sites |
| Dockerfile | | hadolint | |
| Ansible | | ansible-lint (`production` profile) | `--check --diff` against a test host |
| OpenTofu | `tofu fmt` | `tofu validate`, tflint | `tofu plan` in the MR |
| Kubernetes and Flux | | kubeconform, the cluster's own checks | `flux build kustomization` |
| Everything | `.editorconfig` ([template](../templates/editorconfig)) | gitleaks (secrets) | |

## Hooks

- `pre-commit` framework, from [the template](../templates/pre-commit-config.yaml) ([0010](../decisions/0010-pre-commit.md)).
- Installed for three stages: `pre-commit` (format, lint, secrets), `commit-msg` (Conventional Commits),
  `pre-push` (type check, fast tests).
- A hook that takes more than a few seconds moves to `pre-push` or CI.
- Never commit with `--no-verify` to get past a hook; fix the cause. CI runs the same hooks.

## CI

- The `CI passed` job runs `pre-commit run --all-files`, then the repo's tests and build.
- PR titles are checked against Conventional Commits ([0009](../decisions/0009-conventional-commits.md)).
- Tool versions come from lockfiles and the hook config, updated by Renovate.

> **Avoiding conflicts:** formatters own whitespace, import order and quotes, so those never
> show up as conflicts or review comments.
