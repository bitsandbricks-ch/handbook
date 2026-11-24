---
name: pr-review
description: Review a diff against the BitsAndBricks handbook before pushing or when asked to review a PR.
---

Check the diff, not the whole repo. For each point that fails, say file, line and the fix.

- [ ] One topic; the PR template filled in; the issue linked.
- [ ] CI's commands pass locally (from the repo's `AGENTS.md`).
- [ ] No secrets, real hosts, domains, IPs or personal data; examples use `example.com`.
- [ ] New permission or write path checked on the server and audited ([security](https://github.com/bitsandbricks-ch/handbook/blob/main/core/security.md)).
- [ ] No hand edits to versions, lockfiles or shared files synced from `.mason`.
- [ ] Changelog fragment added for a user-facing change (repos with releases only).
- [ ] Migrations timestamped and reversible.
- [ ] Actions pinned to a sha; `permissions` minimal.
- [ ] UI: empty and loading states, accent colour only for plan, approve and apply.
- [ ] Comments say why; no dead code; no new dependency without a reason in the PR.
- [ ] Does this need a handbook update? If another repo should do the same, or this differs from the handbook, add a **Handbook** note and open a handbook PR ([changing the handbook](https://github.com/bitsandbricks-ch/handbook/blob/main/core/changing-the-handbook.md)).
