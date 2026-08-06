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
