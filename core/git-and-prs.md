# Git and pull requests

- Never push to `main`. One topic per branch and per PR.
- Branch names: `<type>/<short-topic>` for people (`fix/safari-export`), `claude/<topic>` for agents.
- Open PRs as **drafts**, fill in the template, link the issue (`Fixes #12`).
- Before pushing, run what CI runs. Fix failing checks yourself; never skip, disable or weaken a test or a check to get green.
- **Done** = task complete, checklist ticked, checks green; then mark ready for review.
- Never merge your own PR, never force-push a branch someone else works on, never push a tag. Merging and releasing are the maintainer's call.
- Keep the PR description current when you push more commits.
- Squash merge: `main` reads one commit per PR.
- Commit messages: imperative subject under 72 characters, body says why.

> **Avoiding conflicts:** small PRs that live a short time. Merge `main` into your branch daily; never rebase a shared branch.
