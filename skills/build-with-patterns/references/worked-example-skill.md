# Worked Example (Skill) — Lite Mode

This is a small, complete example of a **lite-mode** skill produced using the shared passes.

---

## Scenario

**User request:** “Create a skill that turns a pasted block of text into a short study sheet.”

**Mode:** lite

---

## Example Output: `SKILL.md`

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
- If notes length < ~150 words → skip key terms list (use 3–5 terms only).
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

<success_criteria>
- [ ] Output file created: STUDY_SHEET.md
- [ ] Summary is 5 lines
- [ ] 3–10 key terms
- [ ] 5 recall questions
</success_criteria>
```

Notes:
- This is intentionally small; full mode would add optional patterns only if needed.
