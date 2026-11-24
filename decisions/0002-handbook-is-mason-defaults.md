# 0002 The handbook is Mason's defaults repo

**Context:** `bitsandbricks-ch/.mason` held `mason.toml` and shared files; rules lived nowhere.

**Decision:** the handbook holds the rules and `mason.toml`, `rulesets/` and `files/`. `.mason` is retired once Mason points here.

**Consequences:** one PR changes a rule and its enforcement together. TODO: point `bnb init` at `bitsandbricks-ch/handbook`, then archive `.mason`.
