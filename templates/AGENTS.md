# Rules for coding agents

These rules are for AI coding agents (Claude Code, Codex, Copilot, opencode, ...) working on <app>.
`CLAUDE.md` imports this file.

This repo follows the [BitsAndBricks handbook](https://github.com/bitsandbricks-ch/handbook),
profile **<profile>** (see `.mason.toml`). Read its core chapters and the blocks this profile
lists. The rules below are only what is specific to this repo.

## What this is

<one paragraph>

## Before you push

`pre-commit run --all-files`, then <the commands CI runs, e.g. `pytest -q`, `ruff check`, `npm run lint && npm run build`>

## Repo-specific rules

- Commits and PR titles follow Conventional Commits, with these scopes: <scopes>.

- <rule>
