---
title: Code-Based Evals
type: concept
category: evaluation technique
sources: 1
---

# Code-Based Evals

## Definition

Evaluations for LLM outputs that use traditional code assertions to verify deterministic, objective outcomes. The perfect tool for simple failures where there is one—and only one—correct answer.

## When to Use

Use code-based evals for **deterministic failures**:
- Date extraction and parsing
- Structured data extraction
- Format validation
- Objective outcomes measurable against expected values
- Anything that can be verified with code

**Rule**: If a failure can be verified with code, always use a code-based eval.

## How It Works

### 1. Create a Golden Dataset
Assemble test cases covering:
- Common patterns
- Tricky edge cases
- Diverse ways users might express the same thing

Example structure:
| User Query | Expected Output |
|------------|----------------|
| "Can I see the apartment on July 4th, 2026?" | 2026-07-04 |
| "How about tomorrow?" | [date_tomorrow] |
| "Next Friday works" | [next_friday_date] |

### 2. Write Assertion Function
Loop through dataset:
- Pass user query to AI system
- Run assertion function
- Compare AI output to expected output
- Pass/fail based on exact match

Example (Python):
```python
def eval_date_extraction(query, expected):
    result = ai_system.extract_date(query)
    assert result == expected, f"Expected {expected}, got {result}"
```

## Advantages

- **Cheaper to create and maintain**: Only run LLM to generate answer, then simple assertion
- **Faster execution**: No second LLM call needed for judging
- **Run frequently**: Can run on every commit to prevent regressions
- **Clear pass/fail**: No ambiguity in results
- **Regression detection**: Catches when changes break existing functionality

## Example: Date Handling at NurtureBoss

[[NurtureBoss]] identified date handling as a deterministic failure through [[Error Analysis]]:
- User query: "Can I see the apartment on July 4th, 2026?"
- One correct interpretation: 2026-07-04
- AI extracts date and formats for downstream tool
- Code-based eval asserts extracted date matches expected value

## Comparison to LLM-as-Judge

| Aspect | Code-Based Evals | LLM-as-Judge |
|--------|-----------------|--------------|
| Use case | Deterministic failures | Subjective quality |
| Speed | Fast | Slower (requires LLM call) |
| Cost | Cheap | More expensive |
| Frequency | Every commit | Less frequently |
| Ambiguity | None | Requires alignment |

## Integration

- **CI/CD pipelines**: Run on every commit
- **Pre-merge checks**: Prevent regressions before merging
- **Development loop**: Fast feedback during iteration

## Limitations

Cannot evaluate:
- Subjective quality (tone, helpfulness)
- Nuanced judgment calls
- Context-dependent appropriateness
- Multiple valid correct answers

For these cases, use [[LLM-as-Judge]].

## Related Concepts

- [[LLM-as-Judge]] — Complementary approach for subjective failures
- [[Golden Dataset]] — Test cases with expected outputs
- [[Error Analysis]] — Process for discovering what needs code-based evals
- [[LLM Evaluations]] — Broader category containing this technique

## Referenced In

- [[A pragmatic guide to LLM evals for devs]]
