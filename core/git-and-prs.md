# Git and pull requests

- Never push to `main`. One topic per branch and per PR.
- Branch names: `<type>/<short-topic>` for people (`fix/safari-export`), `claude/<topic>` for agents.
- Open PRs as **drafts** while the work is in progress, fill in the template, link the issue (`Fixes #12` on GitHub, `Closes #12` on GitLab); see [issues](issues.md).
- Before pushing, run what CI runs. Fix failing checks yourself; never skip, disable or weaken a test or a check to get green.
- **Done** = task complete, checklist ticked, checks green; then mark the PR **ready for review** right away.
  A draft means work in progress; never leave a finished PR as a draft.
- Never merge your own PR, never force-push a branch someone else works on, never push a tag. Merging and releasing are the maintainer's call.
- Keep the PR description current when you push more commits.
- **Screenshots** in a PR: CI posts them as one comment on the PR, kept up to date on every push
  (see [PR screenshots](#pr-screenshots)). Never link images as `github.com/<repo>/blob/<sha>/<path>?raw=true`:
  github.com throttles those and a random few show as broken images.
- Squash merge: `main` reads one commit per PR, and the **PR title** becomes that commit.
- Commits and PR titles follow [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/)
  ([0009](../decisions/0009-conventional-commits.md)): `<type>(<scope>): <subject>`, for example
  `feat(topology): cap zoom at 1.2x` or `fix!: drop the v1 API`. Types: `feat`, `fix`, `docs`,
  `refactor`, `perf`, `test`, `build`, `ci`, `chore`, `revert`. Subject imperative, lower case, under
  72 characters; body says why; `BREAKING CHANGE:` footer for breaking changes.
- The `commit-msg` hook and a PR title check in CI enforce it ([tooling](tooling.md)).
- Branch type prefixes use the same words (`feat/`, `fix/`, `docs/`).

## PR screenshots

Agents can't drag images into GitHub, and image links in a description an agent writes can come out
wrapped in backticks, so they show as text. CI writes the screenshots instead:

1. The PR commits its images to `.pr-screenshots/<topic>/` and removes them in the next commit.
2. A CI job (`screenshots-comment`, after the job that takes screenshots) finds the images the PR
   added, links each as `https://raw.githubusercontent.com/<repo>/<sha of the adding commit>/<path>`,
   and writes or updates one comment on the PR, marked `<!-- pr-screenshots -->`.
3. If the PR added none, it shows the shots CI took, pushed to the `pr-screenshots` branch under
   `pr-<number>/`.
4. The description points to that comment instead of linking images.

The job needs `contents: write` (push to `pr-screenshots`) and `pull-requests: write`, and skips fork
PRs. Floorbit's `.github/scripts/pr_screenshots.py` is the reference to copy. A private repo can't
serve `raw.githubusercontent.com` to readers: there, attach the CI artifact instead.

> **Avoiding conflicts:** small PRs that live a short time. Merge `main` into your branch daily; never rebase a shared branch.
