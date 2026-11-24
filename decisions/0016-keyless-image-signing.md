# 0016 Images are signed keyless with cosign

**Context:** [security](../core/security.md) asks for signed releases and an SBOM per image, but not
how images are signed. Plugin zips use an ed25519 key per app, which has to be stored, rotated and
revoked. Plumb pilots image signing (plumb#10).

**Decision:** images are signed with cosign in keyless mode: the workflow's GitHub OIDC identity
is the signer, and Sigstore's transparency log records it. There is no key to store or rotate.
The image job alone gets `id-token: write`. SBOM and provenance are attached as attestations by
`docker/build-push-action` (`sbom: true`, `provenance: mode=max`). Verifying checks the identity:
`--certificate-identity-regexp '^https://github.com/<org>/<repo>/'` and
`--certificate-oidc-issuer https://token.actions.githubusercontent.com`. Plugin zips and agent
packages keep their ed25519 keys, because the app verifies them offline.

**Consequences:** the shared image workflow in `bitsandbricks-ch/.github` signs this way;
until it exists, repos that build images copy Plumb's steps. Promotion (0015, if accepted) copies
the signed digest, so a release needs no second signature. A cluster may later enforce signatures
with a Kyverno `verifyImages` policy.
