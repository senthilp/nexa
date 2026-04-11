---
name: Pets vs Cattle
type: concept
category: infrastructure-philosophy
---

# Pets vs Cattle

Infrastructure management philosophy distinguishing between two approaches to servers:

## Definitions

**Pets**: Named, hand-tended individuals you can't afford to lose
- Must be nursed back to health when sick
- Require careful maintenance
- Difficult to replace
- State is precious and coupled

**Cattle**: Interchangeable, numbered units
- If one fails, replace it with a new one
- Auto-recoverable from standard recipes
- Stateless or state lives elsewhere
- Disposable and rebuildable

## In Managed Agents Context

**Initial coupled design = pets**:
- Session + harness + sandbox in one container
- Container failure → lost session
- Unresponsive container → engineer debugs inside it
- Can't afford to lose it, must nurse it back

**Decoupled design = cattle**:
- Session stored externally
- Harness stateless, recoverable via `wake(sessionId)`
- Sandbox provisioned from standard recipe via `provision({resources})`
- Any component fails → spin up new one, pull state from session
- "We no longer had to nurse failed containers back to health"

## Implications

Moving from pets to cattle requires:
1. **External state storage**: Session log outside containers
2. **Standard provisioning**: Reproducible initialization
3. **Stateless components**: Recovery without local state
4. **Fail-fast tolerance**: Errors become tool-call failures, not system failures

## Related Concepts

- [[Agent Decoupling Patterns]] — How to achieve cattle-like components
- [[Session-based Architecture]] — External state storage enabling this
- [[Managed Agents]] — System designed around cattle philosophy

## Sources

- [[Scaling Managed Agents: Decoupling the brain from the hands]] | Added: 2026-04-11
