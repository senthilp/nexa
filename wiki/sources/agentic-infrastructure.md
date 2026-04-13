---
title: "Agentic Infrastructure"
date_added: 2026-04-13
source_type: article
source_path: raw/Agentic Infrastructure.md
url: https://vercel.com/blog/agentic-infrastructure
published: 2026-04-09
---

# Agentic Infrastructure

## Summary

Vercel's article on the infrastructure transition driven by LLMs and coding agents. Every generation of software demands new infrastructure—from manual server configuration to cloud APIs to framework-defined infrastructure. Agents are now driving the next shift: **agentic infrastructure**. This is a three-part evolution: (1) infrastructure for agents to deploy to, (2) infrastructure for building/running agents, and (3) infrastructure that itself is agentic—able to autonomously monitor, analyze, and respond to production issues.

The data is striking: on Vercel, weekly deployments doubled in three months, with agents driving the growth. Over 30% of deployments are agent-initiated (up 1000% in six months), and agent-deployed projects are 20x more likely to call AI inference providers. The bottleneck for agent-driven development is operational friction—agents need programmatic, deterministic deployment surfaces without manual clicks or Terraform state.

## Key Insights

- **Infrastructure generations**: Hand-configured servers → cloud APIs → framework-defined infrastructure → **agentic infrastructure**
- **Agent deployment surge**: 30% of Vercel deployments now agent-initiated (Claude Code 75%, Lovable/v0 6%, Cursor 1.5%), up 1000% in 6 months
- **Agents build agents**: Projects deployed by agents are 20x more likely to use AI inference—agents writing AI-native software
- **Three evolutions**: Infrastructure *for* agents to deploy to, infrastructure *for* building agents, infrastructure that *is* agentic
- **Operational friction as bottleneck**: Manual steps (clicks, Terraform state) break autonomous loops—agents need programmatic surfaces
- **Immutability as prerequisite**: Immutable deployments, preview URLs, instant rollbacks aren't just DX—they're required for machine-driven development
- **Different workload shape**: Agents need long-lived execution, orchestration, model routing, cost controls, sandboxed code execution, abuse resistance (vs. serverless: functions, caching, short requests)
- **From monitoring to responding**: Traditional infra is one-way (code in, logs out, human fixes). Agentic infra has full context across code/models/runtime and can autonomously investigate, analyze, propose fixes
- **Context enables agency**: Unified platform with shared context (code + model calls + runtime behavior) turns infrastructure itself into an agent
- **Human-in-loop today, autonomous tomorrow**: Platform investigates anomalies, reads logs/code, runs root-cause analysis, tests fixes in sandboxes—currently with human approval, trending toward autonomous action

## Entities Mentioned

- [[Vercel]] — Platform providing agentic infrastructure (updated context)
- [[Claude Code]] — 75% of agent-initiated deployments on Vercel
- [[Lovable]] — AI coding tool, 6% of agent deployments
- [[v0]] — AI coding tool, 6% of agent deployments
- [[Cursor]] — AI coding tool, 1.5% of agent deployments

## Concepts

- [[Agentic Infrastructure]] — Three-part evolution: infra for agents to deploy to, infra for building agents, infra that is agentic
- [[Framework-defined Infrastructure]] — Infrastructure derived from application itself (precursor to agentic infra)
- [[Operational Friction]] — Manual steps that break autonomous agent loops
- [[Immutable Deployments]] — Prerequisite for agent-driven development
- [[Preview URLs]] — Every commit gets URL for agent verification
- [[Agent Workload Shape]] — Long-lived execution, orchestration, model routing, sandboxing (vs. serverless)
- [[Unified Platform Context]] — Shared visibility across code, model calls, runtime enabling agentic behavior
- [[Autonomous Operations]] — Infrastructure that investigates, analyzes, and responds to production issues

## Vercel's Agentic Stack

**For agents to deploy to**:
- CLI, API, MCP servers, git integration → programmatic deployment surface
- Immutable deployments, preview URLs, instant rollbacks → deterministic without manual intervention

**For building/running agents**:
- **AI SDK**: Unified way to build AI apps across frameworks/providers; AI SDK 6 adds agent abstraction
- **Chat SDK**: Agents available across dozens of chat apps from single codebase
- **AI Gateway**: Single endpoint for hundreds of models with budgets, monitoring, routing, retries, fallbacks
- **Fluid compute**: Designed for AI workload shape (latency, concurrency, idle waiting)
- **Workflows & Queues**: Pause, resume, retry, state management, background work
- **Sandbox**: Isolated execution for untrusted code
- **Observability**: Trace agent behavior and failures

**Infrastructure that is agentic**:
- Full context across code, model calls, runtime behavior
- Autonomously investigates anomalies (latency spikes, provider drops)
- Queries observability data, reads logs, inspects code
- Performs root-cause analysis, proposes fixes, tests in sandboxes
- Acts on delta between developer intent and system behavior
- Human-in-loop today, trending toward autonomous remediation

## Contradictions/Updates

- **Extends** [[Vercel]] entity—now positioned as provider of agentic infrastructure, not just frontend cloud
- **Relates to** [[Performance Optimization Strategies]]—unified platform eliminates config drift and multi-system debugging
- **Complements** [[Managed Agents]]—both address infrastructure for long-running AI agents, Vercel focuses on deployment/observability, Anthropic on brain-hands-session decoupling
- **Echoes** [[Interface Stability]]—"removing the human from the machine" is same pattern as stable interfaces outlasting implementations

## Quotes

> "Every generation of software eventually demands a new generation of infrastructure."

> "Agents are building, testing, and shipping AI-native software, and they're doing it at a velocity that breaks traditional operations."

> "The bottleneck for agentic engineering is operational friction."

> "Immutable deployments, preview URLs on every commit, and instant rollbacks aren't just developer experience upgrades anymore. They are absolute prerequisites for machine-driven software development."

> "Traditional infrastructure is a one-way street: code goes in, logs come out, and a human reads the logs to fix the code."

> "The platform interprets what the developer intended, observes what the system actually did, and acts on the delta."

> "The history of cloud computing is the history of removing the human from the machine."

## Related Sources

- [[Optimizing Vercel Sandbox snapshots]] — Infrastructure performance underlying agent sandboxes
- [[Scaling Managed Agents: Decoupling the brain from the hands]] — Complementary architecture for long-horizon agents
- [[Framework-defined Infrastructure]] (referenced but not ingested) — Precursor evolution
