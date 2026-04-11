---
name: Harness Design
type: concept
category: ai-infrastructure
---

# Harness Design

The loop that calls Claude and routes Claude's tool calls to the relevant infrastructure.

## Definition

A harness orchestrates the agent execution cycle:
1. Call Claude with current context
2. Receive tool calls from Claude
3. Route tool calls to appropriate infrastructure (sandbox, API, MCP server, etc.)
4. Return results to Claude
5. Repeat

## Key Insight: Assumptions Go Stale

Harnesses encode assumptions about what Claude can't do on its own. These assumptions must be questioned frequently because they go stale as models improve.

**Example**: Claude Sonnet 4.5 showed "context anxiety"—wrapping up tasks prematurely near context limits. Solution: add context resets to harness. But Opus 4.5 didn't have this behavior. The resets became dead weight.

## Meta-Harness Approach

[[Managed Agents]] is a **meta-harness**: unopinionated about the *specific* harness Claude needs, but opinionated about interfaces:

- **Session interface**: `getSession(id)`, `emitEvent(id, event)`, `getEvents()`
- **Sandbox interface**: `provision({resources})`, `execute(name, input) → string`
- **Harness interface**: `wake(sessionId)` to start/resume

This allows any harness to run:
- Claude Code (general-purpose, used widely)
- Task-specific harnesses (excel in narrow domains)
- Future harnesses not yet invented

## Harness as Stateless Component

In decoupled architecture:
- Harness doesn't need to survive crashes
- Session log sits outside harness
- On failure: boot new harness via `wake(sessionId)`, pull log via `getSession(id)`, resume
- Harness writes events during loop via `emitEvent(id, event)`

## Responsibilities

- Query session for events (`getEvents()`)
- Transform events before passing to Claude's context window
- Perform context engineering (caching, trimming, organization)
- Route tool calls to infrastructure
- Handle tool failures/retries
- Maintain no persistent state (relies on session)

## Related Concepts

- [[Managed Agents]] — Meta-harness system
- [[Session-based Architecture]] — External state harness relies on
- [[Agent Decoupling Patterns]] — Harness as "brain" component
- [[Context Engineering]] — Techniques harness implements

## Sources

- [[Scaling Managed Agents: Decoupling the brain from the hands]] | Added: 2026-04-11
