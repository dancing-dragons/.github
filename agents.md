# Organization-wide agent instructions

<!-- BEGIN ORESOFTWARE MANAGED BRANCHING AND GITOPS POLICY -->
## Required `dev`/GitFlow/GitOps policy

Read and follow [`BRANCHING_AND_DEPLOYMENT.md`](BRANCHING_AND_DEPLOYMENT.md) before reviewing, merging, releasing, or deploying changes.

- `dev` is the integration branch; strive for a GitFlow-style branch and promotion model.
- With all configured tests and required checks passing, merge feature/fix PRs into `dev` only when evidence-based AI confidence is strictly greater than **99.1%**.
- Merge `dev` into `main`/`master` only when integration, release, deployment, migration, security, and required checks pass and evidence-based AI confidence is strictly greater than **99.7%**.
- Record the score, evidence, checks, remaining uncertainty, deployment impact, immutable artifact identity, `*-infra` desired-state change, and rollback or roll-forward plan.
- Use the organization's canonical `*-infra` repository, GitHub Actions, immutable artifacts, and GitOps reconciliation for branch-based deployment promotion.
- Required reviews, branch protection, security/compliance gates, and environment approvals always take precedence over any confidence score.
<!-- END ORESOFTWARE MANAGED BRANCHING AND GITOPS POLICY -->

<!-- ore-primary-branch-policy:begin -->
## Primary branch and concurrent-agent policy

This organization policy overrides generic feature-branch and worktree defaults for agent tooling.

- Highly prefer an existing primary branch, in this order: `main`, `dev`, then `master`.
- Work directly on the selected primary branch even when other agents are active. Use another branch only when a human or a repository-specific release process explicitly requires it.
- Never create or use a Git worktree unless a human explicitly instructs you to do so for the current task. Concurrency alone is not permission to use a worktree.
- Concurrent agents must coordinate repository and file ownership through the available agent communication channel, keep edits scoped, inspect live state before each write, and hand off cleanly. Coordinate instead of isolating routine work in worktrees.
- Preserve unrelated in-progress changes and never overwrite another agent's work. If safe ownership of overlapping files cannot be established, pause that overlapping edit and coordinate before continuing.
<!-- ore-primary-branch-policy:end -->

## Git and history policy

Prefer merges over rewrites. The rule is: avoid git rebase in favor of git merge.
A merge records what actually happened, and the automation across this fleet reads
history to decide what has already landed — rewriting that history makes the
judgement wrong, and it makes two checkouts of the same work look unrelated.

On any conflict, resolve it semantically. Read at least 3–10 relevant commits of
surrounding history on both sides before deciding, then merge the two intents.
Picking a side is not a resolution; it silently discards whichever half was
dropped, and the loss is invisible afterwards because the conflict marker is gone.

The commands below destroy work that no remote has ever seen, so an agent does not
run them without explicit human permission:

- `git stash` — stashes live in no remote and appear in neither `git status` nor
  ahead/behind counts, so a repository holding thousands of stashed lines reports a
  clean tree to every tool that scans for unlanded work. Use a `wip/<what-it-is>`
  branch instead. If you find someone else's stash, make it reachable with
  `git branch rescue/<id> refs/stash` — never pop it.
- `git reset` — moves the branch out from under committed work.
- `git clean` — deletes untracked files that have never been pushed anywhere.
- `git filter-repo` — rewrites every commit id in the repository, which breaks
  every pin, submodule pointer and open pull request that referenced the old ones.

Stage explicit paths. Never `git add -A`: most checkouts here carry someone else's
work in progress, and `-A` is how that — plus secrets — gets committed by accident.

Never report work as landed while it is only on local disk. A change is done when
it is committed, pushed, and open as a pull request; compiling is not landing.
