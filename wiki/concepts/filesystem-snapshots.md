---
title: Filesystem Snapshots
type: concept
domain: Infrastructure, Storage
---

# Filesystem Snapshots

Capturing and restoring the complete state of a filesystem at a point in time.

## Implementation in Vercel Sandbox

Vercel's snapshot system works with:
- **Raw disk images** (`.img`) — several GB uncompressed
- **Compressed snapshots** (`.vhs` format) — stored in S3

Operations:
- `sandbox.snapshot()` — compress `.img` → `.vhs` and upload to S3
- `Sandbox.create()` with snapshot — download `.vhs` and decompress

## Key Challenges

1. **Size**: Disk images can be several GBs, making network transfer slow
2. **Compression trade-off**: Reduces transfer size but adds CPU overhead
3. **Restore speed**: Critical for developer experience—40s was unacceptable, sub-second is target

## Optimization Strategies

From [[Optimizing Vercel Sandbox snapshots]]:
- Parallel downloads using HTTP Range requests (2-5x faster)
- Parallel decompression across multiple goroutines (2-4x faster)
- Streaming pipeline eliminating intermediate disk writes (2x faster)
- Local NVMe caching of decompressed images (95% hit rate)

## Use Cases

- Development environment persistence (Vercel's Automatic Persistence)
- Fast sandbox initialization from common base states
- Disaster recovery and rollback scenarios

## Sources

- [[Optimizing Vercel Sandbox snapshots]] (2026-04-08)
