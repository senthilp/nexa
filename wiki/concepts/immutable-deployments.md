---
name: Immutable Deployments
type: concept
category: infrastructure-patterns
---

# Immutable Deployments

Deployment pattern where each deployment is a fixed, reproducible artifact that cannot be modified after creation. Changes require new deployments rather than in-place updates.

## Characteristics

- **No in-place mutation**: Can't modify running deployment
- **Reproducibility**: Same input always produces same deployment
- **Instant rollbacks**: Switch back to previous immutable version
- **No state drift**: Deployment matches source exactly
- **Deterministic**: No "works on my machine" or config drift

## Why It Matters for Agents

Traditional view: "Nice DX feature for developers"

**New reality**: "Absolute prerequisite for machine-driven software development"

**Reasoning**:
- Agents need deterministic surfaces—immutability guarantees same code = same deployment
- Mutable deployments create state drift agents can't reason about
- Manual coordination (updating configs, reconciling state) breaks autonomous loops
- Preview URLs on every commit enable agent verification

## Combined with Other Prerequisites

Forms a necessary stack for agent-driven development:
- **Immutable deployments** → deterministic, no drift
- **Preview URLs** → every commit verifiable
- **Instant rollbacks** → recovery without manual intervention
- Together: agents can write → test → verify → ship → roll back autonomously

## Shift in Perspective

**Before agents**: These were "developer experience upgrades"  
**With agents**: These are operational requirements—without them, autonomous loops break

## Related Concepts

- [[Operational Friction]] — What immutability eliminates
- [[Agentic Infrastructure]] — Infrastructure requiring immutability
- [[Preview URLs]] — Complementary pattern for verification

## Sources

- [[Agentic Infrastructure]] | Added: 2026-04-13
