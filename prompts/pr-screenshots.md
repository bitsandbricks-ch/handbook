# PR screenshots prompt: let CI post them

```
Read "PR screenshots" in the BitsAndBricks handbook's core/git-and-prs.md
(https://github.com/bitsandbricks-ch/handbook) and make this repo follow it.

1. Find how this repo's pull requests show screenshots today: the CI job that takes them (if
   any), the PR template, AGENTS.md. Note PRs whose images don't load: links as
   github.com/.../blob/...?raw=true, or links wrapped in backticks.
2. In one draft PR: add the `screenshots-comment` CI job, copied from bitsandbricks-ch/floorbit's
   .github/workflows/ci.yml and .github/scripts/pr_screenshots.py (adapt the job it waits for and
   the artifact name), and update AGENTS.md and the PR template: commit shots to
   .pr-screenshots/<topic>/, remove them in the next commit, point to the CI comment.
3. Don't edit old PR descriptions yourself (your edits can come out wrapped again). List the
   broken ones in the PR for the maintainer.

If the repo is private or has no UI to screenshot, say so and stop after step 1.
```
