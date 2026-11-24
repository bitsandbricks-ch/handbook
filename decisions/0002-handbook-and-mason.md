# 0002 The handbook holds the rules, `.mason` enforces them

**Context:** `bitsandbricks-ch/.mason` is Mason's defaults repo: `mason.toml` groups, settings,
rulesets and shared files. The rules behind them were written down nowhere.

**Decision:** `.mason` stays the defaults repo. The handbook explains each rule; `.mason` holds
the machine-readable version and links back to the handbook. A rule change is two PRs, handbook
first.

**Consequences:** `mason doctor` and `mason sync` keep working unchanged. Mason's old names (`bnb`,
`bitsandbricks.toml`, `bitsandbricks-defaults`) are cleaned out of `.mason`.
