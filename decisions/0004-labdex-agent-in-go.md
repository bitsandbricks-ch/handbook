# 0004 The Labdex host agent moves to Go

**Context:** the Python agent needs Python and `cryptography` on every host and has never run on a real host.

**Decision:** write the agent protocol as a spec, build the agent in Go against it, keep the Python agent until the Go one passes the same install rehearsal.

**Consequences:** one static binary for x86 and ARM; Ansible still needs Python on hosts it patches.
