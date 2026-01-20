---
name: build-with-patterns
description: Builds skills and prompts by starting with one concrete example, then tightening into a small, testable output contract and 4–6 executable steps. Use when creating new skills or prompts from scratch, or when you want a structured approach to prompt engineering.
---

<essential_principles>
## How This Skill Works

This skill guides you through building reliable skills and prompts using pattern-guided creation. Instead of writing freeform instructions, you'll:

1. Start with one concrete example (happy path)
2. Lock down the output contract (format, required sections, validation)
3. Define inputs explicitly (required/optional, validation, defaults)
4. Write 4–6 checkable steps from inputs to output
5. Add minimal branching (decision points) and quality gates

### Lite vs Full Mode

- **Lite (default)**: 4–6 steps, ≤3 decision points, ≤3 gates. Fast, focused, sufficient for most cases.
- **Full**: Removes caps and adds optional patterns only when triggered by measurable needs.

Use plain language with users; pattern names are internal scaffolding.
</essential_principles>

<intake>
What would you like to build?

1. **Skill** - A Claude Code skill (SKILL.md with YAML frontmatter)
2. **Prompt** - A standalone prompt (markdown headings)

**Mode (optional):**
- Lite (default) - 4–6 steps, ≤3 decision points, ≤3 gates
- Full - Lift caps, add patterns when needed

**Wait for response before proceeding.**
</intake>

<routing>
| Response | Workflow |
|----------|----------|
| 1, "skill", "SKILL.md" | `workflows/build-skill.md` |
| 2, "prompt", "standalone" | `workflows/build-prompt.md` |

**Mode handling:**
- If user says "full" or "full mode" → set mode=full, pass to workflow
- Otherwise → default to lite mode

**After reading the workflow, follow it exactly.**
</routing>

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

<worked_example>
**User request:** "Create a skill that turns a pasted block of text into a short study sheet."

**Example output (what “good” looks like):**

```markdown
---
name: study-sheet
description: Creates a compact study sheet from pasted notes (key terms, questions, and a 5-line summary).
---

<objective>
Turn raw notes into a study sheet that is quick to review and easy to quiz from.
</objective>

<quick_start>
Input: paste your notes
Output: STUDY_SHEET.md
</quick_start>

<inputs_first>
**Required inputs:**
- `notes`: the raw text to summarize

**Optional inputs:**
- `audience`: who this is for (default: "me")
- `tone`: terse|friendly (default: terse)

**Validation:**
- notes must be non-empty

**Missing input handling:**
- If `notes` missing → ask user to paste notes
</inputs_first>

<step_contract>
1. Parse notes → extract 5–10 key terms.
2. Summarize notes → 5-line summary.
3. Generate questions → 5 recall questions.
4. Produce STUDY_SHEET.md → in the required schema.
</step_contract>

<decision_points>
- If notes length < ~150 words → use 3–5 key terms.
- If notes are highly structured (bullets/headings) → preserve headings in summary.
- Else → treat as free text.
</decision_points>

<quality_gates>
G1 (after Step 1): key terms count is between 3 and 10.
G2 (after Step 2): summary is exactly 5 lines.
G3 (after Step 4): STUDY_SHEET.md includes all required sections.
</quality_gates>

<output_schema>
format: markdown
filename: STUDY_SHEET.md
required_sections:
  - Title (H1)
  - Summary (exactly 5 lines)
  - Key Terms (3–10 bullets)
  - Recall Questions (5 numbered)
forbidden_sections:
  - Future Work
validation:
  - all required sections present
  - counts match the constraints
</output_schema>

<stop_conditions>
Done when:
- STUDY_SHEET.md is produced and all gates pass
Don’t:
- add extra sections beyond schema
- rewrite user notes
</stop_conditions>
```
</worked_example>

<quick_start>
**Quick invocations:**
- `build-with-patterns` → ask what to build (skill/prompt) + mode
- `build-with-patterns skill` → build a Claude Code skill (SKILL.md)
- `build-with-patterns prompt` → build a standalone prompt (headings)

**Modes:**
- **lite** (default): 4–6 steps, ≤3 decision points, ≤3 gates
- **full**: lift caps + add optional add-ons only when triggered
</quick_start>

<lite_path>
Use plain language with the user; treat pattern names as internal.

1) **Intake (ask in ONE message, max ~5 questions)**
   - What should this do? (1–2 sentences)
   - One happy-path example: **Input → Output → what “good” looks like**
   - What artifact(s) should be created/modified? (filenames + required/forbidden sections)
   - What inputs will the user provide? (required/optional + defaults)
   - Any hard constraints or non-goals?

2) **Lock the output contract**
   - format, filename(s), required sections/fields, forbidden sections, and how to validate.
   - Confirm once: “Is this the output you want?”

3) **Define inputs (inputs-first)**
   - required / optional+defaults / validation / missing-input behavior.

4) **Write the step contract (4–6 steps)**
   - numbered steps from inputs → output; each step must be checkable and produce a visible deliverable.

5) **Add minimal branching + gates, then assemble + validate**
   - Decision points: **max 3** (If/Else format)
   - Quality gates: **max 3** (one line each; include what to do if a gate fails)
   - Run 2 edge tests: (a) missing required input, (b) conflicting/ambiguous requirement
   - Produce the final artifact (skill or prompt) in the required structure

**Lite hard caps (enforce):** 4–6 steps, ≤3 decision points, ≤3 gates, skip logging/traceability.
</lite_path>

<full_mode>
Lift caps as needed and add optional add-ons only when a measurable trigger is met (only if requested).
Add-ons include: scope fences, mode selection, addressable IDs, approval gates, fallback chains, review steps.
See: `references/patterns-when-needed.md` for triggers.
</full_mode>

<format_notes>
- **Skill:** `SKILL.md` with YAML frontmatter (`name`, `description`) and XML-tag sections.
- **Prompt:** a single prompt using compact markdown headings.
</format_notes>

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
