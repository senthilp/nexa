# Overview

## Current State

This knowledge base contains **5 sources** spanning three domains:
1. **LLM Engineering** (1 source): Systematic evaluation and quality assurance
2. **Developer Productivity** (2 sources): Infrastructure optimization and workflow automation
3. **AI Infrastructure** (2 sources): Agent system architecture and platform design

## Synthesis

The emerging picture is one of **rigor over intuition** in LLM development. The core insight: LLMs' non-deterministic nature doesn't excuse sloppy development—it demands *more* systematic approaches, not fewer.

### Central Framework: Error Analysis as Foundation

The highest-ROI activity is **systematic error analysis**—reviewing conversation traces at scale, coding failures bottom-up, and building targeted evaluations. This replaces "vibe-check development" (shipping based on manual spot-checks) with data-driven quality assurance.

### Two Complementary Evaluation Approaches

1. **Code-based evals** for deterministic failures (date parsing, structured extraction)
   - Cheap, fast, run on every commit
   - Traditional assertions against expected outputs

2. **LLM-as-judge** for subjective quality (tone, judgment calls)
   - More expensive but handles nuance
   - Requires alignment with human expertise
   - Uses PASS/FAIL over rating scales for actionability

### The Three Challenges

LLM development faces three fundamental "gulfs":
- **Comprehension**: Understanding behavior at scale
- **Specification**: Aligning prompts with intent  
- **Generalization**: Reliable performance across all inputs

Error analysis and evaluations systematically bridge these gaps.

### Methodological Roots

The approach adapts battle-tested techniques:
- **Error analysis** from decades of ML practice
- **Grounded theory** from social sciences (open coding, axial coding)
- **Bottom-up discovery** over predefined metrics

## Major Themes

1. **Systematic over ad-hoc**: Data-driven evaluation beats manual spot-checking
2. **Domain-specific over generic**: Custom metrics outperform off-the-shelf scores
3. **Binary over scales**: PASS/FAIL judgments are more actionable than ratings
4. **First failures matter**: In causal systems, early errors cascade downstream
5. **Custom tooling pays off**: A few hours building domain-specific viewers unlocks massive efficiency

## Key Entities

- **[[Hamel Husain]]**: ML engineer, educator, main author—brings decades of ML best practices to LLM evaluation
- **[[NurtureBoss]]**: AI leasing assistant startup serving as case study for the framework in practice
- **Observability ecosystem**: LangSmith, Arize Phoenix, Braintrust for trace viewing

## Open Questions

- How to create high-quality synthetic data when real user data is limited? (Mentioned but not detailed in source)
- What are the specific techniques for building and aligning LLM-as-judge? (Article appears incomplete, cuts off at section 4)
- Integration patterns for evals in CI/CD pipelines? (Mentioned but not detailed)
- Production monitoring approaches? (Section promised but not present in source)
- The "flywheel of improvement" cycle? (Introduced but full workflow not covered)

## Gaps to Fill

- Need more concrete implementation examples (code snippets, tooling)
- Missing: later sections on judge alignment, TPR/TNR metrics, production monitoring
- Would benefit from: contrasting case studies (successes and failures)
- Unexplored: cost/benefit analysis of different eval approaches
- Incomplete: synthetic data generation techniques

## New Domain: Developer Productivity

Two new sources expand the knowledge base into engineering infrastructure and workflows:

### Performance Optimization Philosophy

[[Optimizing Vercel Sandbox snapshots]] demonstrates systematic performance improvement:
- **Reliability precedes optimization**: Never sacrifice correctness for speed
- **Compound gains**: Parallelization (2-5x) × streaming (2x) × caching (95% hit rate) = 40s → sub-second
- **Cache design matters**: Store decompressed data to skip both network and CPU on hits
- **Measure what users feel**: p75/p95 latency over averages

Core strategies: parallel downloads, parallel decompression, streaming pipelines, local NVMe caching.

### Workflow Automation ROI

[[Improving developer velocity with GitHub merge queue]] shows automation eliminating manual toil:
- **CI babysitting is expensive**: 10-min CI × manual "update branch" loops = significant developer time waste
- **Coordination is automatable**: Queue handles PR ordering, retesting, and merging that humans previously did manually
- **Trade-offs are explicit**: CI runs twice (initial + queue) in exchange for zero manual intervention

Key insight: Features that "sound like small quality-of-life improvements" can eliminate entire classes of frustration.

## Cross-Domain Patterns

Despite different domains, sources share themes:

1. **Systematic over ad-hoc**
   - LLM evals: Data-driven evaluation beats vibe-checks
   - Infrastructure: Benchmark-driven optimization beats guesswork
   - AI agents: Stable interfaces outlast fixed assumptions about capabilities

2. **Measure the right thing**
   - LLM evals: PASS/FAIL over rating scales for actionability
   - Infrastructure: p95 latency over averages for user experience
   - AI agents: TTFT (what users feel) over internal metrics

3. **Automation compounds**
   - LLM evals: Error analysis → eval suites → continuous improvement flywheel
   - Developer workflow: Merge queue → eliminated babysitting → focus on coding
   - AI agents: Decoupling → lazy provisioning → independent scaling

4. **Domain-specific beats generic**
   - LLM evals: Custom metrics outperform off-the-shelf scores
   - Infrastructure: Custom `.vhs` format + caching strategy tailored to Vercel's usage patterns
   - AI agents: Meta-harness accommodates task-specific harnesses

5. **Design for evolution**
   - LLM evals: Bottom-up discovery adapts to emerging failure modes
   - Infrastructure: Streaming pipelines + caching allow swapping implementations
   - AI agents: Interface stability allows implementations to change freely
   - Agentic infra: "Removing the human from the machine" as infrastructure generations evolve

6. **Context enables capability**
   - LLM evals: Conversation traces + domain expertise enable accurate judging
   - AI agents: Session as external context enables long-horizon tasks
   - Agentic infra: Unified platform context (code + models + runtime) enables autonomous operations

## New Domain: AI Infrastructure & Agent Systems

[[Scaling Managed Agents: Decoupling the brain from the hands]] introduces a third domain: AI agent platform architecture.

### Core Architectural Insight

**Decouple the "brain" (Claude + harness) from the "hands" (tools/sandboxes) and the "session" (event log).** This virtualization—inspired by how operating systems abstract hardware—allows components to be swapped independently.

**Design for "programs as yet unthought of"**: Interfaces should be stable enough to outlast their implementations. Like `read()` working the same on 1970s disk packs and modern SSDs, agent interfaces (session, harness, sandbox) should accommodate future models and capabilities.

### From Pets to Cattle

**Coupled design** = "pets" (hand-tended servers you can't lose):
- Session + harness + sandbox in one container
- Container failure → lost session, requires nursing back to health
- Upfront provisioning for every session

**Decoupled design** = "cattle" (interchangeable, auto-recoverable):
- Session as durable external log (`getSession`, `emitEvent`, `getEvents`)
- Stateless harness recoverable via `wake(sessionId)`
- Sandbox provisioned lazily via `execute(name, input) → string`
- Component failures → spin up new one, pull state from session

### Key Wins

1. **Performance**: TTFT -60% (p50), -90% (p95) via lazy provisioning
2. **Reliability**: Components are cattle—auto-recoverable on failure
3. **Security**: Credentials isolated from sandbox where generated code runs
4. **Flexibility**: VPC-agnostic, supports many brains controlling many hands

### Session as External Context

The session log lives *outside* Claude's context window and is programmatically queryable. This solves long-horizon context management without irreversible trimming/compaction decisions.

**Separation of concerns**:
- **Session**: Durable, recoverable storage (append-only log)
- **Harness**: Context transformations (fetch via `getEvents()`, engineer for prompt cache, trim)

Can't predict what context engineering future models need, so push it into swappable harness.

### Harness Assumptions Go Stale

Claude Sonnet 4.5 had "context anxiety" near limits → harness added context resets. Opus 4.5 didn't have this behavior → resets became dead weight.

**Meta-harness approach**: Opinionated about interfaces (session, sandbox, harness APIs), unopinionated about implementations. Can run Claude Code, task-specific harnesses, or future harnesses not yet invented.

## New Source: Agentic Infrastructure

[[Agentic Infrastructure]] (Vercel, April 2026) describes the infrastructure transition driven by AI coding agents—a three-part evolution:

### The Three Evolutions

1. **Infrastructure for agents to deploy to**: Programmatic, deterministic surfaces (CLI/API, immutable deployments, preview URLs)
2. **Infrastructure for building/running agents**: Runtime primitives for agent workload shape (long-lived execution, orchestration, sandboxes, model routing)
3. **Infrastructure that is agentic**: Systems that autonomously monitor, investigate, and respond to production issues

### The Data

Agents are driving explosive growth in autonomous deployment:
- Weekly deployments on Vercel **doubled in 3 months**
- **30% of deployments** are agent-initiated (up 1000% in 6 months)
  - Claude Code: 75% of agent deployments
  - Lovable/v0: 6%
  - Cursor: 1.5%
- Agent-deployed projects are **20x more likely** to call AI inference providers
- Pattern: Agents writing AI-native software, agents building agents

### The Bottleneck: Operational Friction

Manual steps (UI clicks, Terraform state, human approvals) break autonomous loops. The solution:
- **Immutable deployments** (not just DX—absolute prerequisite for machine-driven development)
- **Preview URLs** (every commit verifiable programmatically)
- **Programmatic surfaces** (CLI/API, not clicks)

Result: Agents can write → test → verify → ship → rollback autonomously.

### Agent Workload Shape

Different from serverless:
- **Serverless**: Functions, caching, short requests at edge
- **Agents**: Long-lived execution, orchestration, model routing, cost controls, sandboxed code execution, abuse resistance

Requires different primitives: workflows, queues, sandboxes, observability, model gateway, fluid compute.

### Infrastructure That Is Agentic

**Traditional**: Code in → logs out → human reads logs → human fixes  
**Agentic**: Anomaly → platform investigates → reads code/logs → root-cause analysis → proposes fix → tests in sandbox → applies

**Enabler**: Unified platform context (shared visibility across code, model calls, runtime behavior). This context lets infrastructure "interpret what the developer intended, observe what the system actually did, and act on the delta."

**Current**: Human-in-loop approval  
**Future**: Autonomous remediation

### Philosophy

"The history of cloud computing is the history of removing the human from the machine. Agentic infrastructure is the next evolution, moving us from passive tools that wait for commands to proactive systems that act on our behalf."

### Connection to Managed Agents

Both [[Agentic Infrastructure]] and [[Managed Agents]] address long-running AI agents:
- **Managed Agents** (Anthropic): Decouple brain-hands-session, stable interfaces, pets → cattle
- **Agentic Infrastructure** (Vercel): Eliminate operational friction, unified context, autonomous operations

Complementary: Anthropic focuses on agent runtime architecture; Vercel focuses on deployment, observability, and self-healing infrastructure.

## Evolution

- **2026-04-05**: Wiki initialized
- **2026-04-05**: First source ingested—[[A pragmatic guide to LLM evals for devs]]—establishing foundation for LLM evaluation methodology
- **2026-04-08**: Domain expansion—ingested [[Optimizing Vercel Sandbox snapshots]] and [[Improving developer velocity with GitHub merge queue]]—adding infrastructure performance and developer workflow optimization
- **2026-04-11**: Third domain—ingested [[Scaling Managed Agents: Decoupling the brain from the hands]]—adding AI agent platform architecture and system design patterns
- **2026-04-13**: AI infrastructure deepened—ingested [[Agentic Infrastructure]]—adding deployment, observability, and autonomous operations for agent-driven development
