---
description: Resume work from an existing HANDOFF.md. Checks for repo drift and orients the new session before continuing.
---

Resume work from a handoff created in a previous session.

If `$ARGUMENTS` specifies a path, read from there. Otherwise look for `HANDOFF.md` in the current directory. If not found, ask the user for the path.

## Step 1: Read the handoff

Read the entire `HANDOFF.md` carefully before doing anything else.

## Step 2: Check for drift

Run `git status` and `git log --oneline -3` and compare against the handoff:
- Is the branch the same as documented?
- Have there been commits since the handoff was written?
- Are there uncommitted changes not mentioned in the handoff?

If state has drifted, warn the user before proceeding:

> "The repo has changed since this handoff was created: [describe what changed]. Should I proceed with the handoff context anyway, or would you like to describe what happened since?"

Wait for their response before continuing.

## Step 3: Summarize to the user

Give a brief orientation — not the whole document, just enough to confirm you're aligned:

```
Resuming: [title]

Goal: [1 sentence]
Status: [In Progress / Blocked / etc.]
Completed: [X items done]
Next: [first item from Resume Instructions]
```

Then ask: "Ready to continue?"

## Step 4: Respect the handoff's warnings

Before starting work, internalize:
- **Failed Approaches** — do not attempt these again unless the user explicitly asks
- **Warnings** — treat these as hard constraints until told otherwise
- **Key Decisions** — follow established patterns unless the user asks to revisit

## Step 5: Continue the work

Start with the first item in Resume Instructions unless the user redirects.

If anything in the handoff is ambiguous about something critical, ask rather than guess.
