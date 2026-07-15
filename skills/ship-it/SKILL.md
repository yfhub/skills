---
name: ship-it
description: 実装完了後の一連の作業を一気通貫で実行する — ブランチ作成→コミット→developへのPR作成→マージ→issueクローズ→ブランチ削除。
disable-model-invocation: true
---

# Ship it

Take a finished implementation all the way to merged — through commit, PR, merge, issue close, and branch cleanup — in one **ship**.

Requires `gh` authenticated (`gh auth status`; if it fails, tell the user to run `gh auth login` and stop). Base branch is `develop`.

## Steps

1. **Settle the issue and the branch.** Identify the issue number this work belongs to from the conversation context (if unclear, match against `gh issue list`; if still uncertain, ask the user). Check the current branch:
   - On `develop` or `main` (no feature branch yet): refresh develop, then branch off it — `git checkout develop && git pull` → `git checkout -b <type>/<issue-number>-<slug>` (follow the repo's branch naming convention if it has one).
   - Already on a feature branch: use it.
   - Done when: you are on a feature branch dedicated to this issue, branched from an up-to-date develop.

2. **Commit the implementation.** Commit any outstanding changes following the `commit` skill's conventions.
   - Done when: the working tree is clean and the implementation is committed.

3. **Push and open a PR to develop.** Push the branch, then `gh pr create --base develop --title "<change summary>" --body-file <tmp>`. The body carries a change summary and `Closes #<issue-number>` so the merge auto-closes the issue (write multi-line bodies to a temp file first — inline shell quoting breaks them).
   - Done when: a PR exists targeting develop and its body closes the issue with `Closes #<issue-number>`.

4. **Merge and clean up.** `gh pr merge --squash --delete-branch`.
   - Done when: do not call this done until all three hold — the PR is merged, the issue is closed by `Closes` (verify with `gh issue view <N>`), and the feature branch is deleted both locally and remotely.
