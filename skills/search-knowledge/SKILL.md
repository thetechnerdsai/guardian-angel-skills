---
name: search-knowledge
description: >
  Search the consulting firm's knowledge base using natural language. Queries
  tech standards, delivery defaults, and AI preferences simultaneously using
  semantic or keyword search. Use when someone asks "what's our standard for...",
  "how do we handle...", "search knowledge", "what does the firm say about...",
  or any question about firm practices and standards.
---

# Search Knowledge

Answer questions about firm practices by searching across all consulting knowledge.

## Prerequisites

This skill requires the Guardian Angel MCP server. Verify the following tool is
available before proceeding:

- `SearchKnowledge`

If the tool is missing, tell the user:

> To use this skill, add the Guardian Angel MCP server to your Claude Code config:
>
> ```json
> {
>   "mcpServers": {
>     "guardian-angel": {
>       "type": "streamable-http",
>       "url": "http://localhost:3001/mcp"
>     }
>   }
> }
> ```
>
> Then start the server: `docker compose up --build -d` in the Guardian Angel repo.

## Workflow

### Step 1: Gather context

Ask for the following (skip any the user has already provided):

1. **Organization slug** — e.g., `"the-tech-nerds"`
2. **Question** — The user's natural language question. If the user already asked one
   (e.g., "what's our standard backend framework?"), use that directly.

### Step 2: Search

Call **`SearchKnowledge`** with:
- `orgSlug`: the organization slug
- `question`: the user's question
- `maxResults`: 5 (default, increase to 10 if question is broad)

### Step 3: Present results

Format the response based on search results:

#### Direct Answer
Synthesize a clear, direct answer from the matching knowledge entries. Lead with the
answer, not the sources.

#### Sources
For each matching entry:

| Category | Key | Value | Relevance |
|----------|-----|-------|-----------|
| {category} | {key} | {value} | {relevance score or "keyword match"} |

Include the **search type** (semantic or keyword) as a footnote.

#### Follow-up
If the results suggest related topics, mention them:
> "Related: You might also want to check [topic]. Ask me to search for it."

### Edge Cases

- **No results:** Tell the user no matching knowledge was found. Suggest rephrasing
  or checking if the knowledge has been entered into the system.
- **Broad question:** If the question is very broad (e.g., "tell me everything"),
  suggest narrowing it down. Offer category suggestions: tech standards, delivery
  defaults, AI preferences.
- **Multiple topics:** If the question spans multiple knowledge areas, make separate
  `SearchKnowledge` calls for each topic and combine the results.
