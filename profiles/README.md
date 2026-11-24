# Profiles

| Profile | Blocks it uses | Extra rules | Examples |
| --- | --- | --- | --- |
| `web-app` | API, database, jobs, web frontend, components, tokens, auth, i18n, deploy | UI follows the brand guide; Playwright e2e tests | Hubra, Floorbit, Labdex |
| `api-only` | API, database, jobs, auth, deploy, observability | OpenAPI is the contract; versioned paths | Hubra MCP |
| `desktop` (proposed) | web frontend, components, tokens; packaged with Tauri | Signed builds, auto-update from signed releases | none yet |
| `mobile` (proposed) | React Native with Expo; Capacitor when the app is mostly forms and lists | Store release flow, React Native versions of the core components | none yet |
| `static-site` | Astro, tokens | Built and deployed by CI, no server | bitsandbricks.ch |
| `cli-tool` | Python or Go | One install command, `--help` on every command | Mason |
| `agent` | [agent contract](../blocks/local-agents.md), Go or Python | Signed packages | Floorbit agent, Labdex agent |

Desktop and mobile stay "proposed" until the first real app of that kind, so their rules
come from a real project.
