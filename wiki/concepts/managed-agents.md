---
name: Managed Agents
type: concept
category: ai-infrastructure
---

# Managed Agents

Hosted service in the Claude Platform that runs long-horizon AI agents through stable interfaces designed to outlast any particular implementation.

## Core Principle

Design for "programs as yet unthought of"—interfaces should be general enough to accommodate future harnesses, models, and capabilities that don't exist yet. Like operating systems virtualizing hardware (`read()`, `process`, `file`), Managed Agents virtualizes agent components.

## Architecture

Three decoupled components:

1. **Session**: Append-only durable log of everything that happened
   - Interface: `getSession(id)`, `emitEvent(id, event)`, `getEvents()`
   - Lives outside Claude's context window
   - Enables programmatic context interrogation and recovery from failures

2. **Harness**: Loop that calls Claude and routes tool calls to infrastructure
   - Interface: `wake(sessionId)` to start/resume
   - Stateless—can be rebooted, uses session to recover state
   - Unopinionated meta-harness that can run any specific harness (e.g., Claude Code)

3. **Sandbox**: Execution environment for code/files
   - Interface: `provision({resources})`, `execute(name, input) → string`
   - Provisioned lazily only when needed
   - Accessed like any other tool

## Design Philosophy

**Opinionated about interfaces, not implementations**:
- Expect Claude will need state manipulation (session) and computation (sandbox)
- Expect need to scale to many brains and many hands
- Make NO assumptions about specific harness, number/location of brains or hands

**Harness assumptions go stale**: Earlier models had "context anxiety"; Opus 4.5 didn't. The fix became dead weight. Interfaces must evolve.

## Benefits

- **Reliability**: Components are "cattle" not "pets"—auto-recoverable on failure
- **Security**: Credentials isolated from sandbox where generated code runs
- **Performance**: 60% (p50) and 90% (p95) TTFT reduction via lazy provisioning
- **Flexibility**: VPC-agnostic, supports custom tools, MCP servers, multiple sandboxes per brain

## Related Concepts

- [[Agent Decoupling Patterns]] — The separation strategy
- [[Session-based Architecture]] — External context storage
- [[Harness Design]] — The loop implementation
- [[Pets vs Cattle]] — Infrastructure philosophy enabling this approach

## Sources

- [[Scaling Managed Agents: Decoupling the brain from the hands]] | Added: 2026-04-11
