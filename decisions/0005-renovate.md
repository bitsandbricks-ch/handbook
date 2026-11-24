# 0005 Renovate for dependency updates

**Context:** Hubra uses Renovate; the other repos use Dependabot. Dependabot can't update pip git URLs
or version pins inside workflow `run:` lines (Labdex's plugin signing jobs pin `labdex@<tag>` in 22 places).

**Decision:** every repo uses Renovate with one shared preset from `.mason`
(`github>bitsandbricks-ch/.mason//renovate/default`): grouped minor and patch updates, a weekly
schedule, digest pinning for actions, regex managers for version pins in workflows, automerge only
for dev dependencies with green CI. Dependabot stays on for security alerts only.

**Consequences:** `dependabot.yml` version updates are removed by `mason sync`; Renovate needs its
GitHub App installed on both orgs (Mike).
