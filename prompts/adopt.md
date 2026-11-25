# Adopt prompt: bring a repo onto the handbook

Use after the [audit](audit.md), with its report pasted in or linked.

```
Bring this repository in line with the BitsAndBricks handbook
(https://github.com/bitsandbricks-ch/handbook, main), using the audit report below.

1. Start with the files every repo needs, in one PR: AGENTS.md from templates/AGENTS.md
   (CLAUDE.md contains only @AGENTS.md), .mason.toml with [handbook] profile and version,
   PR and issue templates, Renovate config extending the shared preset.
2. Then one draft PR per fix from the audit, in the order of its top-5 list. Each PR says
   which handbook page it follows.
3. Settings (branch rules, labels, security features) are not commits: list the exact
   `mason doctor --fix` or `mason settings` commands for the maintainer instead.
4. Where the repo should keep differing from the handbook, write it in AGENTS.md under
   "Differs from the handbook" with the reason, and offer a handbook PR if others should follow.

Turn on auto-merge where core/git-and-prs.md allows it; never merge by hand, never push tags, never change versions, never weaken a check. Ask when unsure.

Audit report:
<paste here>
```
