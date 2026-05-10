---
name: changelog-update
description: Update a project's CHANGELOG.md with a new session entry capturing what was done, decisions made, failed approaches, accuracy/test results, and next steps. Use this skill whenever the user asks to "update the changelog", "log what we did", "write a session entry", "save progress to CHANGELOG.md", or "document this session". Also trigger proactively when wrapping up a long coding session and a CHANGELOG.md exists in the project.
argument-hint: "Path to CHANGELOG.md (defaults to ./CHANGELOG.md)"
---

# Changelog Update Skill

Write a new session entry to the project's CHANGELOG.md, following the structure established by Anthropic's long-running agent research. The goal is to produce a record that a fresh Claude session can read cold and pick up the work accurately — without re-attempting dead ends or rediscovering decisions that were already made.

## Step 1: Locate the file

If the user passed an argument, use that path. Otherwise look for `CHANGELOG.md` in the project root. If no file exists yet, create one with the structure defined below.

## Step 2: Read the existing file

Read the whole file before writing anything. You need to:
- Understand the current status header so you can update it
- Know what's already been logged so you don't duplicate it
- See the existing "Failed approaches" and "Confirmed correct" sections so you can append, not overwrite

## Step 3: Gather the session facts

Survey the current conversation and any relevant project state (git log, test output, open files) to extract:

**Required**
- What was completed this session (specific, not vague — file names, function names, exact numbers)
- What failed and *why* (the mechanism of failure, not just "it didn't work")
- What the current status of the project is right now
- What the next concrete step is

**Include if present**
- Decisions made with their rationale (especially "why NOT X")
- Accuracy or test results with exact numbers (percentages, times, pass/fail counts)
- Bugs found and fixed, with root cause
- Constraints discovered that bound the solution space
- Git branch / commit range if meaningful

## Step 4: Write the entry

### Placement
Insert the new session entry **immediately after the status header**, before older entries. The file is reverse-chronological.

### Date header format
```
### [Month Day, Year]: [Specific description of what changed — not "updates" or "progress"]
```

The header title should name the actual thing that happened: "RECFAST upgrade + A_s fix + ncdm hierarchy overcorrection found" not "Session 7 updates".

### Entry body structure

**Root cause / discovery** (if applicable)
Lead with the key insight or root cause if the session was primarily diagnostic. One short paragraph.

**Changes** (numbered list)
Each item: what was done + file/location + why. Don't just name the change — explain the reasoning.
```
1. Replaced X with Y in `path/to/file.py` — previous approach caused Z because [mechanism]
2. Added parameter `foo` to `BarClass` — required because [constraint]
```

**Why NOT [alternative]** (if a significant path was rejected)
Explain the mechanism of rejection, not just "it didn't work":
```
**Why NOT approach X**: [Specific reason — OOM at N GB, non-monotonic convergence, breaks AD, etc.]
```

**Results / accuracy table** (if measurable outcomes exist)
Use a Markdown table. Pin to hardware, preset, and parameters so a future session knows the conditions:
```
**V100 timing (fit_cl preset, 100 k-modes):**
| Stage | Time |
|-------|------|
| Background | 0.5s |
| Total | 34s |
```

**Summary bullets** (brief)
3–5 bullets capturing the net outcome:
```
* TT sub-0.1% at l=20–300 (was 0.8% before ncdm hierarchy fix)
* 14x speedup vs prior planck_cl baseline
```

## Step 5: Update the permanent sections

These sections live at the bottom and are **appended to, never rewritten**:

### Bugs found and fixed
If new bugs were resolved this session, add them to the numbered list with format:
```
N. [Short description]: [root cause in one clause]
```

### Failed approaches (do not re-attempt)
If an approach was tried and definitively ruled out, add an entry:
```
* **[Approach name]**: [What was tried] → [Why it failed — the mechanism] → [What to do instead or why it's a dead end]
```

This section is the most important thing you write. A future session reading "tried X, failed" without the mechanism will just try it again.

### Confirmed correct (do not re-investigate)
If something was investigated and definitively closed, add it:
```
* **[What was confirmed]**: [Evidence — specific test, comparison, diagnostic] ([date, hardware if relevant])
```

### Known limitations and remaining work
Update the ordered list. Mark completed items with ~~strikethrough~~. Add new items discovered this session, ordered by impact.

## Step 6: Update the status header

At the very top of the file (after the title), update the status line to reflect where things stand *right now*:

```
## Status: [Current state in one line]

**[One sentence expanding on what's working and what isn't.]**
```

This is what the next session reads first. Make it accurate to this moment.

---

## Content rules (apply to every entry)

**Write status, not instructions.**
Use declarative past tense: "Replaced X with Y" not "Replace X with Y". "Accuracy is now 0.1%" not "Achieve 0.1% accuracy next". The next session should treat this as ground truth to verify, not orders to execute.

**Numbers over words.**
"2x speedup (487s → 243s)" not "significantly faster". "TT sub-0.1% at l=20–300" not "good accuracy at low l". "23 bugs fixed" not "many bugs resolved".

**Mechanism over outcome.**
"CubicSpline interpolation of T_l(k) causes aliasing because T_l oscillates faster than any practical k-grid — use source function interpolation instead" not "CubicSpline didn't work".

**Don't duplicate CLAUDE.md.**
If project conventions, architecture decisions, or tool choices are already in CLAUDE.md, don't restate them. Reference by name if needed.

**Don't duplicate code.**
Reference file paths and line numbers. Don't paste function bodies.

**Pin measurements to conditions.**
Every timing, accuracy, or performance number needs: hardware, preset/config, key parameters. A number without context is useless to a future session on different hardware.

---

## If CHANGELOG.md doesn't exist yet

Create it with this skeleton, then write the first entry:

```markdown
# [Project Name] Development Progress

## Status: [Current state]

**[One sentence describing what's working and what isn't.]**

---

## Changelog

### [Date]: Initial session — [what was done]

[Entry body]

---

## Bugs found and fixed

[Populate as bugs are found]

## Failed approaches (do not re-attempt)

[Populate as approaches are ruled out]

## Confirmed correct (do not re-investigate)

[Populate as things are definitively verified]

## Known limitations and remaining work

[Ordered by impact]
```
