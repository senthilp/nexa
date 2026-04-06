---
title: Golden Dataset
type: concept
category: evaluation
sources: 1
---

# Golden Dataset

## Definition

A curated collection of test cases with expected outputs or expert judgments, used as ground truth for evaluating LLM systems. The foundation for both [[Code-Based Evals]] and [[LLM-as-Judge]] approaches.

## Two Types

### 1. For Code-Based Evals
**Structure**: Input queries with expected outputs

Example from [[NurtureBoss]] date handling:

| User Query | Expected Output |
|------------|----------------|
| "Can I see the apartment on July 4th, 2026?" | 2026-07-04 |
| "How about tomorrow?" | 2026-08-29 |
| "Next Friday works" | 2026-09-06 |
| "I'm free this weekend" | [2026-08-30, 2026-08-31] |

**Characteristics**:
- Deterministic, objective outcomes
- Single correct answer per input
- Can be verified with code assertions
- Expected output is the "gold standard"

### 2. For LLM-as-Judge
**Structure**: Traces with expert judgments and critiques

Example from NurtureBoss handoff evaluation:

| Trace | Judgment | Critique |
|-------|----------|----------|
| [User asks for transfer] | FAIL | "Agent continued trying to solve problem instead of immediate handoff" |
| [User seems confused] | PASS | "Agent asked one clarifying question before appropriate handoff" |
| [Complex price question] | PASS | "Agent correctly identified this as requiring human expertise" |

**Characteristics**:
- Subjective, nuanced quality assessment
- Binary PASS/FAIL (not rating scales)
- Detailed critiques explaining reasoning
- Expert judgment is the "gold standard"

## How to Build

### For Deterministic Cases

1. **Brainstorm variations**: Different ways users might express the same thing
2. **Include edge cases**: Tricky scenarios that might break
3. **Cover common patterns**: Frequent user behaviors
4. **Define expected outputs**: What the correct answer should be

### For Subjective Cases

1. **Domain expert reviews traces**: Someone who knows what "good" looks like
2. **Binary judgments**: PASS or FAIL for each case
3. **Write detailed critiques**: Explain the reasoning
4. **Capture product philosophy**: Encode what matters for your use case

## Why PASS/FAIL Instead of Ratings?

For subjective cases, use binary judgments, not 1-5 scales:

**Problems with rating scales**:
- Subjective and inconsistent across reviewers
- Distinction between "3" and "4" is unclear
- Unactionable for engineers
- Creates noise rather than clarity

**Benefits of PASS/FAIL**:
- Forces clear line between acceptable and unacceptable
- Consistent across reviewers
- Actionable: FAIL = must fix
- Focuses on what truly matters for user success
- Faster for human labelers

## Size Considerations

- **For code-based**: Comprehensive coverage of patterns and edge cases (dozens to hundreds)
- **For LLM-as-judge**: Enough examples to train and validate judge (50-200+ labeled traces)
- **Partition data**: Training set (build judge) + test set (validate judge doesn't memorize)

## Common Coverage Areas

Good golden datasets include:
- **Happy path**: Normal, expected inputs
- **Edge cases**: Unusual but valid scenarios
- **Failure modes**: Known problem patterns
- **Diverse inputs**: Different phrasings, contexts
- **Ambiguous cases**: Boundary conditions

## Role of Synthetic Data

When real user data is limited:
- Use powerful LLM to generate realistic queries
- Cover scenarios and edge cases you want to test
- Bootstrap evaluation before first user
- Requires techniques for grounded, high-quality generation

Note: Synthetic data generation is a deep topic covered in [[Hamel Husain]]'s course.

## Maintenance

Golden datasets are not static:
- **Add new cases** as new failure modes discovered
- **Update expected outputs** when requirements change
- **Retire obsolete cases** when no longer relevant
- **Continuous evolution** as product evolves

## Quality Criteria

A good golden dataset:
- **Representative**: Covers real usage patterns
- **Diverse**: Wide range of scenarios
- **Unambiguous**: Clear what correct answer is
- **Actionable**: Failures point to specific problems
- **Maintainable**: Easy to update as product evolves

## From Golden Dataset to Evals

### Code-Based Flow
1. Create golden dataset with expected outputs
2. Loop through each test case
3. Pass input to AI system
4. Assert output matches expected
5. Aggregate pass/fail results

### LLM-as-Judge Flow
1. Domain expert creates labeled golden dataset
2. Use examples (especially critiques) to build judge
3. Partition data for training and validation
4. Measure judge alignment with expert (TPR/TNR)
5. Use judge to evaluate at scale

## Case Study: NurtureBoss

Created two golden datasets:

**Date handling** (code-based):
- Absolute dates: "July 4th, 2026"
- Relative dates: "tomorrow," "next Friday"
- Ambiguous: "this weekend"
- Edge cases: holidays, month boundaries

**Handoff decisions** (LLM-as-judge):
- Clear handoff requests
- Subtle confusion signals
- Complex questions requiring expertise
- Each with PASS/FAIL + detailed critique from founder

## Related Concepts

- [[Code-Based Evals]] — Uses deterministic golden datasets
- [[LLM-as-Judge]] — Uses labeled golden datasets with critiques
- [[Error Analysis]] — Process for discovering what golden dataset should cover
- [[LLM Evaluations]] — What golden datasets enable

## Referenced In

- [[A pragmatic guide to LLM evals for devs]]
