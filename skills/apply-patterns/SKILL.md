---
name: apply-patterns
description: "Adds reliability patterns to a prompt or skill using gap-driven analysis. Reads target, detects missing patterns, proposes improvements based on actual gaps. Use when (1) improving prompt determinism with quality gates and contracts, (2) adding scope control and stop conditions to skills, (3) analyzing pattern coverage gaps in existing SKILL.md files, or other reliability engineering tasks. Does not change core intent. Requires user approval before modifications."
---

<quick_start>
**Quick invocation:**
- `apply-patterns path/to/file.md` → gap analysis + tailored pattern proposal
</quick_start>

<essential_principles>
**Tiered Pattern System:**
Patterns are organized into three tiers by priority. See [pattern-catalog.md](references/pattern-catalog.md) for tier definitions and full pattern list.

**Gap-Driven Analysis:**
1. Read target file and detect existing patterns
2. Identify missing patterns by tier
3. Propose only patterns that address actual gaps
4. Prioritize by impact: critical > high-value > nice-to-have

**Progressive Reference Loading:**
- Always load: minimal-templates.md
- Load as needed: applicability-guide.md (for tier decisions)
- Load for comprehensive: llmprog.md + pattern-catalog.md (when gaps are complex)

**Structure-Only Improvements:**
Patterns add reliability structure (inputs, contracts, validation) without changing the target's core intent or behavior. When patterns are added, remove any original prose content that duplicates or is superseded by the structured patterns to eliminate redundancy.

**User Approval Gate:**
All pattern applications require explicit user approval before file modification.

**Skill-Specific Context Efficiency:**
Skills use progressive disclosure (metadata → SKILL.md body → bundled resources) and share the context window with system prompts, conversation history, and other skills. When improving skills:
- Keep SKILL.md lean (<500 lines guideline); suggest moving detailed content to references/ when approaching limit
- Skills have scripts/ (executable code), references/ (documentation loaded as needed), assets/ (output files)
- Avoid duplication: content lives in SKILL.md OR references/, not both
- Frontmatter description is the PRIMARY triggering mechanism—must include "when to use" information (body loads AFTER triggering)
- Use imperative/infinitive form when adding content to skills
- Only add context Claude doesn't already have; prefer concise examples over verbose explanations
</essential_principles>

<reference_guides>
**Supporting documentation in references/:**
- [pattern-catalog.md](references/pattern-catalog.md) - Tiered pattern list with applicability
- [minimal-templates.md](references/minimal-templates.md) - Canonical tags + minimal insertion templates
- [llmprog.md](references/llmprog.md) - Full pattern details (22 patterns, ~800 lines)
- [applicability-guide.md](references/applicability-guide.md) - Tier-aware applicability rules + proposal counts
- [integration-guide.md](references/integration-guide.md) - Pattern formatting and ordering
- [examples.md](references/examples.md) - Good/bad integration examples
- [approval-templates.md](references/approval-templates.md) - Presentation formats for user approval
- [output-templates.md](references/output-templates.md) - Report format specifications
- [error-taxonomy.md](references/error-taxonomy.md) - Error codes and recovery (load after Error Handling pattern is approved)
</reference_guides>

<objective>
Improve determinism and debuggability by adding reliability structure (not changing core intent) using gap-driven pattern selection.

**Approach:**
1. **Detect gaps:** Analyze target against tier-appropriate patterns
2. **Assess impact:** Critical gaps (blocks correctness) vs nice-to-have improvements
3. **Propose smartly:**
   - Simple targets with few gaps → propose 2-4 patterns
   - Complex targets with many gaps → propose 4-8 patterns, prioritized
   - Already solid targets → acknowledge + suggest 0-2 optional improvements
4. **Scale analysis:** Load comprehensive references only when gaps warrant deep investigation
</objective>

<interpretation_check>
Before analyzing the target:
- Detect target type (skill vs prompt) from YAML frontmatter
- Assess target complexity (simple/moderate/complex) based on structure
- Restate: "Analyzing {target_path} as {skill|prompt}, {complexity} target"
- List key assumptions: target characteristics, likely gap areas, tier scope
- If target characteristics unclear or ambiguous: ask before proceeding
</interpretation_check>

<inputs_first>
**Required inputs:**
- `target_path`: Path to the prompt or SKILL.md file to improve

**Optional inputs:**
- `max_cycles`: Maximum review-revise cycles (default: 1, upper bound: 5)

**Derived (from reading files):**
- `target_content`
- `target_type`: `skill` (YAML frontmatter with name/description) or `prompt`
- `target_complexity`: `simple` (<100 lines, single-step) | `moderate` (100-300 lines, multi-step) | `complex` (300+ lines, branching logic)
- `existing_patterns`: List of patterns already present
- `gap_severity`: Critical gaps (missing core structure) vs nice-to-have improvements
- `skill_line_count` (skills only): Current SKILL.md line count (warn if approaching 500)
- `has_references_dir` (skills only): Whether references/ directory exists for content offloading
- `description_completeness` (skills only): Does frontmatter description include "when to use" triggers?
</inputs_first>

<scope_fence>
**File scope:**
- Read `target_path`
- Read references as needed based on detected gaps
- Propose patterns and apply only those the user approves

**Pattern selection scope:**

See [pattern-catalog.md](references/pattern-catalog.md) for tier definitions, complete pattern list, and canonical priority ordering.

**Gap-driven proposal guidelines:**
For proposal counts by target complexity (simple/moderate/complex/solid), see [applicability-guide.md](references/applicability-guide.md) § Gap-Driven Selection Flow.

**Tier 1 patterns (consider first):**
See [pattern-catalog.md](references/pattern-catalog.md) § Tier 1 - Core for complete list and applicability criteria. Focus on patterns addressing detected gaps.

**Tier 2/3 patterns:** Propose only when:
- Tier 1 gaps addressed (or present)
- Clear ROI for specific target characteristics
- Tier 3 requires explicit justification

**Out of scope:**
- Changing the target's core intent/behavior (only add reliability structure)
- Editing files other than `target_path`
- Editing reference files
- Proposing 10+ patterns without clear justification
- Proposing patterns that change behavior vs. structure
</scope_fence>

<decision_points>
- Assess target complexity and existing pattern coverage
- If target is already solid (few gaps), propose fewer patterns (0-2 is fine)
- Prefer Tier 1 patterns over Tier 2/3
- Tier 3 patterns require explicit ROI justification based on target characteristics
- If applying a pattern would change behavior (not just structure/format), ask before proposing it
- **Proposal cap:** Generally 2-8 patterns. Beyond 8 requires clear justification
- **Conversational skills:** Flag patterns that check "user engagement" or track state user is actively participating in (poor fit)
- **Length warning:** If pattern additions would increase file length >50%, warn before approval
- **Solid targets:** Don't force improvements. "No critical gaps found" is a valid outcome
- **Skills approaching 500 lines:** Recommend moving detailed content (examples, reference material) to references/ directory instead of adding patterns to SKILL.md
- **Skills with "when to use" sections in body:** Flag as ineffective (body loads AFTER triggering); recommend moving to frontmatter description field
- **Duplicate content in SKILL.md and references/:** Flag violation of anti-duplication rule; recommend consolidating to references/
</decision_points>

<step_contract>
1. Read `target_path`; detect `target_type` and `target_complexity`.
2. Load minimal-templates.md (always needed).
3. Perform gap analysis:
   - Detect existing patterns (Tier 1 → Tier 2 → Tier 3)
   - Identify missing patterns by severity: critical / high-value / nice-to-have
   - Assess applicability based on target characteristics
4. Load additional references if needed (applicability-guide.md for complex gaps).
5. Propose patterns based on gaps (prioritized, with rationale) and wait for approval.
6. Apply approved patterns to the target file.
7. Remove superseded content: Identify and remove original content that duplicates or is made redundant by the newly added patterns. Document removed sections.
8. Review (bounded by `max_cycles`).
9. Report results.
</step_contract>

<quality_gates>
G1 (after Step 3): Are gaps correctly identified and prioritized for the target's characteristics?
G2 (after Step 5): Does proposal address critical gaps without over-engineering (generally 2-8 patterns)?
G3 (after Step 6): Integration introduces no duplicates, conflicts, or broken XML tags?
G4 (after Step 7): Original content that duplicates newly added patterns has been removed?
G5 (after Step 8): All approved patterns present and properly formatted?
G6 (skills only): Content added uses imperative/infinitive form (not past tense or present continuous)?
G7 (skills only): SKILL.md stays under 500 lines OR proposal includes moving content to references/?
If gate fails: fix once → re-check; else ERROR: GATE_FAIL - report issue.

Note: Quality Gates run inline during step execution. Review Step (see <review_step>) runs holistically after all steps complete.
</quality_gates>

<user_approval_gate>
Before editing, require explicit approval using standard gap analysis format.

**Presentation:**
Show gap summary table with tier coverage (present/missing/N/A), categorized recommendations (Critical Gaps, High-Value Improvements, Optional Enhancements), ROI rationale for each pattern, and estimated impact.

For complete presentation templates (standard proposal format and solid target format), see [approval-templates.md](references/approval-templates.md).

**Options:** [approve all] [select subset: P-1, P-3...] [analysis only]

**On analysis only:** produce analysis output; do not modify files.
</user_approval_gate>

<clarifying_questions>
Triggers:
- Pattern applicability uncertain (confidence < 60%)
- Target complexity unclear (ambiguous indicators)
- Tier 3 patterns appear valuable but require validation
- Proposal would include 8+ patterns
Constraints:
- max_per_analysis: 3 (simple targets), 5 (complex targets)
- batch_related: true
- ask_before: proposing Tier 3 patterns or large pattern sets (8+)
Fallback: use conservative approach (prioritize Tier 1, skip uncertain patterns), document in assumptions
</clarifying_questions>

<confidence_signal>
When recommending patterns, indicate confidence and impact:
- **Critical (high confidence + high impact):** Missing pattern blocks correctness/clarity
- **High-value (high confidence + medium impact):** Pattern significantly improves reliability
- **Nice-to-have (medium confidence OR low impact):** Pattern provides incremental benefit
- **Uncertain (<60% confidence):** Ask user before proposing
Include assessment in proposal rationale. Use to prioritize critical > high-value > nice-to-have.
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
- Original content that duplicates newly added patterns has been removed
- No redundant prose instructions remaining that restate structured pattern content
- XML tags balanced (for skills)
- Tier 1 patterns before Tier 2, Tier 2 before Tier 3
- Integration doesn't change target's core intent (see <essential_principles>)
- **Skills only:** Content uses imperative/infinitive form (not declarative or past tense)
- **Skills only:** SKILL.md line count within reasonable bounds (<500 lines guideline)
- **Skills only:** No content duplication between SKILL.md and references/
Max_cycles: default 1 (override via max_cycles input if needed)
Per cycle: identify issues → revise → re-check
Exit: no issues found OR max_cycles reached
If issues remain at max_cycles: report them in output
</review_step>

<assumption_registry>
Log assumptions made during analysis:
- [Target complexity] assumed: "{simple|moderate|complex}" | source: {line_count, step_count, structure} | confidence: {high|medium|low}
- [Gap severity] assumed: "{pattern} gap is {critical|high-value|nice-to-have} because {reason}" | confidence: {level}
- [Pattern applicability] assumed: "{pattern} applicable because {reason}" | confidence: {level}
Validate high-impact + low-confidence assumptions before proposing patterns.
Document all assumptions in analysis output for auditability.
</assumption_registry>

<output_schema>
Return a concise report with:
- Target info (path, type, complexity)
- Gap analysis summary (before/after state for Tier 1 and Tier 2)
- Patterns applied (with addressable IDs and 1-sentence outcomes)
- Superseded content removed (what was cleaned up to eliminate redundancy)
- Patterns deferred (user-rejected patterns)
- Remaining gaps (if any)
- Notes (caveats, follow-ups, observations)

For detailed format specifications (standard report format and analysis-only format), see [output-templates.md](references/output-templates.md).
</output_schema>

<stop_conditions>
Done when:
- All approved patterns integrated into target file
- Superseded/duplicate content removed
- Quality gates pass (G1-G7)
- Review cycle complete (no issues OR max_cycles reached)
- Report generated per output schema
Don't:
- Change target's core intent/behavior (but DO remove content made redundant by pattern additions)
- Add patterns user rejected
- Edit files other than target_path
- Propose 10+ patterns without clear justification (guideline: 2-8)
- Force improvements on solid targets (0-2 optional patterns is valid)
- Apply patterns that check "user engagement" to conversational skills (poor fit)
- **Skills:** Add verbose content when concise examples suffice (context efficiency)
- **Skills:** Push SKILL.md beyond 500 lines without recommending references/ offload
- **Skills:** Duplicate content between SKILL.md and references/
- **Skills:** Add "when to use" sections to body (belongs in frontmatter description)
</stop_conditions>
