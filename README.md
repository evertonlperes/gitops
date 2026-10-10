# gitops

Desired state for a single-node homelab platform, reconciled by [Argo CD](https://argo-cd.readthedocs.io/)
on a single-node k3s cluster. The application code lives in a separate private repo; this repo holds only deployment config.

> Status: scaffold. Phase 1 (one canary service) is next. Plan: see `docs/adr/`.

## How a deploy flows

```
app repo (PR merged) -> GitHub Actions builds linux/amd64 image
  -> pushes to a private registry on the cluster host (tailnet only)
  -> CI commits the new tag to apps/homelab/overlays/home   (this repo)
  -> Argo CD sees the commit and syncs the cluster
```

## Layout

| Path | Purpose |
|---|---|
| `bootstrap/` | One-time Argo CD install and the root app-of-apps |
| `argocd-apps/` | One Argo `Application` per unit (picked up by the root app) |
| `apps/homelab/` | Kustomize `base/` plus `overlays/home/` for the workloads |
| `platform/` | Helm values for third-party components |
| `docs/adr/` | Why each decision was made |

## What stays outside the cluster

Stateful dependencies (PostgreSQL) keep running in Docker on the same host; pods reach them
through selector-less Services. Moving them in is a later, separate decision.

## Public repo rules

This repo is public. It must never contain secrets or network identifiers.

- Secrets are committed only as Sealed Secrets ciphertext. Raw Secrets, `.env` files and
  keys are git-ignored, and `gitleaks` runs in CI.
- Hostnames and IPs are placeholders (`registry.example.internal`). The real values live in a
  private overlay applied on the cluster host and are never committed.
- The image registry is reachable only inside a private Tailscale network, so the images cannot be pulled
  from outside. The manifests are still validated in CI with `kustomize build` + `kubeconform`.

## Validate locally

```bash
kubectl kustomize apps/homelab/overlays/home | kubeconform -strict -summary -ignore-missing-schemas
yamllint .
gitleaks detect --source . --redact
```
