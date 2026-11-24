# Security

Secure by design: everything starts closed and read-only, and each extra right is asked for, granted and logged.

## Rules

- **Deny by default.** Read-only first; each write ability is opted into separately. Infrastructure changes go through plan, approve and apply. Public production needs Mike's go-ahead.
- **RBAC.** A permission is `resource:action` (`host:edit`). A role is a set of permissions. Users get roles through groups, scoped to a site, club or tenant. Built-in roles: `viewer`, `operator`, `admin`, `owner`. The server checks every request; hiding a button is not a check.
- **Least access for machines.** Service accounts and API tokens are scoped, expiring, revocable and hashed at rest. Agents run as a limited user; root is a per-host opt-in on both ends.
- **Sign-in.** SSO across apps where possible, MFA required for admins, sessions that expire.
- **Secrets.** Environment variables or `*_FILE`, a vault, never in a repo, screenshot or log. Each secret has an owner and a rotation plan.
- **Web and data.** Validate all input (Pydantic, zod, or the schema library an existing app already uses, [0007](../decisions/0007-validation-library-per-app.md)), CSP, CSRF protection, rate limits, no secrets or personal data in logs.
- **Audit.** Every write is logged with who, what and when, and kept for a set period.
- **Supply chain.** Pinned actions, signed releases and plugins, an SBOM per image, dependency review on every PR. `CODEOWNERS` routes keys, signing, sign-in and plugin sandboxes to a security review; PRs that touch them say so.
- **Threat model.** One page per app (`docs/threat-model.md`), updated when a new way in is added (API, agent, plugin, webhook).

## Checklist for a new feature

- [ ] Which permission does it need? Is it checked on the server?
- [ ] Does it write anything outside the app? Is that off by default?
- [ ] Is every write audited?
- [ ] Any new secret? Where does it live, who rotates it?
- [ ] Any new input? Validated and size-limited?
