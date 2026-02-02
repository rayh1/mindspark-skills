---
name: assess-skill-value
description: Evaluates whether a proposed skill is worth building using a 7-dimension scoring framework (0-14 points). Provides build/skip recommendation, improvement suggestions ranked by value, and optionally generates a complete specification document. Use when: (1) planning to create a new skill and need objective assessment, (2) questioning if a skill idea adds genuine value beyond Claude's base capabilities, (3) auditing existing skills for redundancy, or (4) applying systematic evaluation to prevent building superfluous capabilities.
---

<objective>
Determine if a skill adds genuine new capabilities to Claude or is redundant with existing Claude knowledge. Apply systematic evaluation to prevent building superfluous skills. **Be brutally honest** in assessments—it's better to discourage a weak skill than waste effort on redundant capabilities.
</objective>

<quick_start>
**Input:** Skill description, skill name, or path to SKILL.md file
**Output:** Evaluation report with 7-dimension scoring, verdict, improvement suggestions, and optional specification document

**Example invocations:**
- `assess-skill-value` → will ask for skill input
- `assess-skill-value "A skill that helps users create morning routines"`
- `assess-skill-value pdf`
- `assess-skill-value /workspace/.claude/skills/tarot-card-generator/SKILL.md`
</quick_start>

<inputs_first>
**Required inputs:**
- `skill_input`: Either:
  - Text description of a proposed skill (2+ sentences)
  - Name of an existing skill (e.g., "pdf", "commit")
  - Path to a SKILL.md file

**Validation:**
- `skill_input` must be non-empty
- If text description: must include what it does, for whom, and what inputs/outputs it uses
- If path: file must exist and be readable

**Missing input handling:**
- If `skill_input` missing → ask user: "Please provide either a skill description, skill name, or path to a SKILL.md file"
- If description is vague or lacks key details (what it does, for whom, with what inputs) → ask user for clarification

**Input type detection:**
- If contains ".md" or "/" → treat as file path (e.g., "/path/to/file.md")
- If single word or hyphenated name → treat as skill name (e.g., "pdf", "assess-skill-value")
- Otherwise → treat as text description
</inputs_first>

<step_contract>
1. **Parse input** → Identify input type (description/name/path) and extract/load skill content
   - If text description → store as-is
   - If file path → Read with Read tool
   - If skill name → Search `.claude/skills/{name}/SKILL.md` with Glob tool
     - If not found → Ask user: "Skill not found at `.claude/skills/{name}/SKILL.md`. Please provide either a full path or a text description."
2. **Validate completeness** (if description) → Reformulate understanding, present to user, wait for confirmation
3. **Evaluate all 7 dimensions** → Assign 0/1/2 score + rationale for each dimension (see references/evaluation-framework.md)
4. **Calculate total & interpret** → Sum scores (0-14), determine verdict (Build/Consider/Skip)
5. **Generate improvements** → If total score < 12, generate concrete suggestions to increase value. If score 12-14, include brief note: "Score already high; only minor refinements possible."
6. **Compile report** → Produce evaluation report with required sections
   → If verdict = "skip" → STOP after Step 7
   → If verdict ≠ "skip" → CONTINUE to Step 8
7. **Review** → Check consistency, arithmetic, alignment before delivery
8. **Show additional improvements** → Present ranked list of features and improvements by value impact [REQUIRED if verdict ≠ skip]
   → CONTINUE to Step 9
9. **Offer specification generation** → Ask user if they want a complete specification document [REQUIRED if verdict ≠ skip]
</step_contract>

<decision_points>
**D1: Input type routing (after Step 1)**
- Follow input type detection logic defined in inputs_first section
- If skill name not found → ask user for clarification or full path

**D2: Completeness validation (after Step 1)**
- If text description AND lacks key details (what it does, for whom, with what inputs) → ask user for more detail
- Else if text description AND complete → reformulate understanding, show to user, wait for confirmation before proceeding
- Else (existing skill file/name) → skip interpretation check (content is unambiguous); proceed to Step 3 (evaluation)

**D3: Improvement section inclusion (after Step 5)**
- If total score < 12 → include "Improvement Suggestions" section with concrete suggestions
- If score 12-14 → include brief note: "Score already high; only minor refinements possible"

**D4: Additional improvements and specification offer (after Step 7)**
- If verdict = "skip" → include initial improvement suggestions (Step 5) in report, STOP after Step 7
- If verdict ≠ skip (verdict = "build" or "consider") → MUST proceed to Steps 8-9 (additional improvements + specification offer)
</decision_points>

<quality_gates>
**G1 (after Step 3 - Evaluate dimensions):**
- All 7 dimensions must be scored
- Each score must be exactly 0, 1, or 2
- Each dimension must have a rationale
- If fail → Re-evaluate missing/invalid dimensions

**G2 (after Step 4 - Calculate total):**
- Total score must equal sum of 7 individual scores
- Verdict must match interpretation rules (see references/evaluation-framework.md)
- If fail → Recalculate

**G3 (after Step 6 - Compile report):**
- All required sections present
- Scoring table includes all 7 dimensions
- All improvement suggestions use [I-N] format per addressable_output section
- If fail → Add missing sections

**G3.5 (after Step 7 - Check next steps):**
- If verdict ≠ skip → must proceed to Steps 8-9
- If verdict = skip → may stop after Step 7
- If fail → Execute remaining required steps

**G4 (after Step 8 - Additional improvements):**
- If verdict ≠ skip, verify additional improvements section present with ≥1 improvement ranked by value impact
- All additional improvements use [I-N] format
- If fail → Generate missing improvements

**G5 (after Step 9 - Specification offer):**
- If verdict ≠ skip, verify specification offer was made
- If fail → Make specification offer
</quality_gates>

<scope_fence>
**In scope:**
- Reading and analyzing target skill/prompt descriptions
- Evaluating against 7 dimensions using framework
- Generating evaluation reports with scores and recommendations
- Proposing improvement suggestions (initial and additional)
- Creating specification documents for "build" or "consider" verdicts
- Including implementation notes in specifications (architecture, tools, patterns)

**Out of scope:**
- Writing actual SKILL.md implementation code
- Writing detailed step-by-step implementation instructions
- Modifying existing skill files
- Comparing to other skills in ecosystem
- Making strategic decisions about whether user should build it

**Clarification on implementation details:**
- Evaluation report: No implementation code or detailed steps
- Specification document: Implementation notes (architectural guidance, tool recommendations) encouraged

**If boundary crossed:** Note the desire, return to pure evaluation mode.
</scope_fence>

<interpretation_check>
**For text descriptions only:**
- Reformulate understanding in own words
- Present inferred name, purpose, and likely features
- Ask: "Is this understanding correct?"
- Wait for confirmation before proceeding to evaluation
- If corrected: update understanding and re-confirm if significant

**For existing skills/files:**
- Skip explicit check (content is unambiguous)
- Proceed directly to evaluation
</interpretation_check>

<Evaluation Framework>
See references/evaluation-framework.md for the complete 7-dimension scoring framework, criteria, and evaluation guidelines.
</evaluation_framework>

<clarifying_questions>
Triggers:
- If text description is consequential but underspecified (who is user, what inputs/outputs, what tools?)
- If confidence < 60% on a scoring decision that would materially change the verdict (i.e., would push score across a verdict boundary: 5/6 or 9/10)
- If multiple plausible interpretations exist for the proposed skill

Constraints:
- max_per_phase: 3
- batch_related: true

Triage rule (if >3 dimensions have low confidence):
- Prioritize questions for dimensions that would change the verdict (near boundary scores)
- For medium confidence (60-85%), proceed with documented uncertainty rather than asking

Fallback:
- use safe defaults and document assumptions in rationales
</clarifying_questions>

<confidence_signal>
Apply confidence assessment to all dimension scoring decisions:

**Thresholds:**
- **High (>85%):** Proceed with scoring; dimension rationale is clear
- **Medium (60-85%):** Proceed but flag uncertainty in rationale; note what would increase confidence
- **Low (<60%):** Ask clarifying question before scoring this dimension (subject to max_per_phase constraint)

**Include in dimension rationales when confidence < 85%:**
- Confidence level + reason for uncertainty
- What information would resolve uncertainty
- Safe default used if proceeding without clarification

**Example:** "Score: 1 point (confidence: 70% - unclear if skill orchestrates MCP tools or just suggests tool use; assuming basic integration)"
</confidence_signal>

<review_step>
**After Step 6 (Compile report), before delivery:**

Check for:
- **Arithmetic:** Total score equals sum of 7 dimensions
- **Alignment:** Verdict matches score range (10-14=Build, 6-9=Consider, 0-5=Skip)
- **Consistency:** Rationales don't contradict each other
- **Completeness:** All required sections present
- **Quality:** Each rationale explains the "why" not just repeats the score

**Max cycles:** 1
**If issues found:** Fix once and re-check. If still failing, note limitation in output.
</review_step>

<output_schema>
**Format:** Conversation output (markdown formatted) + optional specification file

**Required sections (in order):**
1. **Skill Understanding** (only for text descriptions; separate interaction, wait for confirmation)
2. **Scoring Table** (all 7 dimensions labeled [D-1] through [D-7] with scores and rationales)
3. **Total Score** (sum of all dimensions, 0-14)
4. **Interpretation & Verdict** (Build ✅ / Consider ⚠️ / Skip ❌)
5. **Recommendation** (clear guidance)
6. **Improvement Suggestions** (if score < 12 - concrete suggestions with [I-N] format; if score 12-14 - brief note about high score)
7. **Strategic Considerations** (meta-value, ecosystem fit)
8. **Additional Improvement Opportunities** (required if verdict ≠ skip; ranked by value with [I-N] format)
9. **Specification Offer** (required if verdict ≠ skip; wait for user response)

**Optional artifact:**
- **Specification Document** (markdown file generated if user requests it)
  - Saved to scratchpad directory
  - Contains: overview, objective, capabilities, inputs, outputs, process, quality gates, features, edge cases, examples, constraints, implementation notes
  - Only relevant sections included

**Forbidden sections:**
- Comparison to existing skills
- Future roadmap
- Implementation code or detailed implementation steps in the evaluation report (implementation notes allowed in specification document)

**Validation:**
- All 7 dimensions evaluated
- Each dimension scored 0, 1, or 2
- Total equals sum of dimensions
- Verdict matches score interpretation
- Steps 8-9 completed if verdict ≠ skip
</output_schema>

<addressable_output>
**ID format:** `[{prefix}-{number}]`

**Prefixes:**
- `D`: Dimensions (D-1 through D-7 in scoring table)
- `I`: Improvement suggestions (I-1, I-2, etc. in improvement sections)
- `S`: Specification sections (if specification document generated)

**Usage:**
- Dimensions already use [D-1] through [D-7] in output
- Apply [I-N] to each improvement suggestion in Steps 5 and 8
- User can reference IDs in follow-up: "apply I-2 and I-5 to specification", "explain D-3 scoring"
- In specification generation (Step 9), user can select improvements by ID

**Example improvement format:**
```
**Additional Improvement Opportunities** (ranked by value impact):

[I-1] Add MCP integration for Joplin (↑ D-3 by +1, D-6 by +1)
- Rationale: ...

[I-2] Enforce 5-step workflow with quality gates (↑ D-2 by +1, D-4 by +1)
- Rationale: ...
```
</addressable_output>

**See references/improvement-patterns.md for guidance on generating and ranking improvement suggestions.**

**See references/specification-template.md for specification document structure and generation guidance.**

<stop_conditions>
**STOP immediately if:**
- Verdict = "skip" AND Steps 1-7 completed
- Verdict ≠ "skip" AND Steps 1-9 completed
- User declines/completes specification generation

**DO NOT STOP if:**
- Verdict = "build" or "consider" but Steps 8-9 not completed
- Any quality gates (G1-G5) have failed

**Done when:**
- All output_schema requirements met
- All quality_gates (G1-G5) passed
- Steps 8-9 completed if verdict ≠ skip (validated by G3.5, G4, G5)
- Specification document generated if user requested

**Don't:**
- Compare to existing skills in the ecosystem
- Provide implementation details in the evaluation report (allowed in specification document)
- Skip the confirmation step for text descriptions
- Skip steps 8-9 if verdict is "build" or "consider"
</stop_conditions>

<references>
**See also:**
- **references/examples.md** — Three full worked examples demonstrating the complete evaluation flow
- **references/improvement-patterns.md** — Guidance on generating and ranking improvement suggestions
- **references/specification-template.md** — Template structure for generating specification documents
</references>
