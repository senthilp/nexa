---
title: Vercel
type: organization
domain: Developer platform, infrastructure, agentic infrastructure
---

# Vercel

Developer platform company building infrastructure for web applications, developer tooling, and now **agentic infrastructure**—the infrastructure layer for coding agents and AI-native software development.

## Evolution

Initially positioned as "frontend cloud" unifying serverless functions, caching, and edge requests. Now expanding to **agentic infrastructure**: the platform for agents to deploy to, infrastructure for building/running agents, and infrastructure that itself is agentic.

## Agent Deployment Data (2026-04-09)

- Weekly deployments doubled in 3 months, agents driving growth
- **30% of deployments** agent-initiated (up 1000% in 6 months)
  - Claude Code: 75%
  - Lovable/v0: 6%
  - Cursor: 1.5%
- Projects deployed by agents are **20x more likely** to call AI inference providers
- Pattern: agents writing AI-native software, agents building agents

## Products/Infrastructure

### For Agents to Deploy To
- **CLI, API, MCP servers, git integration**: Programmatic deployment surface
- **Immutable deployments**: Deterministic, no manual intervention
- **Preview URLs**: Every commit gets URL for agent verification
- **Instant rollbacks**: Prerequisites for machine-driven development

### For Building/Running Agents
- **AI SDK**: Unified way to build AI apps; AI SDK 6 adds agent abstraction
- **Chat SDK**: Agents across dozens of chat apps from single codebase
- **AI Gateway**: Single endpoint for hundreds of models (budgets, monitoring, routing, retries, fallbacks)
- **Fluid compute**: Designed for AI workload shape (latency, concurrency, idle)
- **Workflows & Queues**: Pause, resume, retry, state, background work
- **Sandbox**: Isolated execution for untrusted code
- **Observability**: Trace agent behavior and failures

### Infrastructure That Is Agentic
- Full context across code, model calls, runtime behavior
- Autonomously investigates anomalies (latency, provider failures)
- Root-cause analysis, proposes fixes, tests in sandboxes
- Acts on delta between developer intent and system behavior
- Human-in-loop today, trending toward autonomous remediation

## Technical Infrastructure

- Runs on metal instances with NVMe storage
- Uses [[Firecracker]] microVMs for isolation
- Custom [[VHS Format]] for snapshot compression
- Unified platform with shared context enabling agentic operations

## Philosophy

"The history of cloud computing is the history of removing the human from the machine. Agentic infrastructure is the next evolution, moving us from passive tools that wait for commands to proactive systems that act on our behalf."

## Sources

- [[Optimizing Vercel Sandbox snapshots]] (2026-04-08)
- [[Agentic Infrastructure]] (2026-04-13)
