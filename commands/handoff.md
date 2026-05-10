---
description: Create or overwrite HANDOFF.md — a task snapshot for the next agent to resume from. Does not touch CHANGELOG.md.
---

Create or fully overwrite `HANDOFF.md` with a point-in-time snapshot of the current task. Use this when you only need the task handoff and not the changelog.

If `$ARGUMENTS` specifies a path, write there instead of `HANDOFF.md`.

## Step 1: Gather context

Run:
- `git status` — uncommitted changes
- `git diff --stat` — files changed
- `git log --oneline -5` — recent commits

Extract from the conversation:
- Original goal and current status
- What was completed (specific — file names, function names, exact numbers)
- What was tried and failed — and the mechanism of failure
- Decisions made and why
- Errors encountered and how resolved
- The single most important next step

## Step 2: Write HANDOFF.md

Overwrite the file completely. Every field is current as of this moment.

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

## Guidelines

- **Failed approaches are the most valuable section** — if anything was tried and abandoned, document the mechanism of failure
- **Show code, don't describe** — include actual signatures, interfaces, response shapes
- **Test steps need expected outcomes** — "verify it works" is useless
- Include error messages verbatim, not paraphrased
- Omit empty sections except Failed Approaches (write "None" there if truly clean)
- Be concise — every word must earn its place
