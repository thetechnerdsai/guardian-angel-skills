---
name: project-health-check
description: >
  Check how a project is tracking. Retrieves hours breakdown, estimate accuracy,
  budget health, and key outcomes, then benchmarks against similar past projects.
  Use when someone asks "how is this project doing", "project health", "check
  project status", "are we on track", or "project report".
---

# Project Health Check

Produce a health report for a consulting engagement with benchmarking against
comparable past projects.

## Prerequisites

This skill requires the Guardian Angel MCP server. Verify the following tools are
available before proceeding:

- `GetProjectInsights`
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

1. **Organization slug** — e.g., `"the-tech-nerds"`
2. **Project name** — The name of the project to check

### Step 2: Retrieve data

Call these MCP tools in parallel:

1. **`GetProjectInsights`** with org slug and project name — returns hours breakdown
   by category, estimate accuracy, budget health, and top outcomes.
2. **`FindSimilarProjects`** with org slug and the project's industry/tech stack/team
   size (extracted from insights) — returns comparable past projects for benchmarking.

### Step 3: Generate health report

Produce a Markdown report with these sections:

#### Project Summary
- Name, status, industry
- Team size, estimated duration, actual duration (if completed)
- Start date, end date (or projected)

#### Hours Breakdown
Present hours by category in a table:

| Category | Hours | % of Total | Benchmark Avg |
|----------|-------|------------|---------------|
| Development | X | Y% | Z% |
| Testing | X | Y% | Z% |
| ... | ... | ... | ... |

Flag categories that are significantly above or below the benchmark average
from similar projects.

#### Estimate Accuracy
- Estimated weeks vs actual weeks (or projected)
- Accuracy percentage
- How this compares to similar past projects' accuracy
- Flag if the project is trending over estimate

#### Budget Health
If budget data is available:
- Budget amount vs spent
- Burn rate (spend per week)
- Projected final cost at current burn rate
- Budget health indicator: On Track / At Risk / Over Budget

#### Key Outcomes
Top outcomes from the project (lessons learned, risks, decisions, metrics):

| Outcome | Category | Impact | Tags |
|---------|----------|--------|------|
| {description} | {category} | {impact level} | {tags} |

#### Benchmark Comparison
Compare against the top 3 similar past projects:

| Metric | This Project | Similar Avg | Status |
|--------|-------------|-------------|--------|
| Total hours | X | Y | Above/Below |
| Dev % | X% | Y% | Normal/High/Low |
| Estimate accuracy | X% | Y% | Better/Worse |

#### Recommendations
Based on the data, provide actionable recommendations:
- If hours are skewing high in a category, flag it
- If estimate accuracy is poor, suggest re-estimation
- If budget is at risk, suggest course corrections
- If similar past projects had specific risks, warn about them

### Edge Cases

- **No time entries:** Report that no hours have been logged yet. Suggest starting
  to track time to enable health reporting.
- **No similar projects:** Skip benchmarking section. Note that comparisons will
  improve as more projects are completed.
- **Project not found:** Tell the user the project name wasn't found. Suggest using
  `ListProjects` to see available projects.
