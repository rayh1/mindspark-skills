# Workflow: Build a Skill (Lean)

<required_reading>
**Read these reference files NOW:**
1. references/tightening-methodology.md
2. references/patterns-when-needed.md (for optional add-ons)
3. references/worked-example-skill.md (if you need an example)
</required_reading>

<process>
## Goal
Build a Claude Code skill quickly using the tightening passes, without forcing the user through a reading wall or 13+ micro-questions.

## Rules (non-negotiable)
- Default to **lite** mode unless the user explicitly wants full.
- **Batch questions** (avoid “wait for response” after every paragraph).
- Use **plain language** with the user (pattern names are internal labels).
- Follow the **lite path** in `../SKILL.md` by default (4–6 steps, ≤3 decisions, ≤3 gates).
- Use `../references/patterns-when-needed.md` for optional add-ons.

## Intake (lite: ask in ONE message, max ~5 questions)
Ask:
1) What should this skill do? (1–2 sentences)
2) One happy-path example: **Input → Output → what “good” looks like**
3) What artifact(s) should be created/modified? (file names + required/forbidden sections)
4) What inputs will the user provide? (required/optional + defaults)
5) Any hard constraints or non-goals? (scope boundaries, length, tone, safety)

If anything is missing, make safe defaults and list assumptions once.

## Build (lite by default)
Follow the **Lite path (default)** in `../SKILL.md`.
- Enforce lite caps.
- Only introduce optional add-ons when a measurable trigger is met (see `../references/patterns-when-needed.md`).

## Output
Render as `SKILL.md`:
- YAML frontmatter (`name`, `description`)
- Prefer the repo’s XML-tag structure for sections, **but** if the user explicitly wants plain markdown, comply and stay consistent.

## Single confirmation
Show the draft once and ask for: **approve / targeted edits / switch to full**.
</process>

<success_criteria>
This workflow is complete when:
- [ ] SKILL.md file created with valid YAML frontmatter (name, description)
- [ ] All required XML tags present (objective/essential_principles, quick_start, success_criteria)
- [ ] Output contract locked (format, required/forbidden sections, validation)
- [ ] Inputs defined (required/optional, validation, defaults)
- [ ] 4–6 checkable steps written (lite mode) or appropriate steps for full mode
- [ ] Decision points (≤3 for lite) and quality gates (≤3 for lite) added
- [ ] Two edge tests run (missing input, conflicting requirement)
- [ ] User has approved the draft or provided targeted edits
</success_criteria>
