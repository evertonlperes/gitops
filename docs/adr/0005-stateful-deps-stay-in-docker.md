# 0005. Stateful dependencies stay in Docker for now

Status: accepted

## Context
Stateful data adds risk to a first migration.

## Decision
Keep PostgreSQL in Docker; pods reach it through selector-less Services. Revisit later.

## Consequences
Smaller blast radius. Backups and the nightly dump keep working unchanged.
