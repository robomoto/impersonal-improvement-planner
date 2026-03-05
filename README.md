# Impersonal Improvement Planner

A self-improvement kit for Claude Code agent teams. Measures execution quality across tracked runs and guides iterative improvement through a four-phase cycle.

## What This Is

Four components that work together to make your agent team faster, cheaper, and more reliable:

1. **Roster Checker** -- Scans your project and ensures the team has the right specialists before work begins
2. **Tracked Run** -- Instruments a work session with metrics: parallel dispatch rate, duplicate reads, token usage
3. **Post-Run Review** -- Verifies self-reported metrics against JSONL ground truth and recommends improvements
4. **Metrics Script** -- Parses Claude Code session logs to extract objective performance data

## The Cycle

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

Each phase is user-triggered. Nothing runs automatically.

## Quick Start

### 1. Copy files into your project

```bash
# From this repo, copy into your agent project:
cp skills/tracked-run/SKILL.md   <project>/.claude/skills/tracked-run/SKILL.md
cp agents/roster-checker.md      <project>/.claude/agents/roster-checker.md
cp scripts/parse-run-metrics.py  <project>/scripts/parse-run-metrics.py
cp docs/post-run-review.md       <project>/docs/post-run-review.md
cp templates/baseline.md         <project>/docs/baseline.md
cp templates/team-log.md         <project>/docs/team-logs/template.md
```

### 2. Add trigger phrases to your project's CLAUDE.md

```markdown
## Improvement Planner

| Trigger | File | Purpose |
|---------|------|---------|
| "tracked run" | `.claude/skills/tracked-run/SKILL.md` | Instrument run with metrics |
| "post-run review" | `docs/post-run-review.md` | Review metrics against JSONL ground truth |
| "roster check" | `.claude/agents/roster-checker.md` | Audit team roster against project needs |
```

### 3. Run your first tracked run

Say "tracked run" followed by your task. The first run establishes a baseline. Do a "post-run review" in a fresh session to verify and record it.

See `docs/setup-guide.md` for the full walkthrough.

## Requirements

- **Claude Code** with Agent tool support
- **Python 3.10+** (for the metrics script)
- An existing hub-and-spoke agent team (lead + specialists)

## What It Measures

| Metric | Why it matters |
|--------|---------------|
| Parallel dispatch rate | Higher = faster, better concurrency |
| Duplicate read rate | Lower = less wasted context |
| Total tokens | Lower = cheaper, more headroom |
| Wall-clock time | Lower = faster delivery |
| Scope overlap | Lower = agents aren't duplicating work |

## Repo Structure

```
impersonal-improvement-planner/
├── README.md                          # This file
├── CLAUDE.md                          # AI-readable: teaches Claude the trigger phrases
├── LICENSE                            # MIT
├── skills/
│   └── tracked-run/
│       └── SKILL.md                   # Tracked-run skill for the lead agent
├── agents/
│   └── roster-checker.md             # Roster-checker agent definition
├── scripts/
│   └── parse-run-metrics.py          # JSONL metrics parser
├── docs/
│   ├── post-run-review.md            # Review protocol
│   ├── setup-guide.md                # Installation walkthrough
│   ├── workflow.md                   # The 4-phase cycle explained
│   └── examples/
│       ├── team-log-example.md       # Example team log
│       └── review-example.md         # Example review output
└── templates/
    ├── baseline.md                   # Baseline recording template
    └── team-log.md                   # Team log template
```

## License

MIT
