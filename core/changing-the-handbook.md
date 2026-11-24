# Changing the handbook

Projects feed the handbook. The test for where a decision goes:
**would another repo have to change, or should another repo do it the same way?**

| Answer | Where it goes | Examples |
| --- | --- | --- |
| Yes | The handbook: a decision record in `decisions/` or a chapter change | A shared component, a stack choice, the plugin spec or agent protocol, a UI pattern others should copy, a security rule |
| No | The project's own `docs/decisions/`, same format | One app's job queue design, one app's member roles |

## How a change flows

1. Someone (a person or an agent) meets something the handbook doesn't cover, or wants to differ from it.
2. They say so in the project PR under a **Handbook** heading.
3. If it is reusable, they open a draft PR here with the decision record or chapter change, and link the two PRs. The project PR may merge first as a **pilot**.
4. When the handbook PR merges, the rule applies to every repo. The PR says which repos it affects and how they follow (usually `mason sync`, or a follow-up PR per repo).

## Rules

- A rule change always comes with its reason; settled questions are not rewritten, a new record supersedes the old one.
- A repo may differ from the handbook with a written reason in its `AGENTS.md`, or an exception with an expiry in `.mason.toml`.
- Design changes follow the same path: a new UI pattern moves into [UI, tokens and brand](../blocks/ui-and-brand.md) once a second app should use it.
- Major versions (`v1`, `v2`) are tagged; repos pin the major version in `.mason.toml`.

> **Avoiding conflicts:** one decision per record file; chapter changes stay small and on one topic.
