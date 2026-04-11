# Nexa Schema & Agent Instructions

This file defines how you (Claude) should maintain and operate this personal knowledge base.

## What is Nexa?

Nexa is a personal knowledge base system where you incrementally build and maintain a persistent wiki from source documents. Instead of retrieving from raw sources on every query (like RAG), you:

1. **Ingest** sources by reading them, extracting key information, and integrating into the wiki
2. **Maintain** the wiki by updating pages, cross-referencing, noting contradictions, and keeping it current
3. **Query** by searching the wiki and synthesizing answers with citations
4. **Lint** periodically to keep the wiki healthy and identify gaps

The wiki is a **compounding artifact** — it gets richer with every source and every question.

## Directory Structure

```
/
├── raw/              # Source documents (immutable, you read but never modify)
│   └── assets/       # Images and media files
├── wiki/             # LLM-maintained wiki (you own this entirely)
│   ├── index.md      # Content catalog
│   ├── log.md        # Chronological history
│   ├── overview.md   # High-level synthesis
│   └── [pages]       # Entity pages, concept pages, summaries, etc.
├── CLAUDE.md         # This schema (co-evolved with user)
└── README.md         # User-facing documentation
```

## Core Files

### wiki/index.md

A content-oriented catalog of everything in the wiki. Structure:

```markdown
# Nexa Index

Last updated: [date]

## Overview
- [Overview](overview.md) — High-level synthesis across all sources

## Sources
- [Source Title](sources/source-name.md) — One-line summary | Added: YYYY-MM-DD

## Entities
- [Entity Name](entities/entity-name.md) — Brief description | Sources: N

## Concepts
- [Concept Name](concepts/concept-name.md) — Brief description | Sources: N

## Comparisons
- [Comparison Title](comparisons/comparison-name.md) — Brief description

## Analyses
- [Analysis Title](analyses/analysis-name.md) — Brief description | Created: YYYY-MM-DD
```

**Update the index on every ingest and whenever you create/modify significant pages.**

### wiki/log.md

An append-only chronological record. Each entry uses this format:

```markdown
## [YYYY-MM-DD] operation | Title

Brief description of what was done.
- Key changes
- Pages created/updated
```

Operations: `ingest`, `query`, `lint`, `maintenance`

**Append to the log after every significant operation.**

## Workflows

### 1. Ingest Workflow

When the user asks you to ingest a source from `raw/`:

1. **Read** the source document completely
2. **Create/update** pages (do NOT discuss with user first—proceed directly):
   - Create a summary page in `wiki/sources/[source-name].md`
   - Update `wiki/overview.md` with new synthesis
   - Create or update relevant entity pages in `wiki/entities/`
   - Create or update relevant concept pages in `wiki/concepts/`
   - Note contradictions with existing pages
3. **Update** `wiki/index.md` with all new/modified pages
4. **Append** entry to `wiki/log.md`
5. **Commit to git**:
   - Add untracked files from `raw/` (source document)
   - Add all new/modified wiki pages
   - Create commit with message: "Ingest: [Source Title]"

**Source page template:**
```markdown
---
title: [Source Title]
date_added: YYYY-MM-DD
source_type: [article/paper/book/image/data]
source_path: raw/[filename]
---

# [Source Title]

## Summary
[1-2 paragraph summary of key points]

## Key Insights
- Insight 1
- Insight 2
- Insight 3

## Entities Mentioned
- [[Entity Name]] — brief context
- [[Another Entity]] — brief context

## Concepts
- [[Concept]] — how it's discussed
- [[Another Concept]] — relevance

## Contradictions/Updates
- Contradicts [[Other Page]]: [explanation]
- Updates understanding of [[Topic]]: [explanation]

## Quotes
> Notable quote 1

> Notable quote 2

## Related Sources
- [[Related Source 1]]
- [[Related Source 2]]
```

### 2. Query Workflow

When the user asks a question:

1. **Search** wiki/index.md for relevant pages
2. **Read** those pages (use the index to find them efficiently)
3. **Synthesize** answer with citations using [[Page Name]] format
4. **Discuss** if this answer should be filed as a new wiki page
5. **If yes**, create the page and update index/log

Query answers can become:
- Comparison pages (`wiki/comparisons/`)
- Analysis pages (`wiki/analyses/`)
- New concept pages
- Enhanced entity pages

### 3. Lint Workflow

When asked to health-check the wiki:

1. **Scan** index.md and sample key pages
2. **Check for**:
   - Contradictions between pages
   - Stale claims superseded by newer sources
   - Orphan pages with no inbound links
   - Concepts mentioned but lacking dedicated pages
   - Missing cross-references
   - Data gaps that could be filled
3. **Report** findings to user
4. **Fix** issues if requested
5. **Log** the lint pass

### 4. Maintenance

Ongoing responsibilities:
- Use wiki-style `[[Page Name]]` links throughout
- Keep cross-references up to date
- Flag contradictions explicitly
- Update overview.md when synthesis shifts
- Keep index.md organized and current
- Use consistent file naming (lowercase-with-hyphens.md)

## Page Organization

```
wiki/
├── index.md
├── log.md
├── overview.md
├── sources/
│   └── [source-name].md
├── entities/
│   └── [entity-name].md
├── concepts/
│   └── [concept-name].md
├── comparisons/
│   └── [comparison-name].md
└── analyses/
    └── [analysis-name].md
```

Create subdirectories as needed for the specific domain.

## Writing Style

- **Concise**: Value density over length
- **Precise**: Cite sources, note uncertainty
- **Connected**: Heavy use of [[Wiki Links]]
- **Skeptical**: Note contradictions and alternative views
- **Current**: Update rather than append when information changes

## Output Formats

Beyond markdown pages, you can generate:
- **Tables** for comparisons
- **Marp slides** for presentations
- **Charts** (matplotlib/mermaid) for visualizations
- **Dataview queries** for dynamic lists (if using Obsidian)

## Tips

- Use frontmatter (YAML) for metadata when useful
- The index.md is your primary navigation tool — keep it clean
- A single source may touch 10-15 pages — that's normal
- Good questions deserve to be filed as pages, not lost in chat
- Update overview.md frequently — it's the thesis of the knowledge base
- When contradictions arise, flag them prominently
- Suggest new sources and questions when you see gaps

## Evolution

This schema will evolve with the knowledge base. The user may:
- Add domain-specific page types
- Request different organizational schemes
- Change workflows based on their style
- Add tools (search, scripts) over time

**Adapt to the user's needs while maintaining the core pattern: read sources, maintain wiki, compound knowledge.**

---

*This schema is a living document. Update it when patterns change or new conventions emerge.*
