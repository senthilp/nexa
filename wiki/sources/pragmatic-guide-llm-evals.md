---
title: "A pragmatic guide to LLM evals for devs"
date_added: 2026-04-05
source_type: article
source_path: raw/a_pragmatic_guide_to_LLM_evals_for_devs.md
author: Hamel Husain (via Gergely Orosz)
published: 2025-12-02
url: https://newsletter.pragmaticengineer.com/p/evals
---

# A pragmatic guide to LLM evals for devs

## Summary

This article presents a systematic engineering approach to evaluating LLM applications, moving teams away from "vibe-check development" toward data-driven quality assurance. Written by [[Hamel Husain]] and published in [[Gergely Orosz]]'s Pragmatic Engineer newsletter, it draws on real-world experience from [[NurtureBoss]] and 40+ other companies. The core workflow centers on error analysis: systematically reviewing conversation traces, coding failures bottom-up, and building targeted evaluations that measure what actually matters to users.

## Key Insights

- **LLMs are non-deterministic**, making traditional testing approaches insufficient. The "infinite surface area" problem isn't just unlimited inputs—it's the vast space of valid, subjective, unpredictable outputs.

- **The three gulfs framework** explains why LLM development is hard:
  - Gulf of Comprehension: understanding data and model behavior at scale
  - Gulf of Specification: gap between intent and prompts
  - Gulf of Generalization: model's ability to apply instructions reliably

- **Error analysis is the highest-ROI activity** in AI development. It's adapted from decades of ML best practices and grounded theory in social sciences.

- **Build custom data viewers** rather than fighting generic observability tools. A few hours of "vibe coding" unlocks massive review throughput.

- **Generic metrics are worse than useless**—they create false security. Bottom-up discovery of failure modes ensures focus on real problems.

- **Two types of evals for two types of failures**:
  - Code-based assertions for deterministic outcomes (cheaper, run on every commit)
  - LLM-as-judge for subjective quality (requires careful alignment)

- **Use PASS/FAIL, not rating scales**. Binary judgments force clarity and are more actionable for engineers.

- **Focus on first upstream failures** in traces. LLM pipelines are causal; early errors cascade downstream.

## Entities Mentioned

- [[Hamel Husain]] — ML engineer, instructor, author of the article
- [[Gergely Orosz]] — Author of Pragmatic Engineer newsletter
- [[NurtureBoss]] — AI leasing assistant startup, case study company
- [[Jacob]] — Founder of NurtureBoss, domain expert for eval labeling

## Concepts

- [[LLM Evaluations]] — Systematic approach to measuring LLM quality
- [[Error Analysis]] — Bottom-up workflow for discovering failure modes
- [[Vibe-Check Development]] — Anti-pattern of shipping based on manual spot-checking
- [[Open Coding]] — Writing open-ended notes on observed failures
- [[Axial Coding]] — Grouping open codes into categorical themes
- [[Code-Based Evals]] — Deterministic assertions for objective failures
- [[LLM-as-Judge]] — Using an LLM to evaluate subjective quality
- [[Conversation Traces]] — Complete records of LLM interactions
- [[Golden Dataset]] — Curated test cases with expected outputs
- [[Three Gulfs Model]] — Framework for understanding LLM development challenges

## Workflow Steps

### Error Analysis Process
1. Build a simple custom data viewer
2. Open coding: Review 100+ diverse traces with open-ended notes
3. Axial coding: Group notes into 5-10 themes
4. Prioritize failures with pivot tables

### Building Evals
- **For deterministic failures**: Create golden dataset → Write code-based assertions
- **For subjective failures**: Domain expert labels with critiques → Build LLM-as-judge

## Tools Mentioned

- LangSmith, Arize Phoenix, Braintrust — LLM observability platforms
- Custom data viewers — Domain-specific review tools

## Contradictions/Updates

*No contradictions with existing knowledge base (first source).*

## Quotes

> "This is the 'vibes-based development' trap, and it's where many AI projects go off the rails."

> "You can't test for correctness before you've systematically observed the range of possible outputs and have defined what 'good' even means for your product."

> "Generic metrics create a false sense of security, leading teams to optimize for scores that don't actually correlate with user satisfaction."

> "Binary decisions force clarity and compel a domain expert to define a clear line between acceptable and unacceptable."

## Related Sources

*First source in the knowledge base.*

## Notes

- Article appears incomplete in the source file (cuts off at section 4)
- NurtureBoss case study provides concrete examples throughout
- Draws on Hamel's course "AI Evals For Engineers & PMs" and upcoming O'Reilly book
- Emphasizes synthetic data for bootstrapping when real user data is limited
