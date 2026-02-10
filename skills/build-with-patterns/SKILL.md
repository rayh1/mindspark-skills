---
name: build-with-patterns
description: Builds reliable skills and prompts using spec-by-tightening methodology - start with one concrete example, then tighten into a small testable output contract and executable step sequence. Use when (1) Creating new Claude Code skills (SKILL.md with YAML frontmatter and XML structure), (2) Building standalone prompts with markdown headings, (3) Applying structured prompt engineering with output contracts and quality gates, (4) Need guidance on progressive disclosure, step sequences, and validation patterns, or any other skill/prompt development task following reliability best practices.
---

<objective>
Build reliable skills or prompts by starting from one concrete happy-path example, then tightening into a small, testable output contract and an executable step sequence.
</objective>

<essential_principles>
**How this skill works**

This skill guides you through building reliable skills and prompts by starting with one concrete example and tightening it into a small, testable contract.

Use plain language with users.

Note: XML tags in this file are **skill-spec structure** (to keep instructions checkable).
- This skill will always run an internal approval checkpoint before finalizing.
- Only add a `<user_approval_gate>` section to the *generated artifact* when the trigger in `references/patterns-when-needed.md` is met (destructive/irreversible or out-of-scope file touches).
Generated artifacts should include only the sections required by the chosen format and the user’s request.
</essential_principles>

<inputs_first>
**Input modes:**

build-with-patterns supports two ways to provide inputs:

1. **Interactive mode** (default):
   ```bash
   build-with-patterns skill
   ```
   Asks questions conversationally. No file required.

2. **Design file mode**:
   ```bash
   build-with-patterns --from-design <file>.skill-design.md
   ```
   Reads markdown design file. File can be from assess-skill-value or manually created.

**Required inputs (any mode):**
- `target_type`: skill | prompt
- `goal`: what this should do (1–2 sentences)
- `happy_path_example`: Input → Output → what "good" looks like
- `artifacts`: files to create/modify + required/forbidden sections

**Optional inputs:**
- `constraints`: hard constraints (length, tone, safety)
- `non_goals`: explicit exclusions
- `tools`: required tools, MCP servers, libraries
- `edge_cases`: important edge cases to consider

**How design file is used:**
- Read entire file as markdown
- Extract required information from ## Specification section
- Look for: Goal, Happy Path Example, Artifact Contract, User Inputs
- If required information is missing or unclear: ask clarifying questions
- If file is significantly incomplete: offer to switch to interactive mode
- Tolerate format variations - focus on extracting necessary content

**Approach:** Forgiving extraction with clarifying questions, not strict validation.

**Validation:**
- Required inputs must be present (extracted from file or asked interactively)

**Missing input handling:**
- In interactive mode: See <clarifying_questions> for the protocol when inputs are missing
- In design file mode: Extract what's available, ask for missing pieces, tolerate format variations
</inputs_first>

<step_contract>
Follow this sequence (detailed in `references/core-passes.md`):

0. Input mode detection
1. Target selection
2. Lock output contract
3. Define inputs
4. Draft steps
5. Assemble + validate
6. Present for approval
</step_contract>

<scope_fence>
**In scope:**
- Building either a Claude Code **skill** (SKILL.md with YAML frontmatter + XML-tag sections) or a standalone **prompt** (markdown headings).
- Producing a draft plus a single approval checkpoint.

**Out of scope:**
- Implementing the built skill/prompt in a repo.
- Adding extra workflows beyond build-skill.md and build-prompt.md.

If the user asks to extend beyond this scope: note it and ask before proceeding.
</scope_fence>

<output_schema>
format: markdown
artifacts:
  - type: skill
    filename: SKILL.md
    required: [YAML frontmatter (name, description), XML-tag sections]
  - type: prompt
    filename: (copy/paste)
    required: [markdown headings]

notes:
  - Skill artifacts are `SKILL.md` with YAML frontmatter (`name`, `description`) and XML-tag sections.
  - Prompt artifacts are a single copy/paste prompt using compact markdown headings.

validation:
  - matches the selected `target_type`
  - required sections present and non-empty
  - no forbidden sections
</output_schema>

<quality_gates>
G1 (after Step 1): required inputs captured and user-confirmed (or assumptions listed once)?
G2 (after Step 2): output contract explicitly defined (format + required/forbidden + validation)?
G3 (after Step 5): produced artifact matches the output schema?
If a gate fails: fix once → re-check; else stop and report what’s missing.
</quality_gates>

<interpretation_check>
Before drafting the final artifact:
- Restate what the user wants to build (skill vs prompt) and what "good" looks like.
- Confirm target artifacts (file names / sections) and any non-goals.
- Ask: "Is this understanding correct?"

IMPORTANT: Wait for user response before proceeding. Do not make tool calls or begin artifact creation until user confirms or corrects the interpretation.

Proceed only after user confirms or corrects these.
</interpretation_check>

<assumption_registry>
Log assumptions as made:
- [Input extraction] assumed: "{what}" | source: {design file / interactive response / default} | confidence: {high|medium|low}
- [Format decision] assumed: "{pattern}" applicable because {reason} | confidence: {level}
- [Scope interpretation] assumed: "{boundary}" | source: {explicit statement / inferred from context} | confidence: {level}
Validate high-impact + low-confidence assumptions before proceeding.
Include assumption log in interpretation check or final report for auditability.
</assumption_registry>

<clarifying_questions>
Triggers:
- Required inputs missing or incomplete
- Ambiguous requirements (e.g., unclear scope, multiple valid interpretations)
- Pattern applicability uncertain (confidence < 60% on which patterns to add)
Constraints:
- max_per_phase: 5 (batch related questions in ONE message)
- ask_early: during intake (Step 1) to avoid rework
- batch_related: true (group questions by topic)
Fallback: use safe defaults (minimal/simpler options), document assumptions, proceed
</clarifying_questions>

<user_approval_gate>
Before finalizing:
- Present the draft once.
- Ask for: approve / targeted edits.
If user rejects: do not continue drafting; ask what to change.
</user_approval_gate>

<post_build_protocol>
After artifact creation is complete:

1. **Announce completion clearly:**
   - "Artifact created at [full_path]"
   - Size: "[N] lines"
   - Key features: Brief summary of what was included

2. **Present next steps options:**
   - Move to different location (if needed)
   - Test/use the artifact
   - Request validation review (optional)

3. **Default behavior:**
   - Give user clear completion signal and file location
   - Let user ask for review if desired
   - Don't automatically present multi-lens analysis unless requested

This ensures user knows exactly where output is and what to do next.
</post_build_protocol>

<review_step>
**Purpose:** Internal validation of artifact structure and coherence.

**Criteria:**
- Completeness: all required sections present
- Consistency: no contradictions in directives
- Format: valid XML tags (skills) or heading hierarchy (prompts)
- Semantic: sections work together coherently

**Process:**
- Run internally after artifact creation
- Validate structure and integration
- Fix critical issues (broken tags, missing sections)
- Max cycles: 2

**User presentation:**
- Default: validate internally, announce completion cleanly (see <post_build_protocol>)
- Only present detailed multi-lens analysis if user explicitly requests artifact review
- Focus on completion signal and next steps, not automatic quality report

This keeps token overhead low while ensuring structural quality.
</review_step>

<lens>
Analyze artifact from multiple perspectives:
- **Correctness lens:** Does artifact correctly implement the requested behavior? Are inputs/outputs aligned? Do steps execute the goal?
- **Format lens:** Does artifact match target type (skill XML tags + YAML vs prompt markdown)? Are required sections present? Structure valid?
- **Usability lens:** Is artifact clear and executable? Will another agent understand it? Are examples/constraints sufficient?
Per lens: findings + severity (critical/major/minor)
Synthesis: merge findings, flag conflicts (e.g., high correctness but poor usability)
Coverage: each lens must report during review step
</lens>

<mode_selection>
workflow_options:
  - skill: "Claude Code skill (SKILL.md with YAML frontmatter + XML-tag sections)"
  - prompt: "Standalone prompt (markdown headings)"
skip_when: user explicitly specifies "skill" or "prompt" in initial request
default: ask "What would you like to build? 1) Skill or 2) Prompt"

After selection: gather required inputs per <inputs_first> (batch questions per <clarifying_questions>).
Use plain language with user; treat pattern names as internal.
</mode_selection>

<decision_points>
- If the user chose **Skill** (or says "skill" / "SKILL.md") → follow `references/workflow-build-skill.md`.
- Else if the user chose **Prompt** (or says "prompt" / "standalone") → follow `references/workflow-build-prompt.md`.
</decision_points>

<workflows_index>
| Workflow | Purpose |
|----------|---------|
| workflow-build-skill.md | Build a Claude Code skill with YAML frontmatter and XML structure |
| workflow-build-prompt.md | Build a standalone prompt with markdown headings |
</workflows_index>

<reference_index>
All domain knowledge in `references/`:

**Workflows:** workflow-build-skill.md, workflow-build-prompt.md
**Core methodology:** patterns-when-needed.md, tightening-methodology.md
**Skill architecture:** skill-architecture.md (bundled resources, progressive disclosure, 500-line guideline)
**Pattern details:** pattern-catalog.md, pattern-catalog-appendix.md
**Contracts & validation:** artifact-contracts.md, core-passes.md, validation-checklists.md
**Examples:** worked-example-skill.md, worked-example-prompt.md
**Format guidance:** core-passes.md (lines 24-32: format mapping)
</reference_index>

<example_anchor>
For full worked examples, see:
- `references/worked-example-skill.md`
- `references/worked-example-prompt.md`
</example_anchor>

<quick_start>
**Invocations:**
- `build-with-patterns` → interactive mode (ask questions)
- `build-with-patterns skill` → build a skill (YAML + XML)
- `build-with-patterns prompt` → build a prompt (markdown)
- `build-with-patterns --from-design <file>.skill-design.md` → build from design file

See <inputs_first> for design file format details.
Add optional patterns only when triggered (see `references/patterns-when-needed.md`).
</quick_start>

<stop_conditions>
Done when:
- output matches the output schema
- all required sections are present and non-empty
- no forbidden sections
- gates pass and edge tests were run (minimum: 2)
</stop_conditions>

