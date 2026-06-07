---
name: rtk-token-proxy
description: "Install, configure, and troubleshoot RTK (Rust Token Killer) — a CLI proxy that compresses terminal output for LLM agents, reducing token consumption by 60-90%."
version: 1.0.0
author: Hermes Agent
license: MIT
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [token-optimization, cli-proxy, claude-code, cursor, windsurf, Hermes]
    related_skills: [claude-code, hermes-agent]
---

# RTK (Rust Token Killer)

RTK is a high-performance CLI proxy written in Rust that reduces LLM token consumption by **60-90%** by filtering and compressing command outputs before they enter the LLM context. Single binary, zero dependencies, less than 10ms overhead.

## When to Use

- User mentions token savings or context window limits
- AI coding agents (Claude Code, Codex, Cursor, etc.) are consuming too many tokens on terminal output
- User wants to optimize coding agent efficiency

## Installation

### Pre-built Binary (Recommended)

```bash
# Download from GitHub Releases — always prefer this over the install script
cd /tmp && curl -fsSL -o rtk.tar.gz https://github.com/rtk-ai/rtk/releases/download/v0.42.3/rtk-x86_64-unknown-linux-musl.tar.gz && tar xzf rtk.tar.gz && install -m 755 rtk /usr/local/bin/rtk
```

### Homebrew
```bash
brew install rtk-ai/tap/rtk
```

### Cargo
```bash
cargo install --git https://github.com/rtk-ai/rtk.git
```

### Pre-built Binaries Available
- `rtk-x86_64-unknown-linux-musl.tar.gz`
- `rtk-x86_64-unknown-linux-gnu.tar.gz`
- `rtk-aarch64-unknown-linux-gnu.tar.gz`
- `rtk-x86_64-apple-darwin.tar.gz`
- `rtk-aarch64-apple-darwin.tar.gz`
- `rtk-x86_64-pc-windows-msvc.zip`

## Quick Verification

```bash
rtk --version    # should print: rtk x.y.z
rtk gain          # should show token savings dashboard
```

If `rtk gain` says "command not found" but `rtk --version` works, you have the wrong package (Rust Type Kit instead of Rust Token Killer). Fix: `cargo uninstall rtk` then reinstall from git.

## Integration with AI Agents

Run `rtk init --agent <name>` for your agent:

| Agent | Command |
|-------|---------|
| Claude Code | `rtk init -g` |
| Cursor | `rtk init -g --agent cursor` |
| Windsurf | `rtk init -g --agent windsurf` |
| Hermes | `rtk init --agent hermes` |
| Codex | `rtk init -g --codex` |
| Gemini CLI | `rtk init -g --gemini` |
| Copilot (VS Code) | `rtk init -g --copilot` |

**Restart required**: After init, restart your AI agent for the plugin to load.

## Manual Usage

Use `rtk` as a prefix for any command:

```bash
rtk git status       # → compact summary instead of full status
rtk git diff         # → structured diff summary
rtk ls -la           # → permissions + filename + size only
rtk grep -r "TODO"   # → filtered results
rtk cargo test       # → test summary
```

## Pitfalls & Gotchas

1. **Install script may return 404** — `https://rtk-ai.app/install.sh` has been reported returning 404. Always have the direct binary URL as fallback.
2. **Cargo naming collision** — another crate named "rtk" (Rust Type Kit) exists on crates.io. Use `--git https://github.com/rtk-ai/rtk.git` to avoid the wrong package.
3. **Claude Code built-in tools bypass hooks** — `Read`, `Grep`, `Glob` tools don't pass through the bash rewrite hook. Use shell equivalents (`cat`, `rg`, `find`) or call `rtk read`, `rtk grep`, `rtk find` directly.
4. **Windows needs WSL** — auto-rewrite hook requires a Unix shell. Native Windows gets CLAUDE.md injection mode only (commands not auto-rewritten).
5. **Config location** — `~/.config/rtk/config.toml` (Linux) or `~/Library/Application Support/rtk/config.toml` (macOS).
6. **Error log on failure** — when a command fails, RTK saves full unfiltered output to `~/.local/share/rtk/tee/<timestamp>.log` for LLM access.

## Compression Examples

**`ls -la` before/after:**
- Before: `total 92\ndrwxr-xr-x  3 root root  4096 Jun  7 09:43 .\ndrwxr-xr-x 14 root root  4096 Jun  7 09:42 ..\n-rw-r--r--  1 root root    9 Jun  7 09:43 file_1.txt` (each line ~70 chars with permissions, owner, full timestamp)
- After: `644  file_1.txt  9B` (each line ~15 chars, only mode + name + size)

**`git status` before/after:**
- Before: 8 lines with full git formatting and suggestions
- After: 2 lines: `* master\n M file_1.txt`

## Token Savings (Real-world measurements)

| Operation | Standard | RTK | Savings |
|-----------|----------|-----|---------|
| `ls` / `tree` | 2,000 | 400 | -80% |
| `cat` / `read` | 40,000 | 12,000 | -70% |
| `grep` / `rg` | 16,000 | 3,200 | -80% |
| `git status` | 3,000 | 600 | -80% |
| `git diff` | 10,000 | 2,500 | -75% |
| `cargo test` / `npm test` | 25,000 | 2,500 | -90% |
| `git add/commit/push` | 1,600 | 120 | -92% |