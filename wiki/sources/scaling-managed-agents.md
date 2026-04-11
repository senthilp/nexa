---
title: "Scaling Managed Agents: Decoupling the brain from the hands"
date_added: 2026-04-11
source_type: article
source_path: raw/Scaling Managed Agents Decoupling the brain from the hands.md
url: https://www.anthropic.com/engineering/managed-agents
authors: Lance Martin, Gabe Cemaj, Michael Cohen
published: 2026
---

# Scaling Managed Agents: Decoupling the brain from the hands

## Summary

Anthropic built Managed Agents, a hosted service for running long-horizon AI agents, by solving an old computing problem: designing for "programs as yet unthought of." The core architectural decision was to **decouple the "brain" (Claude + harness) from the "hands" (sandboxes/tools) and the "session" (event log)**. This virtualization approach—inspired by how operating systems abstract hardware—allows each component to be swapped independently without disturbing the others. The result: dramatically improved reliability, security, performance (60-90% TTFT reduction), and flexibility to accommodate future model capabilities.

## Key Insights

- **Abstractions outlast implementations**: Like OS primitives (`read()`, `process`, `file`), agent interfaces should be stable while implementations evolve
- **From pets to cattle**: Coupled components create "pets" (hand-tended servers you can't lose); decoupling makes them "cattle" (interchangeable, auto-recoverable)
- **Session as external context**: The durable event log lives outside Claude's context window and is programmatically queryable via `getEvents()`, solving long-horizon context management
- **Security via isolation**: Credentials stored in vaults/bundled with resources, never accessible from sandbox where Claude's generated code runs
- **Lazy provisioning wins**: Containers provisioned only when needed via tool call, not upfront—dropped TTFT by 60% (p50) and 90% (p95)
- **Harness assumptions go stale**: Earlier models had "context anxiety" near limits; Opus 4.5 didn't. The fix (context resets) became dead weight. Interfaces must accommodate this evolution.

## Entities Mentioned

- [[Anthropic]] — AI safety/research company, creator of Claude and Managed Agents
- [[Lance Martin]] — Primary author, Anthropic engineer
- [[Gabe Cemaj]] — Co-author, Anthropic engineer
- [[Michael Cohen]] — Co-author, Anthropic engineer
- [[Nodir Turakulov]] — Contributor, conversations on topics
- [[Jeremy Fox]] — Contributor, conversations on topics
- [[Jake Eaton]] — Contributor, Agents API team

## Concepts

- [[Managed Agents]] — Hosted service for running long-horizon agents via stable interfaces
- [[Pets vs Cattle]] — Infrastructure philosophy: hand-tended individuals vs. interchangeable units
- [[Agent Decoupling Patterns]] — Separating brain (harness), hands (tools/sandboxes), session (log)
- [[Session-based Architecture]] — Durable event log as external, queryable context object
- [[Context Engineering]] — Techniques for managing Claude's context window (compaction, trimming, memory tool)
- [[Harness Design]] — Loop that calls Claude and routes tool calls to infrastructure
- [[Sandbox Virtualization]] — Execution environment abstraction for code/file operations
- [[Security Boundaries in AI Systems]] — Isolating credentials from untrusted code execution
- [[Time-to-first-token]] — Latency metric users most acutely feel (session start to first response)
- [[Interface Stability]] — Designing systems where top-level APIs persist as internals change

## Architecture Evolution

**Coupled design (initial)**:
- Session + harness + sandbox in one container
- File edits as direct syscalls, no service boundaries
- **Problems**: Container becomes "pet," debugging requires shell access to user data, VPC peering required for customer resources, upfront container provisioning for every session

**Decoupled design (final)**:
- **Session**: Durable log accessed via `getSession(id)`, `emitEvent(id, event)`
- **Harness**: Stateless, recoverable via `wake(sessionId)`, calls tools via `execute(name, input) → string`
- **Sandbox**: Provisioned lazily via `provision({resources})`, accessed like any other tool
- **Benefits**: Cattle not pets, independent failure domains, no VPC assumptions, lazy provisioning, credential isolation

## Performance Impact

- **p50 TTFT**: ~60% reduction
- **p95 TTFT**: >90% reduction
- **Cause**: Containers provisioned only when needed via tool call, not upfront. Sessions not needing sandbox start inference immediately after pulling session log.

## Security Model

**Threat**: Prompt injection → Claude reads its own credentials → spawns unrestricted sessions

**Mitigations**:
1. **Git tokens**: Bundled during sandbox init, wired into local git remote. Claude never handles token.
2. **Custom tools**: MCP proxy holds session token, fetches OAuth credentials from vault, makes external calls. Harness never sees credentials.
3. **Structural boundary**: Tokens never reachable from sandbox where generated code runs.

## Contradictions/Updates

- **Extends** [[Harness Design]] with meta-harness concept—unopinionated about specific harness implementation
- **Relates to** [[Context Engineering]]—session provides external context storage without irreversible trimming decisions
- **Echoes** [[Performance Optimization Strategies]]—lazy provisioning, measuring what users feel (TTFT), eliminating dead time

## Quotes

> "Harnesses encode assumptions about what Claude can't do on its own. However, those assumptions need to be frequently questioned because they can go stale as models improve."

> "The challenge we faced is an old one: how to design a system for 'programs as yet unthought of.'"

> "The abstractions on top stayed stable while the implementations underneath changed freely."

> "We no longer had to nurse failed containers back to health."

> "The session provides this same benefit, serving as a context object that lives outside Claude's context window."

## Related Sources

- [[Building Effective Agents]] — Referenced as prior work on agent design
- [[Effective Harnesses for Long-Running Agents]] — Referenced as prior work on harness design patterns
- [[Effective Context Engineering for AI Agents]] — Referenced for context management techniques
- [[The Bitter Lesson]] (Rich Sutton) — Referenced re: assumptions going stale
- [[The Art of Unix Programming]] (ESR) — Referenced re: "programs as yet unthought of"
