# Observability Log Format Templates

Used in `<observability>` during each research session.

## Phase Start Log

Produce at the start of every phase:

```
[PHASE X: {phase_name}] - {timestamp}
Status: {in_progress|complete|failed}
```

## Review-Fix Cycle Log

Produce per cycle for Phase 3.5 (Review-Fix):

```
[PHASE 3.5: REVIEW-FIX] Cycle {N}/{max_review_cycles} - {timestamp}
Issues found: {count} | Fixed: {count} | Unfixable: {count}
{list of unfixable items if any}
```

Produce per cycle for Phase 7.5 (Consolidation Review-Fix):

```
[PHASE 7.5: CONSOLIDATION REVIEW-FIX] Cycle {N}/{max_review_cycles} - {timestamp}
Issues found: {count} | Fixed: {count} | Unfixable: {count}
{list of unfixable items if any}
```

## Session End Summary

Produce at end of session and append to journal file:

```
## Session Summary - {timestamp}
**Research topic:** {topic}
**Mode:** {research_mode}
**Phases completed:** {phase_list}
**Sources analyzed:** {count} articles
**Key decisions:**
- {decision_1}
- {decision_2}
**Failures:** {any gate failures or fallbacks triggered}
```
