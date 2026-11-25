# Git and pull requests

- Never push to `main`. One topic per branch and per PR.
- Branch names: `<type>/<short-topic>` for people (`fix/safari-export`), `claude/<topic>` for agents.
- Open PRs as **drafts** while the work is in progress, fill in the template, link the issue (`Fixes #12` on GitHub, `Closes #12` on GitLab); see [issues](issues.md).
- Before pushing, run what CI runs. Fix failing checks yourself; never skip, disable or weaken a test or a check to get green.
- **Done** = task complete, checklist ticked, checks green; then mark the PR **ready for review** right away.
  A draft means work in progress; never leave a finished PR as a draft.
- **Auto-merge by default** ([0017](../decisions/0017-auto-merge-by-default.md)): when a PR is done and marked ready,
  its author (person or agent) turns on GitHub auto-merge with squash
  (`gh pr merge <n> --auto --squash`). GitHub merges it once the required check passes; nobody clicks merge.
  Turn it on when the PR is marked ready: its own review done (the [pr-review skill](../skills/pr-review/SKILL.md) for agents)
  and the screenshots in. Don't wait for CI: GitHub waits for the required check, and it refuses auto-merge on a PR
  whose checks have all passed already. Then squash-merge it directly (`gh pr merge <n> --squash`), which is what
  auto-merge would have done. A new push that needs another look: turn auto-merge off first.
- No auto-merge, the maintainer merges by hand: PRs whose merge deploys or publishes to public production
  (infra applies, the marketplace index, a site deployed on merge), version PRs from `mason bump`, PRs that touch
  keys, signing, sign-in, the plugin sandbox or release workflows (`CODEOWNERS` security paths), and handbook PRs.
  The maintainer can turn auto-merge off on any PR.
- Never merge by hand, never force-push a branch someone else works on, never push a tag, never change a version. Releasing is the maintainer's call.
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
