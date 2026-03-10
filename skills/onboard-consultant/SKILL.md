---
name: onboard-consultant
description: >
  Onboard a consultant to a new engagement. Retrieves the firm's complete
  knowledge profile, finds similar past projects, and generates a structured
  briefing. Use when someone says "onboard me", "brief me on this project",
  "I'm joining an engagement", or "what do I need to know for this project".
---

# Onboard Consultant

Generate a comprehensive engagement briefing for a consultant starting a new project.

## Prerequisites

This skill requires the Guardian Angel MCP server. Verify the following tools are
available before proceeding:

- `GetCompanyProfile`
- `ListProjects`
- `FindSimilarProjects`

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

### Step 1: Gather context

Ask for the following (skip any the user has already provided):

1. **Organization slug** — e.g., `"the-tech-nerds"`. This identifies the consulting firm.
2. **Project context** — Client name, industry (e.g., Healthcare, Fintech), and a brief
   description of what the engagement involves.

### Step 2: Retrieve firm knowledge

Call these MCP tools in parallel:

1. **`GetCompanyProfile`** with the org slug — returns the firm's tech standards, delivery
   defaults, and AI preferences.
2. **`ListProjects`** with the org slug — returns all past and current engagements.
3. **`FindSimilarProjects`** with the org slug, industry, and any known tech stack or team
   size — returns similar past projects ranked by similarity score.

### Step 3: Generate briefing

Produce a Markdown document with these sections:

#### Firm Standards Overview
- **Tech Stack:** Summarize cloud provider, backend/frontend frameworks, CI/CD, IaC
- **Delivery Methodology:** Sprint cadence, estimation approach, definition of done
- **AI Guardrails:** Model preferences, prompt standards, approved tools

#### Similar Past Engagements
For each similar project (top 3-5):
- Project name, industry, status
- Team size and duration
- Key outcomes and lessons learned
- Similarity score and what made it comparable

#### Recommended Approach
Based on firm standards and similar project outcomes:
- Suggested tech stack for this engagement
- Delivery methodology adjustments (if industry/client warrants them)
- Known risks from comparable past projects
- AI tooling recommendations

#### Kickoff Checklist
- [ ] Review firm tech standards for the relevant stack
- [ ] Check client-specific overrides (if client exists in system)
- [ ] Set up project repo using firm scaffolding standards
- [ ] Review lessons learned from similar past engagements
- [ ] Confirm sprint cadence and estimation approach with project lead
- [ ] Set up CI/CD pipeline per firm standards
- [ ] Document any deviations from firm defaults
