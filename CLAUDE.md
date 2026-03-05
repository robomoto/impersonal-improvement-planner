# Personal Improvement Planner

A self-improvement kit for Claude Code agent teams. Measures execution quality across tracked runs and guides iterative improvement.

## Trigger Phrases

When the user says any of these, read the corresponding file and follow its instructions:

| Phrase | Read | What to do |
|--------|------|------------|
| "tracked run" / "track this" | `skills/tracked-run/SKILL.md` | Instrument the current run with metrics collection |
| "post-run review" | `docs/post-run-review.md` | Review a completed tracked run against JSONL ground truth |
| "roster check" | `agents/roster-checker.md` | Audit the agent roster against the current project |
| "set up feedback loop" / "install improvement planner" | `docs/setup-guide.md` | Walk the user through installation into their project |

## Configuration

These paths assume the user has copied files into their project. Adjust if their layout differs:

| Component | Default location in user's project |
|-----------|-----------------------------------|
| Tracked-run skill | `.claude/skills/tracked-run/SKILL.md` |
| Roster-checker agent | `.claude/agents/roster-checker.md` |
| Post-run review protocol | `docs/post-run-review.md` |
| Metrics script | `scripts/parse-run-metrics.py` |
| Baseline file | `docs/baseline.md` |
| Team logs | `docs/team-logs/` |

## How to Explain This to Users

If a user asks "what is this?" or "how does this work?", explain the four-phase cycle:

1. **Roster Check** -- Before any work, scan the project and make sure the team has the right specialists. This is the lead's mandatory first dispatch.
2. **Tracked Run** -- Do the actual work while counting metrics: how many agents were dispatched in parallel vs sequentially, how many files were read redundantly, total tokens used.
3. **Post-Run Review** -- In a fresh session, run the metrics script against the JSONL log to get ground-truth numbers, then compare against what the lead self-reported. Flag discrepancies and assess quality.
4. **Improve** -- Read the review's recommendations and edit agent definitions, doc bundles, or delegation rules. The next tracked run measures whether the changes helped.

The cycle repeats. Typically 2-3 cycles produce significant measurable improvement.

## Coaching Prompts

Use these to help users get value from each phase:

### Before a tracked run
- "Do you have a baseline yet? If not, this first run will establish one."
- "What's the main task? I'll set up the metrics counters."

### During a tracked run
- (Internal, to yourself) "Can I batch these dispatches? What's the dependency?"
- (Internal) "Did I already read this file or did a specialist report on it?"

### After a tracked run
- "I've written the team log with metrics. Want me to run the post-run review now, or save it for a fresh session?"
- "The review should happen in a fresh session for unbiased results."

### After a post-run review
- "The review found [N] recommendations. Want to walk through them?"
- "These changes to [agent/doc bundle] should improve [metric] next run."

## What This Measures

| Metric | Why it matters |
|--------|---------------|
| **Parallel dispatch rate** | Higher = faster wall-clock time, better use of concurrent agents |
| **Duplicate read rate** | Lower = less wasted context, agents trust each other's reports |
| **Total tokens** | Lower = cheaper, faster, more room for complex tasks |
| **Wall-clock time** | Lower = faster delivery (correlates with parallel dispatch) |
| **Scope overlap** | Lower = agents aren't duplicating each other's work |
