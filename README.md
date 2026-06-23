# OpenCode Configuration

> A reusable OpenCode / AI CLI configuration — skills, MCP servers, and overrides.

[![GitHub](https://img.shields.io/badge/github-elmghari--mohammed/opencode--config-181717?logo=github)](https://github.com/elmghari-mohammed/opencode-config.git)

This repo is designed to work with **OpenCode** and any **AI CLI tool** that supports [MCP servers](https://modelcontextprotocol.io) and custom skill definitions.

---

## ✨ Features

- **11 custom skills** — architecture, backend, code-quality, debugging, designer, frontend, performance, refactoring, review, superpower, testing
- **7 pre-configured MCP servers** — Playwright, GitHub, Context7, Brave Search, Firecrawl, Vercel, Composio
- **Superpowers plugin** — advanced workflows, brainstorming, TDD, plan execution, and more
- **Portable** — easy to fork, clone, and adapt to your own workflow

---

## 🚀 Quick Start

### 1. Clone

```bash
git clone https://github.com/elmghari-mohammed/opencode-config.git ~/.opencode
cd ~/.opencode
```

### 2. Install dependencies

```bash
npm install
```

### 3. Configure API keys

Copy the environment template and fill in your keys:

```bash
cp .env.example .env
```

Edit `.env` with your actual API keys. The config uses these services:

| Variable | Service | Required for |
|---|---|---|
| `CONTEXT7_API_KEY` | [Context7](https://context7.com) | Documentation lookup |
| `BRAVE_API_KEY` | [Brave Search](https://brave.com/search/api/) | Web search |
| `FIRECRAWL_API_KEY` | [Firecrawl](https://firecrawl.dev) | Web scraping & crawling |

> Some MCP servers (Playwright, GitHub, Vercel, Composio) don't require API keys — GitHub needs a [GitHub PAT](https://github.com/settings/tokens) in your shell environment as `GITHUB_TOKEN`.

### 4. Configure GitHub token (optional)

For the GitHub MCP server, set this environment variable:

```bash
# PowerShell
$env:GITHUB_TOKEN = "ghp_..."
```

Or add it to your shell profile.

### 5. Link OpenCode

OpenCode will auto-discover the config at `~/.opencode/opencode.json`. If you're using another AI CLI, symlink or copy the config as needed.

---

## 📁 Structure

```
~/.opencode/
├── opencode.json       # Main configuration (skills paths, MCP servers)
├── skills/             # Custom skill definitions (markdown)
│   ├── architecture/
│   ├── backend/
│   ├── code-quality/
│   ├── debugging/
│   ├── designer/
│   ├── frontend/
│   ├── performance/
│   ├── refactoring/
│   ├── review/
│   ├── superpower/
│   └── testing/
├── mcp/                # Standalone MCP server JSON files
│   ├── github.json
│   └── playwright.json
├── overrides/          # Behavior overrides (add your own)
├── .env.example        # Template for secrets
├── .gitignore
├── package.json
└── README.md
```

---

## 🧠 Skills

Each skill is a markdown file under `skills/<name>/SKILL.md` with front-matter metadata. When you invoke a skill in OpenCode, it loads these instructions to guide the AI's behavior.

| Skill | Use Case |
|---|---|
| `architecture` | System design, module boundaries, layering |
| `backend` | APIs, data flow, server logic, service integration |
| `code-quality` | Linting, review, maintainability |
| `debugging` | Bug diagnosis, failure tracing, root cause analysis |
| `designer` | UI/UX design, layout decisions, visual polish |
| `frontend` | React UI, components, state, forms, browser behavior |
| `performance` | Profiling, optimization, reducing wasted work |
| `refactoring` | Safe refactors, simplifying code, reducing duplication |
| `review` | Code review, risk spotting, regression detection |
| `superpower` | Fast context gathering, task execution discipline |
| `testing` | Test strategy, writing tests, improving coverage |

> **Tip:** Edit any skill's `SKILL.md` to match your team's conventions, preferences, and standards.

---

## 🔌 MCP Servers

The config currently bundles **7 MCP servers**:

| Server | Type | Purpose |
|---|---|---|
| [Playwright](https://github.com/microsoft/playwright-mcp) | Local | Browser automation & end-to-end testing |
| [GitHub](https://github.com/modelcontextprotocol/servers) | Local | Issues, PRs, repos, code search |
| [Context7](https://context7.com) | Local | Framework & library documentation lookup |
| [Brave Search](https://brave.com/search/api/) | Local | Web search (requires API key) |
| [Firecrawl](https://firecrawl.dev) | Local | Web scraping & crawling (requires API key) |
| [Vercel](https://vercel.com) | Remote | Deployments, projects, teams |
| [Composio](https://composio.dev) | Remote | 500+ app integrations (Gmail, Slack, Notion, etc.) |

### Adding a new MCP server

Edit `opencode.json` and add a new entry under `mcp`:

```json
"my-server": {
  "type": "local",
  "command": ["npx", "-y", "@scope/my-server"],
  "environment": {
    "MY_KEY": "${MY_KEY}"
  }
}
```

### Adding from the `mcp/` directory

Create a JSON file in `mcp/`:

```json
{
  "name": "my-server",
  "description": "What it does",
  "command": "npx",
  "args": ["-y", "@scope/my-server"]
}
```

Then reference it in `opencode.json`.

---

## 🛠️ Customization

### Add your own skills

```bash
mkdir skills/my-skill
```

Create `skills/my-skill/SKILL.md`:

```markdown
---
name: my-skill
description: What this skill does
---

# My Skill

Instructions for the AI when this skill is invoked.
```

### Add overrides

Place override files in `overrides/`. These can modify OpenCode's default behavior (e.g., custom agent instructions, tool permissions).

### Use with other AI CLIs

The `.opencode/` directory structure works with any AI CLI that supports:
- **MCP servers** (standard protocol)
- **Custom instructions** via markdown files

Most tools can symlink or reference this directory. Check your CLI's documentation for how to load external configuration.

---

## 🔐 Security

- **Never commit `.env`** — it's in `.gitignore`
- API keys are injected as environment variables, never hardcoded
- Remote MCP servers (Vercel, Composio) handle auth via OAuth — no tokens in config

---

## 📦 Updating

If you forked this repo, pull upstream changes:

```bash
cd ~/.opencode
git pull origin main
npm install
```

---

## 📄 License

MIT — use freely, adapt as needed.
