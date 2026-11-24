# Agent protocol (version 1)

What a host agent and its hub agree on, whatever language either is written in. Based on the
Labdex agent (`backend/labdex/agent/protocol.py`), which is the reference implementation until
the Go agent passes the same tests.

## Link

- The agent connects **out** to the hub over WebSocket on TLS: `wss://<hub>/api/agents/link`. The host opens no port.
- Headers on connect: `<App>-Agent-Key` (the agent's key, prefix per app, e.g. `lxa_`), `<App>-Agent-Version`, `<App>-Agent-Protocol` (`1`).
- mTLS: the hub is a small CA for its agents. The certificate is issued at enrolment, renewed over the link (a `csr` message) and pinned by SHA-256. Hubs may run it as optional, required or off.
- Messages are JSON objects with a type field `t`; one message is at most 16 MiB.
- Heartbeat every 30 s, carrying metrics. Reconnect with backoff on any close except the fatal ones below.

| Close code | Meaning | Agent does |
| --- | --- | --- |
| 4401 | Unknown or revoked key | Stops |
| 4403 | Runs as root, and this host may not have a root agent | Stops |
| 4426 | Protocol versions don't match; one side must update | Stops |
| 4495 | No or wrong client certificate | Retries later |

## Enrolment

1. An admin creates a one-time enrolment token for one host in the hub.
2. The install command sends the token, host facts and (when mTLS is on) a CSR to `POST /api/agents/enrol`.
3. The hub answers with the agent's key (kept only as a hash on the hub), its certificate, and the hub's job-signing public key, which the agent pins.
4. Revoking the agent in the hub closes the link at once and the key stops working.

## Messages

| `t` | Direction | Purpose |
| --- | --- | --- |
| `hello` | agent → hub | Version, protocol, hostname, whether it runs as root, capabilities switched on locally, job types it knows, host facts, buffered results, certificate fingerprint |
| `heartbeat` | agent → hub | Alive, with metrics |
| `job` | hub → agent | A signed job (below) |
| `ack` | agent → hub | Job accepted or refused, with the reason |
| `result` | agent → hub | Job finished: status, output, data |
| `bundle.get` | agent → hub | Fetch a signed bundle a job refers to (e.g. a playbook) |
| `csr` | agent → hub | Renew the client certificate |

## Jobs

A job travels as `{"t": "job", "job": "<JSON text>", "sig": "<base64 Ed25519 signature>"}`. The
signature covers the exact text, so neither side has to agree on how JSON is serialised.

The envelope inside `job`: `v` (1), `id`, `agent` (the agent's id), `type`, `params` (object),
`issued` and `expires` (Unix seconds).

The agent refuses a job, and says why in its `ack` and both audit logs, when:

- the signature doesn't verify against the pinned hub key;
- it is addressed to another agent;
- `issued` is more than 300 s in the future, or `expires` more than 300 s in the past (clock skew allowed);
- it was seen before (replay);
- the type is not in the allowlist;
- it needs a write capability that is not switched on **in the agent's own config**.

A job not started within 600 s is dropped.

## Job types (Labdex today)

| Type | Writes | Timeout |
| --- | --- | --- |
| `ping` | no | 10 s |
| `facts` | no | 30 s |
| `discovery.services` | no | 120 s |
| `patch.report` | no | 600 s |
| `patch.apply` | capability `patch` (and `reboot` if it reboots) | 3600 s |
| `ansible.run` | capability `ansible` | 3600 s |

There is no job type that runs a shell command, and there never will be. A new app adds its own
types with the same rules: read-only by default, every write behind a named capability.

## Conformance

TODO: a test suite of recorded message exchanges (valid, expired, replayed, wrong agent, bad
signature, missing capability) that every agent implementation must pass in CI.
