# Nexa Log

This is a chronological record of all operations performed on this knowledge base.

---

## [2026-04-13] ingest | Agentic Infrastructure

Ingested Vercel article on agentic infrastructure—the three-part evolution for agent-driven software development.

**Source added:**
- **Agentic Infrastructure** — Infrastructure for/by/as agents: deployment surfaces, agent primitives, autonomous operations

**Pages created:**
- 1 source summary page
- 4 entity pages (Claude Code, Lovable, v0, Cursor—AI coding tools)
- 1 entity updated (Vercel—expanded with agentic infrastructure positioning)
- 8 concept pages (Agentic Infrastructure, Operational Friction, Immutable Deployments, Preview URLs, Agent Workload Shape, Autonomous Operations, Unified Platform Context, Framework-defined Infrastructure)

**Key changes:**
- Updated `overview.md` with agentic infrastructure synthesis, agent deployment data, complementary relationship with Managed Agents
- Updated `index.md` with new AI Infrastructure: Agentic Infrastructure section, Tools category for coding agents
- Updated statistics: 5 sources total, 57 total pages

**Main topics covered:**
- Three evolutions: infra *for* agents to deploy to, *for* building agents, that *is* agentic
- Agent deployment surge: 30% of Vercel deployments agent-initiated (up 1000% in 6 months)
- Operational friction as bottleneck: manual steps break autonomous loops
- Immutable deployments, preview URLs as prerequisites (not just DX)
- Agent workload shape: long-lived execution, orchestration, sandboxing (vs. serverless)
- Autonomous operations: infrastructure that monitors, investigates, proposes fixes
- Unified platform context enables agency: code + models + runtime visibility

**Notable insights:**
- Agents writing AI-native software: projects deployed by agents 20x more likely to use AI inference
- Claude Code dominates: 75% of agent-initiated deployments
- Shift from "developer convenience" to "operational requirement" for immutability/preview URLs
- Traditional infra: one-way (code in, logs out, human fixes). Agentic infra: closed loop (platform acts)
- "The history of cloud computing is the history of removing the human from the machine"

**Cross-domain connections:**
- Complements [[Managed Agents]]: Anthropic focuses on agent runtime (brain-hands-session), Vercel on deployment/observability
- Echoes [[Performance Optimization Strategies]]: unified platform eliminates multi-system complexity
- Extends [[Interface Stability]]: infrastructure generations as evolving abstractions
- New cross-domain pattern: context enables capability (eval traces, session logs, platform context)

---

## [2026-04-11] ingest | Scaling Managed Agents: AI Infrastructure Domain

Ingested Anthropic article on Managed Agents architecture, adding third domain: AI infrastructure and agent system design.

**Source added:**
- **Scaling Managed Agents: Decoupling the brain from the hands** — Platform architecture for long-horizon agents via virtualized components

**Pages created:**
- 1 source summary page
- 4 entity pages (Anthropic, Lance Martin, Gabe Cemaj, Michael Cohen)
- 8 concept pages (Managed Agents, Agent Decoupling Patterns, Session-based Architecture, Harness Design, Context Engineering, Pets vs Cattle, Time-to-first-token, Interface Stability)

**Key changes:**
- Updated `overview.md` with AI infrastructure domain synthesis and extended cross-domain patterns
- Updated `index.md` with new AI Infrastructure section and reorganized entities
- Updated `performance-optimization-strategies.md` to note TTFT connection
- Updated statistics: 4 sources total, 43 total pages

**Main topics covered:**
- Decoupling brain (Claude + harness) from hands (tools/sandboxes) and session (event log)
- "Design for programs as yet unthought of" — interfaces outlasting implementations
- Pets-to-cattle transformation via external session storage
- Security boundaries: credentials isolated from sandbox
- Performance: 60-90% TTFT reduction via lazy provisioning
- Meta-harness accommodating future capabilities

**Notable insights:**
- Harness assumptions go stale as models improve (context anxiety disappeared in Opus 4.5)
- Session as external context object, programmatically queryable
- Many brains can control many hands, hands passed between brains
- Interface stability like OS abstractions (`read()` unchanged since 1970s)

**Cross-domain connections:**
- Same "measure what users feel" philosophy as infrastructure optimization (TTFT vs p95 latency)
- Systematic design over ad-hoc implementation (LLM evals, infrastructure, agent systems)
- Domain-specific beats generic (custom metrics, custom formats, task-specific harnesses)

---

## [2026-04-08] ingest | Developer Productivity Domain Expansion

Ingested two sources expanding knowledge base into developer productivity domain: infrastructure performance optimization (Vercel) and workflow automation (GitHub).

**Sources added:**
1. **Optimizing Vercel Sandbox snapshots** — Performance case study showing 40s → sub-second restore times
2. **Improving developer velocity with GitHub merge queue** — Workflow automation eliminating PR babysitting

**Pages created:**
- 2 source summary pages
- 2 entity pages (Nicholas C. Zakas, Vercel)
- 3 concept pages (Filesystem Snapshots, GitHub Merge Queue, Performance Optimization Strategies)

**Key changes:**
- Updated `overview.md` with new domain synthesis and cross-domain patterns
- Reorganized `index.md` to group sources and concepts by domain
- Updated statistics: 3 sources total, 30 total pages

**Main topics covered:**
- Performance optimization: parallelization, streaming, caching (95% hit rate)
- Workflow automation: merge queue eliminating manual CI coordination
- Cross-domain pattern: systematic over ad-hoc applies to both infrastructure and LLM engineering

**Notable insights:**
- Compound gains: multiple optimizations multiply, not just add
- Cache design: storing decompressed data skips both network and CPU
- "Small quality-of-life improvements" can eliminate entire classes of friction
- Measurement matters: p95 latency for user experience, PASS/FAIL for actionability

---

## [2026-04-05] ingest | A pragmatic guide to LLM evals for devs

Ingested first source: comprehensive article on systematic LLM evaluation by Hamel Husain.

**Key changes:**
- Created source summary page: `sources/pragmatic-guide-llm-evals.md`
- Created 4 entity pages: Hamel Husain, Gergely Orosz, NurtureBoss, Jacob
- Created 10 concept pages covering evaluation frameworks, methodologies, and anti-patterns
- Updated `overview.md` with first synthesis: systematic evaluation as foundation for LLM quality
- Updated `index.md` with all new pages organized by category

**Main topics covered:**
- Error analysis as highest-ROI activity in AI development
- Two-pronged eval approach: code-based for deterministic, LLM-as-judge for subjective
- Three gulfs model explaining fundamental LLM development challenges
- Bottom-up discovery via open coding and axial coding
- Case study: NurtureBoss leasing assistant

**Note:** Article appears incomplete in source file (cuts off at section 4), missing content on judge alignment, production monitoring, and full flywheel cycle.

---

## [2026-04-05] initialization | Nexa Setup

Initial setup of the Nexa knowledge base system.
- Created directory structure: `raw/`, `raw/assets/`, `wiki/` with subdirectories
- Created CLAUDE.md schema file defining wiki maintenance workflows
- Created index.md and log.md for navigation and history
- Wiki initialized and ready for first source ingest
