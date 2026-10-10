# 0004. Tailscale operator for exposure

Status: accepted

## Context
Services are reachable only to tailnet peers; one webhook endpoint must be reachable publicly via Funnel.

## Decision
Use the Tailscale Kubernetes operator with an Ingress per service and the Funnel annotation for the webhook.

## Consequences
Per-service HTTPS hostnames. The public Funnel hostname changes, so the webhook registration must be updated at cutover.
