# Guardian Angel Skills

Claude Code skills for AI-powered consulting oversight, powered by the
[Guardian Angel](https://github.com/thetechnerdsai/tech-consulting-guardian-angel)
MCP server.

## Skills

| Skill | Description |
|-------|-------------|
| **onboard-consultant** | Brief a consultant joining an engagement |
| **scope-engagement** | Scope new work using past project data |
| **scaffold-project** | Set up a repo per firm engineering standards |
| **search-knowledge** | Natural language Q&A against firm knowledge |
| **project-health-check** | Check how a project is tracking |

## Setup

### 1. Install the skills

```bash
npx skills add thetechnerdsai/guardian-angel-skills -a claude-code
```

### 2. Add the Guardian Angel MCP server

Add to your Claude Code MCP config:

```json
{
  "mcpServers": {
    "guardian-angel": {
      "type": "streamable-http",
      "url": "http://localhost:3001/mcp"
    }
  }
}
```

### 3. Start the MCP server

```bash
# In the Guardian Angel repo
docker compose up --build -d
```

## Usage

Once installed, the skills activate automatically when you ask Claude things like:

- "Onboard me to this healthcare project"
- "Scope a new fintech engagement for 12 weeks"
- "Scaffold a new .NET project"
- "What's our standard for CI/CD?"
- "How is the data pipeline project doing?"

## Development

For local development, clone this repo and add it as a local plugin:

```bash
git clone https://github.com/thetechnerdsai/guardian-angel-skills.git
# In Claude Code:
/plugin add /path/to/guardian-angel-skills
```

## License

MIT
