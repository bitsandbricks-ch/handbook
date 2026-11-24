---
name: new-migration
description: Add a database migration the BitsAndBricks way (Alembic, timestamped, reversible). Use when a change alters the schema.
---

1. Generate it with the project's command (`flask db revision -m "..."` or `alembic revision -m "..."`); keep the timestamped file name it gets, never renumber.
2. Write `downgrade()` too, unless the migration drops data; then say so in the PR.
3. One migration per PR. If CI reports two heads after merging `main`, add a merge revision; don't edit the other branch's file.
4. Test upgrade and downgrade on SQLite and Postgres when the app supports both.
5. A data migration on a large table runs in batches and says how long it takes in the PR.
