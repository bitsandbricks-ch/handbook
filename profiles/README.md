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
| `infra` | [Kubernetes and GitOps](../blocks/kubernetes.md) (Flux), [Ansible](../blocks/ansible.md), [OpenTofu](../blocks/opentofu.md), security, releases | Plan or `--check --diff` in the MR, apply after merge; backup and rollback note for production; no apply to public production without Mike's go-ahead; SOPS for secrets | the cluster repo |
| `agent` | [agent contract](../blocks/local-agents.md), Go or Python | Signed packages | Floorbit agent, Labdex agent |
| `service` | API, observability, deploy, Go or Python | Headless: no UI, `/healthz` and `/metrics`, config from env, one container; ships to the cluster | Plumb (Discord bot), workers |
| `plugin` | [modules and plugins](../blocks/modules-and-plugins.md), the app's own profile for code | `plugin.yaml`, changelog fragments collected into `CHANGELOG.md` at release, signed release zip, release workflow pinned to an app tag, files synced from the app's plugin template, listed in the marketplace | `labdex-plugin-*`, `floorbit-plugin-*`, Hubra plugins |
| `marketplace` | [modules and plugins](../blocks/modules-and-plugins.md) | `plugins.yaml` lists the plugin repos; CI builds a signed `index.json` from their releases and publishes it on merge to `main`; no version and no changelog ([0012](../decisions/0012-no-changelog-without-releases.md)); a version is withdrawn in `plugins.yaml`, never by deleting a release | `hubra-plugins`, `labdex-plugins`, `floorbit-plugins` |
| `library` | components, tokens, or the language's packaging | Semantic versions with changelog fragments and `mason bump` ([0014](../decisions/0014-one-release-flow.md)), published by CI, a changelog per package, docs or Storybook, no app-specific code | `bitsandbricks-ch/ui`, plugin SDKs |
| `template` | the profile it creates | Must build and pass CI as it is; every placeholder marked `<...>`; changes reach repos through `mason sync` | `<app>-plugins-template`, `.mason` |
| `docs` | Markdown, MkDocs or Astro | Links checked and `--strict` builds in CI; rules, then a table, then the conflict box | this handbook, app docs sites |

## Checks every profile runs

| Profile | CI beyond `pre-commit run --all-files` and the PR title check |
| --- | --- |
| `web-app` | backend tests, frontend lint, type check, unit tests, build, Playwright e2e, image build |
| `api-only`, `service` | tests, OpenAPI diff (api-only), image build |
| `multi-tool`, `cli-tool` | tests on every supported Python or Go version, package build, web UI build (multi-tool) |
| `library` | tests, type check, build, Storybook or docs build, publish on release |
| `plugin` | tests against the lowest and highest app version it supports, signed package build |
| `marketplace` | `plugins.yaml` check, index build that verifies every archive's checksum and signature |
| `agent` | tests, cross-platform builds, signature check |
| `infra` | `flux build`, kubeconform, ingress label check, `tofu plan`, `ansible-lint`, `--check` runs |
| `static-site`, `docs` | build, link check |

Each profile also has a `<name>.toml` here that Mason reads: the chapters on top of `core/`, the files a repo
must have, the labels, the agent skills `mason sync` copies into `.claude/skills`, and what `mason check` looks
at offline (changelog fragments, one migration head). `mason handbook` prints a repo's chapters, `mason adopt`
starts a repo on a profile, and `mason lint` checks these files. Change a profile here, not in the repos.

Desktop and mobile stay "proposed" until the first real app of that kind, so their rules
come from a real project.
