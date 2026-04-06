# Nexa Commands Reference

Quick reference for common operations with your Nexa knowledge base.

## Ingest Commands

### Single source
```
Ingest raw/article-name.md
```

### Be specific about what to focus on
```
Ingest raw/paper.pdf and focus on the methodology section
```

### Batch ingest
```
Ingest all files in raw/ that haven't been processed yet
```

## Query Commands

### Simple queries
```
What does the wiki say about [topic]?
Summarize what we know about [entity]
```

### Comparisons
```
Compare [Source A] and [Source B] on [topic]
What are the different perspectives on [concept]?
```

### Analysis
```
Analyze the relationship between [X] and [Y]
What patterns emerge across sources about [theme]?
What's missing from our understanding of [topic]?
```

### File a query as a page
```
[After a good answer] File this as a comparison page
[After analysis] Save this analysis to the wiki
```

## Maintenance Commands

### Health check
```
Health-check the wiki
Lint the wiki for contradictions and gaps
```

### Specific maintenance
```
Update all cross-references to [Page Name]
Check if [Concept] needs its own page
Find orphaned pages
```

### Overview updates
```
Update the overview to reflect recent sources
What are the major themes emerging?
```

## Navigation Commands

### Browse
```
Show me the current index
What are the most-connected pages?
What was the most recent ingest?
```

### Search
```
Find pages mentioning [term]
Which sources discuss [topic]?
```

## Specialized Outputs

### Generate visualizations
```
Create a comparison table of [A, B, C]
Make a timeline of [events/developments]
Generate a Marp slide deck summarizing [topic]
```

### Data queries (if using Dataview in Obsidian)
```
List all sources added in the last month
Show entities with more than 5 source references
```

## Tips

- **Be conversational**: Claude understands natural language, no need for rigid commands
- **Stay involved**: Especially during ingests, guide what gets emphasized
- **Ask follow-ups**: "Update that page to include..." or "Add a section on..."
- **File good answers**: If a query yields valuable synthesis, ask to save it as a page
- **Evolve the schema**: "Add this pattern to CLAUDE.md for future sessions"

## Example Session

```
> Add sources to raw/

> Ingest raw/article-1.md

[Claude reads, creates pages, updates wiki]

> What are the key themes so far?

[Claude synthesizes from wiki]

> File that as an analysis page

[Claude creates wiki/analyses/key-themes.md]

> Ingest raw/article-2.md

[Claude integrates, notes connections to first article]

> Compare the two articles

[Claude creates comparison from wiki]

> Health-check the wiki

[Claude scans for issues, suggests improvements]
```

---

**Remember**: This is your knowledge base. Command it however feels natural. These are just examples.
