---
name: analyze-skill
description: "Comprehensive skill analysis: evaluates value (7-dimension scoring, 0-14 points), explores enhancement opportunities (5 vectors), and generates specifications. Use when: (1) deciding if a skill idea is worth building, (2) auditing existing skills for value and growth potential, (3) exploring what capabilities could be added to a skill, (4) generating specifications for new or enhanced skills. Works for both proposed skills (text descriptions) and existing skills (SKILL.md files)."
---

<objective>
Analyze skills comprehensively: evaluate their value using a 7-dimension framework, explore enhancement opportunities using 5 vectors, and optionally generate specifications. Works for both proposed skills (pre-build decision) and existing skills (post-build analysis).

**Be brutally honest.** Users benefit more from candid assessment than discovering redundancy after building.
</objective>

<quick_start>
**Invocations:**
- `analyze-skill` → interactive mode (will ask for input)
- `analyze-skill "A skill that helps users create morning routines"` → evaluate text description
- `analyze-skill pdf` → analyze existing skill by name
- `analyze-skill /path/to/SKILL.md` → analyze existing skill by path
- `analyze-skill pdf --output enhanced-pdf.skill-design.md` → generate spec file

**Output:** Evaluation report with scoring, verdict, enhancement opportunities, and optional specification
</quick_start>

<inputs_first>
**Required:**
- `skill_input`: One of:
  - Text description of proposed skill (2+ sentences)
  - Name of existing skill (e.g., "pdf", "commit")
  - Path to SKILL.md file

**Optional flags:**
- `--output <path>`: Auto-save design file to specified path
- `--enhancements-only`: Skip evaluation, focus on enhancement exploration (existing skills only)

**Input type detection:**
- Contains ".md" or "/" → file path
- Single word or hyphenated → skill name (resolve to ~/.claude/skills/{name}/SKILL.md)
- Otherwise → text description

**Validation:**
- If text description: must include what it does; ask for clarification if vague
- If path/name: file must exist and be readable
</inputs_first>

<step_contract>
**For text descriptions (proposed skills):**
1. Parse input → identify as text description
2. Reformulate understanding → present to user, wait for confirmation
3. Evaluate 7 dimensions → score each 0/1/2 with rationale
4. Calculate total & verdict → sum scores, determine Build/Consider/Skip
5. Generate initial improvements → suggestions to increase score
6. Compile report → scoring table, verdict, recommendations
7. Review → verify arithmetic, consistency
8. Enhancement exploration → systematic exploration using 5 vectors [if verdict ≠ skip]
9. Offer specification → interactive or auto-save if --output [if verdict ≠ skip]

**For existing skills (SKILL.md):**
1. Parse input → identify as path/name, load file
2. Skip confirmation → content is unambiguous
3-7. Same as above (evaluate current state)
8. Enhanced exploration → more thorough analysis using 5 vectors + existing capability mapping
9. Same as above

**For --enhancements-only mode:**
1. Load existing skill
2. Map current capabilities
3. Run 5-vector enhancement exploration
4. Present ranked enhancements
5. Offer specification for selected enhancements
</step_contract>

<decision_points>
**D1: Input routing**
- Text description → Steps 1-9 with confirmation at Step 2
- Existing skill → Steps 1-9, skip Step 2 confirmation
- --enhancements-only → abbreviated flow (exploration only)

**D2: Verdict-based flow**
- Skip (0-5) → Stop after Step 7, include improvement suggestions
- Consider (6-9) → Continue to Steps 8-9
- Build (10-14) → Continue to Steps 8-9

**D3: Enhancement depth**
- New skills → focus on "what would increase score"
- Existing skills → comprehensive capability expansion using all 5 vectors

**D4: Specification generation**
- --output flag → auto-generate, validate, save, exit
- No flag → offer interactively, let user select enhancements to include
</decision_points>

<evaluation_framework>
**7 Dimensions (0-2 points each, 0-14 total):**

| Dimension | Question |
|-----------|----------|
| D-1: Knowledge Gap | Contains specialized knowledge Claude doesn't have? |
| D-2: Structural Process | Provides systematic, repeatable process Claude applies inconsistently? |
| D-3: Tool Integration | Orchestrates tools/systems Claude can't access directly? |
| D-4: Consistency & Guardrails | Enforces discipline Claude might skip? |
| D-5: Complexity Management | Manages complexity that overwhelms Claude's context? |
| D-6: User Experience | Significantly reduces friction/cognitive load? |
| D-7: Specialization Depth | Goes significantly deeper than Claude's general knowledge? |

**Scoring:** 2=strong yes, 1=partial, 0=no
**Verdicts:** 10-14=Build, 6-9=Consider, 0-5=Skip

See references/evaluation-framework.md for complete scoring criteria.

**Acid Test:** "Could I get 80% of this value by just asking Claude directly?"
- YES → Likely redundant
- NO → Genuinely new capability
</evaluation_framework>

<enhancement_vectors>
**5 Vectors for systematic exploration:**

| Vector | Focus |
|--------|-------|
| V-1: Input/Output | Additional formats, batch processing, streaming |
| V-2: Tool Integration | MCP servers, APIs, cross-skill orchestration |
| V-3: Intelligence Layer | Validation, recommendations, auto-correction, learning |
| V-4: User Experience | Interactive modes, progress tracking, customization |
| V-5: Domain Depth | Advanced techniques, edge cases, professional features |

**For each enhancement found:**
- Map to dimension impact: (↑ D-X by +N)
- Assess effort: Low/Medium/High
- Calculate priority: impact × feasibility

See references/enhancement-vectors.md for detailed exploration prompts.
</enhancement_vectors>

<quality_gates>
**G1 (after Step 3):** All 7 dimensions scored (0/1/2) with rationales
**G2 (after Step 4):** Total = sum of dimensions; verdict matches score range
**G3 (after Step 6):** All required sections present; improvements use [I-N] format
**G4 (after Step 7):** Arithmetic correct; rationales don't contradict
**G5 (after Step 8):** If verdict ≠ skip, enhancements explored using all 5 vectors
**G6 (if spec generated):** Format valid; metadata correct; sections complete; length <300 lines (warn >300, fail >500); focuses on WHAT not HOW; doesn't duplicate large portions of original artifact

If gate fails → fix once → re-check; else report issue
</quality_gates>

<output_schema>
**Format:** Markdown in chat + optional specification file

**Required sections:**
1. **Skill Understanding** (text descriptions only, separate interaction)
2. **Scoring Table** (all 7 dimensions with [D-N] labels)
3. **Total Score** (0-14)
4. **Verdict** (Build/Consider/Skip with emoticon)
5. **Recommendation** (clear guidance)
6. **Initial Improvements** ([I-N] format if score < 12)
7. **Strategic Considerations**
8. **Enhancement Opportunities** (5-vector exploration, required if verdict ≠ skip)
9. **Specification Offer** (required if verdict ≠ skip)

**Enhancement format:**
```
[I-N] Enhancement Name (Vector) [Priority]
- What: Brief description
- Impact: (↑ D-X by +N, ↑ D-Y by +N)
- Effort: Low/Medium/High
- Rationale: Why valuable
```

**Specification file format:** {skill-name}.skill-design.md
- Metadata: Created by, timestamp, score, verdict
- Evaluation summary
- Complete specification (Goal, Happy Path, Inputs, Constraints, etc.)

**Specification quality guidelines:**
- Target length: <300 lines (warn if >300, critical if >500)
- Focus on WHAT changes: capabilities added, behavior, key decisions, examples
- NOT HOW to implement: avoid inline modifications, implementation details, duplicating original artifact
- Design specification vs implementation plan: describe the enhancement, don't rewrite the skill
</output_schema>

<addressable_output>
**ID formats:**
- `[D-N]`: Dimensions (D-1 through D-7)
- `[I-N]`: Improvements/Enhancements (sequential)

**Usage:** User can reference in follow-up:
- "explain D-3 scoring"
- "include I-1, I-3, I-5 in specification"
- "what would I-2 require to implement?"
</addressable_output>

<interpretation_check>
**For text descriptions only:**
- Reformulate understanding in own words
- Present inferred: name, purpose, likely features
- Ask: "Is this understanding correct?"
- Wait for confirmation before evaluating

**For existing skills:** Skip (content is unambiguous)
</interpretation_check>

<clarifying_questions>
**Triggers:**
- Text description lacks key details (what, for whom, inputs/outputs)
- Confidence < 60% on dimension that would change verdict
- Multiple plausible interpretations

**Constraints:**
- max_per_phase: 3
- batch_related: true

**Fallback:** Use safe defaults, document assumptions in rationales
</clarifying_questions>

<confidence_signal>
**Thresholds:**
- High (>85%): Proceed with scoring
- Medium (60-85%): Proceed but flag uncertainty in rationale
- Low (<60%): Ask clarifying question before scoring

**Include when confidence < 85%:**
- Confidence level + reason
- What would increase confidence
- Safe default used
</confidence_signal>

<scope_fence>
**In scope:**
- Evaluating skill value (7 dimensions)
- Exploring enhancements (5 vectors)
- Generating specifications
- Reading skill files

**Out of scope:**
- Writing SKILL.md implementation code
- Modifying existing skill files
- Comparing to other skills in ecosystem
- Making strategic decisions for user

**If boundary crossed:** Note the desire, return to analysis mode
</scope_fence>

<review_step>
**After Step 6, before delivery:**
- Arithmetic: total = sum of 7 dimensions
- Alignment: verdict matches score range
- Consistency: rationales don't contradict
- Completeness: all required sections present

**Max cycles:** 1
</review_step>

<stop_conditions>
**Complete when:**
- All quality gates passed
- Required steps executed based on verdict
- If verdict ≠ skip: Steps 8-9 executed

**Never:**
- Skip confirmation for text descriptions
- Skip Steps 8-9 when verdict is Build/Consider
- Provide implementation code in evaluation report
</stop_conditions>

<reference_index>
**Reference files (load as needed):**
- references/evaluation-framework.md - Full 0/1/2 criteria for each dimension
- references/enhancement-vectors.md - Detailed exploration prompts for 5 vectors
- references/improvement-patterns.md - Common improvement patterns and ranking
- references/examples.md - Complete workflow examples
</reference_index>
