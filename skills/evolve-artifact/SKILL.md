---
name: evolve-artifact
description: "Evolves SKILL.md files and prompt .md files by analyzing conversation context to extract learnings and generate type-appropriate recommendations. Automatically detects artifact type (skill vs prompt) and applies relevant quality standards. Use when (1) a skill or prompt was just used in conversation and evolutions are evident, (2) conversation revealed gaps or issues in an artifact, (3) user requests evolutions to a .md file, (4) consolidating learnings after testing a skill or prompt, or (5) any artifact evolution task. Supports both single-artifact and multi-artifact evolution workflows. Trigger keywords: learnings, review, improve, refine, iterate, post-execution, retrospective, feedback, enhancement opportunities."
---

Analyze the current conversation to identify learnings and actionable recommendations for evolving a specific skill or prompt. Automatically detects artifact type and applies appropriate quality standards.

<interpretation_check>
Before starting, output to user:
- Restate the target artifact(s) in your own words
- Note if artifact type will be auto-detected or is already known
- List key assumptions (if any)
- If anything is ambiguous, ask the user to confirm before proceeding

This output demonstrates understanding and catches misalignment early.
</interpretation_check>

<type_detection>
1. Read first 10 lines of target file
2. Check for YAML frontmatter (starts with `---` on line 1)
3. If frontmatter present with both `name:` AND `description:` fields → **skill**
4. If frontmatter has only `name` OR only `description` (not both) → **prompt**
5. If no frontmatter → **prompt**
6. Report detected type before proceeding
7. Apply appropriate quality lens for analysis
</type_detection>

<inputs_first>
**Required:**
- `conversation_context`: Current session history (automatic)
- `target_artifact`: Artifact to evolve (infer from context if not specified)

**Derived (automatic):**
- `artifact_type`: Determined by type_detection logic (skill or prompt)

**Validation:**
- Conversation contains artifact execution or discussion
- Target file exists and is readable
</inputs_first>



<step_contract>
1. **Output interpretation check** → restate target artifact, note detection approach, list assumptions; then read target artifact → detect type via type_detection, understand current structure, identify existing patterns
2. **Analyze conversation:**
   a. Apply each lens (Correctness, Integration, ROI, Type-specific, Pattern, Artifact Consistency) → report findings with severity
   b. Check pattern detection signals from pattern-catalog.md
   c. Extract learnings (L-n) with evidence
   d. Formulate using artifact's domain concepts (general pattern, not one-off)
3. **Generate recommendations** (R-n) → apply standards from references/, cross-check against current artifact, include confidence for consequential changes; consolidate redundant recommendations that address same issue
4. **Present proposal** → show Learnings (with severity), Recommendations (with impact/risk/standard/confidence), Filtered items
5. **User approval gate** → [approve all] | [select subset: R-1, R-3...] | [analysis only]
6. **Apply chosen recommendations** → edit artifact file (skip if analysis only)
7. **Verify** → two-tier review (structural + semantic + pattern structure)
</step_contract>

<decision_points>
**Common (all artifacts):**
- No learnings found → report "no significant learnings identified"
- Learning is vague/generic → exclude (e.g., "artifact worked well")
- Learning has no actionable recommendation → flag as "observation only"
- Recommendation already exists in artifact → mark as "already addressed" in Filtered
- User selects subset → apply only selected recommendation IDs
- User selects none → analysis only, no file edits
- Record applied recommendations + filtered-out learnings in output

**Type-specific:**
- Artifact type unclear during detection → use detection rules, document in interpretation_check
- Multiple artifacts provided → process sequentially, present consolidated proposal
- Skill exceeds 500 lines → add recommendation [R-X] suggesting split strategy per § Split Strategy from quality-standards.md
- Prompt exceeds 300 lines → flag as MEDIUM severity in learnings, over 500 → flag as HIGH severity
</decision_points>

<output_schema>
**Format:** Markdown in chat (not file)

**ID Format:** See <addressable_output> for format specification

**Sections:**

*Learnings:*
- Bulleted list with [L-n] IDs
- Format: `[L-1] {observation} | evidence: {from conversation} | severity: CRITICAL/MAJOR/MINOR`
- Severity: CRITICAL (blocks execution), MAJOR (degrades reliability), MINOR (enhancement)

*Recommendations:*
- Bulleted list with [R-n] IDs
- Format: `[R-1] {action} | addresses: L-1 | impact: high/medium/low | risk: safe/moderate/breaking | standard: {quality criterion} | confidence: high (90%) - {reason}`
- Each maps to at least one learning
- Standard: cite criterion from references/ (e.g., "Description Field Excellence", "Heading Structure")
- Risk: safe (additive) | moderate (modifies) | breaking (removes/changes behavior)
- Confidence: required for consequential recommendations (CRITICAL/MAJOR severity or breaking/moderate risk); format per <confidence_signal> thresholds with percentage and reason

*Filtered:*
- Learnings excluded and why (transparency)
- Format: `[L-X] {exact observation text from analysis} | reason: {why excluded}`
- Use exact learning text that was filtered during G1, not reworded summaries

*Applied (after user approval):*
- List of applied recommendation IDs
- Format: `Applied: R-1, R-3`

**Validation:**
- All IDs unique
- All recommendations traceable to learnings
- No vague learnings without evidence
- No recommendations without learning linkage
- All learnings have severity assigned
- All recommendations have impact, risk, and standard assigned
</output_schema>

<confidence_signal>
**Thresholds:**
- high (>85%): proceed with recommendation
- medium (60-85%): proceed but flag for user review
- low (<60%): ask user before proposing recommendation

**What to include:** confidence assessment + reason + what would increase confidence

**Apply to:** consequential recommendations (CRITICAL/MAJOR severity, breaking/moderate risk)

**Factors affecting confidence:**
- Evidence quality: Multiple concrete examples from conversation (↑) vs single vague observation (↓)
- Artifact complexity: Simple, well-structured artifact (↑) vs complex, intertwined sections (↓)
- Change scope: Localized fix (↑) vs system-wide restructuring (↓)
- Standard alignment: Change directly addresses documented quality standard (↑) vs interpretation required (↓)
</confidence_signal>

<user_approval_gate>
Before Step 6 (applying recommendations):
- Present complete proposal with Learnings, Recommendations, Filtered sections
- Show: severity levels, impact/risk assessments, quality standards cited, addressable IDs
- Prompt user with options:
  - `[approve all]` → apply all recommendations
  - `[select subset: R-1, R-3...]` → apply only specified IDs
  - `[analysis only]` → skip Steps 6-7, no file edits

On approval: proceed to Step 6
On subset selection: apply only selected recommendation IDs
On analysis only: skip Steps 6-7, report analysis results without file edits
</user_approval_gate>

<clarifying_questions>
**Triggers:**
- Target artifact unclear (multiple artifacts in conversation)
- Learning ambiguous (insufficient evidence)
- Recommendation has multiple valid approaches
- Confidence < 60% on consequential change
- Artifact type cannot be determined automatically (edge case)

**Constraints:**
- max_per_phase: 3
- batch_related: true

**Fallback:**
- Use conservative interpretation
- Document assumption in [Filtered] section
</clarifying_questions>

<stop_conditions>
**Done when:**
- All chosen recommendations applied to target artifact file (or analysis only mode)
- Review step passes (no syntax errors, coherent integration)
- Summary report generated with applied IDs

**Don't:**
- Apply recommendations user didn't approve
- Modify files other than target artifact
- Change artifact's core intent (only structural/quality evolutions)
- Add features beyond fixing identified issues
</stop_conditions>

<quality_gates>
**G1 (after Step 2): Learnings meet quality criteria?**
- Actionable: can translate to specific artifact changes (exclude vague/generic observations like "worked well", "successful execution")
- Appropriate generality: formulated using artifact's domain concepts (general enough to cover all instances, specific enough to be actionable)
- Evidence-based: grounded in actual execution behavior
- Artifact-relevant: directly relates to artifact's scope/behavior
- Pattern-revealing: systematic issues, not one-off quirks
- Severity assigned: CRITICAL | MAJOR | MINOR
- Type-specific lens applied correctly (skills vs prompts)?
- Learnings map to appropriate quality standards?
- Apply decision_points filtering: exclude learnings without actionable recommendations, mark "observation only" if no action possible

**G2 (after Step 3): Recommendations validated?**
- Not duplicate (already exists in artifact or within current recommendation set)
- Compatible (doesn't conflict with existing patterns)
- Consistent with artifact's documented content
- Impact/risk assessed
- Standard cited from quality_criteria
- Type-specific evolutions (skill description vs prompt headings)?

**G3 (after Step 4): Traceability check**
- Each recommendation maps to at least one learning
- Each recommendation cites a quality standard
- All severity/impact/risk assignments present

**G4 (after Step 7): Applied changes validated?**
- See <review_step> for detailed validation method (structural + semantic + pattern structure)
- Verify all quality checks pass before completion

If gate fails: filter out low-quality items → re-check; else report issue
</quality_gates>

<lens>
Analyze findings from multiple perspectives:

**Correctness lens:**
- Does this pattern/directive prevent errors or ambiguity?
- Report findings + severity (critical/major/minor)

**Integration lens:**
- Conflicts with existing structure? Duplicates content?
- Report findings + severity (critical/major/minor)

**ROI lens:**
- Does value (reliability gain) exceed overhead (token cost)?
- Report findings + severity (critical/major/minor)

**Type-specific lens:**
- Skills: Description quality? Progressive disclosure? Resource organization?
- Prompts: Heading structure? Section order? Content efficiency?
- Report findings + severity (critical/major/minor)

**Pattern lens:**
- Missing patterns? Tier 1 gaps (inputs, steps, gates, scope)? Tier 2 opportunities (approval, review)?
- Broken/incomplete patterns? Quality gates without validation? Steps without sequence?
- Pattern-revealed issues from conversation? "Failed silently" → missing gates?
- Report findings + severity (critical/major/minor)

**Artifact Consistency lens:**
- Do recommendations contradict artifact's documented content?
- Report findings + severity (critical/major/minor)

**Synthesis:**
- Merge findings across lenses
- Flag conflicts (e.g., high correctness but low ROI, or recommendation contradicts artifact)

**Coverage requirement:**
- Each lens must report for critical issues and contradictions

**Pattern detection checklist (use with Pattern lens):**
- Load pattern-catalog.md references/
- Check conversation for detection signals: "failed silently" → missing Quality Gates, "asked what inputs needed" → missing Inputs First, "kept going beyond purpose" → missing Stop Conditions, etc.
- Cross-reference target artifact structure against Tier 1 patterns (always consider), Tier 2 (when warranted), Tier 3 (situational)
- Report missing or broken patterns with tier + purpose

Report findings from each lens with severity before extracting learnings. Findings inform learning formulation.
</lens>


<scope_fence>
**In scope:**
- Analyze current conversation context
- Propose evolutions to specific artifact's structure and quality
- Apply approved changes to artifact file only
- Support both skills and prompts with type-appropriate standards

**Minimalism principle:**
- Recommendations should be minimal effective changes: one location per issue, general form preferred over enumeration, consolidate redundant solutions

**Permitted changes:**
- Add/evolve YAML frontmatter fields (skills)
- Restructure sections for clarity
- Add missing quality gates or contracts
- Split content to references/ (skills)
- Fix heading hierarchy (prompts)
- Consolidate redundant content
- Remove dead references

**Out of scope:**
- Evolving multiple artifacts in one pass (process sequentially)
- Modifying reference files or scripts directly
- Changing artifact behavior vs structure
- Adding new features or capabilities
- Modifying files outside artifact directory

**If unclear which artifact to evolve:** ask user to specify
</scope_fence>

<review_step>
**Tier 1 - Structural (max_cycles: 1):**
- Applied changes don't break XML tags or markdown structure
- Type-specific structure valid:
  - Skills: YAML frontmatter has name + description
  - Prompts: H1 used exactly once, no level skips in headings
- Recommendations integrate coherently with existing content
- No duplicate sections added

**Tier 2 - Semantic (after tier_1 passes):**
- Each applied recommendation addresses its linked learning
- If learning was about a failure, verify fix would prevent it
- New patterns align with existing artifact conventions
- No regressions introduced (existing patterns still work)
- Completeness: all parts of recommendation implemented

**Validation method:**
- Cross-reference: [R-1] addresses [L-1]? Check that L-1's problem is solved
- Consistency: new quality gates follow same format as other patterns
- Simulation: if L-1 was "failed when X", would new code catch X?
- Type-specific checks: YAML valid for skills, headings correct for prompts

**Exit conditions:**
- tier_1 passes + tier_2 passes → done
- max_cycles: 2 (1 fix attempt per tier)
- If issues remain: report in summary with recommendation status

**Output in summary:**
- verification_results: `[R-1] ✓ solves L-1 | [R-2] ⚠️ partial (needs X)`
</review_step>

<addressable_output>
Assign unique IDs to output items for follow-up reference:

**Format:** `[{prefix}-{number}]`

**Prefixes:**
- Learnings = L (e.g., [L-1], [L-2])
- Recommendations = R (e.g., [R-1], [R-2])

**Presentation:** Inline in proposal sections (Learnings, Recommendations, Filtered)

**Usage:** User can reference specific items:
- "apply R-1 and R-3" → execute only those recommendations
- "explain L-2" → provide more context on that learning
- "skip R-4" → exclude from application

**Maintain sequence:** IDs persist across proposal and application phases for traceability
</addressable_output>

<multi_artifact_handling>
**When multiple artifacts provided:**
- Process sequentially (detect type for each)
- Present consolidated proposal with artifact-specific sections:
  ```
  ## Analysis: skill-name (type: skill)
  ### Learnings
  [L-1] ...
  ### Recommendations
  [R-1] ...

  ## Analysis: prompt-name (type: prompt)
  ### Learnings
  [L-2] ...
  ### Recommendations
  [R-2] ...
  ```
- Allow selective approval: "R-1, R-3 for skill-X; R-2 for prompt-Y"
- Apply changes sequentially per artifact
- Verify each artifact independently
</multi_artifact_handling>

<reference_index>
**Reference files (load as needed):**

- **references/quality-standards.md**: Detailed quality criteria for skills and prompts (SHARED, SKILLS ONLY, PROMPTS ONLY sections)
  - Load when: Assessing quality aspects, applying quality standards, evaluating description/structure

- **references/pattern-catalog.md**: Complete pattern tier definitions (Tier 1/2/3) and detection signals
  - Load when: Detecting pattern gaps, analyzing broken patterns, checking conversation for pattern-revealed issues

- **references/learning-examples.md**: Example learning formulations showing good vs bad patterns
  - Load when: Formulating learnings, validating learning quality, ensuring appropriate generality

**Navigation:** Load references during Step 2 (conversation analysis) as needed per lens analysis
</reference_index>
