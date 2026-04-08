---
title: Vercel
type: organization
domain: Developer platform, infrastructure
---

# Vercel

Developer platform company building infrastructure for web applications and developer tooling.

## Products/Projects Mentioned

- **Vercel Sandbox**: Isolated development environments with filesystem snapshot support
- **[[Hive]]**: Internal builds infrastructure powering sandboxes and builds
- **Automatic Persistence**: Beta feature using snapshots for automatic save/restore

## Technical Infrastructure

- Runs on metal instances with NVMe storage
- Uses [[Firecracker]] microVMs for isolation
- Custom [[VHS Format]] for snapshot compression

## Sources

- [[Optimizing Vercel Sandbox snapshots]] (2026-04-08)
