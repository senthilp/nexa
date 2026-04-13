---
name: Agent Workload Shape
type: concept
category: ai-infrastructure
---

# Agent Workload Shape

The computational and operational characteristics of AI agent workloads, which differ fundamentally from traditional serverless workloads.

## Serverless Workload Shape

- Functions (short-lived, stateless)
- Caching (read-heavy, cacheable responses)
- Short-lived requests at edge (sub-second)
- Request-response pattern
- CPU/memory-bound

## Agent Workload Shape

- **Long-lived execution**: Tasks spanning minutes to hours
- **Multi-step orchestration**: Sequences of dependent actions
- **Model routing**: Intelligent switching between models/providers
- **Cost controls**: Budget management, rate limiting
- **Sandboxed code execution**: Untrusted code from model outputs
- **Abuse resistance**: Protection against prompt injection, resource exhaustion
- **Latency, concurrency, idle all matter**: Complex resource profile
- **Stateful**: Needs to pause, resume, retry with state preservation

## Infrastructure Implications

**Different primitives required**:
- Workflows (pause/resume)
- Queues (background work, retry logic)
- Sandboxes (isolated execution for generated code)
- Observability (trace multi-step agent behavior)
- Model gateway (routing, fallbacks, retries across providers)
- Fluid compute (handles unusual latency/concurrency/idle patterns)

**Penalty for DIY**: Running this stack yourself compounds:
- Wasted requests burn inference dollars
- Provider outages take agent offline
- Untrusted code opens prompt injection vectors
- Multi-system complexity creates config drift

## Why Unified Platform Matters

Serverless: Vercel unified functions + caching + edge into frontend cloud

Agents: Vercel unifying AI SDK + gateway + workflows + queues + sandbox + observability into agentic infrastructure

**Pattern**: Complex stack with compounding failure modes → unified platform that handles primitives as integrated system

## Related Concepts

- [[Agentic Infrastructure]] — Infrastructure designed for this workload shape
- [[Managed Agents]] — Anthropic's approach to similar challenges (brain-hands-session)
- [[Operational Friction]] — What complex, multi-system stacks create

## Sources

- [[Agentic Infrastructure]] | Added: 2026-04-13
