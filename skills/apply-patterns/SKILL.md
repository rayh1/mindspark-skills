---
name: apply-patterns
description: Adds reliability patterns to a prompt or skill using tiered pattern system. Requires explicit mode selection (lite/Tier 1, standard/Tier 1+2, full/all tiers). Use when improving determinism, debuggability, and scope control without changing core intent. Requires user approval before any modifications.
---

<quick_start>
**Quick invocations:**
- `apply-patterns path/to/file.md mode:lite` → Tier 1 patterns
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
See <objective> for detailed mode descriptions.

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
- [error-taxonomy.md](references/error-taxonomy.md) - Error codes and recovery (load after Error Handling pattern is approved)
</reference_guides>

<objective>
Improve determinism and debuggability by adding reliability structure (not changing core intent) using a tier-appropriate set of patterns.

Modes:
- **lite:** Propose 2-4 patterns from Tier 1; single approval gate; single self-check.
- **standard:** Consider Tier 1 + Tier 2; propose 3-6 recommended + 0-3 optional; hard cap 8.
- **full (opt-in):** Audit against all 22 patterns; produce prioritized report; no hard cap.
</objective>

<interpretation_check>
Before analyzing the target:
- Detect target type (skill vs prompt) from YAML frontmatter
- Determine mode (lite/standard/full) from args
- Restate: "Analyzing {target_path} as {skill|prompt} in {mode} mode"
- List key assumptions: target complexity, existing patterns detected, tier scope
- If mode ambiguous or target characteristics unclear: ask before proceeding
</interpretation_check>

<inputs_first>
**Required inputs:**
- `target_path`: Path to the prompt or SKILL.md file to improve
- `mode`: `lite`, `standard`, or `full`

**Optional inputs:**
- `max_cycles`: Maximum review-revise cycles.
  - Default: 1 (lite), 1 (standard), 2 (full)
  - Upper bound: 5 (only when user explicitly requests)

**Derived (from reading files):**
- `target_content`
- `target_type`: `skill` (YAML frontmatter with name/description) or `prompt`
</inputs_first>

<scope_fence>
**File scope:**
- Read `target_path`
- Read references as needed based on `mode`
- Propose patterns and apply only those the user approves

**Pattern selection scope:**

Tier structure: See <essential_principles> for tier definitions.

Mode-specific budgets:
- `lite`: Source Tier 1 only; hard cap **4** patterns
- `standard`: Source Tier 1 + Tier 2; recommend 3-6, optional 0-3; hard cap **8 total** (unless user requests more)
- `full`: Source all tiers; no hard cap; must prioritize as must-have / should-have / optional

Tier 1 priority order (for lite mode):
1. Inputs First (almost always)
2. Step Contract (if multi-step)
3. Scope Fence (if boundaries matter)
4. Output Schema (if output structure matters)
5. Stop Conditions (if scope could creep)
6. Quality Gates (if validation matters)
7. Interpretation Check (if complex/ambiguous)
8. Decision Points (if branching exists)

Pick 2-4 based on target needs. Substitute if another pattern is clearly higher-yield.

**Out of scope:**
- Changing the target's core intent/behavior (only add reliability structure)
- Editing files other than `target_path`
- Editing reference files
- Exceeding mode caps without explicit user approval
- Applying Tier 3 patterns in lite/standard modes
- Proposing patterns that change behavior vs. structure
</scope_fence>

<decision_points>
- Detect `mode`:
  - If `full`: require explicit opt-in wording (if ambiguous, ask once)
- If the target is already short and unambiguous, propose fewer patterns (0-2 is fine).
- Prefer Tier 1 patterns over Tier 2.
- Do not propose Tier 3 patterns in `lite` or `standard` mode.
- If applying a pattern would change behavior (not just structure/format), ask before proposing it.
- In `full`, Tier 3 patterns (Observability, Error Handling) require explicit ROI justification.
- **Full mode max: propose 6-8 patterns** (not 10+) unless user explicitly requests comprehensive coverage.
- **Conversational skills:** Flag patterns that check "user engagement" or track state user is actively participating in (poor fit).
- **Length warning:** If pattern additions would increase file length >50%, warn before approval.
</decision_points>

<step_contract>
1. Confirm `mode` is provided; if missing, ask the user to choose `lite`, `standard`, or `full`.
2. Read `target_path`; detect `target_type`.
3. Load references needed for the mode.
4. Analyze target against applicable tier(s).
5. Propose patterns (respect mode budgets) and wait for approval.
6. Apply approved patterns to the target file.
7. Review once (`lite`) OR review-revise cycles (`standard`/`full`, bounded by `max_cycles`).
8. Report results.
</step_contract>

<quality_gates>
G1 (after Step 4): Are applicable patterns correctly identified for the target's characteristics?
G2 (after Step 5): Does proposal respect mode budget (lite: ≤4, standard: ≤8, full: prioritized)?
G3 (after Step 6): Integration introduces no duplicates, conflicts, or broken XML tags?
G4 (after Step 7): All approved patterns present and properly formatted?
If gate fails: fix once → re-check; else ERROR: GATE_FAIL - report issue.

Note: Quality Gates run inline during step execution. Review Step (see <review_step>) runs holistically after all steps complete.
</quality_gates>

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

⚠️ **Warnings:**
- Proposing {n} patterns (guideline: 6-8 max for full mode)
- Estimated length increase: {percent}% (warn if >50%)
- Poor fits flagged: {list any patterns inappropriate for skill type}

[approve all] [select subset] [reject all] [analysis only]
```

On cancel/analysis only: produce analysis output; do not modify files.
</user_approval_gate>

<clarifying_questions>
Triggers:
- Pattern applicability uncertain (confidence < 60%)
- Mode detection ambiguous ("full" vs user just wants thorough analysis)
- Target has conflicting indicators (looks simple but user wants comprehensive audit)
Constraints:
- max_per_analysis: 3 (lite/standard), 5 (full)
- batch_related: true
- ask_before: proposing Tier 3 patterns that require justification
Fallback: use safe default (lite mode, skip uncertain patterns), document in assumptions
</clarifying_questions>

<confidence_signal>
When recommending patterns, indicate confidence:
- High (>85%): pattern clearly applicable based on target characteristics
- Medium (60-85%): pattern situationally applicable, ROI depends on context
- Low (<60%): pattern might help, ask user before including
Include assessment in proposal rationale. In full mode, use to prioritize must-have vs should-have vs optional.
</confidence_signal>

<fallback_chain>
Primary: integrate patterns using canonical tag names from minimal-templates.md
Fallback_1: if tag name conflicts with existing non-pattern tag → prefix with "pattern_" (e.g., `<pattern_inputs_first>`). Note: Use "pattern_" prefix only when tag name conflicts with existing non-pattern tag.
Fallback_2: if XML structure breaks → use text format inside `<pattern>{pattern_name}...content...</pattern>` wrapper
Fallback_3: if integration creates irresolvable conflicts → skip pattern, note in report
Terminal: ERROR: MERGE_FAIL - surface to user with conflict details
Trigger: validation detects broken XML, duplicate sections, or semantic conflicts
</fallback_chain>

<addressable_output>
In proposals, assign IDs to each recommended pattern:
- Format: `[P-{number}]` for patterns
- Example: `[P-1] Inputs First: clearly applicable, target takes inputs`
- User can reference: "apply P-1, P-3, and P-5 only"
In reports, use IDs for:
- Patterns applied: `[P-1], [P-2], [P-4]`
- Patterns deferred: `[P-3] User Approval Gate - deferred, user skipped`
</addressable_output>

<lens>
Analyze pattern applicability from multiple perspectives:
- **Correctness lens:** Does pattern prevent errors or ambiguity in this target?
- **Integration lens:** Will pattern conflict with existing structure or duplicate content?
- **ROI lens:** Does value (reliability gain) exceed overhead (added complexity)?
Per lens: findings + severity (critical/major/minor)
Synthesis: merge findings, flag conflicts (e.g., pattern scores high on correctness but low on ROI)
Coverage: each lens must report for recommended patterns
</lens>

<review_step>
After integration, perform holistic review:
Criteria:
- All approved patterns present and properly formatted
- No duplicate sections (same pattern added twice)
- XML tags balanced (for skills)
- Tier 1 patterns before Tier 2, Tier 2 before Tier 3
- Integration doesn't change target's core intent (see <essential_principles>)
Max_cycles: 1 (lite), 1 (standard), 2 (full)
Per cycle: identify issues → revise → re-check
Exit: no issues found OR max_cycles reached
If issues remain at max_cycles: report them in output
</review_step>

<assumption_registry>
Log assumptions made during analysis:
- [Mode detection] assumed: "{detected_mode}" | source: {args|default} | confidence: {high|medium|low}
- [Target complexity] assumed: "{simple|moderate|complex}" | source: {line_count, step_count} | confidence: {level}
- [Pattern applicability] assumed: "{pattern} applicable because {reason}" | confidence: {level}
Validate high-impact + low-confidence assumptions before proposing patterns.
Document all assumptions in analysis output for auditability.
</assumption_registry>

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

<stop_conditions>
Done when:
- All approved patterns integrated into target file
- Quality gates pass (G1-G4)
- Review cycle complete (no issues OR max_cycles reached)
- Report generated per output schema
Don't:
- Change target's core intent/behavior (structure only)
- Add patterns user rejected
- Edit files other than target_path
- Exceed mode budget without user approval
- Propose 10+ patterns in full mode (guideline: 6-8 max)
- Apply patterns that check "user engagement" to conversational skills (poor fit)
</stop_conditions>
