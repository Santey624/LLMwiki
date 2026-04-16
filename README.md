# LLMwiki — Claude-Maintained Knowledge Base

A personal knowledge base maintained by Claude Code, inspired by [Andrej Karpathy's LLM wiki pattern](https://karpathy.ai).

## The Concept

This project explores an alternative to traditional RAG (Retrieval-Augmented Generation).

**Traditional RAG** retrieves raw document chunks on every query:
```
Raw docs → Chunk → Embed → Vector DB
Query → Embed → Similarity Search → Top-k chunks → LLM synthesizes
```

**LLMwiki** pre-digests knowledge into a structured wiki at ingest time:
```
Raw docs → Claude reads & synthesizes → Structured wiki pages
Query → Read index → Read relevant pages → Answer
```

The key insight: the expensive "understanding" work happens **once at ingest time**, not on every query. The wiki is Claude's pre-computed, synthesized memory of the domain.

### Why This Works Better

| RAG Problem | Wiki Solution |
|---|---|
| Chunk boundaries cut context mid-sentence | Wiki pages are full, coherent summaries |
| Retrieval can miss relevant chunks | Index + wiki-links form a navigable graph |
| Query-time latency (embed + search) | Just read files — no vector math |
| Raw chunks are noisy, redundant | Wiki synthesizes across sources |
| Hard to audit what the model "knows" | Wiki pages are human-readable and editable |
| Stale knowledge is invisible | Outdated pages are visible and fixable |

## Project Purpose

This specific wiki tracks **Santosh's AI engineering job search in Japan** — a focused domain where synthesized, cross-linked knowledge is more useful than raw document retrieval.

## Folder Structure

```
raw/        — Source documents (immutable — never modified)
wiki/       — Markdown pages maintained by Claude
  index.md  — Table of contents for the entire wiki
  log.md    — Append-only record of all operations
CLAUDE.md   — Instructions for Claude on how to maintain this wiki
```

## How It Works

### Ingesting a Source

Drop a file into `raw/` and ask Claude to ingest it. Claude will:

1. Read the full source document
2. Discuss key takeaways before writing anything
3. Create a summary page in `wiki/`
4. Create or update concept pages for each major idea
5. Add `[[wiki-links]]` to connect related pages
6. Update `wiki/index.md`
7. Append an entry to `wiki/log.md`

A single source may touch 10–15 wiki pages. That is normal.

### Asking Questions

Ask Claude anything about the domain. Claude will:

1. Read `wiki/index.md` to find relevant pages
2. Read those pages and synthesize an answer
3. Cite specific wiki pages in the response
4. Offer to save valuable new answers as wiki pages

### Wiki Page Format

Every page follows a consistent structure:

```markdown
# Page Title

**Summary** One to two sentences describing this page.
**Sources** List of raw source files this page draws from.
**Last updated** Date of most recent update.

---

Main content with [[wiki-links]] to related pages.

## Related Pages
- [[related-page-1]]
- [[related-page-2]]
```

## Tradeoffs

This pattern works well when:
- The domain is **focused** (manageable number of sources)
- Queries benefit from **synthesized** knowledge over raw retrieval
- You want **auditable, human-readable** knowledge

It is less suited for:
- Massive corpora (millions of documents) where pre-reading everything isn't feasible
- Domains where verbatim retrieval of original text matters

## Stack

- **Claude Code** — wiki maintenance, ingest, question answering
- **Markdown** — wiki page format (portable, human-readable)
- **FastAPI + Uvicorn** — optional API layer (`requirements.txt`)
