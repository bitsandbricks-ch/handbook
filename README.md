# BitsAndBricks handbook

How we build BitsAndBricks apps: one set of rules for developers and coding agents.
The rules live here; their enforcement lives in [`bitsandbricks-ch/.mason`](https://github.com/bitsandbricks-ch/.mason),
Mason's defaults repo (`mason.toml`, rulesets, shared files), which `mason doctor` and
`mason sync` apply to every repo.

> Status: **draft v1**, under review by Mike. Nothing here is enforced yet.

## How to read it

A project reads only what applies to it, in three layers:

1. **[Core](core/)**: rules every project follows.
2. **[Building blocks](blocks/)**: one chapter per technical area. Pick what you use.
3. **[Profiles](profiles/)**: one page per project type, listing the blocks it uses.

Each repo has a `.mason.toml` that names its profile and the handbook version it follows,
and an `AGENTS.md` that links here and lists only what is different for that repo.

```toml
# .mason.toml in an app repo (Mason's per-repo settings file)
[handbook]
profile = "web-app"
version = "v1"
```

## Contents

| Part | Pages |
| --- | --- |
| Core | [principles](core/principles.md) · [code](core/code.md) · [git and PRs](core/git-and-prs.md) · [GitHub](core/github.md) · [releases](core/releases.md) · [security](core/security.md) · [docs](core/docs.md) · [agents and AI](core/ai.md) · [merge conflicts](core/merge-conflicts.md) · [changing the handbook](core/changing-the-handbook.md) |
| Blocks | [stack standard](blocks/README.md) · [modules and plugins](blocks/modules-and-plugins.md) · [local agents](blocks/local-agents.md) · [UI and brand](blocks/ui-and-brand.md) · [Kubernetes](blocks/kubernetes.md) ([spec](blocks/kubernetes/), [templates](templates/kubernetes/)) |
| Profiles | [all profiles](profiles/README.md) |
| Decisions | [decision records](decisions/) |
| Agents | [agent protocol](blocks/agent-protocol.md) |
| Brand | [brand guide](brand/README.md) |
| Templates | [AGENTS.md](templates/AGENTS.md) · [.mason.toml](templates/mason.toml) · [prompt for agent chats](templates/prompt.md) |

## Layout

```
core/        every project
blocks/      pick what you use
profiles/    web-app, api-only, desktop, mobile, static-site, cli-tool, multi-tool, agent, infra, plugin
decisions/   one short record per decision
templates/   AGENTS.md, .mason.toml, repo skeletons
skills/      shared agent skills, synced into repos by mason sync
brand/       the generic brand guide (app assets live in each app's brand/)
tokens/      shared design tokens package (planned)
LICENSE.md   CC BY 4.0 for the text, MIT for templates and skills
```

## Changing the handbook

Open a draft PR. A rule change that affects other repos says which ones and how they
follow (usually a `mason sync` run). Releases are tagged `v1`, `v1.1`, ...; repos pin the
major version in `.mason.toml`.
