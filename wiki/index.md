# Nexa Index

Last updated: 2026-04-13

## Overview
- [Overview](overview.md) — High-level synthesis across LLM engineering and developer productivity domains

## Sources

### LLM Engineering
- [A pragmatic guide to LLM evals for devs](sources/pragmatic-guide-llm-evals.md) — Moving from vibe-check development to systematic evaluation | Added: 2026-04-05 | Author: Hamel Husain

### Developer Productivity
- [Optimizing Vercel Sandbox snapshots](sources/sandbox-snapshots.md) — Performance optimization: 40s → sub-second restores via parallelization, streaming, caching | Added: 2026-04-08 | Authors: Vercel team
- [Improving developer velocity with GitHub merge queue](sources/github-merge-queue.md) — Automating PR testing and merging to eliminate manual "update branch" cycles | Added: 2026-04-08 | Author: Nicholas C. Zakas

### AI Infrastructure
- [Scaling Managed Agents: Decoupling the brain from the hands](sources/scaling-managed-agents.md) — Virtualized agent architecture: brain, hands, session as independent components | Added: 2026-04-11 | Authors: Lance Martin, Gabe Cemaj, Michael Cohen
- [Agentic Infrastructure](sources/agentic-infrastructure.md) — Infrastructure for/by/as agents: deployment surfaces, agent primitives, autonomous operations | Added: 2026-04-13 | Published: 2026-04-09 | Vercel

## Entities

### People
- [Hamel Husain](entities/hamel-husain.md) — ML engineer, educator, author specializing in LLM evaluations | Sources: 1
- [Gergely Orosz](entities/gergely-orosz.md) — Pragmatic Engineer newsletter author | Sources: 1
- [Jacob](entities/jacob.md) — NurtureBoss founder, domain expert for eval labeling | Sources: 1
- [Nicholas C. Zakas](entities/nicholas-zakas.md) — Amazon engineer, author on developer workflows | Sources: 1
- [Lance Martin](entities/lance-martin.md) — Anthropic engineer, agent systems | Sources: 1
- [Gabe Cemaj](entities/gabe-cemaj.md) — Anthropic engineer | Sources: 1
- [Michael Cohen](entities/michael-cohen.md) — Anthropic engineer | Sources: 1

### Tools
- [Claude Code](entities/claude-code.md) — AI coding agent, 75% of agent deployments on Vercel | Sources: 1
- [Lovable](entities/lovable.md) — AI coding tool, ~6% of agent deployments | Sources: 1
- [v0](entities/v0.md) — AI coding tool, ~6% of agent deployments | Sources: 1
- [Cursor](entities/cursor.md) — AI coding tool, ~1.5% of agent deployments | Sources: 1

### Organizations
- [NurtureBoss](entities/nurtureboss.md) — AI leasing assistant startup, primary case study | Sources: 1
- [Vercel](entities/vercel.md) — Developer platform, agentic infrastructure provider | Sources: 2
- [Anthropic](entities/anthropic.md) — AI safety/research company, creator of Claude and Managed Agents | Sources: 1

## Concepts

### LLM Engineering: Evaluation Frameworks
- [LLM Evaluations](concepts/llm-evaluations.md) — Systematic approaches to measuring LLM quality | Sources: 1
- [Code-Based Evals](concepts/code-based-evals.md) — Assertions for deterministic failures | Sources: 1
- [LLM-as-Judge](concepts/llm-as-judge.md) — LLM evaluating subjective quality | Sources: 1
- [Golden Dataset](concepts/golden-dataset.md) — Curated test cases with ground truth | Sources: 1

### LLM Engineering: Methodologies
- [Error Analysis](concepts/error-analysis.md) — Bottom-up workflow for discovering failure modes | Sources: 1
- [Open Coding](concepts/open-coding.md) — Writing open-ended notes on failures | Sources: 1
- [Axial Coding](concepts/axial-coding.md) — Grouping notes into categorical themes | Sources: 1
- [Vibe-Check Development](concepts/vibe-check-development.md) — Anti-pattern of shipping based on manual spot-checks | Sources: 1
- [Three Gulfs Model](concepts/three-gulfs-model.md) — Framework for LLM development challenges | Sources: 1
- [Conversation Traces](concepts/conversation-traces.md) — Complete records of LLM interactions | Sources: 1

### Developer Productivity: Infrastructure & Workflows
- [Filesystem Snapshots](concepts/filesystem-snapshots.md) — Capturing and restoring complete filesystem state | Sources: 1
- [GitHub Merge Queue](concepts/github-merge-queue.md) — Automated PR testing and merging system | Sources: 1
- [Performance Optimization Strategies](concepts/performance-optimization-strategies.md) — Parallelization, streaming, caching patterns | Sources: 3

### AI Infrastructure: Agent Systems
- [Managed Agents](concepts/managed-agents.md) — Hosted service for long-horizon agents with virtualized components | Sources: 1
- [Agent Decoupling Patterns](concepts/agent-decoupling-patterns.md) — Separating brain, hands, and session | Sources: 1
- [Session-based Architecture](concepts/session-based-architecture.md) — External, queryable event log as context object | Sources: 1
- [Harness Design](concepts/harness-design.md) — Loop that calls Claude and routes tool calls | Sources: 1
- [Context Engineering](concepts/context-engineering.md) — Managing Claude's context window for long tasks | Sources: 1
- [Pets vs Cattle](concepts/pets-vs-cattle.md) — Infrastructure philosophy: hand-tended vs. interchangeable | Sources: 1
- [Time-to-first-token](concepts/time-to-first-token.md) — User-perceived latency metric | Sources: 1
- [Interface Stability](concepts/interface-stability.md) — Abstractions outlasting implementations | Sources: 1

### AI Infrastructure: Agentic Infrastructure
- [Agentic Infrastructure](concepts/agentic-infrastructure.md) — Three evolutions: for agents to deploy to, for building agents, infrastructure that is agentic | Sources: 1
- [Operational Friction](concepts/operational-friction.md) — Manual steps breaking autonomous agent workflows | Sources: 1
- [Immutable Deployments](concepts/immutable-deployments.md) — Prerequisite for machine-driven development | Sources: 1
- [Preview URLs](concepts/preview-urls.md) — Auto-generated URLs for agent verification | Sources: 1
- [Agent Workload Shape](concepts/agent-workload-shape.md) — Computational characteristics differing from serverless | Sources: 1
- [Autonomous Operations](concepts/autonomous-operations.md) — Infrastructure that monitors, analyzes, responds autonomously | Sources: 1
- [Unified Platform Context](concepts/unified-platform-context.md) — Shared visibility across code, models, runtime enabling agency | Sources: 1
- [Framework-defined Infrastructure](concepts/framework-defined-infrastructure.md) — Infra derived from application (precursor to agentic) | Sources: 1

## Comparisons
*Comparison pages will appear here as queries are filed.*

## Analyses
*Analysis pages will appear here as queries are filed.*

---

**Total pages:** 57 (index + log + overview + 5 sources + 13 entities + 30 concepts)  
**Total sources:** 5 (1 LLM engineering, 2 developer productivity, 2 AI infrastructure)  
**Last ingest:** 2026-04-13 (Agentic Infrastructure)
