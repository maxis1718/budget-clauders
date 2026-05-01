---
name: bc-delegate
description: Use when about to dispatch a subagent, Agent tool call, or background task involving file reads, research, or multi-step exploration.
---

# Delegate

Main session context = scarce. Delegate everything that isn't planning, deciding, or writing critical code.

## What to delegate

- File reads >8KB
- Search / grep / directory exploration
- Research (web, docs, codebase)
- Test analysis
- Any independent subtask

## Model selection

| Task | Model | Effort |
|------|-------|--------|
| grep / list / single-value lookup / format | haiku 4.5 | low |
| Single-file read / extract / validate | haiku 4.5 | low |
| Multi-file / debug (known direction) | sonnet 4.6 | med |
| Code review / first explore | sonnet 4.6 | med |
| Root cause unknown (3+ attempts failed) | opus 4.7 | high |

Run budget-clauders:cost-guard before picking opus.

## Prompt template

```
CAVEMAN MODE: ultra. Drop all filler. Fragments OK.

Goal: [one sentence]
Context: [what you already know]
Scope: [exact files/dirs/URLs to look at]
Return format: [see table below]
Research only / Write code: [pick one]
```

## Return format (pick by complexity)

| Task | Format |
|------|--------|
| Lookup / grep result | 1-line answer, no explanation |
| Analysis / debug finding | `{finding, cause, action}` — structured fields, no prose |
| Large research / multi-file audit | Write to `tmp/research-YYYYMMDD-topic.md`, return path + 3 bullets |
| Code output | Write to target file, return path + 1-line summary |

## Dispatch announcement (required, every time)

Before dispatching, print in the conversation's language:

**Chinese example:**
```
派出 2 個 agents：
  [1] haiku 4.5 / low   — 在 src/routes/ grep API 路由定義
  [2] sonnet 4.6 / med  — 讀取並摘要 capital-allocation state schema
```

**English example:**
```
Dispatching 2 agents:
  [1] haiku 4.5 / low   — grep API route definitions in src/routes/
  [2] sonnet 4.6 / med  — read + summarize capital-allocation state schema
```

**Rules:**
- Match language to the conversation (中文對話 → 中文；English conversation → English)
- Include full model version: `haiku 4.5`, `sonnet 4.6`, `opus 4.7`
- opus 4.7 requires an explicit reason on the same line:

```
  [1] opus 4.7 / high   — 根因不明，已試 3 種方向無解，需跨檔架構判斷
```

- Print this before every dispatch, including single-agent dispatches

## Parallel dispatch

Independent tasks → same message, multiple Agent calls. Cap: 2 default, 3 max (haiku/sonnet only).

## After subagent returns

Extract conclusion immediately. Do not leave raw output in main context.
