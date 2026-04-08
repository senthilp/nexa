---
title: Improving developer velocity with GitHub merge queue
date_added: 2026-04-08
source_type: article
source_path: raw/Improving developer velocity with GitHub merge queue.md
author: Nicholas C. Zakas
organization: Amazon
published: 2026-04-08
---

# Improving developer velocity with GitHub merge queue

## Summary

GitHub merge queue eliminates the manual "update branch and wait for CI" cycle that frustrates developers in repositories with linear commit history requirements. Instead of repeatedly clicking "Update Branch" as other PRs land, developers add their PR to a queue. GitHub automatically tests each PR on top of preceding ones in temporary branches, removes failing PRs, and merges passing batches atomically. This automation transforms babysitting pull requests into a set-it-and-forget-it workflow.

## Key Insights

- **Manual churn is expensive**: In high-velocity repos with 10-minute CI, developers waste significant time repeatedly updating branches and waiting, only to find another PR merged while they were waiting
- **Queue as coordination mechanism**: Merge queue acts as intermediate step between approval and merge, handling the coordination that developers previously did manually
- **Automatic retesting**: Every PR is tested on top of all preceding PRs in the queue, ensuring integration without manual intervention
- **Configurable batch processing**: Default max batch size of 5 balances temporary branch overhead against failure likelihood
- **CI runs twice**: Once on initial PR, again during queue processing on temporary branch—this is the trade-off for automation
- **Squash merges required**: Each PR should land as single commit to make reverts straightforward and align with queue's batch merge behavior

## How It Works

### Process Flow

1. Developer clicks "Merge when ready" to add approved PR to queue
2. GitHub waits for max batch size (default: 5) or timeout (default: 5 min)
3. Creates temporary branch (`gh-readonly-queue/`) based on current `HEAD`
4. Adds PRs sequentially to temp branch, testing each one after the previous passes
5. If PR fails CI: removed from queue, author notified, original PR stays open
6. When all PRs in batch pass: atomic merge to `HEAD` in queue order
7. Process repeats with next batch

### Key Advantages

- **Zero manual updates**: "Update Branch" button becomes obsolete
- **Ordering control**: Queue is editable—reorder or remove PRs before merge
- **Automatic conflict resolution**: PRs tested on latest state without developer intervention
- **Predictable landing**: Commits land in same order as queued

## Setup Steps

### 1. Configure CI for merge queue

Add `merge_group` trigger to GitHub workflow:

```yaml
on:
  pull_request:
    branches: [main]
  merge_group:
    branches: [main]
    types: [checks_requested]
```

Skip jobs that need PR context:

```yaml
jobs:
  lint:
    if: github.event_name != 'merge_group'
    # or: if: ${{ !startsWith(github.ref, 'refs/heads/gh-readonly-queue/') }}
```

**Critical**: Skipped jobs count as successful checks—required status checks must work in both PR and merge queue contexts.

### 2. Enable squash merges

Repository settings → Pull Requests → Check "Allow squash merges" + uncheck other merge types.

**Why**: Queue merges entire temp branch at once—squashing ensures each PR adds single commit for easy reverts.

### 3. Enable merge queue via ruleset

Repository settings → Rules → Rulesets → Check "Require merge queue"

## Configuration Options

- **Build concurrency**: Max PRs running checks simultaneously
- **Minimum group size**: Minimum PRs before creating temp branch (default: 1—prevents stalling)
- **Maximum group size**: Max PRs per temp branch (default: 5—balances overhead vs failure risk)
- **Wait time**: Minutes to wait for min group size (default: 5—prevents indefinite waiting)
- **Require all to pass** (default: checked): Test each PR individually vs only HEAD of temp branch
  - Checked: slower but identifies failing PR immediately
  - Unchecked: faster but failures harder to diagnose
- **Status check timeout**: Minutes before assuming checks failed (prevents infinite waiting)

## Example Scenario

Queue with 7 PRs, default settings:
1. PRs 1-3 added to temp branch sequentially
2. PR 3 fails CI → removed from queue, PRs 1-2 merge to main
3. New temp branch starts with PR 4
4. PR 3 fixed and re-queued (goes to end of queue)
5. PRs 4-7 + fixed PR 3 all pass → all merge to main

## Entities Mentioned

- [[Nicholas C. Zakas]] — author, Amazon engineer
- [[Amazon]] — organization
- [[GitHub]] — platform

## Concepts

- [[GitHub Merge Queue]] — automated PR testing and merging system
- [[Linear Commit History]] — branch protection requiring PRs test on HEAD
- [[Temporary Merge Branches]] — `gh-readonly-queue/` branches for testing batches
- [[Squash Merges]] — combining all commits into single commit
- [[Ruleset]] — GitHub's branch protection rule mechanism
- [[CI Babysitting]] — anti-pattern of manually monitoring and updating PRs

## Contradictions/Updates

- Complements [[Vercel Sandbox snapshots]] in developer productivity domain but different layer: workflow automation vs infrastructure optimization

## Quotes

> "The GitHub merge queue is one of those features that sounds like a small quality-of-life improvement until you start using it and realize how much time you were spending babysitting pull requests."

> "By automating the 'update and wait' cycle, it frees developers to focus on writing code instead of monitoring CI dashboards."

> "The linear commit history requirement means that each pull request must be tested on top of `HEAD` in CI before it can be merged."

## Related Sources

- [[Vercel Sandbox snapshots]] — infrastructure performance optimization for developer tools
