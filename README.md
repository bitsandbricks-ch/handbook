# BitsAndBricks handbook

How we build BitsAndBricks apps: one set of rules for developers and coding agents.
It is also Mason's defaults repo: `mason.toml` here says which settings, rulesets and
files every repo should have, and `bnb doctor` / `bnb sync` apply them.

> Status: **draft v1**, under review by Mike. Nothing here is enforced yet.

## How to read it

A project reads only what applies to it, in three layers:

1. **[Core](core/)**: rules every project follows.
2. **[Building blocks](blocks/)**: one chapter per technical area. Pick what you use.
3. **[Profiles](profiles/)**: one page per project type, listing the blocks it uses.

Each repo has a `.mason.toml` that names its profile and the handbook version it follows,
and an `AGENTS.md` that links here and lists only what is different for that repo.

```toml
# .mason.toml in an app repo
profile  = "web-app"
handbook = "v1"
```

## Contents

| Part | Pages |
| --- | --- |
| Core | [principles](core/principles.md) · [code](core/code.md) · [git and PRs](core/git-and-prs.md) · [GitHub](core/github.md) · [releases](core/releases.md) · [security](core/security.md) · [docs](core/docs.md) · [agents and AI](core/ai.md) · [merge conflicts](core/merge-conflicts.md) |
| Blocks | [stack standard](blocks/README.md) · [modules and plugins](blocks/modules-and-plugins.md) · [local agents](blocks/local-agents.md) · [UI and brand](blocks/ui-and-brand.md) |
| Profiles | [all profiles](profiles/README.md) |
| Decisions | [decision records](decisions/) |
| Templates | [AGENTS.md](templates/AGENTS.md) · [.mason.toml](templates/mason.toml) |

## Layout

```
core/        every project
blocks/      pick what you use
profiles/    web-app, api-only, desktop, mobile, static-site, cli-tool, agent
decisions/   one short record per decision
templates/   AGENTS.md, .mason.toml, repo skeletons
skills/      shared agent skills, synced into repos by bnb sync
brand/       the generic brand guide (app assets live in each app's brand/)
tokens/      shared design tokens package (planned)
mason.toml   groups, settings, rulesets, files to sync (moved from bitsandbricks-ch/.mason)
rulesets/    branch and tag rulesets as JSON
files/       shared files per group
```

## Changing the handbook

Open a draft PR. A rule change that affects other repos says which ones and how they
follow (usually a `bnb sync` run). Releases are tagged `v1`, `v1.1`, ...; repos pin the
major version in `.mason.toml`.
