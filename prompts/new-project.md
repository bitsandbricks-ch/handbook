# New-project prompt: start an app, plugin, agent or tool

```
I want to start a new BitsAndBricks project: <one paragraph: what it does, who uses it>.
Follow the handbook: https://github.com/bitsandbricks-ch/handbook (main).

1. Propose the profile from profiles/README.md and the repo name and owner from core/github.md.
   A plugin is <app>-plugin-<name>, created by the maintainer with `mason new` from the app's
   plugin template; don't create repos yourself.
2. Write a short plan: stack from blocks/README.md, the blocks the profile uses, data model,
   permissions (core/security.md), what it needs from a cluster (blocks/kubernetes.md) and
   anything that would differ from the handbook, with the reason.
3. Wait for my go. Then scaffold in draft PRs: skeleton with AGENTS.md, .mason.toml, CI,
   Renovate, LICENSE as I choose, brand/ folder; then the first feature.

Ask about anything the handbook doesn't settle (licence, accent colour, domain) instead of
guessing.
```
