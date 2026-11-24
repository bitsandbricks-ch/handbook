# Building blocks and the stack standard

The standard is what Hubra and Floorbit already run. Labdex, Mason and Seatbit keep their
stacks and adopt the shared tokens and rules; new apps start on the standard.

| Block | Standard | Today (survey 2026-11-24) |
| --- | --- | --- |
| Web frontend | React, TypeScript, Vite, Mantine, TanStack Query and Table, i18next | Hubra (4 frontends), Floorbit; Seatbit on Next.js and Tailwind; Labdex and Mason on Vue 3 |
| Components | `@bitsandbricks/ui`, grown from Hubra's `packages/ui` Mantine theme | Hubra only |
| Tokens | CSS variables: greys, spacing, radii, fonts, status colours; each app sets its accent | none shared yet |
| API | Flask, Pydantic for config and input, one JSON error format, OpenAPI | Hubra, Floorbit, Labdex, Mason |
| Database | SQLAlchemy and Alembic; Postgres in production, SQLite allowed for small self-hosts | Hubra (Postgres), Labdex (both), Floorbit (JSON file), Seatbit (Drizzle, SQLite) |
| Jobs | A job table and worker inside the app before Redis or Celery | Labdex |
| Auth | SSO across apps, MFA for admins, scoped API tokens | Hubra |
| i18n | German and English from day one | Hubra, Floorbit |
| Deploy | Docker image per app, Compose for self-hosting, k3s and Flux for the cluster | all |
| Observability | `/healthz`, `/metrics` for VictoriaMetrics, JSON logs with an `audit` flag | Plumb, Labdex agents |
| Static sites | Astro | bitsandbricks.ch |
| Small services and agents | Go when one binary must run where you control nothing | Plumb; Labdex agent next |

Dependencies are updated by Renovate in every repo ([decision 0005](../decisions/0005-renovate.md)).

Chapters:

- [Modules, plugins and signing](modules-and-plugins.md)
- [Local agents](local-agents.md)
- [Agent protocol](agent-protocol.md)
- [UI, tokens and brand](ui-and-brand.md)
