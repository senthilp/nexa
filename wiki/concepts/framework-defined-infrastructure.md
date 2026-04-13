---
name: Framework-defined Infrastructure
type: concept
category: infrastructure-evolution
---

# Framework-defined Infrastructure

Infrastructure generation where infrastructure configuration is derived from the application itself, rather than manually specified. Identified as the precursor to [[Agentic Infrastructure]].

## Infrastructure Generations

1. **Hand-configured servers**: Manual setup, SSH, config files
2. **Cloud APIs**: Infrastructure as programmable APIs (AWS, etc.)
3. **Framework-defined infrastructure**: Infrastructure derived from application code
4. **Agentic infrastructure**: Infrastructure for/by/as agents (current transition)

## Concept

Instead of:
- Writing application code
- Separately writing infrastructure config (Terraform, etc.)
- Deploying both

Framework-defined infrastructure:
- Application code declares its needs (via framework primitives)
- Infrastructure automatically provisioned based on application structure
- Deployment unified, config drift eliminated

## Why It Mattered

Reduced operational friction by eliminating the gap between application intent and infrastructure reality. Application and infrastructure in sync by design.

## Next Evolution: Agentic Infrastructure

Framework-defined reduced human operational work. Agentic infrastructure goes further:
- Infrastructure not just derived from app, but deployed/maintained/healed by agents
- Infrastructure that is itself agentic
- "Removing the human from the machine"

## Related Concepts

- [[Agentic Infrastructure]] — Next generation after framework-defined
- [[Operational Friction]] — What these evolutions aim to eliminate

## Sources

- [[Agentic Infrastructure]] | Added: 2026-04-13 (referenced but not detailed)
