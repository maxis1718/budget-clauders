---
name: setup
description: Use once to configure budget-clauders on a new machine. Installs caveman dependency and writes cost-effective orchestration rules to ~/.claude/CLAUDE.md.
---

# Budget Clauders Setup

One-time install. Permanent behavior change.

## Steps

### 1. Install caveman (if not present)

```bash
ls ~/.claude/plugins/cache/caveman 2>/dev/null || npx skills add juliusbrussee/caveman -g -y
```

### 2. Append rules to ~/.claude/CLAUDE.md

Check if `## Budget Clauders` section already exists. If yes, skip. If no, append:

```markdown
## Budget Clauders — Cost-Effective Orchestration

**Model tier (down-tier first):**
| Task | Model | Effort |
|------|-------|--------|
| grep / list / format / single-value lookup | haiku 4.5 | low |
| Single-file read / extract / validate | haiku 4.5 | low |
| Multi-file / general coding / known debug | sonnet 4.6 | med |
| Code review / first-time codebase explore | sonnet 4.6 | med |
| Root cause unknown + tried 3+ approaches | opus 4.7 | high |
| Architecture / ambiguous tradeoff (undecomposable) | opus 4.7 | high |
| Security / irreversible decisions | opus 4.7 | high |

**Effort rules:**
- Known path / SOP → low
- Multi-file / first debug → med
- Root cause unknown + 3+ attempts → high
- Task ambiguous → decompose first, then pick

**Before opus:** invoke budget-clauders:cost-guard
**Before subagent dispatch:** invoke budget-clauders:delegate
**Context hygiene:** files >8KB → delegate read+summarize to subagent, never in main session
**Tools:** rg/fd/bat over grep/find/cat — invoke budget-clauders:tools for full table
```

### 3. Confirm

Tell user:
- caveman status (installed / already present)
- CLAUDE.md section added (or already present)
- "Budget Clauders active. Behavior changes take effect next session."
