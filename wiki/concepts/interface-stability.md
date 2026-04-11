---
name: Interface Stability
type: concept
category: software-architecture
---

# Interface Stability

Design principle: create abstractions stable enough to outlast their underlying implementations, allowing internals to change freely while top-level APIs remain constant.

## Core Idea

"Design for programs as yet unthought of" — the interfaces should be general enough for use cases that don't exist yet.

## Historical Example: Operating Systems

Operating systems solved this decades ago by virtualizing hardware into abstractions:
- **Process**: Abstraction for execution
- **File**: Abstraction for storage
- **`read()`**: Same command works on 1970s disk packs and modern SSDs

**Key insight**: The abstractions outlasted the hardware. `read()` is agnostic to storage medium. The interface on top stayed stable while implementations underneath changed freely.

## In Managed Agents

[[Managed Agents]] follows the same pattern:

**Stable interfaces**:
- `getSession(id)`, `emitEvent(id, event)`, `getEvents()` — Session operations
- `wake(sessionId)` — Harness operations
- `provision({resources})`, `execute(name, input) → string` — Sandbox operations

**Variable implementations**:
- Harness specifics (Claude Code, task-specific harnesses, future harnesses)
- Sandbox types (containers, phones, emulators, MCP servers)
- Context engineering strategies (compaction, trimming, caching)
- Model capabilities (Sonnet 4.5 "context anxiety" vs. Opus 4.5 without it)

**Benefit**: Implementations can be swapped without disturbing others. The meta-harness is opinionated about interface shape, not what runs behind them.

## Design Philosophy

**Be opinionated about interfaces**:
- Expect Claude will need state manipulation (session)
- Expect Claude will need computation (sandbox)
- Expect need to scale to many brains and many hands

**Don't be opinionated about implementations**:
- No assumptions about specific harness
- No assumptions about number/location of brains or hands
- No assumptions about future model capabilities

## Relation to "The Bitter Lesson"

Harnesses that encode assumptions about what Claude can't do will go stale. Interfaces that accommodate capability growth outlast fixed assumptions.

## Related Concepts

- [[Managed Agents]] — System implementing this principle
- [[Agent Decoupling Patterns]] — How components are separated
- [[Harness Design]] — Specific component that benefits from swappability

## Sources

- [[Scaling Managed Agents: Decoupling the brain from the hands]] | Added: 2026-04-11
