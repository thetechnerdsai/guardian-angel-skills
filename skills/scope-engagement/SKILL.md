---
name: scope-engagement
description: >
  Scope a new consulting engagement using past project data. Finds similar past
  projects, compares effort and outcomes, and produces a scoping document with
  timeline, team composition, and risk factors. Use when someone says "scope this
  engagement", "estimate this project", "what would this take", or "how should we
  staff this".
---

# Scope Engagement

Produce a data-driven scoping document for a new consulting engagement, benchmarked
against similar past projects.

## Prerequisites

This skill requires the Guardian Angel MCP server. Verify the following tools are
available before proceeding:

- `GetCompanyProfile`
- `FindSimilarProjects`
- `CompareProjects`

If any tool is missing, tell the user:

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

### Step 1: Gather engagement details

Ask for the following (skip any the user has already provided):

1. **Organization slug** — e.g., `"the-tech-nerds"`
2. **Engagement description** — What the client needs, the problem being solved
3. **Industry** — e.g., Healthcare, Fintech, E-commerce, Education
4. **Estimated duration** — In weeks (rough estimate is fine)
5. **Tech stack** — Comma-separated if known, e.g., ".NET, React, AWS"
6. **Team size** — Expected number of consultants (if known)

### Step 2: Retrieve data

1. Call **`GetCompanyProfile`** with the org slug — get delivery defaults, methodology,
   sprint cadence.
2. Call **`FindSimilarProjects`** with org slug, industry, tech stack, team size, and
   estimated weeks — get ranked list of comparable past projects.
3. If a strong match exists (similarity score > 0.6), call **`CompareProjects`** with
   the top match and second-best match — get side-by-side effort breakdown.

### Step 3: Generate scoping document

Produce a Markdown document with these sections:

#### Engagement Summary
- Client need and problem statement
- Industry and domain context
- Proposed tech stack (aligned with firm standards from `GetCompanyProfile`)

#### Comparable Past Projects
For each similar project (top 3):
- Name, industry, duration, team size
- Total hours logged and effort distribution by category
- Key outcomes (lessons learned, risks encountered, decisions made)
- Similarity score

If `CompareProjects` was called, include:
- Side-by-side comparison table of the two closest matches
- Key differences in effort distribution and outcomes

#### Engagement Shape
Based on firm delivery defaults:
- Recommended methodology and sprint cadence
- Estimation approach (story points, t-shirt sizing, etc.)
- Definition of done
- Documentation standards

#### Team Composition
- Recommended team size (benchmarked against similar projects)
- Roles needed based on tech stack and engagement type
- Ramp-up considerations

#### Timeline Estimate
- Estimated duration range (based on similar projects' actuals vs estimates)
- Milestone breakdown by phase (discovery, build, testing, handoff)
- Estimate accuracy from comparable projects (estimated vs actual weeks)

#### Risk Factors
From similar project outcomes (filtered by `OutcomeCategory` = Risk, Lesson Learned):
- Known risks in this industry/tech stack combination
- Mitigation strategies from past engagements
- Common failure modes

#### Budget Estimate
If similar projects have budget data:
- Budget range based on comparable projects
- Hours breakdown by category (development, testing, meetings, etc.)
- Budget health indicators from past projects (over/under budget)
