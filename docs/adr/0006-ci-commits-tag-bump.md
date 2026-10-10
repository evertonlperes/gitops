# 0006. CI commits image tag bumps

Status: accepted

## Context
Argo CD needs to learn about new images.

## Decision
After pushing an image, CI runs `kustomize edit set image` in the overlay and commits it to this repo.

## Consequences
Every deploy is a visible commit. Argo CD Image Updater can replace this later.
