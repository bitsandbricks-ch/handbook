# 0014 One release flow for every repo

**Context:** the handbook said `mason bump` and `mason release tag`, but not who creates the GitHub
release, whether releases are immutable, or how npm packages publish. The repos differ: some
create releases in the web UI, `bitsandbricks-ch/ui` used changesets. Mike wants every repo the same.

**Decision:** `mason bump` opens the version PR, `mason release tag` pushes the tag, and the shared
release workflow publishes (images, signed artefacts, npm packages) behind the `release`
environment, then creates the GitHub release from the changelog section. Immutable releases are on
everywhere. No releases or tags from the web UI. Repos with npm packages use `changelog.d` and
`mason bump` too, not changesets.

**Consequences:** the reusable release workflow in `bitsandbricks-ch/.github` gains the "create the
release last" step; each repo turns on immutable releases (`mason doctor` checks it). `bitsandbricks-ch/ui`
moves from changesets to Mason after its first release. TODO: `mason bump` must update several
`package.json` versions in one repo (a `version = [...]` list in `mason.toml`); confirm or add it in Mason.
