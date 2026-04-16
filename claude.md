# LLM wiki

A personal knowledge base maintained by claude code. Based on Andrej Karpathy's LLM wiki pattern

Purpose
This wiki is a structured, interlinked knoweldge base for finding out Jobs in Japan. Claude maintains the wiki. The human curates sources, ask questions, and guides the analysis.

Folder structure
raw/                    -- source documents (immutable --never modify these)
wiki/                   -- markdown pages maintained by claude
wiki/index.md           -- Table of contents for the entire wiki
wiki/log.md             -- Apend-only record of all operations


Ingest workflow

When the user adds a new source to raw/ and asks you to ingest it:

1. Read the full source documents
2. Discuss key takeaways with the user before writing anything
3. Create a summary page in wiki/ named after the source
4. Create or update concept pages for each major idea or entity
5. Add wiki-links (page-name) to connect related pages
6. Update wiki/index.md with new pages and one-line descriptions
7. Append an entry to wiki/log.md with the date, source name, and what changed


A single source may touch 10-15 wiki pages. That is normal

Page format
Every wiki page should follow this structure:

# Page Title

** summary ** One to two sentences describing this page
** Sources ** List of raw source files this page draws from.
** Last updated ** Date of most recent update


--
Main content goes here. Use clear headings and short paragraphs.
Link to related concepts using [[wiki-links]] throughout the text.

## Related Pages

- [[related-concept-1]]
- [[related concept-2]]

Citation rules

- Every factual claim should reference its source file
- Use the format (source: filename.pdf) after the claim
- If two sources disagree, note the contradiction explicitly
- If a claim has no source, mark it as needing verification

Question answering

When the user asks a question:

1. Read wiki/index.md first to find relevant pages
2. Read those pages and synthesize an answer
3. Cite specific wiki pages in your response
4. If the answer is not in the wiki, say so clearly
5. If the answer is valuable offer to save it as a new wiki page

