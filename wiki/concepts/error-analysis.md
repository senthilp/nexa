---
title: Error Analysis
type: concept
category: methodology
sources: 1
---

# Error Analysis

## Definition

A systematic, bottom-up workflow for discovering and prioritizing failure modes in LLM applications by reviewing conversation traces and coding failures into taxonomies. Described by [[Hamel Husain]] as "the single highest-ROI activity in AI development."

## Origins

Error analysis is not new to LLMs—it's a battle-tested discipline that has been:
- A cornerstone of machine learning for decades
- Adapted from rigorous qualitative research methods like [grounded theory](https://en.wikipedia.org/wiki/Grounded_theory) from social sciences
- Applied to ensure focus on real problems rather than vanity metrics

## The Process

### 1. Build a Simple Data Viewer
- Most important investment
- Custom web app tailored to your domain
- Shows all necessary context in one place
- Makes capturing feedback trivial
- Often more efficient than fighting generic observability tools

### 2. Open Coding
- Review at least 100 diverse [[Conversation Traces]]
- Write open-ended notes on any undesirable behavior
- **Critical technique**: Identify and annotate only the **first upstream failure**
  - LLM pipelines are causal systems
  - Early errors create cascades of downstream issues
  - Prevents getting bogged down cataloging symptoms
  - Fixing the first error often resolves entire chains of failures
- No predefined checklists (avoid "hallucination," "toxicity" labels)
- Let the data speak for itself

Example notes:
- "Agent missed opportunity to re-engage price-sensitive user"
- "Asked to send text confirmation twice in a row"
- "Kept trying to solve problem instead of handing off to human"

### 3. Axial Coding
- Group open-ended notes into 5-10 themes
- Use an LLM to suggest initial clusters
- Always have human review and refine final categories
- Creates taxonomy of failure modes

### 4. Prioritize with Data
- Use pivot tables or scripts to count failure frequency
- Transforms qualitative insights into quantitative roadmap
- Reveals exactly where to focus engineering efforts

## Key Insight: The Antidote to Generic Metrics

Many teams grab pre-built metrics like "hallucination score" or "helpfulness," but these:
- Often worse than useless
- Create false sense of security
- Don't correlate with actual user satisfaction
- Are unactionable (can't tell what makes a "3" vs "4")

Bottom-up error analysis ensures evaluation efforts focus on **real problems from your data**.

## When There's No User Data

Use **synthetic data**:
- Powerful LLM generates diverse, realistic user queries
- Cover scenarios and edge cases you want to test
- Bootstrap entire error analysis process before first user
- Requires techniques for creating high-quality, grounded synthetic data

## The Three Gulfs Addressed

Error analysis helps bridge:
- **Gulf of Comprehension**: Systematic review reveals model behavior at scale
- **Gulf of Specification**: Failures show gaps between intent and prompts
- **Gulf of Generalization**: Edge cases reveal where model fails to apply instructions

## Tools That Support Error Analysis

- [[Conversation Traces]] viewers: LangSmith, Arize Phoenix, Braintrust
- Custom data viewers (often better than generic tools)
- Pivot tables for frequency analysis
- LLMs for suggesting initial categorizations

## Outputs

Error analysis produces:
- Prioritized list of failure modes
- Data-driven roadmap for improvements
- Foundation for building targeted [[LLM Evaluations]]
- Clear understanding of what "good" means for your product

## Case Study

[[NurtureBoss]] used error analysis to discover their top three issues:
1. Date handling errors
2. Handoff failures
3. Conversation flow issues

This data-driven insight replaced guesswork with clear engineering priorities.

## Related Concepts

- [[Open Coding]] — First step: bottom-up annotation
- [[Axial Coding]] — Second step: categorization into themes
- [[Conversation Traces]] — Raw material for analysis
- [[Vibe-Check Development]] — What error analysis replaces
- [[LLM Evaluations]] — What you build after error analysis

## Referenced In

- [[A pragmatic guide to LLM evals for devs]]
