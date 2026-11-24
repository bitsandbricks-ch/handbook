# Profiles

| Profile | Blocks it uses | Extra rules | Examples |
| --- | --- | --- | --- |
| `web-app` | API, database, jobs, web frontend, components, tokens, auth, i18n, deploy | UI follows the brand guide; Playwright e2e tests | Hubra, Floorbit, Labdex |
| `api-only` | API, database, jobs, auth, deploy, observability | OpenAPI is the contract; versioned paths | Hubra MCP |
| `desktop` (proposed) | web frontend, components, tokens; packaged with Tauri | Signed builds, auto-update from signed releases | none yet |
| `mobile` (proposed) | React Native with Expo; Capacitor when the app is mostly forms and lists | Store release flow, React Native versions of the core components | none yet |
| `static-site` | Astro, tokens | Built and deployed by CI, no server | bitsandbricks.ch |
| `cli-tool` | Python or Go | One install command, `--help` on every command, `--json` output | small scripts and helpers |
| `multi-tool` | CLI, plus TUI and web app on the same core; API, database and web frontend blocks for the web part | One core library, interfaces stay thin: every action works from the CLI first, the TUI and web app call the same functions; one version and one package (extras such as `[tui,server]`); the web UI is built into the package and the Docker image | Mason (CLI, Textual TUI, Flask and Vue web app) |
| `infra` | [Kubernetes and GitOps](../blocks/kubernetes.md), security, releases | Flux applies from git; reviewed MRs; backup and rollback note for production; no apply to public production without Mike's go-ahead | the cluster repo |
| `agent` | [agent contract](../blocks/local-agents.md), Go or Python | Signed packages | Floorbit agent, Labdex agent |
| `plugin` | [plugin spec](../blocks/modules-and-plugins.md) | Version in `plugin.yaml`; changelog fragments collected into `CHANGELOG.md` at release; tested against the app at a pinned commit; signed and released by the tag workflow | `floorbit-plugin-*`, the plugin templates, the marketplace index repos |

Desktop and mobile stay "proposed" until the first real app of that kind, so their rules
come from a real project.
