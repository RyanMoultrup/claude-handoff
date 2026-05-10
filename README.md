# claude-handoff

A Claude Code plugin for session handoff — writes structured `CHANGELOG.md` entries so a fresh Claude session can pick up exactly where you left off, without re-attempting dead ends or rediscovering decisions already made.

## Skills

### `/claude-handoff:changelog-update [path]`

Writes a new session entry to `CHANGELOG.md`. Captures:

- What was completed (specific file names, function names, exact numbers)
- What failed and **why** (the mechanism, not just "it didn't work")
- Decisions made and rationale (especially "why NOT X")
- Test/accuracy results with exact numbers
- Current project status and next concrete step

The optional argument overrides the default `./CHANGELOG.md` path.

Claude also invokes this skill automatically when you say things like:
- "update the changelog"
- "log what we did"
- "write a session entry"
- "save progress"

## Install

### From this repo (local)

```bash
claude --plugin-dir /path/to/claude-handoff
```

### From GitHub (once pushed)

In Claude Code:
```
/plugin marketplace add yourusername/claude-plugins
/plugin install claude-handoff@claude-plugins
```

## Usage

At the end of a session:
```
/claude-handoff:changelog-update
```

Or with a custom path:
```
/claude-handoff:changelog-update docs/CHANGELOG.md
```

## Why

Claude has no memory between sessions. A well-written changelog entry is the difference between a fresh session that hits the ground running and one that wastes 20 minutes rediscovering what you already know.

The format follows Anthropic's long-running agent research: reverse-chronological entries, permanent "failed approaches" and "confirmed correct" sections, and a status header that reflects the current state of the project — not aspirations.
