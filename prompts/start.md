# Start prompt: work in a repo by the handbook

Paste this at the start of a chat or project with any coding agent. For a repo you work in
often, put the same rules in its `AGENTS.md` instead (start from [the template](../templates/AGENTS.md)).

```
This project follows the BitsAndBricks handbook: https://github.com/bitsandbricks-ch/handbook

Before you start:
1. Read the handbook's README, all of core/, and the profile that fits this project
   (profiles/README.md). If the repo has a .mason.toml with a [handbook] profile, use that.
2. Read the blocks/ chapters that profile lists, and decisions/ so you don't reopen settled questions.
3. Read this repo's AGENTS.md. Where it differs from the handbook, AGENTS.md wins for this repo.

While you work:
- Follow core/git-and-prs.md: your own branch, a draft PR with the template, marked ready
  for review once done and green, never merge, never push tags, never change versions by hand.
- Follow core/security.md: read-only first, permission checks on the server, no secrets,
  no real hosts, domains or IPs (use example.com).
- Use the skills in skills/ when they fit (release, new-migration, pr-review,
  security-review, new-plugin), and run pr-review on your own diff before you push.
- If the handbook doesn't cover something, ask instead of guessing.

Feeding the handbook (core/changing-the-handbook.md):
- When you make a decision another repo should follow, or you differ from the handbook,
  say so under a "Handbook" heading in your PR and offer a handbook PR with a decision record.
- Decisions that only matter here go into this repo's docs/decisions/.

If this repo has no AGENTS.md yet, propose one from templates/AGENTS.md as your first PR.
```
