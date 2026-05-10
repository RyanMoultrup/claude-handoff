# claude-handoff

Session continuity for Claude Code. Writes `HANDOFF.md` (task snapshot for the next agent) and `CHANGELOG.md` (permanent project history). Use together at end of session, or independently as needed.

## Install

```bash
# Local
claude --plugin-dir /path/to/claude-plugins/claude-handoff

# From private GitHub marketplace
/plugin marketplace add ryanscomputer/claude-plugins
/plugin install claude-handoff@claude-plugins
```

## Commands

| Command | What it does |
|---------|-------------|
| `/claude-handoff:wrap-up` | **Use this most of the time.** Writes both HANDOFF.md and CHANGELOG.md in one pass. |
| `/claude-handoff:handoff` | Writes only HANDOFF.md — full task snapshot for the next agent. |
| `/claude-handoff:quick` | Writes a minimal HANDOFF.md — 5 fields, for simple tasks. |
| `/claude-handoff:resume` | Reads HANDOFF.md, checks for repo drift, orients the new session. |
| `/claude-handoff:changelog-update` | Writes only a new CHANGELOG.md entry — no handoff file touched. |

## The two files

### HANDOFF.md — task snapshot
Fully overwritten every session. Tells the next agent exactly where things stand right now:
- What's done and what isn't
- What was tried and failed (and *why* — the mechanism)
- Key decisions and rationale
- Current broken state with verbatim error messages
- Step-by-step resume instructions with expected outcomes

This file is optimized for resuming a specific task. It has no memory of previous sessions — only the current one.

### CHANGELOG.md — project ledger
Append-only, never overwritten. Accumulates across every session:
- Reverse-chronological entries with specific, descriptive headers
- Permanent "Failed approaches" section (the most important section — prevents re-attempting dead ends)
- Permanent "Confirmed correct" section (closes investigations)
- Status header that reflects where the project stands right now

This file is optimized for long-running project memory. A fresh session can read it cold and understand the full history of what was tried and why.

## Auto-detection

The plugin's skill activates automatically:

- **Session start**: If `HANDOFF.md` exists, Claude reads it and asks if you want to resume
- **Wrap-up signals**: "let's stop here", "I need to go", "wrap this up" → Claude suggests `/wrap-up`
- **Handoff signals**: "handoff", "switching agents", "continue later" → suggests appropriate command
- **Changelog signals**: "update the changelog", "log what we did" → suggests `/changelog-update`
- **Resume signals**: "pick up where we left off", "resume" → suggests `/resume`

## Typical session flow

```
# Start of session — Claude detects HANDOFF.md automatically
/claude-handoff:resume

# ... do work ...

# End of session
/claude-handoff:wrap-up
```
