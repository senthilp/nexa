---
title: Performance Optimization Strategies
type: concept
domain: Systems engineering, Infrastructure
---

# Performance Optimization Strategies

Common patterns for improving system performance, illustrated through infrastructure case studies.

## Core Strategies

### 1. Parallelization

**Pattern**: Break sequential operations into concurrent work units.

**Example from [[Optimizing Vercel Sandbox snapshots]]**:
- Parallel S3 downloads via HTTP Range requests (2-5x faster)
- Parallel decompression across goroutines (2-4x faster)
- Requires benchmarking to find optimal concurrency/chunk size

**When to use**: Operations with independent work units and available CPU/network capacity

### 2. Streaming Pipelines

**Pattern**: Eliminate intermediate storage by piping data between stages.

**Example**: S3 download → decompression without writing to disk (2x faster)

**When to use**: Multi-stage processing where intermediate results aren't reused

### 3. Local Caching

**Pattern**: Store frequently accessed data close to computation.

**Example**: NVMe cache with LRU eviction (95% hit rate for base snapshots)

**Key decisions**:
- What to cache: decompressed vs compressed (decompressed skips both network and CPU)
- Eviction policy: LRU works when access patterns have temporal locality
- Size limit: total disk space vs entry count

**When to use**: High locality of reference (reusing common base states)

### 4. Workflow Automation

**Pattern**: Remove manual steps from repetitive processes.

**Example from [[GitHub merge queue]]**: Automated PR testing and merging eliminates "update branch" babysitting

**When to use**: Humans performing mechanical, rule-based coordination

## Optimization Philosophy

From Vercel case study:
1. **Reliability first**: Never sacrifice correctness for speed
2. **Measure before optimizing**: Benchmark different approaches
3. **Compound gains**: Multiple strategies multiply (not just add)
4. **Cold path matters**: "Caching as bonus, not requirement"

## Measurement Approaches

- **Percentile-based**: p75, p95 (better than averages for user experience)
- **Before/after comparison**: 40s → sub-second tells clear story
- **Cache hit rate**: Validates locality assumptions

## Related Concepts

- [[Filesystem Snapshots]]
- [[LRU Cache]]
- [[Parallel Downloads]]
- [[Streaming Decompression]]

## Sources

- [[Optimizing Vercel Sandbox snapshots]] (2026-04-08)
- [[Improving developer velocity with GitHub merge queue]] (2026-04-08)
