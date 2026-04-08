---
title: Optimizing Vercel Sandbox snapshots
date_added: 2026-04-08
source_type: article
source_path: raw/sandbox-snapshots.md
authors: Tom Lienard, Rob Herley, Luke Phillips-Sheard, Guðmundur Bjarni Ólafsson
published: 2026-04-02
---

# Optimizing Vercel Sandbox snapshots

## Summary

Vercel Sandbox enables filesystem snapshots to save and restore entire sandbox states. The team optimized snapshot restore performance from 40+ seconds (p75) to sub-second through three key strategies: parallelization of downloads and decompression, streaming pipelines that eliminate intermediate disk writes, and local NVMe caching with LRU eviction achieving 95% hit rate. The optimizations power the Automatic Persistence feature (beta), making stop-and-resume cycles feel instant.

## Key Insights

- **Reliability first, performance second**: Team prioritized never failing to snapshot or losing data before tackling performance bottlenecks
- **Parallelization compounds**: Parallel S3 range requests (2-5x) + parallel decompression (2-4x) delivered multiplicative gains
- **Streaming eliminates latency**: Piping downloads directly into decompression (another 2x) removed the wait for intermediate file writes
- **Local caching dominates**: 95% cache hit rate from reusing base snapshots makes most restores sub-second, avoiding the network entirely
- **Decompression matters**: Caching the decompressed `.img` rather than compressed `.vhs` skips both download and decompression on cache hits

## Architecture Context

Vercel Sandbox runs on [[Hive]], Vercel's internal builds infrastructure. Each sandbox is an isolated container inside a [[Firecracker]] microVM running on metal instances with NVMe disks (several terabytes of fast local storage).

Snapshot files:
- **Raw disk image** (`.img`) — several GBs uncompressed
- **Compressed snapshot** (`.vhs` — Vercel Hive Snapshot format) — uploaded/downloaded from S3

## Optimization Journey

### 1. Parallel Downloads (2-5x faster)
Original: Single S3 request downloading entire `.vhs` file (several seconds to tens of seconds for 200MB–few GB files)

Optimized: HTTP Range headers to download chunks in parallel using AWS Go SDK's `transfermanager` API. Benchmarked different concurrency levels and chunk sizes.

### 2. Parallel Decompression (2-4x faster)
Original: Single-threaded sequential decoding/decompression of VHS frames

Optimized: One decoder feeding N decompression goroutines. The `.vhs` format stores a header plus a frame per allocated disk region, enabling parallel processing.

### 3. Streaming Pipeline (2x faster)
Original: Download complete → write to disk → read from disk → decompress

Optimized: Pipe S3 range request streams directly into decompression workers, eliminating intermediate disk I/O.

### 4. Local NVMe Cache (95% hit rate)
Original: No cache — every restore fetched from S3

Optimized: LRU cache on local NVMe storing decompressed `.img` files directly. Sized by total disk space rather than entry count. Cache hits skip both download and decompression.

**Why it works**: Most customers reuse a "base" snapshot across many sandboxes, creating extremely high locality.

## Results

- **p75**: 40s → sub-second
- **p95**: 50s → 5s
- **Cache hit rate**: 95%+ sustained

## Future Directions

- **Cache affinity**: Route sandboxes to metal instances that already have the requested snapshot cached (risks thundering herds and hotspots, requires deliberate design)
- **Cold path optimization**: Goal is to make the cold path fast enough that caching is a bonus, not a requirement

## Entities Mentioned

- [[Tom Lienard]] — co-author
- [[Rob Herley]] — co-author
- [[Luke Phillips-Sheard]] — co-author
- [[Guðmundur Bjarni Ólafsson]] — co-author
- [[Vercel]] — company
- [[Hive]] — Vercel's builds infrastructure
- [[Firecracker]] — microVM technology

## Concepts

- [[Filesystem Snapshots]] — capturing entire disk state
- [[VHS Format]] — Vercel Hive Snapshot compression format
- [[Parallel Downloads]] — HTTP Range requests
- [[Streaming Decompression]] — piping network streams into CPU workers
- [[LRU Cache]] — Least Recently Used eviction policy
- [[NVMe Storage]] — fast local solid-state storage

## Related Sources

- Vercel Sandbox documentation on snapshots
- AWS S3 performance optimization guidelines (Range requests)

## Quotes

> "p75 snapshot restores were taking over 40 seconds, and through parallelization and local caching, we brought that under one second."

> "Most customers reuse a 'base' snapshot across many sandboxes, which gives us a 95% cache hit rate."

> "Long term, we want the cold path fast enough that caching is a bonus, not a requirement."
