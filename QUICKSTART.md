# Nexa Quick Start

Get up and running with your personal knowledge base in 5 minutes.

## ✅ Setup Complete

Your Nexa instance is ready to go! Here's what was created:

```
nexa/
├── raw/                 # Add your source documents here
├── wiki/                # LLM-maintained wiki (auto-generated)
├── CLAUDE.md            # Instructions for Claude on how to maintain the wiki
├── README.md            # Full documentation
├── COMMANDS.md          # Command reference
└── .obsidian/           # Obsidian configuration
```

## Step 1: Try the Example (Optional)

Let's do a quick test with the sample file:

```
Ingest raw/EXAMPLE-getting-started.md
```

Watch how Claude:
- Reads the source
- Creates a summary page
- Updates the wiki index
- Logs the operation

Then browse `wiki/` to see what was created.

## Step 2: Add Your First Real Source

### Option A: Web Article (Easiest)

1. Install [Obsidian Web Clipper](https://obsidian.md/clipper) browser extension
2. Configure it to save to this vault's `raw/` directory
3. Visit an article and click the clipper
4. Tell Claude: `Ingest raw/[article-name].md`

### Option B: PDF or Local File

1. Copy a file to `raw/`:
   ```bash
   cp ~/Documents/paper.pdf raw/
   ```
2. Tell Claude: `Ingest raw/paper.pdf`

### Option C: Manual Markdown

1. Create a file in `raw/interesting-topic.md`
2. Write or paste content
3. Tell Claude: `Ingest raw/interesting-topic.md`

## Step 3: Ask Questions

Once you have 2-3 sources, start exploring:

```
What are the main themes across sources?
Compare [Source A] and [Source B]
What does the wiki say about [topic]?
```

Claude synthesizes answers from the wiki with citations.

## Step 4: Browse in Obsidian

1. Install [Obsidian](https://obsidian.md)
2. Open this folder as a vault
3. Check out the graph view (see the knowledge network)
4. Browse `wiki/index.md` to navigate

## Ongoing Workflow

```
Add source to raw/ → Ingest → Ask questions → File good answers → Repeat
```

Every source makes the wiki richer. Every question builds new connections.

## Pro Tips

- **Ingest one source at a time** initially — stay involved, guide the process
- **File valuable insights** as wiki pages so they don't disappear
- **Run health-checks** occasionally: `Health-check the wiki`
- **Version control**: `git init && git add . && git commit -m "Initial commit"`
- **Customize CLAUDE.md** as you discover what works for your domain

## Common Questions

**Q: Can I delete the example file?**  
A: Yes! Delete `raw/EXAMPLE-getting-started.md` when you're ready.

**Q: Do I write the wiki myself?**  
A: No! Claude maintains it entirely. You just add sources and ask questions.

**Q: What file formats work?**  
A: Markdown, PDF, images, plain text. Claude can read them all.

**Q: Can I edit wiki pages manually?**  
A: You can, but Claude will maintain them. Better to ask Claude to make changes.

**Q: How big can this get?**  
A: Hundreds of sources and thousands of pages work well. For larger scale, consider adding search tools.

**Q: Can I use this for [specific domain]?**  
A: Yes! Customize CLAUDE.md to define domain-specific page types and workflows.

## What Makes This Different

Traditional RAG: Upload files → Query → Retrieve chunks → Generate answer  
**Every query starts from scratch.**

Nexa: Upload files → Ingest into wiki → Query wiki → Synthesize answer  
**Knowledge compounds over time.**

## Next Steps

1. Add your first real source
2. Ingest it with Claude
3. Browse the wiki
4. Ask a question
5. Add another source and watch connections emerge

---

**You're ready! Start building your knowledge base.**

Need help? Check:
- `README.md` — Full documentation
- `COMMANDS.md` — Command reference  
- `CLAUDE.md` — See how Claude operates (read-only)
