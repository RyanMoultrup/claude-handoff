---
description: Create a minimal HANDOFF.md — just the essentials. Use for simple tasks or quick context transfers.
---

Write a minimal `HANDOFF.md` with only what the next agent absolutely needs. Use this for simple, short tasks where a full handoff would be overkill.

If `$ARGUMENTS` specifies a path, write there instead of `HANDOFF.md`.

Output exactly this format — no additional sections:

```markdown
# Handoff: [task in 5 words or less]

**Goal**: [one sentence]

**Done**: [comma-separated list of completed items, or "Nothing yet"]

**Next**: [the single most important next step]

**Failed**: [anything tried that didn't work and why, or "None"]

**Watch out**: [one key warning, or "Nothing special"]
```

Nothing else. No extra sections. If the task is complex enough to need more than this, use `/claude-handoff:handoff` instead.
