---
name: Agent Decoupling Patterns
type: concept
category: software-architecture
---

# Agent Decoupling Patterns

Design strategy for AI agent systems: separate the "brain" (Claude + harness) from the "hands" (tools/sandboxes) and the "session" (event log) so each can fail or be replaced independently.

## The Decoupling

### Brain (Claude + Harness)
- **Role**: Intelligence, decision-making, tool orchestration
- **Interface**: `wake(sessionId)` to start/resume
- **State**: Stateless—recovers from session log
- **Scaling**: Many brains can run concurrently, connect to different hands

### Hands (Tools/Sandboxes)
- **Role**: Execution, action-taking, computation
- **Interface**: `execute(name, input) → string`
- **Provisioning**: Lazy, via `provision({resources})` only when needed
- **Flexibility**: Can be containers, phones, emulators, MCP servers—harness doesn't know or care

### Session (Event Log)
- **Role**: Durable record of everything that happened
- **Interface**: `getSession(id)`, `emitEvent(id, event)`, `getEvents()`
- **Storage**: External to both brain and hands
- **Purpose**: Recovery from failures, programmatic context access

## Why Decouple?

**Coupled design problems**:
1. **Single point of failure**: One container holds everything
2. **Debugging requires user data access**: Can't troubleshoot without shell into container
3. **Network assumptions baked in**: VPC peering required for customer resources
4. **Upfront provisioning cost**: Every session pays full container setup, even if unused
5. **Security boundary issues**: Credentials in same environment as generated code

**Decoupled design benefits**:
1. **Independent failure domains**: Harness fails ≠ sandbox fails ≠ session fails
2. **Observability**: Session log provides debugging window without user data access
3. **Network-agnostic**: Hands provisioned wherever resources live
4. **Lazy initialization**: Provision only what's needed, when needed
5. **Security isolation**: Credentials in vault/bundled with resources, never in sandbox

## Implementation Patterns

### Harness leaves the container
- Harness calls sandbox like any other tool: `execute(name, input) → string`
- Sandbox becomes cattle—if it dies, catch as tool error, pass to Claude, retry with new container

### Session as external context
- Session log lives outside harness
- Harness writes via `emitEvent(id, event)` during agent loop
- On harness failure, new harness calls `wake(sessionId)`, pulls log via `getSession(id)`, resumes

### Many brains, many hands
- Brains are stateless harnesses—scale by starting more
- Each brain can control multiple hands
- Hands can be passed between brains
- No 1:1 coupling

## Performance Impact

**Time-to-first-token (TTFT)**:
- Coupled: Every session waits for container provision, repo clone, process boot
- Decoupled: Container provisioned via tool call only if needed
- Result: p50 -60%, p95 -90%

## Security Model

**Credential isolation**:
- Git tokens: Bundled during init, wired into local git remote
- Custom tools: MCP proxy fetches from vault, makes calls
- Result: Generated code in sandbox never sees credentials

## Related Concepts

- [[Managed Agents]] — System implementing these patterns
- [[Pets vs Cattle]] — Philosophy enabling this approach
- [[Session-based Architecture]] — External context storage
- [[Harness Design]] — The brain implementation
- [[Interface Stability]] — Why these abstractions matter

## Sources

- [[Scaling Managed Agents: Decoupling the brain from the hands]] | Added: 2026-04-11
