---
title: Conversation Traces
type: concept
category: observability
sources: 1
---

# Conversation Traces

## Definition

The complete record of an LLM interaction, including the initial user query, all intermediate reasoning steps, any tool calls made, and the final user-facing response. Everything needed to reconstruct what actually happened.

## Components

A trace typically includes:
- **Initial user query**: What the user asked
- **Intermediate reasoning**: LLM's internal thought process
- **Tool calls**: External APIs or functions invoked
- **Tool outputs**: Results from those calls
- **Final response**: What was shown to the user
- **Metadata**: Timestamps, model versions, parameters

## Purpose

Traces are the raw material for:
- **[[Error Analysis]]**: Systematic review to discover failure modes
- **Debugging**: Understanding why specific interactions failed
- **Quality assessment**: Evaluating LLM behavior at scale
- **Audit trails**: Reconstructing user experiences

## Viewing Traces

### Generic Tools
LLM-specific observability platforms:
- **LangSmith** (LangChain)
- **Arize Phoenix** (open source)
- **Braintrust**

These offer decent starting points but are generic by design.

### Custom Data Viewers

Often more effective to build domain-specific viewers:
- Show all necessary context on one screen
- Tailored to specific use case needs
- Collapsible sections for tool calls
- Simple annotation for adding notes
- Dramatically faster review throughput

[[NurtureBoss]] example: Built custom viewer in a few hours using AI assistant, unlocking significant improvement in review speed vs. fighting generic tools.

## Role in Error Analysis

### 1. Review Volume
Review at least 100 diverse traces for effective [[Error Analysis]].

### 2. Open Coding on Traces
Write open-ended notes on undesirable behavior observed in each trace.

### 3. Focus on First Upstream Failure
**Critical technique**: Identify and annotate only the first failure in a trace.
- LLM pipelines are causal systems
- Early errors cascade downstream
- Prevents cataloging every symptom
- Fixing first error often resolves entire chain

### 4. Bottom-Up Discovery
Let failure patterns emerge from traces rather than checking against predefined lists.

## Building Custom Viewers

[[Hamel Husain]]'s recommendation:
- **Invest a few hours** in custom tooling
- Use AI assistant to "vibe code" a simple viewer
- Doesn't need to be fancy
- Solve core problem: show context clearly
- Include easy annotation mechanism

This is described as "a game-changer" for review throughput.

## What Makes a Good Trace

- **Complete**: All steps from input to output
- **Contextual**: Domain-specific information visible
- **Actionable**: Easy to identify what went wrong
- **Annotatable**: Can add notes for error analysis

## Common Friction Points

Generic observability tools often:
- Require many clicks to see full context
- Don't show domain-specific data (e.g., property details for [[NurtureBoss]])
- Make annotation cumbersome
- Slow down review process

Custom viewers solve these specific pain points.

## From Traces to Improvements

Workflow:
1. Collect traces from production or synthetic data
2. Review systematically using trace viewer
3. Annotate failures ([[Open Coding]])
4. Group into themes ([[Axial Coding]])
5. Build targeted [[LLM Evaluations]]
6. Fix issues
7. Validate with more traces

Traces are both input (error analysis) and validation (post-fix verification).

## Related Concepts

- [[Error Analysis]] — Primary use of traces
- [[Open Coding]] — Annotation technique on traces
- [[Vibe-Check Development]] — What traces help you avoid
- [[LLM Evaluations]] — Built based on insights from traces

## Referenced In

- [[A pragmatic guide to LLM evals for devs]]
