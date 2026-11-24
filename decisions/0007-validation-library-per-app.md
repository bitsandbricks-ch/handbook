# 0007 Existing apps keep their validation library

**Context:** the handbook names Pydantic and zod for input validation. Hubra validates every request body with Marshmallow schemas.

**Decision:** new apps validate with Pydantic (Python) and zod (TypeScript). An existing app keeps the schema library it already uses, as [0001](0001-stack-standard.md) does for frontends. What matters is that every input is validated and size-limited, not which library does it.

**Consequences:** [security](../core/security.md) names both. Hubra keeps Marshmallow; no repo has to change.
