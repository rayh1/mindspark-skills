# Workflow: Build a Prompt (Lean)

<required_reading>
**Read these reference files NOW:**
1. references/tightening-methodology.md
2. references/patterns-when-needed.md (for optional add-ons)
3. references/worked-example-prompt.md (if you need an example)
</required_reading>

<process>
## Goal
Build a standalone prompt quickly using the tightening passes, without forcing the user through a reading wall or repetitive checkpointing.

## Rules (non-negotiable)
- Default to **lite** mode unless the user explicitly wants full.
- **Batch questions** (avoid “wait for response” after every paragraph).
- Use **plain language** with the user (pattern names are internal labels).
- Follow the **lite path** in `../SKILL.md` by default (4–6 steps, ≤3 decisions, ≤3 gates).
- Use `../references/patterns-when-needed.md` for optional add-ons.

## Intake (lite: ask in ONE message, max ~5 questions)
Ask:
1) What should this prompt accomplish? (1–2 sentences)
2) One happy-path example: **Input → Output → what “good” looks like**
3) What should the output look like? (format + required/forbidden sections)
4) What inputs will the user provide? (required/optional + defaults)
5) Any hard constraints or non-goals? (scope boundaries, length, tone, safety)

If anything is missing, make safe defaults and list assumptions once.

## Build (lite by default)
Follow the **Lite path (default)** in `../SKILL.md`.
- Enforce lite caps.
- Only introduce optional add-ons when a measurable trigger is met (see `../references/patterns-when-needed.md`).

## Output
Render as a single copy/paste prompt with headings (no YAML).

## Single confirmation
Show the draft once and ask for: **approve / targeted edits / switch to full**.
</process>

<success_criteria>
This workflow is complete when:
- [ ] Standalone prompt created with markdown headings (no YAML)
- [ ] Output contract locked (format, required/forbidden sections, validation)
- [ ] Inputs defined (required/optional, validation, defaults)
- [ ] 4–6 checkable steps written (lite mode) or appropriate steps for full mode
- [ ] Decision points (≤3 for lite) and quality gates (≤3 for lite) added
- [ ] Two edge tests run (missing input, conflicting requirement)
- [ ] Prompt is copy/paste ready
- [ ] User has approved the draft or provided targeted edits
</success_criteria>
