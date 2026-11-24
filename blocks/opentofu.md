# OpenTofu

For cloud and API resources that aren't Kubernetes objects: DNS zones, cloud networks and VMs,
GitHub and identity provider settings, object storage. Profile `infra`. OpenTofu (`tofu`), not
Terraform, because of its open licence and built-in state encryption.

## Layout

```
terraform/                    # or opentofu/; one folder per stack
├── modules/<module>/         # reusable, no provider or backend blocks
└── stacks/<stack>/<env>/     # main.tf, variables.tf, outputs.tf, versions.tf, backend.tf
```

A **stack** is what one `tofu apply` changes. Keep stacks small (one DNS zone, one site), so a plan
is easy to review and a mistake can't reach everything.

## Rules

- **Plan in the MR, apply after merge.** CI posts `tofu plan` on every MR; apply runs from CI on
  `main` (or by hand with the plan file) after review. Public production waits for Mike's go-ahead.
- **State:** remote backend with locking and OpenTofu state encryption; never in git, never local
  for shared stacks. State holds secrets, so access to it is access to them.
- **Pinned:** `required_version` and every provider pinned in `versions.tf`; `.terraform.lock.hcl`
  committed with hashes for every platform CI and people use. Renovate updates both.
- **Variables:** typed, with `description` and `validation` where it helps; no defaults for
  anything environment-specific. Secrets come from SOPS (`carlpett/sops` provider) or environment
  variables, are marked `sensitive`, and never appear in outputs.
- **Names:** resources `snake_case`; modules named for what they make (`dns_zone`, not `cloudflare`).
- **No drift by hand.** A change made in a web console is either imported and committed or undone.
  A scheduled `tofu plan` reports drift.
- **Lint:** `tofu fmt`, `tofu validate` and tflint in pre-commit and CI.
- **Imports and moves** use `import` and `moved` blocks in code, not CLI commands, so they are
  reviewed.

Where a project uses Pulumi or Labdex's own IaC plugins instead, the same rules apply: plan in the
review, apply after merge, state encrypted, versions pinned, no drift by hand.

> **Avoiding conflicts:** one stack per folder and per state file; shared code goes into modules,
> which change through their own PRs.
