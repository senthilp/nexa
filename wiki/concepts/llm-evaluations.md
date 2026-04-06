---
title: LLM Evaluations
type: concept
category: AI engineering
sources: 1
---

# LLM Evaluations

## Definition

Systematic approaches to measuring the quality of LLM-based applications. Evals are becoming a core part of the AI engineering toolset and CI/CD pipelines, addressing the challenge that LLMs are non-deterministic and cannot be tested with traditional automated tests.

## The Core Challenge

Unlike traditional software with deterministic outputs, LLMs:
- Don't guarantee the same answer to the same question twice
- Produce subjective, context-dependent responses
- Have "infinite surface area" of valid outputs
- Require new evaluation methodologies beyond conventional testing

## Why Traditional TDD Falls Short

Test-Driven Development assumes:
- Given input → single, deterministic, knowable output
- Can assert against correct output

With LLMs:
- Given input → thousands of potentially valid outputs
- Must first observe output range and define what "good" means
- Can't test for correctness before understanding the space of possibilities

## Two Main Approaches

### 1. Code-Based Evals
For deterministic, objective failures:
- Use assertions against expected outputs
- Cheaper to create and maintain
- Can run on every commit
- Example: date parsing, structured data extraction

### 2. LLM-as-Judge
For subjective, nuanced quality:
- Use an LLM to evaluate outputs based on criteria
- Requires careful alignment with human expertise
- More expensive but handles ambiguity
- Example: conversation handoffs, tone assessment

## Best Practices

- **Bottom-up discovery**: Let failure modes emerge from data, not pre-defined checklists
- **Use PASS/FAIL**: Binary judgments over rating scales for clarity and actionability
- **Avoid generic metrics**: "Hallucination score" or "helpfulness" often don't correlate with real user satisfaction
- **Focus on first failures**: In causal systems, early errors cascade downstream
- **Build custom tooling**: Generic observability tools create friction

## The Flywheel of Improvement

1. **Analyze** — Error analysis on traces
2. **Measure** — Build targeted evals
3. **Improve** — Fix identified issues
4. **Automate** — Scale evaluation
5. **Repeat** — Continuous improvement

## Integration Points

- CI/CD pipelines for regression testing
- Production monitoring for ongoing validation
- Development workflow for rapid iteration

## Related Concepts

- [[Error Analysis]] — Core workflow for discovering what to evaluate
- [[Code-Based Evals]] — Deterministic evaluation approach
- [[LLM-as-Judge]] — Subjective evaluation approach
- [[Vibe-Check Development]] — Anti-pattern evals solve

## Referenced In

- [[A pragmatic guide to LLM evals for devs]]
