# Opencode Configuration

Personal Opencode config — skills, MCP servers, and overrides.

## Clone

```bash
git clone https://github.com/<your-username>/opencode-config.git
```

Or pull updates into your local `~/.opencode`:

```bash
cd ~/.opencode
git pull
```

## Structure

```
.opencode/
├── opencode.json       # Single source of truth
├── skills/             # Skill definitions (markdown)
├── mcp/                # MCP server configs
├── overrides/          # Behaviour overrides
├── .gitignore
└── README.md
```

## Adding a skill

Create a new directory under `skills/` with a `SKILL.md`:

```markdown
---
name: my-skill
description: ...
---
```

## Adding an MCP server

Create a JSON file under `mcp/`:

```json
{
  "name": "server-name",
  "command": "npx",
  "args": ["-y", "@scope/server"]
}
```

## Platforms

Works on Windows and Linux.
