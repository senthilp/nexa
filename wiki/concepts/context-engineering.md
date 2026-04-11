---
name: Context Engineering
type: concept
category: llm-engineering
---

# Context Engineering

Techniques for managing Claude's context window when dealing with long-horizon tasks that exceed window limits.

## Standard Approaches

All involve irreversible decisions about what to keep:

1. **Compaction**: Claude saves summary of context window
   - Harness removes compacted messages from window
   - Original messages recoverable only if stored externally

2. **Context trimming**: Selectively remove tokens
   - Old tool results, thinking blocks, etc.
   - Requires predicting what future turns won't need

3. **Memory tool**: Claude writes context to files
   - Enables learning across sessions
   - Often paired with trimming

## The Challenge

Difficult to know which tokens future turns will need. Irreversible decisions can lead to failures.

## Session-based Solution

In [[Managed Agents]]:
- **Session** provides recoverable context storage (durable log outside context window)
- **Harness** handles context transformations (fetch via `getEvents()`, transform, pass to Claude)
- Separation of concerns: session guarantees durability, harness handles model-specific optimizations

This allows:
- Context organization for high prompt cache hit rate
- Arbitrary transformations in harness
- No information loss—session remains queryable
- Future-proofing: can't predict what context engineering future models need, so push it into swappable harness

## Prior Work

Anthropic's prior research explored these techniques. Referenced but not detailed in [[Scaling Managed Agents: Decoupling the brain from the hands]].

## Related Concepts

- [[Session-based Architecture]] — External context storage enabling flexibility
- [[Harness Design]] — Component that performs transformations
- [[Managed Agents]] — System separating context storage from context management

## Sources

- [[Scaling Managed Agents: Decoupling the brain from the hands]] | Added: 2026-04-11 (mentions concept, references prior work)
