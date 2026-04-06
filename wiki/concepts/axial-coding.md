---
title: Axial Coding
type: concept
category: methodology
sources: 1
---

# Axial Coding

## Definition

A qualitative research technique where open-ended notes from [[Open Coding]] are grouped into categorical themes or buckets. The second step in [[Error Analysis]], transforming raw observations into a structured taxonomy of failure modes.

## Origin

Borrowed from [grounded theory](https://en.wikipedia.org/wiki/Grounded_theory) in social sciences, where it's used to organize data around central categories after initial open coding.

## How It Works

### Input
Open-ended notes from reviewing 100+ [[Conversation Traces]]. For example:
- "Agent missed opportunity to re-engage price-sensitive user"
- "Asked to send text confirmation twice in a row"
- "Kept trying to solve problem instead of handing off to human"

### Process
1. **Group similar notes** into 5-10 themes
2. **Use LLM to suggest** initial clusters (as an assistant)
3. **Human review and refine** final categories (essential step)
4. **Name each theme** descriptively

### Output
Categorical buckets (axial codes) like:
- "Date Handling Errors"
- "Handoff Failures"
- "Conversation Flow Issues"

## Why 5-10 Themes?

- **Too few** (< 5): Loses important nuance, buckets too broad
- **Too many** (> 10): Dilutes focus, harder to prioritize
- **Sweet spot**: Enough granularity to be actionable, few enough to guide engineering

## LLM as Assistant

LLMs are useful for:
- Suggesting initial groupings from many notes
- Finding patterns humans might miss
- Speeding up the clustering process

**Critical**: Human must review and refine:
- LLM suggestions may miss domain context
- Categories need to align with product understanding
- Final taxonomy requires human judgment

## Visualizing Results

After axial coding, create a **pivot table** or frequency count:

| Failure Category | Count |
|-----------------|-------|
| Date Handling Errors | 45 |
| Handoff Failures | 38 |
| Conversation Flow Issues | 27 |
| Price Quote Problems | 12 |
| ... | ... |

This transforms qualitative insights into **quantitative roadmap**.

## From Themes to Action

The frequency count reveals:
1. **What to fix first**: Highest count = highest impact
2. **Clear priorities**: Data-driven, not guesswork
3. **ROI guidance**: Focus where most failures occur
4. **Eval targets**: What to build [[Code-Based Evals]] or [[LLM-as-Judge]] for

## Case Study: NurtureBoss

After open coding hundreds of traces:
1. **Collected** rich, messy notes
2. **Used LLM** to suggest initial groupings
3. **Reviewed and refined** categories
4. **Created pivot table** showing frequency
5. **Discovered** three issues accounted for most problems:
   - Date handling (deterministic → [[Code-Based Evals]])
   - Handoff failures (subjective → [[LLM-as-Judge]])
   - Conversation flow

Result: Clear, data-driven priority list replacing vibes-based guesswork.

## The Process Flow

1. **[[Open Coding]]** (previous step)
   - Review traces, write open-ended notes

2. **Axial Coding** (this step)
   - Group notes into 5-10 themes
   - LLM suggests, human refines

3. **Prioritization** (next step)
   - Count frequency per theme
   - Create engineering roadmap

## Common Patterns

Good axial codes are:
- **Descriptive**: "Date Handling Errors" not "Type A Failures"
- **Actionable**: Clear what needs fixing
- **Distinct**: Minimal overlap between categories
- **Domain-specific**: Reflect your product's actual problems

Poor axial codes:
- Generic ("Low Quality")
- Overlapping ("Errors" and "Failures")
- Too abstract ("Category 1")
- Too granular (50 micro-categories)

## Tools

- Spreadsheets for grouping and counting
- LLMs for suggesting clusters
- Simple scripts for frequency analysis
- Pivot tables for visualization

## Benefits

- **Finds signal in noise**: Many raw notes → clear themes
- **Data-driven priorities**: Frequency counts guide decisions
- **Shared understanding**: Team alignment on key issues
- **Foundation for evals**: Themes become evaluation targets
- **Avoids vanity metrics**: Focus on real problems from your data

## Antidote to Generic Metrics

Many teams use off-the-shelf metrics like "hallucination score" or "helpfulness." These:
- Don't reflect domain-specific issues
- Create false security
- Are unactionable

Axial coding ensures themes emerge from **your data**, addressing **your problems**.

## Related Concepts

- [[Open Coding]] — Previous step: collecting raw observations
- [[Error Analysis]] — Overall workflow containing this technique
- [[LLM Evaluations]] — Built based on themes from axial coding
- [[Vibe-Check Development]] — What systematic coding replaces

## Referenced In

- [[A pragmatic guide to LLM evals for devs]]
