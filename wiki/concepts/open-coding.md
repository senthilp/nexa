---
title: Open Coding
type: concept
category: methodology
sources: 1
---

# Open Coding

## Definition

A qualitative research technique adapted for LLM evaluation where reviewers write open-ended, descriptive notes about observed failures without using predefined categories or checklists. The first step in [[Error Analysis]].

## Origin

Borrowed from [grounded theory](https://en.wikipedia.org/wiki/Grounded_theory) in social sciences, where it's used to let patterns emerge from data rather than imposing pre-existing frameworks.

## How It Works

### What to Do
- Review at least 100 diverse [[Conversation Traces]]
- Write open-ended notes on any undesirable behavior
- Be descriptive, not categorical
- Let the data speak for itself

### What NOT to Do
- ❌ Use predefined checklists ("hallucination," "toxicity")
- ❌ Force observations into existing categories
- ❌ Rate on scales
- ❌ Catalog every symptom in a trace

### Critical Technique: First Upstream Failure Only

**Focus on the first observable failure in each trace.**

Why:
- LLM pipelines are causal systems
- Single early error creates cascades of downstream issues
- Prevents getting bogged down in symptoms
- More efficient use of review time
- Fixing the first error often resolves entire chains

## Example Notes

From [[NurtureBoss]] case study:

Good open coding notes:
- "The agent missed a clear opportunity to re-engage a price-sensitive user."
- "It asked to send a text confirmation twice in a row."
- "Once the user asked to be transferred to a human, the agent kept trying to solve the problem instead of just making the handoff."

Poor notes (too categorical):
- "Hallucination detected"
- "Low quality response"
- "Failed" (no details)

## Why Open-Ended?

Predefined categories like "hallucination" or "toxicity":
- Impose assumptions about failure modes
- Miss domain-specific problems
- Create false precision
- Lead to generic, unactionable metrics

Open coding:
- Discovers actual failure patterns in your data
- Surfaces domain-specific issues
- Produces actionable, specific insights
- Ensures focus on real problems

## The Process Flow

1. **Open Coding** (this step)
   - Review traces
   - Write open-ended notes
   - Minimum 100 traces for patterns to emerge

2. **[[Axial Coding]]** (next step)
   - Group open-ended notes into 5-10 themes
   - Create taxonomy of failure modes
   - Use LLM to suggest clusters, human to refine

3. **Prioritization**
   - Count frequency of each theme
   - Build data-driven roadmap

## Tools That Help

- Custom data viewers with annotation fields
- Simple text boxes for notes
- LLM assistance for later grouping (but not during open coding)

## Volume Needed

**At least 100 diverse traces** for effective error analysis.

Why 100+:
- Patterns need sufficient examples to emerge
- Edge cases need representation
- Statistical significance in frequency counts
- Balances thoroughness with practicality

## Common Pitfalls

1. **Too categorical**: Forcing notes into preconceived buckets
2. **Too vague**: "This is bad" doesn't help
3. **Cataloging symptoms**: Noting every downstream error instead of root cause
4. **Too few traces**: Patterns won't emerge from 10-20 examples

## Benefits

- Discover failure modes specific to your domain
- Avoid false security of generic metrics
- Create foundation for targeted [[LLM Evaluations]]
- Ensure engineering efforts focus on real problems
- Build understanding of what "good" means for your product

## Case Study

[[NurtureBoss]] used open coding to:
- Review hundreds of leasing assistant conversations
- Annotate failures in natural language
- Let patterns emerge organically
- Discover their top three issues were date handling, handoffs, and conversation flow

This bottom-up approach replaced guesswork with data-driven priorities.

## Related Concepts

- [[Axial Coding]] — Next step: grouping open codes into themes
- [[Error Analysis]] — Overall workflow containing this technique
- [[Conversation Traces]] — What you perform open coding on
- [[Vibe-Check Development]] — Anti-pattern that open coding replaces

## Referenced In

- [[A pragmatic guide to LLM evals for devs]]
