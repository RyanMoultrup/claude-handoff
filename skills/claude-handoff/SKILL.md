---
name: claude-handoff
description: Manages session continuity. Triggers at session end or on wrap-up signals to write HANDOFF.md (task snapshot) and/or CHANGELOG.md (project history). Activates on session start if HANDOFF.md exists. Trigger words — wrap up, handoff, hand off, pass this off, switching agents, I need to go, let's stop here, save progress, end of session, update the changelog, log what we did, write a session entry, document this session, pick up where we left off, resume, continue later, transfer context.
---

# Claude Handoff Skill

Manages context continuity across sessions — both point-in-time task handoffs and permanent project history.

## On Session Start

Check if `HANDOFF.md` exists in the working directory. If found:

1. Read it silently
2. Tell the user: "Found a handoff from a previous session: [title]. [1-sentence goal summary]. Want to resume from here?"
3. If yes, follow `/claude-handoff:resume`
4. If no, continue normally — don't delete or mention the file again

## Trigger Words

Activate this skill when the user says any of:

**Wrap-up signals** (suggest `/claude-handoff:wrap-up`):
- "wrap up", "wrap this up", "end of session", "let's stop here", "I need to go", "that's it for today", "let's call it"

**Handoff-specific signals** (suggest `/claude-handoff:handoff` or `/claude-handoff:quick`):
- "handoff", "hand off", "pass this off", "switching agents", "another agent", "continue later", "transfer context", "save state"

**Changelog-specific signals** (suggest `/claude-handoff:changelog-update`):
- "update the changelog", "log what we did", "write a session entry", "save progress to changelog", "document this session"

**Resume signals** (suggest `/claude-handoff:resume`):
- "pick up where we left off", "resume", "continue from the handoff", "what was I doing"

## Choosing the Right Command

| Situation | Command |
|-----------|---------|
| End of session — write both files | `/claude-handoff:wrap-up` |
| Only need task handoff, no changelog | `/claude-handoff:handoff` |
| Simple task, minimal handoff | `/claude-handoff:quick` |
| Only need changelog update | `/claude-handoff:changelog-update` |
| Resuming from previous session | `/claude-handoff:resume` |

## Proactive Suggestions

Suggest `/claude-handoff:wrap-up` when:
- The user signals they're done for the day
- A significant milestone is reached and the session has been long
- The user has completed the main goal they stated at the start

Say: "Want me to write a handoff and update the changelog before you go?"

Do not suggest after every task or repeatedly within the same session.

## What Each File Is For

**HANDOFF.md** — point-in-time snapshot, fully overwritten each session. Tells the next agent exactly where things stand right now: what's done, what's broken, what not to retry, and step-by-step resume instructions. Optimized for resuming a specific task.

**CHANGELOG.md** — permanent project ledger, append-only. Accumulates every session's decisions, failures, discoveries, and results. Optimized for long-running project memory — a future session can read it cold and understand the entire history of what was tried and why.
