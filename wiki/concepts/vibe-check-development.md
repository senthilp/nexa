---
title: Vibe-Check Development
type: concept
category: anti-pattern
sources: 1
---

# Vibe-Check Development

## Definition

An anti-pattern in LLM application development where teams ship changes based on manual spot-checking and subjective assessment ("looks good to me") rather than systematic evaluation. Also called "vibes-based development."

## The Pattern

1. Developer changes a prompt
2. Tests a few inputs manually
3. If it "looks good to me" (LGTM), ship it
4. Hope for the best in production

## Why It Happens

Organizations embedding LLMs face unique challenges:
- Non-deterministic outputs
- Subjective and context-dependent responses
- No single "correct" answer
- Vast space of valid outputs makes comprehensive manual testing impossible

This ambiguity makes developers fall back on gut feelings rather than data.

## The Three Gulfs

Vibe-check development fails to address three fundamental gaps:

### 1. Gulf of Comprehension
The gap between a developer and true understanding of their data and model's behavior at scale. Impossible to manually read every query and inspect every response.

### 2. Gulf of Specification  
The gap between what we *want* the LLM to do and what our prompts *actually instruct* it to do. LLMs cannot read our minds; underspecified prompts force them to guess intent, leading to inconsistent outputs.

### 3. Gulf of Generalization
The gap between a well-written prompt and the model's ability to apply instructions reliably across all possible inputs. Even with perfect instructions, model can fail on new or unusual data.

## Why TDD Doesn't Solve It

Traditional Test-Driven Development assumes:
- Given input → single, deterministic, knowable output
- Can write test before implementation

With LLMs:
- Given input → thousands of potentially valid outputs
- Can't test for correctness before observing output range
- Must first define what "good" even means for your product

The challenge is both infinite input space AND vast space of valid, subjective, unpredictable outputs.

## Consequences

- **No regression detection**: Can't tell if changes break existing functionality
- **No systematic improvement**: Unclear which changes actually help
- **False confidence**: Things that "look good" in spot-checks fail at scale
- **Unactionable feedback**: When issues arise, no data to understand root causes

## The Solution

Replace vibe-checking with systematic [[Error Analysis]]:
1. Review conversation traces systematically
2. Code failures bottom-up
3. Build targeted [[LLM Evaluations]]
4. Measure improvements objectively

Move from guesswork to repeatable engineering discipline.

## Case Study

[[NurtureBoss]] was stuck in this trap:
- Built sophisticated agent
- Development felt like guesswork
- Changed prompts and tested a few inputs
- Shipped if it LGTM'd

After adopting [[Error Analysis]] and [[LLM Evaluations]]:
- Clear, data-driven priorities
- Systematic measurement of quality
- Repeatable improvement process

## Related Concepts

- [[Error Analysis]] — The systematic alternative
- [[LLM Evaluations]] — Tools for objective measurement
- [[Three Gulfs Model]] — Framework explaining why vibe-checking fails

## Referenced In

- [[A pragmatic guide to LLM evals for devs]]
