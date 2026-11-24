# 0001 Stack standard for new apps

**Context:** Hubra and Floorbit run React, TypeScript, Vite, Mantine and TanStack Query with Flask backends; Labdex and Mason run Vue 3; Seatbit runs Next.js.

**Decision:** new apps use React, TypeScript, Vite and Mantine, with Flask, SQLAlchemy and Alembic. Existing Vue and Next.js apps stay as they are and adopt the shared tokens.

**Consequences:** shared components (`@bitsandbricks/ui`) serve the React apps; Vue apps share tokens only.
