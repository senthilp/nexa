# Nexa Log

This is a chronological record of all operations performed on this knowledge base.

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
