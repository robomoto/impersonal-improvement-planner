# Setup Guide

How to install the personal improvement planner into an existing Claude Code agent project.

## Prerequisites

Before starting, confirm the project has:

- [ ] A `.claude/agents/` directory with at least a lead agent
- [ ] A hub-and-spoke architecture (lead dispatches to specialists via the Agent tool)
- [ ] Python 3.10+ available (for the metrics script)

If the project doesn't have an agent team yet, this kit won't help -- it measures and improves an existing team.

## Step 1: Copy Files

From the `impersonal-improvement-planner/` repo, copy these into your project:

```
# Skill (lead uses this during tracked runs)
cp skills/tracked-run/SKILL.md  <project>/.claude/skills/tracked-run/SKILL.md

# Agent (lead dispatches this before every task)
cp agents/roster-checker.md     <project>/.claude/agents/roster-checker.md

# Metrics script
cp scripts/parse-run-metrics.py <project>/scripts/parse-run-metrics.py

# Review protocol
cp docs/post-run-review.md      <project>/docs/post-run-review.md

# Templates
cp templates/baseline.md        <project>/docs/baseline.md
cp templates/team-log.md        <project>/docs/team-logs/template.md
```

Create directories as needed (`mkdir -p` for `.claude/skills/tracked-run`, `scripts`, `docs/team-logs`).

## Step 2: Add Trigger Phrases to Project CLAUDE.md

Add this section to the project's `CLAUDE.md` so Claude knows when to activate each component:

```markdown
## Improvement Planner

| Trigger | File | Purpose |
|---------|------|---------|
| "tracked run" | `.claude/skills/tracked-run/SKILL.md` | Instrument run with metrics |
| "post-run review" | `docs/post-run-review.md` | Review metrics against JSONL ground truth |
| "roster check" | `.claude/agents/roster-checker.md` | Audit team roster against project needs |
```

## Step 3: Add Roster Check to Lead Agent

In your lead agent definition (`.claude/agents/lead.md` or equivalent), add this rule:

```markdown
### Mandatory First Dispatch

Before any other work on a new task, dispatch the roster-checker agent. This blocks all other dispatches until it returns. The roster-checker ensures the team has the right specialists and doc bundles for the project at hand.
```

## Step 4: Run Your First Tracked Run

1. Start a new Claude Code session in your project
2. Say: "tracked run" followed by your task description
3. The lead will read the tracked-run skill and instrument the session
4. When complete, the lead writes a team log to `docs/team-logs/`
5. Since there's no baseline yet, this run establishes one

## Step 5: Do a Post-Run Review

In a **fresh** Claude Code session (important -- the reviewer must not inherit the lead's context):

1. Say: "post-run review"
2. Claude reads the review protocol and analyzes the JSONL log
3. The review compares self-reported metrics against ground truth
4. Results go to `docs/post-run-review-<date>.md`

## Step 6: Record the Baseline

After the first review, fill in `docs/baseline.md` with the ground-truth numbers. This becomes the comparison point for future runs.

## Verification

After setup, confirm everything is wired correctly:

- [ ] `python3 scripts/parse-run-metrics.py --help` prints usage info
- [ ] Project CLAUDE.md has the trigger phrases table
- [ ] Lead agent definition references roster-checker as mandatory first dispatch
- [ ] `docs/baseline.md` exists (even if empty before first run)
- [ ] `.claude/skills/tracked-run/SKILL.md` exists and is readable
