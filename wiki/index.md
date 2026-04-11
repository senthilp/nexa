# Nexa Index

Last updated: 2026-04-11

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

## Entities

### People
- [Hamel Husain](entities/hamel-husain.md) — ML engineer, educator, author specializing in LLM evaluations | Sources: 1
- [Gergely Orosz](entities/gergely-orosz.md) — Pragmatic Engineer newsletter author | Sources: 1
- [Jacob](entities/jacob.md) — NurtureBoss founder, domain expert for eval labeling | Sources: 1
- [Nicholas C. Zakas](entities/nicholas-zakas.md) — Amazon engineer, author on developer workflows | Sources: 1
- [Lance Martin](entities/lance-martin.md) — Anthropic engineer, agent systems | Sources: 1
- [Gabe Cemaj](entities/gabe-cemaj.md) — Anthropic engineer | Sources: 1
- [Michael Cohen](entities/michael-cohen.md) — Anthropic engineer | Sources: 1

### Organizations
- [NurtureBoss](entities/nurtureboss.md) — AI leasing assistant startup, primary case study | Sources: 1
- [Vercel](entities/vercel.md) — Developer platform, infrastructure provider | Sources: 1
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

## Comparisons
*Comparison pages will appear here as queries are filed.*

## Analyses
*Analysis pages will appear here as queries are filed.*

---

**Total pages:** 43 (index + log + overview + 4 sources + 9 entities + 22 concepts)  
**Total sources:** 4 (1 LLM engineering, 2 developer productivity, 1 AI infrastructure)  
**Last ingest:** 2026-04-11 (Scaling Managed Agents: Decoupling the brain from the hands)
