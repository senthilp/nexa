---
name: Preview URLs
type: concept
category: infrastructure-patterns
---

# Preview URLs

Automatically generated URLs for every commit/deployment, allowing verification of changes before merging or shipping to production.

## How It Works

- Every commit or PR gets unique URL
- Running version of code at that URL
- Can test, verify, share before production
- Deterministic: same commit = same preview URL

## Why It Matters for Agents

Agents need to verify output. Without preview URLs:
- Agent writes feature → can't see if it works
- Verification requires local setup or manual deployment
- Autonomous loop breaks

With preview URLs:
- Agent writes feature → preview URL generated
- Agent makes request to URL → verifies output
- Agent confirms success → ships to production
- Fully autonomous

## Part of Agent-Driven Prerequisites

Along with [[Immutable Deployments]] and instant rollbacks, preview URLs are not just "developer convenience" but **absolute prerequisites for machine-driven software development**.

**Traditional use**: Humans review changes before merge  
**Agent use**: Agents programmatically verify output in autonomous loop

## Related Concepts

- [[Immutable Deployments]] — Ensures preview matches what will ship
- [[Operational Friction]] — What preview URLs eliminate
- [[Agentic Infrastructure]] — Infrastructure providing preview URLs programmatically

## Sources

- [[Agentic Infrastructure]] | Added: 2026-04-13
