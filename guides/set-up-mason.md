# Set up Mason

[Mason](https://github.com/bitsandbricks-ch/mason-cli) keeps many repos in line: settings, rulesets,
labels, shared files and agent skills. It reads two repos:

- `bitsandbricks-ch/.mason`, the **defaults** (`mason.toml`, rulesets, shared files), which says how
  repos are configured;
- this **handbook**, which says what each kind of project needs (`profiles/<name>.toml`) and which
  skills it gets.

Mason's own README is the reference for every command; this guide is the order to do things in.

## 1. Install

```sh
pipx install "mason-cli[tui,server] @ git+https://github.com/bitsandbricks-ch/mason-cli"
gh auth login                      # Mason talks to GitHub through gh and never sees a token
glab auth login                    # only for GitLab
mason init bitsandbricks-ch/.mason
mason config                       # shows the defaults repo, orgs, groups and the handbook it found
```

Update with the same `pipx install` plus `--force`.

## 2. Point Mason at the handbook

Mason has no handbook built in. It takes the first of:

1. the repo's `.mason.toml`, `[handbook] repo = "bitsandbricks-ch/handbook"` (written by `mason adopt`);
2. `MASON_HANDBOOK` or `handbook = "bitsandbricks-ch/handbook"` in your Mason config;
3. `handbook = "bitsandbricks-ch/handbook"` in `.mason`'s `mason.toml`.

Set it in `.mason` once, so every repo and person gets it:

```toml
# bitsandbricks-ch/.mason: mason.toml
handbook = "bitsandbricks-ch/handbook"
```

Check it: `mason handbook --profiles` lists the profiles, `mason handbook --profile web-app` shows
what one asks for.

## 3. Look before changing anything

```sh
mason doctor --summary     # one line per repo, only checks
mason doctor <repo>        # everything that differs for one repo
```

## 4. Bring one repo in line, then the rest

```sh
mason adopt <repo> --profile <profile>   # one PR: .mason.toml, AGENTS.md, CLAUDE.md, skills
mason doctor <repo> --fix                # settings, labels, security features
mason sync <repo>                        # one PR with the shared files (pre-commit, editorconfig, issue forms)
```

Merge those PRs, run `mason doctor <repo>` again, and only then do all repos:

```sh
mason adopt --profile <profile> <repos...>   # grouped by profile
mason doctor --fix
mason sync
```

Turn on branch rulesets (`protect`, `rulesets` in `.mason`) only after every repo has a `CI passed`
check ([GitHub](../core/github.md)); before that they block every merge.

## 5. Check every checkout offline

`mason check` needs no token, so it runs before a push and in CI:

```sh
mason check --install-hook   # git push runs it first
```

```yaml
# in the repo's CI (pin actions to a sha, as core/github.md asks)
- run: pipx install "mason-cli @ git+https://github.com/bitsandbricks-ch/mason-cli"
- run: mason check
```

It checks `.mason.toml`, rulesets, workflow pinning, the profile's files, a changelog fragment on the
branch, one migration head, the handbook link in `AGENTS.md`, and that synced skills weren't edited
by hand. For the handbook itself, `mason lint .` checks the profiles and skills.

## 6. Web app (optional)

```sh
cp deploy/mason.env.example deploy/mason.env      # your URL, an OAuth app, who may sign in
docker compose -f deploy/docker-compose.yml up -d
mason login https://<your-mason-url>              # the CLI then uses the web app's API key
```

People sign in with GitHub or GitLab, so every call uses their own token. A repo's page has a
handbook panel with the profile, the chapters to read and an **adopt** button; `mason check` and
`mason lint` stay on your machine and in CI.

## Day to day

| Job | Command |
| --- | --- |
| What needs me | `mason prs`, `mason approve`, `mason issues --mine` |
| Drift | `mason doctor --summary`, `mason auto` from a scheduled workflow |
| A handbook change | Merge it here, then `mason sync` (skills, shared files) and the [catch-up prompt](../prompts/catch-up.md) |
| Release | `mason bump <repo>`, merge, `mason release tag <repo>` ([releases](../core/releases.md)) |

> **Avoiding conflicts:** shared files and skills are edited in `.mason` or here and reach repos only
> through `mason sync`, so nobody edits the same file in forty repos.
