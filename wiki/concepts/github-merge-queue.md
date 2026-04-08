---
title: GitHub Merge Queue
type: concept
domain: Developer workflow, CI/CD
---

# GitHub Merge Queue

Automated system for testing and merging pull requests in order, eliminating manual "update branch" cycles in repositories with linear commit history requirements.

## The Problem It Solves

**Manual PR babysitting**: In repos requiring PRs to be tested on `HEAD` before merge:
1. Developer clicks "Update Branch" to rebase on latest
2. CI runs (e.g., 10 minutes)
3. While waiting, another PR merges
4. "Update Branch" button reappears—must repeat entire cycle
5. With multiple open PRs, this becomes significant time waste

## How It Works

1. Developers add approved PRs to queue via "Merge when ready"
2. GitHub creates temporary branch (`gh-readonly-queue/`)
3. PRs added sequentially to temp branch, each tested after previous passes
4. Failed PRs removed from queue, author notified
5. Passing PRs merged atomically to `HEAD` in queue order

## Key Features

- **Automatic retesting**: Every PR tested on top of all preceding PRs
- **Batch processing**: Configurable group size (default max: 5)
- **Editable queue**: Reorder or remove PRs before merge
- **CI runs twice**: Initial PR check + queue check on temp branch

## Configuration Options

- **Build concurrency**: Max PRs testing simultaneously
- **Min/max group size**: Batch size bounds (default: 1–5)
- **Wait time**: Timeout before processing partial batch (default: 5 min)
- **Require all to pass**: Test each PR vs only HEAD (checked = slower but clearer failures)
- **Status check timeout**: Failsafe for hanging checks

## Setup Requirements

1. Add `merge_group` trigger to CI workflows
2. Enable squash merges (required for atomic batch merging)
3. Enable via repository ruleset

## Trade-offs

**Pros**:
- Zero manual "Update Branch" clicking
- Developers freed from CI monitoring
- Automatic conflict handling

**Cons**:
- CI runs twice per PR (initial + queue)
- Adds slight latency (batch waiting period)
- Requires squash merge strategy

## Related Patterns

- **[[Linear Commit History]]**: Branch protection requiring test-on-HEAD
- **[[CI Babysitting]]**: Anti-pattern this solves

## Sources

- [[Improving developer velocity with GitHub merge queue]] (2026-04-08)
