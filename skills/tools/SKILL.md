---
name: tools
description: Use when about to run grep, find, cat, sed, or awk. Faster modern alternatives exist that produce less noise and fewer tokens.
---

# Better Tools

Drop-in replacements. Less output noise = fewer tokens consumed by tool results.

## Core replacements

| Slow | Fast | Why |
|------|------|-----|
| `grep` | `rg` (ripgrep) | Faster, respects .gitignore, color output |
| `find` | `fd` | Simpler syntax, respects .gitignore |
| `cat` | `bat` | Syntax highlighting, line numbers |
| `cat large.json` | `jless` / `gron` | Navigate/grep JSON without loading all |
| `curl` | `xh` | Cleaner API debugging output |

## Semantic tools (Claude Code specific)

| Tool | Use for |
|------|---------|
| `sg` (ast-grep) | TS/JS semantic search + refactor — finds patterns, not text |
| `difft` | Structural diff — understands code structure, not just lines |
| `hyperfine` | Benchmarking commands |
| `watchexec` | File watcher for re-running on change |

## Token-saving patterns

**Large JSON state:**
```bash
# Instead of cat state.json (dumps everything)
gron state.json | rg "keyYouWant"
```

**Surgical file read (avoid loading full file):**
```bash
# Instead of reading whole file
sed -n '42,55p' file.ts
```

**File size check before reading:**
```bash
wc -c file.ts   # bytes — if >8000, delegate to subagent
```

## Install

```bash
brew install ripgrep fd bat jless ast-grep difft hyperfine watchexec xh
npm install -g gron
```
