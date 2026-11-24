# 0006 Several migration heads are allowed

**Context:** the handbook said CI fails on two Alembic heads. Hubra lets parallel pull requests each add a migration on the head they saw, so `main` can have several heads after they merge. That way nobody has to add a merge migration after each merge.

**Decision:** an app may allow several heads, as long as heads that are not joined change different tables and a test checks that (Hubra: `tests/test_migrations_heads.py`). Upgrades then always apply every head (`flask db upgrade heads`). A release may join the heads with a merge migration that changes nothing. Apps that don't have such a test keep the old rule: CI fails on two heads.

**Consequences:** [merge conflicts](../core/merge-conflicts.md) lists both options. Hubra already works this way; no other repo has to change.
