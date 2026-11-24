# 0008 Settings admins change live in the app, env gives the default

**Context:** the handbook said config comes from environment variables through one settings object. Hubra keeps most configuration as platform settings in its database, edited in its admin UI; each setting starts from its environment variable until an admin saves a value.

**Decision:** an app may store settings that admins change at runtime in its database and edit them in its admin UI, with the environment variable as the default. Values needed before the database is reachable (database URL, secrets that unlock others) stay environment-only, read through the one settings object. Secrets stored this way are encrypted at rest.

**Consequences:** [code](../core/code.md) names both. Hubra already works this way; no other repo has to change.
