# Output Report Templates

Use these templates for reporting pattern application results.

---

## Standard Report Format

Use this format after applying patterns to report results:

```
**Pattern Application Report**

Target: {target_path}, {skill|prompt}
Complexity: {simple|moderate|complex}

**Gap Analysis:**
- Tier 1: {present}/{total} present before → {present}/{total} after
- Tier 2: {present}/{applicable} present before → {present}/{applicable} after
- Critical gaps addressed: {n}

**Patterns Applied:**
- [P-1] {Pattern Name} (Tier {n}) - {1-sentence outcome}
- [P-2] {Pattern Name} (Tier {n}) - {1-sentence outcome}

**Superseded Content Removed:**
- {description of what was removed and why it was redundant}
- (omit this section if no content was removed)

**Patterns Deferred:**
- [P-5] {Pattern Name} - {reason user skipped}

**Remaining Gaps (if any):**
- {Pattern Name} (Tier {n}) - {why not applied}

**Notes:** {caveats, follow-ups, or observations}
```

---

## Analysis-Only Report Format

Use this format when user requests analysis only (no file modifications):

```
**Pattern Analysis Report (Analysis Only)**

Target: {target_path}, {skill|prompt}

**Coverage Assessment:**
{gap summary table}

**Recommended Improvements:**
{list with rationale}

No changes applied (analysis only requested).
```

---

## Usage Guidelines

**Standard Report - Required sections:**
- Target info (path, type, complexity)
- Gap analysis summary (before/after state)
- Patterns applied (with IDs and outcomes)
- Superseded content removed (what was cleaned up)
- Patterns deferred (user-rejected patterns)
- Remaining gaps (opportunities for future improvement)
- Notes (observations, follow-ups, caveats)

**Analysis-Only Report - Required sections:**
- Target info
- Coverage assessment (gap summary)
- Recommended improvements
- Explicit note: "No changes applied (analysis only requested)"

**Best practices:**
- Use addressable IDs ([P-1], [P-2]) for pattern references
- Provide 1-sentence outcomes for each applied pattern
- Document what content was removed to eliminate redundancy
- Note any follow-up actions or caveats
- Keep report concise but complete
