---
name: new-plugin
description: Start a plugin for a BitsAndBricks app from its template, following the shared plugin spec.
---

Read [blocks/modules-and-plugins.md](https://github.com/bitsandbricks-ch/handbook/blob/main/blocks/modules-and-plugins.md).

1. Ask whether it should be a module (in the app repo) or a plugin (own repo, own releases). Default to a module unless others will write or release it separately.
2. A plugin repo is created by the maintainer with `mason new <org>/<app>-plugin-<name>`, which copies the app's plugin template; don't create repos yourself.
3. Fill in `plugin.yaml`: id, version `0.1.0`, the app API range, capabilities, and the smallest set of permissions.
4. Read-only first: write capabilities come in a later PR, each one off by default.
5. Never touch the release workflow's signing step or its pinned app tag.
