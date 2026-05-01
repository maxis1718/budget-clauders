---
name: work
description: Set orchestration intensity for this session. /work team = full orchestrator, delegate aggressively. /work solo = direct execution, no subagents. Default = auto (self-assess per message).
---

# Work Mode

Sets how aggressively to orchestrate this session. Use when you know upfront a session is complex — before the first prompt signals it.

## Modes

### `team`
Full orchestrator. Treat this session as a multi-step project.

- Decompose every task before acting — no direct execution without a plan
- Delegate aggressively: haiku for lookups, sonnet for analysis/coding, main session for planning + decisions only
- bc-delegate rules apply to every subagent dispatch
- No direct file reads >8KB, no grep in main session — delegate it
- Parallel dispatch for independent subtasks (cap 2 default, 3 max for haiku/sonnet)
- Write intermediate findings → `tmp/` or `plans/`

### `solo`
Direct execution. Skip orchestrator overhead.

- Handle everything in main session
- Read files directly, grep directly
- No subagent dispatch unless task genuinely requires parallel work
- Minimal decomposition — bias to immediate action

### Default (auto)
Self-assess per message:
- Trivial (<3 tool calls, single file, single question) → solo behavior
- Project (multi-file, exploratory, multi-step) → team behavior

## On Activation

State:
```
WORK MODE: <team|solo>
[2-3 bullet overrides active this session]
/work auto to reset.
```

## Reset

`/work auto` → back to default self-assessment.
