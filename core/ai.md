# Agents and AI

Coding agents work like any contributor: on a branch, through a draft PR, and they never merge, release or touch production.

## Instructions

- `AGENTS.md` is the one instruction file every tool reads (Claude Code, Codex, Copilot, opencode). `CLAUDE.md` only contains `@AGENTS.md`.
- `AGENTS.md` names the profile, links this handbook, and lists only what is specific to the repo. Start from [the template](../templates/AGENTS.md).
- Commits written by an agent carry a `Co-Authored-By` line; PR bodies say they were generated.

## Prompts

- For chats and one-off jobs, use the [prompts](../prompts/): start, audit, adopt, new project, catch-up, UI migration.
- A prompt that gets pasted again and again inside repos becomes a skill.

## Skills

- BitsAndBricks skills live in [`skills/`](../skills/) here and are synced into repos by `mason sync`, pinned by hash (like Hubra's `skills-lock.json`).
- Third-party skills are added only after a read-through: a skill is instructions an agent will follow.
- Planned first set: `release`, `new-migration`, `new-module`, `new-plugin`, `docs-screenshots`, `pr-review` (this handbook as a checklist), `security-review`.

## Tools

| Use | Tool | Note |
| --- | --- | --- |
| Coding agent | Claude Code, local and in the cloud | Main tool; reads `AGENTS.md`, skills and hooks |
| Second agent | opencode or Codex | Optional; the same `AGENTS.md` keeps results consistent |
| Library docs | Context7 MCP | Already in Hubra's opencode config |
| Browser checks | Playwright | e2e tests and screenshots |
| GitHub | GitHub MCP, Mason | Agents open PRs; Mason applies settings |
| App access | Each app's own MCP server (Hubra has `app/mcp`) | Read-only by default, scoped token, audited like any API client |

## AI inside the apps

Opt-in per club or site, provider chosen by the admin, no personal data sent to a provider without consent. Every action an assistant takes goes through the same RBAC and audit as a person.
