---
name: build-with-patterns
description: Builds skills and prompts by starting with one concrete example, then tightening into a small, testable output contract and 4–6 executable steps. Use when creating new skills or prompts from scratch, or when you want a structured approach to prompt engineering.
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
**Required inputs:**
- `target_type`: skill | prompt
- `goal`: what this should do (1–2 sentences)
- `happy_path_example`: Input → Output → what “good” looks like
- `artifacts`: files to create/modify + required/forbidden sections

**Optional inputs:**
- `mode`: lite (default) | full
- `constraints`: hard constraints (length, tone, safety)
- `non_goals`: explicit exclusions

**Validation:**
- Required inputs must be non-empty.

**Missing input handling:**
- Ask up to ~5 intake questions in ONE message; if still missing, make safe defaults and list assumptions once.
</inputs_first>

<step_contract>
1. Mode selection → get `target_type` + `mode`, then collect the remaining required inputs.
2. Lock output contract → define format, required/forbidden sections, validation (confirm once).
3. Define inputs → required/optional/defaults + missing-input behavior.
4. Draft steps → 4–6 checkable steps (lite) or appropriate steps (full), plus minimal branching + gates.
5. Assemble + validate → render final SKILL.md or prompt; run gates + edge tests.
6. Present for approval → approve / targeted edits / switch mode.
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
- Restate what the user wants to build (skill vs prompt) and what “good” looks like.
- Confirm the chosen mode (lite vs full).
- Confirm target artifacts (file names / sections) and any non-goals.
Proceed only after user confirms or corrects these.
</interpretation_check>

<user_approval_gate>
Before finalizing:
- Present the draft once.
- Ask for: approve / targeted edits / switch mode.
If user rejects: do not continue drafting; ask what to change.
</user_approval_gate>

<review_step>
criteria: completeness, consistency, format, no contradictions
max_cycles: 1 (lite) / 2 (full)
</review_step>

<mode_selection>
What would you like to build?

1. **Skill** - A Claude Code skill (SKILL.md with YAML frontmatter)
2. **Prompt** - A standalone prompt (markdown headings)

**Mode (optional):**
- Lite (default)
- Full

**Wait for response before proceeding.**

**Lite mode (default)**
Use plain language with the user; treat pattern names as internal.

- Ask in ONE message (max ~5 questions):
  - What should this do? (1–2 sentences)
  - One happy-path example: **Input → Output → what “good” looks like**
  - What artifact(s) should be created/modified? (filenames + required/forbidden sections)
  - What inputs will the user provide? (required/optional + defaults)
  - Any hard constraints or non-goals?
- Enforce lite hard caps: see `references/shared-checklists.md`.
- Add optional add-ons only when a measurable trigger is met: `references/patterns-when-needed.md`.

**Full mode**
Lift caps as needed and add optional add-ons only when a measurable trigger is met (only if requested).
Add-ons include: scope fences, mode selection, addressable IDs, approval gates, fallback chains, review steps.
See: `references/patterns-when-needed.md` for triggers.
</mode_selection>

<decision_points>
- If the user chose **Skill** (or says "skill" / "SKILL.md") → follow `workflows/build-skill.md`.
- Else if the user chose **Prompt** (or says "prompt" / "standalone") → follow `workflows/build-prompt.md`.

**Mode handling:**
- If user says "full" or "full mode" → set mode=full.
- Otherwise → default to lite mode.
</decision_points>

<workflows_index>
| Workflow | Purpose |
|----------|---------|
| build-skill.md | Build a Claude Code skill with YAML frontmatter and XML structure |
| build-prompt.md | Build a standalone prompt with markdown headings |
</workflows_index>

<reference_index>
All domain knowledge in `references/`:

**Core methodology:** patterns-when-needed.md, tightening-methodology.md
**Pattern details:** pattern-scaffold.md, pattern-scaffold-appendix.md
**Contracts & validation:** artifact-contracts.md, core-passes.md, shared-checklists.md
**Examples:** worked-example-skill.md, worked-example-prompt.md
**Format guidance:** format-differences.md
**Author notes:** methodology-notes-for-authors.md
</reference_index>

<example_anchor>
For full worked examples, see:
- `references/worked-example-skill.md`
- `references/worked-example-prompt.md`
</example_anchor>

<quick_start>
**Quick invocations:**
- `build-with-patterns` → ask what to build (skill/prompt) + mode
- `build-with-patterns skill` → build a Claude Code skill (SKILL.md)
- `build-with-patterns prompt` → build a standalone prompt (headings)

**Modes:**
- **lite** (default): see `references/shared-checklists.md`
- **full**: lift caps + add optional add-ons only when triggered
</quick_start>

<stop_conditions>
Done when:
- output matches the output schema
- all required sections are present and non-empty
- no forbidden sections
- gates pass and edge tests were run (lite: 2)
</stop_conditions>

<references>
- `references/patterns-when-needed.md` (measurable triggers)
- `references/methodology-notes-for-authors.md` (archived notes; optional)
</references>
