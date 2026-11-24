# Shared agent skills

Skills here are synced into each repo's `.claude/skills/` by `mason sync` and pinned by hash.
Each skill is short and points at the handbook chapter it applies, so the rule lives in one place.

| Skill | When an agent uses it |
| --- | --- |
| [release](release/SKILL.md) | Asked to prepare a release or bump a version |
| [new-migration](new-migration/SKILL.md) | A change needs a database schema change |
| [pr-review](pr-review/SKILL.md) | Asked to review a PR or its own diff before pushing |
| [security-review](security-review/SKILL.md) | A change touches sign-in, permissions, secrets, signing, agents or plugins |
| [new-plugin](new-plugin/SKILL.md) | Asked to start a plugin for an app |

Third-party skills are added only after a read-through, since an agent follows them as instructions.
