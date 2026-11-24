# Local agents

Every agent follows the same contract, whatever its language. The language follows from where it runs.

| | Floorbit agent | Labdex agent | Plumb |
| --- | --- | --- | --- |
| Runs on | a box in the event hall | every managed server | the cluster (a service, not a client agent) |
| Language, packaging | Python in Docker | Go binary with systemd, .deb and .rpm (moving from a Python zipapp) | Go binary in a small image |
| Why | Bundles Telegraf, syslog, SNMP traps and Python agent plugins on a box you control | Needs host access, can't assume Python or Docker on the host, runs on x86 and ARM | One static binary, low memory, many connections, read-only |

## Choosing

- **Python** when the agent shares code or plugins with a Python server, or runs Ansible.
- **Docker** when it bundles several services on a box you control.
- **A native package** on hosts you manage.
- **Go** when it must be one binary on machines you don't control (Windows, macOS, ARM, routers), or for a small always-on service.

## The contract

- Outbound connection only (WebSocket over TLS); no open ports.
- One-time enrol token, then a per-agent key or mTLS certificate. Revoke cuts the link at once.
- Signed jobs from a fixed list. No shell.
- Each write ability switched on separately, at both ends. Root only as a per-host opt-in.
- Offline buffer, health and metrics, updates only from signed releases, audit log at both ends.
- One written protocol: [agent protocol](agent-protocol.md), with tests that a Go and a Python agent both pass.
