# 0001. Separate public GitOps repo

Status: accepted

## Context
Manifests inside the app repo make every image-tag bump a commit that triggers app CI, and mix deployment history with code history.

## Decision
Keep deployment config in its own repo, `gitops`, which is public as a portfolio piece.

## Consequences
Clean audit trail and rollback by revert. Needs a scoped token for CI to push tag bumps, and strict secret and identifier hygiene (see README).
