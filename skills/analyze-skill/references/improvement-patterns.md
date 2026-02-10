# Improvement Patterns

## Table of Contents

- [Common Improvement Categories](#common-improvement-categories) - Lines 15-68
- [Generating Improvements](#generating-improvements) - Lines 70-91
- [Improvement Format](#improvement-format) - Lines 93-146
- [Anti-Patterns (Filter Out)](#anti-patterns-filter-out) - Lines 148-170
- [Mapping Improvements to Specifications](#mapping-improvements-to-specifications) - Lines 172-182

---

## Common Improvement Categories

### Category 1: Tool Integration (+D-3)
**Pattern:** "Integrate with [X tool/API] to [specific capability]"

Examples:
- Integrate with Joplin MCP to persist outputs
- Connect to GitHub API for repository analysis
- Use Playwright for web scraping/automation
- Add database queries for data retrieval

### Category 2: Knowledge Base (+D-1, +D-7)
**Pattern:** "Add curated database of [domain-specific data]"

Examples:
- Include research database on [topic]
- Add best practices catalog for [domain]
- Incorporate industry standards and guidelines
- Include historical data for trend analysis

### Category 3: Guardrails (+D-4)
**Pattern:** "Add mandatory checkpoints for [specific steps]"

Examples:
- Require validation before destructive operations
- Add quality gates between workflow stages
- Enforce review step before final output
- Add rollback capability for errors

### Category 4: Depth (+D-7)
**Pattern:** "Expand [dimension] with [specific framework/methodology]"

Examples:
- Add expert-level analysis for [edge case]
- Include professional-grade [feature]
- Implement industry-standard [methodology]
- Add specialized handling for [scenario]

### Category 5: Complexity (+D-5)
**Pattern:** "Add multi-stage workflow for [complex aspect]"

Examples:
- Break into phases with state tracking
- Add parallel processing for independent tasks
- Implement chunking for large inputs
- Add synthesis step for multiple factors

### Category 6: UX (+D-6)
**Pattern:** "Create interactive interface for [user interaction]"

Examples:
- Add guided mode with step-by-step prompts
- Implement progress tracking for long operations
- Add preset configurations for common use cases
- Create resumable sessions for interrupted work

### Category 7: Structure (+D-2)
**Pattern:** "Define systematic process for [current ad-hoc step]"

Examples:
- Formalize the decision tree for [choice]
- Add explicit step sequence for [workflow]
- Create template for [recurring output]
- Define validation criteria for [quality check]

---

## Generating Improvements

### For Low Scores (0-5, Skip verdict)
Focus on **transformative** improvements that could move to Consider/Build:
- What would add 4+ points?
- What's the minimum viable enhancement to be worth building?
- Be honest if nothing would help (some ideas are just redundant)

### For Medium Scores (6-9, Consider verdict)
Focus on **targeted** improvements to reach Build territory:
- What 2-3 enhancements would add 4+ points?
- Prioritize by ROI (impact per effort)
- Identify "quick wins" (low effort, high impact)

### For High Scores (10-14, Build verdict)
Focus on **polishing** improvements:
- What would make a great skill even better?
- What edge cases should be handled?
- Brief note is sufficient: "Score already high; only minor refinements possible"

---

## Improvement Format

### Standard Format
```
[I-N] Enhancement Name (Vector) [Priority]
- What: Brief description of the enhancement
- Impact: (↑ D-X by +N, ↑ D-Y by +N)
- Effort: Low/Medium/High
- Rationale: Why this is valuable
```

### Example: Full Enhancement List
```
## Enhancement Opportunities (Ranked by Priority)

### HIGH Priority

[I-1] Batch Processing Mode (V-1) [HIGH]
- What: Process multiple items in single invocation with glob patterns
- Impact: (↑ D-5 by +1, ↑ D-6 by +1)
- Effort: Low
- Rationale: Power users process 10+ items; currently requires repeated invocations

[I-2] Joplin MCP Integration (V-2) [HIGH]
- What: Save outputs directly to Joplin notes
- Impact: (↑ D-3 by +2, ↑ D-6 by +1)
- Effort: Medium
- Rationale: Eliminates manual copy-paste; integrates into knowledge management workflow

### MEDIUM Priority

[I-3] Input Validation Layer (V-3) [MEDIUM]
- What: Validate inputs against schema before processing
- Impact: (↑ D-4 by +1)
- Effort: Low
- Rationale: Catches errors early; prevents wasted processing on invalid inputs

### LOW Priority

[I-4] Custom Templates (V-4) [LOW]
- What: Allow users to define output templates
- Impact: (↑ D-6 by +1)
- Effort: Medium
- Rationale: Nice-to-have for power users; not essential for core functionality

## Quick Wins (Low Effort + High Impact)
- [I-1] Batch Processing Mode
- [I-3] Input Validation Layer

## Potential Improved Score
- With I-1 and I-2: 12/14 (Build territory)
- With all improvements: 14/14 (maximum)
```

---

## Anti-Patterns (Filter Out)

**Vague improvements:**
- "Make it faster" → WHERE? HOW?
- "Add more features" → WHICH features?
- "Improve UX" → WHAT specifically?

**Non-improvements:**
- "Fix typos" → That's just maintenance
- "Better error messages" → Structural, not enhancement
- "Rewrite in Rust" → Implementation detail, not capability

**Unfeasible improvements:**
- "Add quantum computing support" → Not realistic
- "Integrate with proprietary API we don't have access to" → Blocked
- "Train custom ML model" → Scope creep

**Out-of-scope improvements:**
- Enhancements that violate skill's stated scope fence
- Features that would make it a different skill entirely
- Capabilities that belong in a separate skill

---

## Mapping Improvements to Specifications

When user selects improvements for specification generation:

1. **Integrate, don't append:** Selected improvements become core capabilities, not add-ons
2. **Update inputs:** New inputs required by improvements
3. **Update outputs:** New outputs produced by improvements
4. **Update steps:** Modify workflow to include new capabilities
5. **Update constraints:** Any new limitations from improvements
6. **Note dependencies:** Tools, MCP servers, or prerequisites needed
