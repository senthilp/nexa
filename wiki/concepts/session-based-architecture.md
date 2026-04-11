---
name: Session-based Architecture
type: concept
category: software-architecture
---

# Session-based Architecture

Design pattern where the session (append-only log of events) lives outside the agent's context window and serves as a durable, programmatically accessible context object.

## Core Idea

**Session ≠ Claude's context window**

The session is external, recoverable, and queryable. Claude's context window is a *view* into the session, not the session itself.

## Interface

- `getSession(id)` — Retrieve full session log
- `emitEvent(id, event)` — Append event to durable log
- `getEvents()` — Query specific slices of event stream

## Why External Storage?

**Problem with in-context approaches**:
- Long-horizon tasks exceed context window
- Standard solutions (compaction, trimming, memory tool) make irreversible decisions
- Hard to know which tokens future turns will need
- Transformations remove messages from Claude's window—only recoverable if stored elsewhere

**Session as external object**:
- Durable storage outside context window
- Programmatically accessible via `getEvents()`
- Enables positional slicing:
  - Resume from last read position
  - Rewind before specific moment to see lead-up
  - Reread context before specific action
- No irreversible information loss

## Separation of Concerns

**Session**: Recoverable context storage (durable, append-only)

**Harness**: Context management (transformations, caching, engineering)
- Fetches events from session via `getEvents()`
- Transforms before passing to Claude's window
- Transformations can include: context organization, prompt cache optimization, selective retention
- These transformations are "whatever the harness encodes"—session is agnostic

**Why separate?**
Can't predict what context engineering future models will require. Session guarantees durability and availability; harness handles model-specific optimizations.

## Recovery from Failure

When harness crashes:
1. New harness boots via `wake(sessionId)`
2. Calls `getSession(id)` to get event log
3. Resumes from last event
4. Session continuity maintained despite harness failure

## Comparison to REPL Pattern

Prior work explored context as REPL object that LLM accesses by writing code to filter/slice. Session provides same benefit but:
- Stored durably, not in sandbox
- Survives sandbox failures
- Accessible to harness, not just Claude

## Related Concepts

- [[Agent Decoupling Patterns]] — Session as one of three decoupled components
- [[Context Engineering]] — Harness-side transformations of session data
- [[Managed Agents]] — System implementing this architecture
- [[Harness Design]] — Component that queries session and manages context

## Sources

- [[Scaling Managed Agents: Decoupling the brain from the hands]] | Added: 2026-04-11
