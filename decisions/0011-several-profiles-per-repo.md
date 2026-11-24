# 0011 A repo may name several profiles

**Context:** `.mason.toml` names one profile. Some repos hold two kinds of project: reyki.me is a
static page on GitHub Pages plus a small Python server and its Kubernetes manifests. No single
profile covers both, and inventing a new profile for each mix doesn't scale.

**Decision:** `profile` in `.mason.toml` is a name or a list of names
(`profile = ["static-site", "service"]`). The repo follows the core rules once and the extra rules
and blocks of every profile it names. The first profile is the main one. `AGENTS.md` says which
folders follow which profile. A repo names a second profile only when a whole part of it is that
kind of project, not for one script.

**Consequences:** [profiles](../profiles/README.md) and the [`.mason.toml` template](../templates/mason.toml)
show the list form. Mason has to accept a list where it reads the profile (it doesn't read the
table yet). No repo has to change; reyki.me is the first to use it.
