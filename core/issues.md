# Issues and tracking

Work is tracked in the issues of the repo it changes, on GitHub or GitLab. Chats, notes and
memory are not a tracker: if it matters later, it's an issue.

## Rules

- **Issue first.** Every PR or MR links an issue, except typo fixes, Renovate updates and
  release PRs. An agent that starts work without one opens it first, with what it is about to do.
- **One issue, one outcome.** Bigger work is an epic issue with sub-issues (GitHub sub-issues,
  GitLab child issues or a task list), each small enough for one PR.
- **Write it to be picked up cold:** what is wrong or wanted, why, how to check it is done
  (acceptance criteria as a checklist), and links. No secrets, real hosts or personal data.
- **Link, don't copy.** The PR says `Fixes #12` (GitHub) or `Closes #12` (GitLab); across repos
  `owner/repo#12` or `group/project#12`. The issue closes when the PR merges, not before.
- **Progress lives on the issue:** a short comment when work starts, when it is blocked (with
  `blocked` and what it waits on) and when the decision changes. Decisions made in an issue
  that others must follow become a decision record.
- **Agents** may open issues, comment and link PRs. They never close, re-label or re-prioritise
  an issue they did not open; the maintainer triages.
- **Security reports** go through private vulnerability reporting (GitHub) or a confidential
  issue (GitLab), never a public issue.

## Labels

One set on every repo and both hosts, created by Mason (`mason doctor --fix` on GitHub, the same
names on GitLab):

| Label | Values |
| --- | --- |
| `type:` | `bug`, `feature`, `docs`, `chore`, `security` |
| `area:` | per app, from its `.mason.toml` (`area:topology`, `area:plugins`) |
| `prio:` | `high`, `normal`, `low` |
| status | `needs-triage` (set by the forms), `blocked`, `good first issue` |
| `handbook` | the change affects the handbook ([changing the handbook](changing-the-handbook.md)) |

GitLab's `::` scoped labels need a paid tier, so both hosts use the single colon.

## Lifecycle

| State | GitHub | GitLab |
| --- | --- | --- |
| New, untriaged | open, `needs-triage` | open, `needs-triage` |
| Triaged | labels set, `needs-triage` removed, milestone if planned | same |
| In progress | assignee + linked draft PR | assignee + linked draft MR |
| Blocked | `blocked` + comment saying on what | same |
| Done | closed by the merged PR | closed by the merged MR |
| Won't do | closed as "not planned" with the reason | closed with the reason |

- **Milestones** are releases (`v1.4`) or dated goals; `mason bump` lists what a release closes.
- **Boards:** one GitHub Project per org (`bitsandbricks-ch`, each app org) with the columns of
  the lifecycle; GitLab uses its issue board per group. Mason shows both in one list
  (`mason issues --mine`, `mason issues --label blocked`).
- **Stale:** issues with no activity for 90 days get a nudge from `mason auto` and close 30 days
  later unless labelled `prio:high` or `blocked`.

## Forms and templates

- GitHub: issue forms in `.github/ISSUE_TEMPLATE/` (bug, feature, task) with blank issues off.
- GitLab: `.gitlab/issue_templates/` (Bug, Feature, Task) with the same fields, and
  `.gitlab/merge_request_templates/Default.md` mirroring the PR template.
- Starting points: [templates/issues/](../templates/issues/). They are synced from `.mason` by
  `mason sync`; edit them there.

> **Avoiding conflicts:** one issue per outcome and one PR per issue; the issue is where two
> people (or agents) find out they are on the same thing, before their branches meet.
