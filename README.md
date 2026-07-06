# OpenCode Configuration

> A reusable OpenCode / AI CLI configuration — skills, MCP servers, and overrides.

[![GitHub](https://img.shields.io/badge/github-elmghari--mohammed/opencode--config-181717?logo=github)](https://github.com/elmghari-mohammed/opencode-config.git)

This repo is designed to work with **OpenCode** and any **AI CLI tool** that supports [MCP servers](https://modelcontextprotocol.io) and custom skill definitions.

---

## ✨ Features

- **13 custom skills** — architecture, backend, code-quality, concept-guide, debugging, designer, frontend, performance, refactoring, review, smart-ocr, superpower, testing
- **8 pre-configured MCP servers** — Playwright, GitHub, Context7, Brave Search, Firecrawl, Vercel, Composio, NotebookLM
- **Superpowers plugin** — advanced workflows, brainstorming, TDD, plan execution, and more
- **MCP timeout raised to 30s** — prevents timeouts on slow MCP responses
- **Portable** — clone on any machine, copy `.env`, and go

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
| `VERCEL_ACCESS_TOKEN` | [Vercel](https://vercel.com) | Deployments & project management |

> Some MCP servers (Playwright, NotebookLM, Composio) don't require API keys. GitHub uses a [GitHub PAT](https://github.com/settings/tokens) set as `GITHUB_TOKEN` in your shell.

### 4. Configure GitHub token (optional)

Set this environment variable in your shell profile:

```bash
export GITHUB_TOKEN="ghp_..."
```

### 5. Link OpenCode

OpenCode auto-discovers config at `~/.opencode/opencode.json`. If you use another AI CLI, symlink or copy the config as needed.

---

## 📁 Structure

```
~/.opencode/
├── opencode.json         # Main config: skills paths, 8 MCP servers
├── skills/
│   ├── architecture/     # System design & layering
│   ├── backend/          # APIs & data flow
│   ├── code-quality/     # Linting & maintainability
│   ├── concept-guide/    # Technical concept explanations
│   ├── debugging/        # Bug diagnosis
│   ├── designer/         # UI/UX design
│   ├── frontend/         # React & components
│   ├── performance/      # Profiling & optimization
│   ├── refactoring/      # Safe refactors
│   ├── review/           # Code review
│   ├── smart-ocr/        # OCR & text extraction
│   ├── superpower/       # Task execution discipline
│   └── testing/          # Test strategy
├── .env                  # API keys (gitignored)
├── .env.example          # Template for .env
├── .gitignore
├── package.json
└── README.md
```

---

## 🧠 Skills

Each skill is a markdown file under `skills/<name>/SKILL.md` with front-matter metadata. When you invoke a skill in OpenCode, it loads these instructions to guide the AI's behavior.

| Skill | Use Case |
|---|---|---|
| `architecture` | System design, module boundaries, layering |
| `backend` | APIs, data flow, server logic, service integration |
| `code-quality` | Linting, review, maintainability |
| `concept-guide` | Explain technical concepts with step-by-step examples |
| `debugging` | Bug diagnosis, failure tracing, root cause analysis |
| `designer` | UI/UX design, layout decisions, visual polish |
| `frontend` | React UI, components, state, forms, browser behavior |
| `performance` | Profiling, optimization, reducing wasted work |
| `refactoring` | Safe refactors, simplifying code, reducing duplication |
| `review` | Code review, risk spotting, regression detection |
| `smart-ocr` | OCR & text extraction from images/scanned PDFs |
| `superpower` | Fast context gathering, task execution discipline |
| `testing` | Test strategy, writing tests, improving coverage |

> **Tip:** Edit any skill's `SKILL.md` to match your team's conventions, preferences, and standards.

---

## 🔌 MCP Servers

The config currently bundles **8 MCP servers**:

| Server | Type | Purpose |
|---|---|---|
| [Playwright](https://github.com/microsoft/playwright-mcp) | Local | Browser automation & end-to-end testing |
| [GitHub](https://github.com/modelcontextprotocol/servers) | Local | Issues, PRs, repos, code search |
| [Context7](https://context7.com) | Local | Framework & library documentation lookup |
| [Brave Search](https://brave.com/search/api/) | Local | Web search (requires API key) |
| [Firecrawl](https://firecrawl.dev) | Local | Web scraping & crawling (requires API key) |
| [NotebookLM](https://notebooklm.google.com) | Local | AI research notebook, podcast generation |
| [Vercel](https://vercel.com) | Remote | Deployments, projects, teams |
| [Composio](https://composio.dev) | Remote | 500+ app integrations (Gmail, Slack, Notion, etc.) |

### Adding a new MCP server

Edit `opencode.json` and add a new entry under `mcp`:

```json
"my-server": {
  "type": "local",
  "command": ["npx", "-y", "@scope/my-server"],
  "environment": {
    "MY_KEY": "{env:MY_KEY}"
  }
}
```

All MCP servers are configured centrally in `opencode.json`. No additional files needed.

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
