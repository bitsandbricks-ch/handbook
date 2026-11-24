# Git and pull requests

- Never push to `main`. One topic per branch and per PR.
- Branch names: `<type>/<short-topic>` for people (`fix/safari-export`), `claude/<topic>` for agents.
- Open PRs as **drafts**, fill in the template, link the issue (`Fixes #12` on GitHub, `Closes #12` on GitLab); see [issues](issues.md).
- Before pushing, run what CI runs. Fix failing checks yourself; never skip, disable or weaken a test or a check to get green.
- **Done** = task complete, checklist ticked, checks green; then mark ready for review.
- Never merge your own PR, never force-push a branch someone else works on, never push a tag. Merging and releasing are the maintainer's call.
- Keep the PR description current when you push more commits.
- Squash merge: `main` reads one commit per PR, and the **PR title** becomes that commit.
- Commits and PR titles follow [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/)
  ([0009](../decisions/0009-conventional-commits.md)): `<type>(<scope>): <subject>`, for example
  `feat(topology): cap zoom at 1.2x` or `fix!: drop the v1 API`. Types: `feat`, `fix`, `docs`,
  `refactor`, `perf`, `test`, `build`, `ci`, `chore`, `revert`. Subject imperative, lower case, under
  72 characters; body says why; `BREAKING CHANGE:` footer for breaking changes.
- The `commit-msg` hook and a PR title check in CI enforce it ([tooling](tooling.md)).
- Branch type prefixes use the same words (`feat/`, `fix/`, `docs/`).

> **Avoiding conflicts:** small PRs that live a short time. Merge `main` into your branch daily; never rebase a shared branch.
