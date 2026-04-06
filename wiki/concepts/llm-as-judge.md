---
title: LLM-as-Judge
type: concept
category: evaluation technique
sources: 1
---

# LLM-as-Judge

## Definition

An evaluation approach where an LLM is used to assess the quality of another LLM's outputs based on subjective or nuanced criteria. The judge LLM automates domain expert expertise by applying their reasoning consistently to thousands of traces.

## When to Use

Use LLM-as-judge for **subjective failures**:
- Conversation handoff decisions
- Tone and appropriateness
- Contextual judgment calls
- Cases with multiple valid answers
- Nuanced quality assessment

Example: Should an AI hand off to a human when a user says "I'm confused"? No single right answer—it depends on context and product philosophy.

## How It Works

### 1. Create Hand-Labeled Golden Dataset

Domain expert reviews traces and provides:
- **Binary PASS/FAIL judgment** (not rating scale)
- **Detailed critique** explaining reasoning

Example from [[NurtureBoss]] handoff evaluation:

| Trace | Judgment | Critique |
|-------|----------|----------|
| User asks for transfer | FAIL | "Agent continued trying to solve problem instead of immediate handoff" |
| User seems confused | PASS | "Agent asked one clarifying question before appropriate handoff" |
| Price negotiation | PASS | "Agent correctly identified this as requiring human expertise" |

### 2. Build the Judge

The hand-labeled examples, especially detailed critiques, become:
- Training data for the judge's reasoning
- Basis for prompts that encode domain expertise
- Reference for consistent application of standards

The judge learns to apply the domain expert's criteria at scale.

## Key Design Decision: PASS/FAIL vs Rating Scales

**Use binary PASS/FAIL, not Likert scales (1-5 ratings).**

### Why PASS/FAIL Works Better

- **Forces clarity**: Compels domain expert to define clear line between acceptable and unacceptable
- **Consistent**: Distinction between "3" and "4" is subjective and inconsistent across reviewers
- **Actionable**: "FAIL" is clear signal to fix; "3" is ambiguous
- **Faster**: Easier for human labelers
- **Focuses on what matters**: User success, not arbitrary point values

### Problems with Rating Scales

- Subjective and noisy
- Different reviewers interpret scales differently
- Unactionable for engineers
- False sense of precision
- Creates ambiguity instead of cutting through it

## Challenges

### 1. Judge Memorization
Risk: Judge memorizes answers from training data instead of generalizing criteria.

Solution: Partition data and measure how judge generalizes to unfamiliar data.

### 2. Alignment with Human Expertise
The judge must match human expert judgments.

Validation metrics:
- **True Positive Rate (TPR)**: How often judge correctly identifies failures
- **True Negative Rate (TNR)**: How often judge correctly identifies successes
- Compare judge decisions to human expert on held-out data

### 3. Trust
Engineers must trust the judge's assessments to act on them.

Building trust:
- Regular validation against human labels
- Clear explanation of judge's reasoning (critiques)
- Transparency about edge cases where judge struggles

## Cost vs Value Trade-off

- **More expensive** than [[Code-Based Evals]] (requires LLM call for each evaluation)
- **Run less frequently** (not on every commit)
- **Handles subjectivity** that code-based evals cannot
- **Scales human expertise** to thousands of evaluations

## Case Study: NurtureBoss Handoff Eval

Problem: When should AI hand off conversation to human?
- Not deterministic—context matters
- Product philosophy required (immediate handoff vs. try to help first)

Solution:
- Founder [[Jacob]] labeled examples with PASS/FAIL + critiques
- Built LLM-as-judge encoding his expertise
- Judge applies consistent reasoning at scale

## Comparison to Code-Based Evals

| Aspect | LLM-as-Judge | Code-Based Evals |
|--------|--------------|-----------------|
| Use case | Subjective quality | Deterministic failures |
| Speed | Slower (LLM call) | Fast |
| Cost | More expensive | Cheap |
| Frequency | Less frequent | Every commit |
| Ambiguity | Requires alignment | None |
| Handles nuance | Yes | No |

## Integration

- **CI/CD**: Can run, but less frequently than code evals
- **Production monitoring**: Validate quality continuously
- **Development**: Rapid iteration on subjective improvements
- **Flywheel**: Part of analyze → measure → improve → automate cycle

## Related Concepts

- [[Code-Based Evals]] — Complementary approach for deterministic failures
- [[Golden Dataset]] — Labeled examples for training/validation
- [[Error Analysis]] — Process for discovering what needs LLM-as-judge
- [[LLM Evaluations]] — Broader category containing this technique

## Referenced In

- [[A pragmatic guide to LLM evals for devs]]
