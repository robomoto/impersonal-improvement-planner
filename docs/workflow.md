# The Improvement Cycle

Four phases, each triggered by the user. Nothing runs automatically.

```
  ROSTER CHECK ───> TRACKED RUN ───> POST-RUN REVIEW ───> IMPROVE
   (Phase 1)        (Phase 2)        (Phase 3)            (Phase 4)
   Scan project     Do the work      Run metrics          Edit agent
   Check roster     Count metrics    script for           definitions
   Fill gaps        Write team log   ground truth         Update docs
                    + self-critique  Compare & verdict    Set baseline
                                     Recommend fixes          |
         <───────────────────────────────────────────────────-+
```

## Phase 1: Roster Check

**Trigger:** Lead's mandatory first dispatch on any new task.
**Actor:** roster-checker agent (dispatched by lead).
**Inputs:** Project's CLAUDE.md, build files, existing `.claude/agents/` directory.
**Outputs:** Structured report listing gaps, created specialists, doc bundle status.
**Stored at:** Returned directly to the lead (not saved to a file).

The roster-checker scans the project for languages, frameworks, platforms, and services, then compares against the existing agent roster. It creates missing specialists and doc bundles, or reports gaps in report-only mode.

## Phase 2: Tracked Run

**Trigger:** User says "tracked run" or "track this".
**Actor:** Lead agent, using the tracked-run skill.
**Inputs:** Baseline file (`docs/baseline.md`), the actual task to perform.
**Outputs:** Completed task + team log with metrics and self-critique.
**Stored at:** `docs/team-logs/<task-name>-<date>.md`

During the run, the lead maintains mental counters for dispatch parallelism, read discipline, and token usage. After completing the task, it writes a team log with a metrics table, comparison against baseline (if one exists), and a self-critique section.

## Phase 3: Post-Run Review

**Trigger:** User says "post-run review" in a fresh session.
**Actor:** Claude (or a reviewer subagent), using the review protocol.
**Inputs:** JSONL session log (parsed by `parse-run-metrics.py`), team log from Phase 2, baseline.
**Outputs:** Review report with metrics verification, quality assessment, and recommendations.
**Stored at:** `docs/post-run-review-<date>.md`

This MUST happen in a fresh session. The reviewer runs the metrics script to get ground-truth numbers from the JSONL, compares them against the lead's self-reported metrics, and assesses dispatch quality, read discipline, and specialist output quality. It ends with a verdict (IMPROVED / MIXED / REGRESSED / BASELINE) and concrete recommendations.

## Phase 4: Improve

**Trigger:** User reads the review and decides what to change.
**Actor:** User (with Claude's help if requested).
**Inputs:** Review report and its recommendations.
**Outputs:** Edited agent definitions, updated doc bundles, new delegation rules.
**Stored at:** Changes go directly into `.claude/agents/`, `.claude/docs/`, CLAUDE.md, etc.

This is the human-in-the-loop phase. The review's recommendations are suggestions, not commands. The user decides which changes to make. Common improvements include:
- Adding parallel dispatch rules to the lead
- Tightening agent scope descriptions to reduce overlap
- Creating doc bundles for technologies that caused errors
- Adding read discipline reminders to specific agents

## What Flows Between Phases

```
Phase 1 -> Phase 2:  Roster report (team is ready, specialists created)
Phase 2 -> Phase 3:  Team log file path + JSONL session log
Phase 3 -> Phase 4:  Review report with recommendations
Phase 4 -> Phase 1:  Updated agent definitions (used in next roster check)
```

## When to Skip Phases

- **Skip Phase 1** if the project hasn't changed since the last roster check and no new specialists are needed.
- **Skip Phase 3** if this was a trivial task and you don't need formal metrics (but you lose the ground-truth verification).
- **Never skip Phase 2** during a tracked run -- that's the whole point.
- **Phase 4 is always optional** -- if the review says IMPROVED with no issues, there may be nothing to change.

## How Long Before You See Improvement

- **Run 1:** Establishes the baseline. Expect rough numbers -- this is the "before" picture.
- **Run 2:** After applying improvements from Run 1's review. Typically shows the biggest gains (parallel dispatch and duplicate reads are the easiest to fix).
- **Run 3+:** Diminishing returns on the easy metrics. Improvements shift to specialist quality, scope overlap, and token efficiency.

In practice, two cycles are enough to fix the most impactful issues. Continue tracking if you're tuning for cost or speed.
