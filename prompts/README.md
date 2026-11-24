# Prompts

Copy-paste prompts for agent chats and projects. A prompt is for a one-off or a chat without
the repo's setup; when the same job repeats inside repos, it becomes a [skill](../skills/)
that `mason sync` ships with the repo.

| Prompt | Use it when | Covers |
| --- | --- | --- |
| [start](start.md) | Starting any agent chat or project on a BitsAndBricks repo | All repos |
| [audit](audit.md) | Checking a repo against the handbook; report first, fix after | Apps, plugin and template repos, agents, tools, sites, the cluster repo |
| [adopt](adopt.md) | Bringing a repo onto the handbook after an audit | All repos |
| [new-project](new-project.md) | Starting a new app, plugin, agent or tool | New repos |
| [catch-up](catch-up.md) | The handbook changed and a repo should follow | Repos whose `.mason.toml` pins an older handbook version |
| [pr-screenshots](pr-screenshots.md) | PR screenshots don't load, or a repo has no CI screenshot comment yet | Every repo with PRs that show UI |
| [ui-migration](ui-migration.md) | Moving an app onto `@bitsandbricks/tokens`, `ui` and `widgets` | Hubra, Floorbit (ui); Labdex, Mason, Seatbit (tokens only) |

Every prompt keeps the same rules: draft PRs marked ready for review once done and green,
one topic per branch, never merge, never push tags, never change versions, never weaken a check, ask instead of guessing.
