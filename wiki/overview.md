# Overview

## Current State

This knowledge base contains **3 sources** spanning two distinct domains:
1. **LLM Engineering** (1 source): Systematic evaluation and quality assurance
2. **Developer Productivity** (2 sources): Infrastructure optimization and workflow automation

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

2. **Measure the right thing**
   - LLM evals: PASS/FAIL over rating scales for actionability
   - Infrastructure: p95 latency over averages for user experience

3. **Automation compounds**
   - LLM evals: Error analysis → eval suites → continuous improvement flywheel
   - Developer workflow: Merge queue → eliminated babysitting → focus on coding

4. **Domain-specific beats generic**
   - LLM evals: Custom metrics outperform off-the-shelf scores
   - Infrastructure: Custom `.vhs` format + caching strategy tailored to Vercel's usage patterns

## Evolution

- **2026-04-05**: Wiki initialized
- **2026-04-05**: First source ingested—[[A pragmatic guide to LLM evals for devs]]—establishing foundation for LLM evaluation methodology
- **2026-04-08**: Domain expansion—ingested [[Optimizing Vercel Sandbox snapshots]] and [[Improving developer velocity with GitHub merge queue]]—adding infrastructure performance and developer workflow optimization
