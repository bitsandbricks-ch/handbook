---
name: security-review
description: Security review for changes that touch sign-in, roles, tokens, secrets, signing, agents, plugins or webhooks.
---

Read [core/security.md](https://github.com/bitsandbricks-ch/handbook/blob/main/core/security.md) first.

1. List every new way in: route, token type, webhook, agent job, plugin capability.
2. For each: who may call it (permission `resource:action`), where the server checks it, what it can write, whether that write is off by default and audited.
3. Secrets: where they come from (env or `*_FILE`), that they never reach logs, responses or the frontend.
4. Input: validated and size-limited; no shell, SQL or template built from input.
5. Signing and keys: no private key in the repo or a workflow running `main`; verification fails closed.
6. Report findings by severity. A finding stays open until the maintainer decides; don't weaken a check to close it.
7. Point out changes to files that `CODEOWNERS` routes to the security team.
