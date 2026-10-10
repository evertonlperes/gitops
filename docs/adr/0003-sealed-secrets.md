# 0003. Sealed Secrets for secrets

Status: accepted

## Context
A public repo needs secrets encrypted at rest in git, and Argo CD should need no plugins.

## Decision
Use Sealed Secrets. Ciphertext is committed; the controller decrypts in-cluster.

## Consequences
Simple with Argo. The controller's private key is the only protection: back it up offline, never commit it, rotate if exposed.
