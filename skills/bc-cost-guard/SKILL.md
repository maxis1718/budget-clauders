---
name: bc-cost-guard
description: Use when considering opus model, task complexity is unclear, or a decision carries high token cost. Do not proceed until checklist is complete.
---

# Cost Guard

Opus costs 5× sonnet, 25× haiku. Run this before every potential opus call.

## Checklist (answer all three)

**1. Why can't sonnet handle this?**
- If answer is "it probably can" → use sonnet
- If answer is "unclear" → decompose first (see below)
- If answer is "genuinely needs judgment/architecture at scale" → opus ok

**2. Can this be decomposed into smaller problems?**
- Decomposable → split into haiku/sonnet subtasks, solve in parallel
- Not decomposable (truly cross-cutting judgment) → opus ok

**3. Is there a test or SOP protecting the output?**
- Yes → downgrade one level (opus→sonnet, sonnet→haiku). Tests catch mistakes.
- No → stay at selected tier but consider adding test first

## Decomposition Pattern

Instead of:
```
→ opus: "analyze this 5-file architecture and recommend changes"
```

Do:
```
→ haiku: read file A, extract interfaces          (parallel)
→ haiku: read file B, extract interfaces          (parallel)
→ sonnet: cross-reference + recommend changes
```

Same output quality. Fraction of cost.

## Decision

| Checklist result | Action |
|-----------------|--------|
| All 3 answered → sonnet sufficient | Use sonnet |
| Decomposable | Decompose → haiku/sonnet |
| Test exists | Downgrade one tier |
| Genuinely undecomposable + no test + sonnet insufficient | Use opus |

## Output

State your decision: model chosen + one-line justification. Then proceed.
