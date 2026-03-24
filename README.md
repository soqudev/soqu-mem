<p align="center">
  <strong>soqu-mem — Persistent memory for AI coding agents</strong><br>
  <em>Agent-agnostic. Single binary. Zero dependencies.</em>
</p>

<p align="center">
  <a href="docs/INSTALLATION.md">Installation</a> &bull;
  <a href="docs/AGENT-SETUP.md">Agent Setup</a> &bull;
  <a href="docs/ARCHITECTURE.md">Architecture</a> &bull;
  <a href="docs/PLUGINS.md">Plugins</a> &bull;
  <a href="CONTRIBUTING.md">Contributing</a> &bull;
  <a href="DOCS.md">Full Docs</a>
</p>

---

> **soqu-mem** — memoria persistente para el ecosistema **SOQU-AI** (Software Quality Guard).

Your AI coding agent forgets everything when the session ends. **soqu-mem** gives it a durable memory.

A **Go binary** with SQLite + FTS5 full-text search, exposed via CLI, HTTP API, MCP server, and an interactive TUI. Works with **any agent** that supports MCP — Claude Code, OpenCode, Gemini CLI, Codex, VS Code (Copilot), Antigravity, Cursor, Windsurf, or anything else.

```
Agent (Claude Code / OpenCode / Gemini CLI / Codex / VS Code / Antigravity / ...)
    ↓ MCP stdio
soqu-mem (single Go binary)
    ↓
SQLite + FTS5 (~/.soqu-mem/soqu-mem.db)
```

## Quick Start

### Install

```bash
go install github.com/soqudev/soqu-mem/cmd/soqu-mem@latest
```

Windows, Linux, Homebrew (cuando el tap esté publicado) y otros métodos → [docs/INSTALLATION.md](docs/INSTALLATION.md)

### Setup Your Agent

| Agent | One-liner |
|-------|-----------|
| Claude Code | `claude plugin marketplace add soqudev/soqu-mem && claude plugin install soqu-mem` |
| OpenCode | `soqu-mem setup opencode` |
| Gemini CLI | `soqu-mem setup gemini-cli` |
| Codex | `soqu-mem setup codex` |
| VS Code | `code --add-mcp '{"name":"soqu-mem","command":"soqu-mem","args":["mcp"]}'` |
| Cursor / Windsurf / Any MCP | See [docs/AGENT-SETUP.md](docs/AGENT-SETUP.md) |

Full per-agent config, Memory Protocol, and compaction survival → [docs/AGENT-SETUP.md](docs/AGENT-SETUP.md)

That's it. No Node.js, no Python, no Docker. **One binary, one SQLite file.**

## How It Works

```
1. Agent completes significant work (bugfix, architecture decision, etc.)
2. Agent calls mem_save → title, type, What/Why/Where/Learned
3. soqu-mem persists to SQLite with FTS5 indexing
4. Next session: agent searches memory, gets relevant context
```

Full details on session lifecycle, topic keys, and memory hygiene → [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md)

## MCP Tools

| Tool | Purpose |
|------|---------|
| `mem_save` | Save observation |
| `mem_update` | Update by ID |
| `mem_delete` | Soft or hard delete |
| `mem_suggest_topic_key` | Stable key for evolving topics |
| `mem_search` | Full-text search |
| `mem_session_summary` | End-of-session save |
| `mem_context` | Recent session context |
| `mem_timeline` | Chronological drill-in |
| `mem_get_observation` | Full content by ID |
| `mem_save_prompt` | Save user prompt |
| `mem_stats` | Memory statistics |
| `mem_session_start` | Register session start |
| `mem_session_end` | Mark session complete |

Full tool reference → [docs/ARCHITECTURE.md#mcp-tools](docs/ARCHITECTURE.md#mcp-tools)

## Terminal UI

```bash
soqu-mem tui
```

**Navigation**: `j/k` vim keys, `Enter` to drill in, `/` to search, `Esc` back. Catppuccin Mocha theme.

## Git Sync

Share memories across machines. Uses compressed chunks — no merge conflicts, no huge files.

```bash
soqu-mem sync                    # Export new memories as compressed chunk
git add .soqu-mem/ && git commit -m "sync soqu-mem memories"
soqu-mem sync --import           # On another machine: import new chunks
soqu-mem sync --status           # Check sync status
```

Full sync documentation → [DOCS.md](DOCS.md)

## CLI Reference

| Command | Description |
|---------|-------------|
| `soqu-mem setup [agent]` | Install agent integration |
| `soqu-mem serve [port]` | Start HTTP API (default: 7437) |
| `soqu-mem mcp` | Start MCP server (stdio) |
| `soqu-mem tui` | Launch terminal UI |
| `soqu-mem search <query>` | Search memories |
| `soqu-mem save <title> <msg>` | Save a memory |
| `soqu-mem timeline <obs_id>` | Chronological context |
| `soqu-mem context [project]` | Recent session context |
| `soqu-mem stats` | Memory statistics |
| `soqu-mem export [file]` | Export to JSON |
| `soqu-mem import <file>` | Import from JSON |
| `soqu-mem sync` | Git sync export |
| `soqu-mem version` | Show version |

## Documentation

| Doc | Description |
|-----|-------------|
| [Installation](docs/INSTALLATION.md) | All install methods + platform support |
| [Agent Setup](docs/AGENT-SETUP.md) | Per-agent configuration + Memory Protocol |
| [Architecture](docs/ARCHITECTURE.md) | How it works + MCP tools + project structure |
| [Plugins](docs/PLUGINS.md) | OpenCode & Claude Code plugin details |
| [Comparison](docs/COMPARISON.md) | Design vs claude-mem |
| [Contributing](CONTRIBUTING.md) | Contribution workflow + standards |
| [Full Docs](DOCS.md) | Complete technical reference |

## License

MIT

---

**Inspired by [claude-mem](https://github.com/thedotmack/claude-mem)** — agent-agnostic, single binary, built for long-lived coding sessions.
