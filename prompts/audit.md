# Audit prompt: check a repo against the handbook

Paste this into an agent chat that has the repo attached. It works for app repos, plugin
repos, agents, tools and the cluster repo. The agent reports first and changes nothing until
you pick what to fix.

```
Check this repository against the BitsAndBricks handbook:
https://github.com/bitsandbricks-ch/handbook (read it from main).

1. Pick the profile. Use [handbook] profile in .mason.toml if there is one; otherwise choose
   from profiles/README.md and say why. A repo named <app>-plugin-<name> is a plugin repo:
   use its app's profile plus blocks/modules-and-plugins.md.
2. Read the handbook's core/, the blocks/ that profile lists, and decisions/.
   Read this repo's AGENTS.md, README, CI workflows, settings files and a sample of the code.
3. Check each area and give each a status: ok / gap / differs on purpose / not applicable.
   - Repo basics: README, AGENTS.md (CLAUDE.md imports it), LICENSE as the app decided,
     SECURITY.md, CODEOWNERS, PR and issue templates, labels, .mason.toml.
   - Git and PRs (core/git-and-prs.md, core/github.md): branch rules, required "CI passed"
     check, squash merges, draft PR flow, owner and org the repo should live in.
   - CI/CD: lint, typecheck, tests, build; actions pinned to full SHAs with version comments;
     minimal permissions; no ${{ github.event.* }} in run:; release flow via mason bump/release
     (core/releases.md); images tagged as the handbook says.
   - Dependencies: Renovate config extends the shared preset (decision 0005); no Dependabot
     version updates left over.
   - Code and stack (blocks/README.md): matches the standard, or the difference is known.
   - Security (core/security.md): deny by default, permission checks on the server, scoped
     tokens, no secrets or real hosts, domains or IPs in the repo or its history.
   - UI (blocks/ui-and-brand.md): shared tokens, own accent and logo in brand/, no
     copied components that @bitsandbricks/ui will provide.
   - Docs and decisions: docs/decisions/ for local decisions, changelog kept.
   - Plugin repos also: plugin.yaml (id, version, app API range, capabilities, permissions);
     signed release zip; release workflow pins the app to a release tag, never main;
     files synced from <app>-plugins-template; SDK licence allows the plugin's licence;
     listed in the app's marketplace index.
   - Agents also: blocks/local-agents.md and blocks/agent-protocol.md (signed packages,
     outbound-only, scoped token).
   - Cluster repo also: blocks/kubernetes.md and its four spec pages.
4. Report as one table: area, status, finding with file:line or setting, the handbook page,
   and the fix with its size (S/M/L). Then list the top 5 fixes in order.
   Say which findings need a setting changed by the maintainer or Mason (mason doctor --fix)
   rather than a commit.
5. Where this repo does something better than the handbook, or the handbook doesn't fit,
   list it under "Handbook feedback" (core/changing-the-handbook.md). Don't change the handbook.

Rules: change nothing until I say which fixes to do. Then one topic per branch and draft PR,
following core/git-and-prs.md. Never merge by hand, never push tags, never change versions, never
weaken a check. If something is unclear, ask.
```

To check many repos at once, run the same prompt per repo; `mason doctor --summary` covers
the settings part across all of them.
