# 0015 Image tags

**Context:** the handbook named `latest`, `dev` and `sha-<7hex>` but no version tags. Labdex follows it;
Hubra builds `sha-<7>` and `last-build` and promotes the same digest on release; Floorbit puts `latest`
on every `master` commit and adds `master-<time>-<sha>` and a 12-character sha.

**Decision:** `main` pushes `dev` and `sha-<7hex>`; a release promotes that digest (no rebuild) to
`X.Y.Z`, `X.Y`, `X` and `latest`; a pre-release gets only its own version. `sha-*` and `X.Y.Z` are never
overwritten. Deployments pin a digest or `X.Y.Z`. One reusable image workflow in `bitsandbricks-ch/.github`,
modelled on Hubra's promotion step.

**Consequences:** Hubra drops `last-build` and adds `dev` and `X`. Floorbit stops moving `latest` on
`master`; its deployments that follow `latest` change to a pinned version first, which is public
production and needs Mike's go-ahead. Labdex already matches.
