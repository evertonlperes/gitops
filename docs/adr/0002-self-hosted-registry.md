# 0002. Self-hosted registry on the cluster host

Status: accepted

## Context
GitHub Free includes 0.5 GB of package storage and 1 GB of transfer a month, too tight for eight images pulled on every rollout.

## Decision
Run `registry:2` in Docker on the host, reachable only over Tailscale. CI pushes through the Tailscale GitHub Action with a tagged OAuth client.

## Consequences
No quota or cost. One more container to monitor; images cannot be pulled from outside the private network.
