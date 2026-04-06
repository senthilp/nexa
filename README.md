# Nexa

A personal knowledge base system where an LLM incrementally builds and maintains a persistent wiki from your sources.

## What is this?

Instead of traditional RAG (retrieve chunks on every query), Nexa maintains a **persistent, compounding wiki** that gets richer over time. The LLM:

- **Ingests** your sources by reading them and integrating key information into the wiki
- **Maintains** cross-references, updates pages, and flags contradictions
- **Synthesizes** answers from the wiki with citations
- **Compounds** knowledge — every source and query makes the wiki more valuable

You curate sources and ask questions. The LLM does all the bookkeeping.

## Quick Start

### 1. Add a source

Place a document in the `raw/` directory:

```bash
# Example: clip a web article to markdown using Obsidian Web Clipper
# or manually add files to raw/
cp ~/Downloads/interesting-article.md raw/
```

### 2. Ingest it

Tell Claude Code:

```
Ingest the article in raw/interesting-article.md
```

Claude will:
- Read the source
- Create a summary page in `wiki/sources/`
- Extract and create entity/concept pages
- Update the overview and index
- Log the ingest

### 3. Ask questions

```
What are the main themes across all sources?
How does [Source A] compare to [Source B]?
What does the wiki say about [Topic]?
```

Answers are synthesized from the wiki with citations. Good answers can be filed as new pages.

### 4. Browse the wiki

Open `wiki/` in Obsidian or any markdown editor. The graph view in Obsidian shows the knowledge network.

Key files:
- `wiki/index.md` — content catalog
- `wiki/log.md` — chronological history  
- `wiki/overview.md` — high-level synthesis

## Directory Structure

```
nexa/
├── raw/              # Your source documents (immutable)
│   └── assets/       # Images and media
├── wiki/             # LLM-maintained wiki
│   ├── index.md      # Content catalog
│   ├── log.md        # Operation history
│   ├── overview.md   # Synthesis
│   ├── sources/      # One page per source
│   ├── entities/     # People, orgs, places, etc.
│   ├── concepts/     # Ideas, theories, themes
│   ├── comparisons/  # Filed comparison queries
│   └── analyses/     # Filed analysis queries
├── CLAUDE.md         # Schema for LLM behavior
└── README.md         # This file
```

## Operations

### Ingest

```
Ingest raw/article.md
```

The LLM reads the source, discusses key points with you, creates/updates wiki pages, and logs the operation.

### Query

```
What does the wiki say about X?
Compare A and B
Analyze the relationship between C and D
```

The LLM searches the index, reads relevant pages, synthesizes an answer with citations, and optionally files it as a new page.

### Lint

```
Health-check the wiki
```

The LLM scans for contradictions, orphaned pages, missing cross-references, and gaps to investigate.

### Maintenance

The wiki stays current because the LLM updates it continuously. You never write the wiki yourself.

## Tips

- **One source at a time**: Stay involved in the ingest process to guide what gets emphasized
- **File good queries**: Turn valuable explorations into wiki pages so they compound
- **Use Obsidian**: Graph view, web clipper, image downloads, and Dataview queries make it powerful
- **Version control**: Initialize git to track wiki evolution
- **Search tools**: For larger wikis, add [qmd](https://github.com/tobi/qmd) or similar
- **Customize**: Edit CLAUDE.md to adapt workflows to your domain

## Use Cases

- **Personal knowledge**: Goals, health, psychology, self-improvement over time
- **Research**: Deep-dive on a topic, building a comprehensive wiki with evolving thesis
- **Reading companion**: Build a fan-wiki-style guide as you read books
- **Business/team**: Internal wiki fed by Slack, meetings, docs (with human review)
- **Any domain**: Competitive analysis, trip planning, course notes, hobbies

## Philosophy

The tedious part of maintaining a knowledge base is bookkeeping — updating cross-references, noting contradictions, keeping pages current. LLMs excel at this mechanical maintenance.

Your job: curate sources, ask good questions, think about meaning.  
LLM's job: everything else.

The wiki becomes a **compounding artifact** — richer with every source, every query, every conversation.

## Getting Started

1. ✅ **Structure created** — you're ready to go
2. Add your first source to `raw/`
3. Tell Claude Code to ingest it
4. Open `wiki/` in Obsidian to browse
5. Start asking questions

## Next Steps

- Initialize git: `git init && git add . && git commit -m "Initialize Nexa"`
- Install [Obsidian](https://obsidian.md) for graph visualization
- Add the [Obsidian Web Clipper](https://obsidian.md/clipper) extension
- Customize `CLAUDE.md` as your workflow evolves

---

**The wiki is a living artifact. It grows with you.**
