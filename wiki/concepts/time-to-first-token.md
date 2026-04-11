---
name: Time-to-first-token
type: concept
category: performance-metrics
---

# Time-to-first-token (TTFT)

Latency metric measuring how long a session waits between accepting work and producing its first response token. This is the latency users most acutely *feel*.

## Why It Matters

TTFT represents the perceived "hang" before the agent starts responding. Unlike throughput or total completion time, TTFT is the user's first signal that the system is working.

## In Managed Agents

**Coupled architecture** (brain + hands in one container):
- Every session pays full container setup cost upfront
- Container provision, repo clone, process boot—all before first token
- Even sessions that would never need sandbox waited for it

**Decoupled architecture** (lazy provisioning):
- Containers provisioned via tool call `execute(name, input) → string` only if needed
- Sessions not needing sandbox start inference immediately after pulling session log
- Inference can begin as soon as orchestration layer pulls events from session

**Impact**:
- **p50 TTFT**: ~60% reduction
- **p95 TTFT**: >90% reduction

## Measurement Philosophy

Similar to [[Performance Optimization Strategies]] pattern of measuring what users feel:
- TTFT over average latency
- p75/p95 over p50
- Focus on perceived responsiveness

## Related Concepts

- [[Performance Optimization Strategies]] — Broader philosophy of measuring user-perceived metrics
- [[Agent Decoupling Patterns]] — Architectural change enabling TTFT improvement
- [[Managed Agents]] — System achieving these gains

## Sources

- [[Scaling Managed Agents: Decoupling the brain from the hands]] | Added: 2026-04-11
