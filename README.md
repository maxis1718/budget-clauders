# budget-clauders ✈️

**Same destination. Less spend.**

Like budget airlines — you still get there, you just don't burn the premium fuel.

Install this once. Claude Code starts using haiku where it used to reach for opus. Your quota goes further. Quality stays.

---

## The problem

| Model | Output cost | When Claude actually needs it |
|-------|-------------|-------------------------------|
| Haiku 4.5 | $5/MTok | Search, grep, single-file reads |
| Sonnet 4.6 | $15/MTok | Multi-file, coding, debugging |
| Opus 4.7 | $25/MTok | Root-cause unknown, architecture, security |

Most sessions overuse sonnet for haiku tasks, and reach for opus when sonnet is fine. Budget Clauders fixes the default.

## Before / After

Real usage stats (7-day comparison):

| Metric | Before | After (target) |
|--------|--------|----------------|
| Opus share | ~24% | <10% |
| Haiku share | ~4% | >20% |
| Sonnet output tokens | bloated | compressed (caveman) |

*Track your own: Claude Code → `/status` → Stats → Models*

---

## Install

```bash
npx skills add maxis1718/budget-clauders -g -y
```

Then in Claude Code, run once:

```
/bc-setup
```

That's it. Takes 30 seconds. Permanent.

---

## What changes

**Automatic (written to `~/.claude/CLAUDE.md`):**
- Model tier table — haiku → sonnet → opus, with explicit triggers
- Context hygiene — files >8KB delegate to subagent
- Effort rules — reasoning depth matched to task complexity
- Claude knows to check cost-guard before escalating to opus

**On-demand skills (auto-invoked when conditions match):**
- `cost-guard` — 3-question checklist before any opus call
- `delegate` — structured subagent dispatch with model selection + return format
- `tools` — CLI replacements (rg/fd/bat/sg/difft) for less noisy output
- `work` — session orchestration intensity dial (`/work team` or `/work solo`)

**Dependency (auto-installed):**
- [`juliusbrussee/caveman`](https://github.com/juliusbrussee/caveman) — compresses Claude's output ~75%, fewer output tokens

---

## Work mode

Some sessions are obviously complex from the start — but the first prompt won't signal that to Claude. Use `/work team` to declare upfront:

```
/work team
Migrate the capital allocation system from JSON state to SQLite
```

Claude enters full orchestrator mode: decompose → delegate → integrate. Main session handles planning only.

`/work solo` reverses it — skip orchestration, direct execution.

Default (no command): auto-assess per message.

---

## How it works

Skills are lazy-loaded. Claude reads the description of each skill and auto-invokes when conditions match — no manual `/invoke` needed. `setup` and `work` are the only ones you ever run manually.

The `~/.claude/CLAUDE.md` addition is minimal by design. Heavy content lives in skills, loaded on-demand. No bloating your baseline context.

---

## Benchmark your results

1. Screenshot `/status → Stats → Models` before installing
2. Install + run setup
3. Work normally for 7 days
4. Screenshot again
5. Compare opus% and haiku%

Share your before/after — PRs with real data welcome.

---

## 中文說明

**一樣的 quota，幹更多事。**

Budget Clauders 讓 Claude Code 自動用對模型：能 haiku 解決的不給 sonnet，能 sonnet 解決的不升 opus。

**安裝：**
```bash
npx skills add maxis1718/budget-clauders -g -y
```

**Claude Code 內跑一次：**
```
/bc-setup
```

**之後自動生效：**
- 模型選型表寫入 `~/.claude/CLAUDE.md`
- 升 opus 前自動跑 checklist
- Subagent 派發自動用正確格式
- 大檔案自動 delegate，不污染主 session context

**手動切換工作模式：**
- `/work team` — 這 session 很重，強制 full orchestrator
- `/work solo` — 快問快答，跳過 orchestration overhead

---

## Contributing

Built something that saves tokens? PR welcome.
Data from your own before/after? Even better.

MIT License
