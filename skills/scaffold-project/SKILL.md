---
name: scaffold-project
description: >
  Set up a new project repository using the consulting firm's engineering
  standards. Queries tech standards for CI/CD, IaC, coding standards, and
  cloud provider config, then generates the project structure. Use when
  someone says "scaffold a project", "set up a new repo", "create project
  structure", or "initialize a new project".
---

# Scaffold Project

Set up a new project repository using the firm's engineering standards and templates.

## Prerequisites

This skill requires the Guardian Angel MCP server. Verify the following tools are
available before proceeding:

- `QueryTechStandards`
- `QueryAiPreferences`

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

### Step 1: Gather project details

Ask for the following (skip any the user has already provided):

1. **Organization slug** — e.g., `"the-tech-nerds"`
2. **Project name** — kebab-case, e.g., `"healthcare-data-pipeline"`
3. **Primary stack** — e.g., `.NET`, `Python`, `Next.js`, `Go`
4. **Target directory** — Where to create the project (default: current directory)

### Step 2: Retrieve firm standards

Call these MCP tools in parallel:

1. **`QueryTechStandards`** with the org slug — returns cloud provider, frameworks,
   CI/CD, IaC, coding standards.
2. **`QueryAiPreferences`** with the org slug — returns model preferences, agent
   frameworks, prompt standards.

### Step 3: Generate project structure

Based on the firm's standards and the chosen stack, create the project with these
components. Adapt to the primary stack (e.g., .NET uses `src/`, Python uses package
directories, Next.js uses `app/`).

#### Directory Structure
Create the standard layout for the chosen stack:
- Source code directory
- Test directory
- Documentation directory (`docs/`)
- Infrastructure directory (if firm uses IaC)

#### CI/CD Pipeline
Generate CI/CD config based on firm standards:
- Use the CI/CD tool from `QueryTechStandards` (category: `ci_cd`)
- Include: build, test, lint/format check stages
- Match the firm's standard pipeline structure

#### CLAUDE.md
Generate an AI assistant config file with:
- Project description and architecture overview
- Common commands (build, test, run)
- Key conventions from firm standards
- Reference to firm's AI preferences (model, guardrails)

#### README.md
Generate a README with:
- Project name and description
- Tech stack (from firm standards)
- Quick start instructions
- Development setup
- Testing instructions

#### .gitignore
Generate appropriate .gitignore for the chosen stack.

#### Coding Standards
If the firm has coding standards defined (category: `coding_standards`):
- Apply linter/formatter config (e.g., `.editorconfig`, `.prettierrc`, `stylecop.json`)
- Add pre-commit hooks if firm standards specify them

### Step 4: Initialize and commit

```bash
cd <target-directory>/<project-name>
git init
git add .
git commit -m "chore: scaffold project with firm standards"
```

### Adaptation by Stack

| Stack | Source Dir | Test Framework | Build Tool | Package File |
|-------|-----------|---------------|------------|--------------|
| .NET | `src/` | xUnit | `dotnet` | `*.csproj` |
| Python | `<project>/` | pytest | pip/poetry | `pyproject.toml` |
| Next.js | `app/` or `src/` | Jest/Vitest | npm/pnpm | `package.json` |
| Go | `cmd/`, `internal/` | go test | go | `go.mod` |

Use the firm's standards to fill in specifics. If the firm doesn't specify a
preference for a given aspect, use sensible defaults for the stack.
