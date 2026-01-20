---
name: apply-patterns
description: Adds reliability patterns to a prompt or skill using tiered pattern system. Modes - lite (default, Tier 1), standard (Tier 1+2), full (all tiers). Use when improving determinism, debuggability, and scope control without changing core intent.
---

<quick_start>
**Quick invocations:**
- `apply-patterns path/to/file.md` → lite (default, Tier 1 patterns)
- `apply-patterns path/to/file.md mode:standard` → Tier 1 + Tier 2 patterns
- `apply-patterns path/to/file.md mode:full` → comprehensive 22-pattern audit
</quick_start>

<essential_principles>
**Tiered Pattern System:**
Patterns are organized into three tiers by priority:
- **Tier 1 - Core (8 patterns):** High-yield, low-overhead. Always consider.
- **Tier 2 - Recommended (8 patterns):** Valuable when situation warrants.
- **Tier 3 - Situational (6 patterns):** Specific use cases. Require justification.

**Mode-Based Selection:**
- **lite (default):** Tier 1 only. Fast, 2-4 patterns.
- **standard:** Tier 1 + Tier 2. Balanced, 3-6 recommended + 0-3 optional.
- **full:** All tiers. Comprehensive audit with prioritization.

**Progressive Reference Loading:**
- `lite`: minimal-templates.md only
- `standard`: + applicability-guide.md
- `full`: + llmprog.md + pattern-catalog.md

**Structure-Only Improvements:**
Patterns add reliability structure (inputs, contracts, validation) without changing the target's core intent or behavior.

**User Approval Gate:**
All pattern applications require explicit user approval before file modification.
</essential_principles>

<reference_guides>
**Supporting documentation in references/:**
- [pattern-catalog.md](references/pattern-catalog.md) - Tiered pattern list with applicability
- [minimal-templates.md](references/minimal-templates.md) - Canonical tags + minimal insertion templates
- [llmprog.md](references/llmprog.md) - Full pattern details (22 patterns, ~800 lines)
- [applicability-guide.md](references/applicability-guide.md) - Tier-aware applicability rules
- [integration-guide.md](references/integration-guide.md) - Pattern formatting and ordering
- [examples.md](references/examples.md) - Good/bad integration examples
- [error-taxonomy.md](references/error-taxonomy.md) - Error codes and recovery (used only for Error Handling in `full`)

Load only what you need:
- `lite`: minimal-templates.md
- `standard`: + applicability-guide.md
- `full`: + llmprog.md, pattern-catalog.md (+ error-taxonomy.md if applying Error Handling)
</reference_guides>

<objective>
Improve determinism and debuggability by adding reliability structure (not changing core intent) using a tier-appropriate set of patterns.

Modes:
- **lite (default):** Propose 2-4 patterns from Tier 1; single approval gate; single self-check.
- **standard:** Consider Tier 1 + Tier 2; propose 3-6 recommended + 0-3 optional; hard cap 8.
- **full (opt-in):** Audit against all 22 patterns; produce prioritized report; no hard cap.
</objective>

<inputs_first>
**Required inputs:**
- `target_path`: Path to the prompt or SKILL.md file to improve

**Optional inputs:**
- `mode`: `lite` (default), `standard`, or `full`
- `max_cycles`: For `standard`/`full` only. Maximum review-revise cycles.
  - Default: 1 (standard), 2 (full)
  - Upper bound: 5 (only when user explicitly requests)

**Derived (from reading files):**
- `target_content`
- `target_type`: `skill` (YAML frontmatter with name/description) or `prompt`
</inputs_first>

<scope_fence>
**In scope:**
- Read `target_path`
- Read references as needed based on `mode`
- Propose patterns and apply only those the user approves

**Out of scope:**
- Changing the target's core intent/behavior (only add reliability structure)
- Editing files other than `target_path`
- Editing reference files
</scope_fence>

<pattern_budget>
**Tier Structure:**
- Tier 1 (Core): 8 patterns - always consider
- Tier 2 (Recommended): 8 patterns - when situation warrants
- Tier 3 (Situational): 6 patterns - require justification

**Mode-specific budgets:**

- `lite`:
  - Source: Tier 1 only
  - Hard cap: **4** patterns
  - Priority: Inputs First → Step Contract → Scope Fence → Output Schema

- `standard`:
  - Source: Tier 1 + Tier 2
  - Recommended: **3-6** patterns
  - Optional: **0-3** patterns
  - Hard cap (unless user asks): **8 total**

- `full`:
  - Source: All tiers
  - No hard cap; prioritization required
  - Must produce: must-have / should-have / optional split

**Tier 1 priority order (for lite):**
1. Inputs First (almost always)
2. Step Contract (if multi-step)
3. Scope Fence (if boundaries matter)
4. Output Schema (if output structure matters)
5. Stop Conditions (if scope could creep)
6. Quality Gates (if validation matters)
7. Interpretation Check (if complex/ambiguous)
8. Decision Points (if branching exists)

Pick 2-4 based on target. Substitute if another pattern is clearly higher-yield.
</pattern_budget>

<fast_path>
Applies to `standard` mode by default:
- If the target is short/simple (< ~150 lines) OR applicable set is ≤5 patterns: single-pass workflow.
- Use multi-cycle refinement only when integration introduces conflicts or user requests deeper integration.
</fast_path>

<decision_points>
- Detect `mode`:
  - If missing: default to `lite`
  - If `full`: require explicit opt-in wording (if ambiguous, ask once)
- If the target is already short and unambiguous, propose fewer patterns (0-2 is fine).
- Prefer Tier 1 patterns over Tier 2.
- Do not propose Tier 3 patterns in `lite` or `standard` mode.
- If applying a pattern would change behavior (not just structure/format), ask before proposing it.
- In `full`, Tier 3 patterns (Observability, Error Handling) require explicit ROI justification.
</decision_points>

<user_approval_gate>
Before editing, require explicit approval.

**`lite` presentation format:**
```
**Pattern Proposal (Lite)**

Target: {target_path}
Type: {skill|prompt}
Source: Tier 1

Recommended (max 4):
- {pattern}: {1-sentence rationale}

[proceed] [select subset] [cancel]
```

**`standard` presentation format:**
```
**Pattern Proposal (Standard)**

Target: {target_path}
Type: {skill|prompt}
Source: Tier 1 + Tier 2

**Gap Analysis:**
- Tier 1: {present}/{total} present
- Tier 2: {applicable} applicable

**Recommended Patterns** (max 6)
| Pattern | Tier | Status | Rationale |
|---------|------|--------|-----------|
| {name}  | 1/2  | missing/partial | {why it helps} |

**Optional Patterns** (max 3)
| Pattern | Tier | Rationale |
|---------|------|-----------|
| {name}  | 2    | {lower priority} |

[approve all] [select subset] [reject all]
```

**`full` presentation format:**
```
**Pattern Audit (Full)**

Target: {target_path}
Type: {skill|prompt}

**Coverage Summary:**
| Tier | Present | Partial | Missing | N/A |
|------|---------|---------|---------|-----|
| 1    | {n}     | {n}     | {n}     | {n} |
| 2    | {n}     | {n}     | {n}     | {n} |
| 3    | {n}     | {n}     | {n}     | {n} |

**Must-Have** (blocks correctness)
| Pattern | Tier | Status | Rationale |
|---------|------|--------|-----------|

**Should-Have** (improves reliability)
| Pattern | Tier | Status | Rationale |
|---------|------|--------|-----------|

**Optional** (nice to have)
| Pattern | Tier | Status | Rationale |
|---------|------|--------|-----------|

[approve all] [select subset] [reject all] [analysis only]
```

On cancel/analysis only: produce analysis output; do not modify files.
</user_approval_gate>

<step_contract>
1. Read `target_path`; detect `target_type`.
2. Determine `mode` (default `lite`).
3. Load references needed for the mode.
4. Analyze target against applicable tier(s).
5. Propose patterns (respect mode budgets) and wait for approval.
6. Apply approved patterns to the target file.
7. Self-check once (`lite`) OR review-revise cycles (`standard`/`full`, bounded by `max_cycles`).
8. Report results.
</step_contract>

<self_check>
After applying patterns, verify:
- No accidental behavior change (structure only).
- No duplicated/conflicting sections introduced.
- For skills: YAML frontmatter remains valid; XML tags (if used) are balanced.
- The result is still minimal; avoid adding "framework" sections.
- Tier 1 patterns integrated before Tier 2; Tier 2 before Tier 3.
</self_check>

<output_schema>
Return a concise report.

For `lite`:
- Target: {target_path}, {skill|prompt}
- Mode: lite (Tier 1)
- Patterns applied: {list}
- Notes: {caveats, follow-ups}

For `standard`:
- Target: {target_path}, {skill|prompt}
- Mode: standard (Tier 1 + 2)
- Gap summary: {2-4 bullets}
- Patterns applied: {list}
- Remaining issues: {if any}

For `full`:
- Target: {target_path}, {skill|prompt}
- Mode: full (all tiers)
- Coverage table: present/partial/missing/N/A by tier
- Patterns applied: {list with tier}
- Patterns deferred: {list with reason}
</output_schema>

<success_criteria>
- YAML frontmatter remains valid and the description is third-person with clear "Use when..." triggers.
- No markdown headings (`#`, `##`, `###`) appear in the skill body outside fenced code blocks.
- The proposal respects the selected mode's tier scope and budget.
- Applied edits add reliability structure without changing the target's core intent/behavior.
- A self-check confirms no duplicated/conflicting sections and balanced XML tags.
- Patterns are prioritized: Tier 1 > Tier 2 > Tier 3.
</success_criteria>
