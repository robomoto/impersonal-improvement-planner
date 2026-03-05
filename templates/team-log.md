# Team Log: [Task Name]

**Date:** YYYY-MM-DD
**Task:** [Brief description of the task]
**Lead model:** [Opus/Sonnet]

## Delegation Plan

[Numbered list of planned dispatch phases with dependencies noted]

## Phase 1: [Phase Name]

[What was dispatched, what came back, key findings]

## Phase 2: [Phase Name]

[Continue for each phase...]

## Run Metrics

| Metric | Value |
|--------|-------|
| Dispatches (total) | N |
| Dispatches (parallel) | N (M batches) |
| Dispatches (sequential) | N |
| Dispatches (background) | N |
| Reads (total, lead only) | N |
| Reads (unique files) | N |
| Reads (duplicate) | N |
| Duplicate read rate | X% |
| Parallel dispatch rate | X% |
| Total tokens (all agents) | ~NK |
| Wall-clock time | ~N min |

## Comparison vs Baseline

| Metric | Baseline (Run N) | This Run | Delta |
|--------|-----------------|----------|-------|
| Parallel dispatch rate | X% | Y% | +/-Z% |
| Duplicate read rate | X% | Y% | +/-Z% |
| Total tokens | ~NK | ~NK | +/-NK |
| Wall-clock time | ~N min | ~N min | +/-N min |

## Self-Critique

1. **What dispatches were sequential that could have been parallel?**
   [List each with the reason it happened]

2. **What files were read redundantly?**
   [List each duplicate and why it happened]

3. **What specialist output did I re-verify unnecessarily?**
   [List files you re-read despite having a specialist report]

4. **What would I do differently next time?**
   [Concrete changes, not vague intentions]
