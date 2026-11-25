# 0017 Pull requests merge themselves once green

**Context:** every PR waited for the maintainer to press merge, even when it was done, reviewed by
its author and green. Finished work sat for hours and went stale against `main`. The rulesets
already make the `CI passed` check required, so GitHub can merge safely on its own.

**Decision:** auto-merge is the default. When a PR is done and marked ready, its author turns on
GitHub auto-merge with squash; GitHub merges it when the required check passes. Nobody merges by
hand. Exceptions stay manual: merges that deploy or publish to public production, `mason bump`
version PRs, PRs on keys, signing, sign-in, the plugin sandbox or release workflows, and handbook
PRs. Tags and versions stay with the maintainer.

**Consequences:** every repo allows auto-merge (already in [repo settings](../core/github.md#repo-settings))
and requires the `CI passed` check on `main`, or auto-merge would merge without CI; `mason doctor`
reports repos where either is missing. [Git and PRs](../core/git-and-prs.md), [agents](../core/ai.md)
and the [prompts](../prompts/README.md) say so. Repos follow with `mason sync`; an `AGENTS.md` that says
"never merge your own PR" changes to this rule.
