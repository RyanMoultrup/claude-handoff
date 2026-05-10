---
description: End-of-session wrap-up — writes both HANDOFF.md and CHANGELOG.md in one command. Use this at the end of every session.
---

Run both the handoff and changelog writes sequentially, sharing a single context-gathering pass.

## Step 1: Gather shared context (once, used by both writes)

Run:
- `git status` — uncommitted changes
- `git diff --stat` — files changed
- `git log --oneline -5` — recent commits

Extract from the conversation:
- Original goal and current status
- Everything completed this session (file names, function names, exact numbers)
- Everything attempted that failed — and the mechanism of failure, not just "it didn't work"
- Decisions made and rationale, especially "why NOT X"
- Any errors encountered and how they were resolved
- What the single most important next step is

## Step 2: Write HANDOFF.md

Overwrite (or create) `HANDOFF.md` completely. This file is a point-in-time snapshot for the next agent — it gets fully replaced every session.

```markdown
# Handoff: [Brief Task Title]

**Generated**: [date and time]
**Branch**: [current git branch]
**Status**: [In Progress / Blocked / Ready for Review]

## Goal

[1-2 sentences: what the user is trying to achieve]

## Completed

- [x] [Specific completed item with file/location]
- [x] [Another completed item]

## Not Yet Done

- [ ] [Remaining task — be specific, include file/function if known]
- [ ] [Another remaining task]

## Failed Approaches (Don't Repeat These)

[Always include this section. Write "None" only if truly nothing was tried and abandoned.]

- **[Approach name]**: [What was tried] → [Why it failed — the mechanism, not "it didn't work"] → [What to use instead]

## Key Decisions

| Decision | Rationale |
|----------|-----------|
| [Choice made] | [Why this over the alternative] |

## Current State

**Working**: [What's functional right now]
**Broken**: [What's not working — include error messages verbatim if relevant]
**Uncommitted changes**: [Summary of staged/unstaged changes, or "None"]

## Code Context

[Show, don't describe. Include what the next agent needs to call or modify things correctly.]

Key signatures/interfaces:
\`\`\`typescript
// path/to/file.ts
function exampleFn(param: Type): ReturnType
\`\`\`

Non-obvious logic:
[Anything tricky that isn't self-documenting]

## Resume Instructions

[Step-by-step, with expected outcomes. Not "test it" but "run X, expect Y, if Z check W".]

1. [First action — exact command or file to edit]
   - Expected: [what should happen]
   - If it fails: [what to check]
2. [Next step]

## Setup Required

[Only if prerequisites exist — env vars, test accounts, required services]

## Warnings

[Gotchas, things that look wrong but are intentional, traps to avoid]
```

Guidelines for HANDOFF.md:
- Failed approaches are mandatory — this is the highest-value section
- Show code signatures and API shapes, don't describe them
- Every test step needs an expected outcome
- Include error messages verbatim, not paraphrased
- Omit empty sections except Failed Approaches (write "None" there if needed)
- Be brutally concise — every sentence must earn its place

## Step 3: Write CHANGELOG.md

Append a new entry to `CHANGELOG.md`. This file is a permanent, cumulative project ledger — never overwrite existing entries.

If `CHANGELOG.md` doesn't exist, create it with the skeleton defined in the `changelog-update` skill before writing the first entry.

Follow the full entry format and content rules from `/claude-handoff:changelog-update`.

The two writes share the context gathered in Step 1 — do not re-run git commands or re-scan the conversation.

## Step 4: Confirm

Tell the user:
- HANDOFF.md written (or updated) — one sentence on the current status captured
- CHANGELOG.md updated — the date header of the new entry
