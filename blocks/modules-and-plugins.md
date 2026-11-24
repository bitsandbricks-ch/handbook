# Modules, plugins and signing

Apps stay small at the core and grow through modules and plugins. Labdex, Hubra and Floorbit
each have a plugin system today; this chapter is the one spec they converge on.

| | Module | Plugin |
| --- | --- | --- |
| Lives in | the app repo, its own folder | its own repo, `<app>-plugin-<name>` |
| Ships | with each app release | on its own releases, as a signed zip |
| Written by | the app's maintainers | anyone, from the plugin template |
| Turned on | in settings | installed from the marketplace |

## Modules

A module owns its folder: routes, nav entries, settings, migrations, permissions, translations
and tests register themselves. Adding a module edits no central list. A module can move out
into a plugin later without a rewrite.

## Plugin spec

- `plugin.yaml` manifest: id, version, the app API range it supports, capabilities, permissions it asks for.
- Permissions are shown before install and enforced at runtime.
- Signed with an ed25519 key per app. The public key ships with the app. Keys rotate with an overlap period and a revocation list.
- One marketplace index repo per app, published by CI.
- One plugin template repo per app, kept in line by Mason (`from = "<app>-plugins-template"` in `mason.toml`).
- The release workflow pins the app to a release tag, never `main`, because it holds the signing key.
- The plugin SDK is permissively licensed even when the app is AGPL, so plugins are not forced to AGPL.

> **Avoiding conflicts:** self-registering modules; shared plugin repo files synced from the template.
