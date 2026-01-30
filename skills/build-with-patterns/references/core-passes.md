# Core Passes (Shared): Spec-by-Tightening

This file is the **canonical, shared workflow logic** for building either:
- a **Claude Code skill** (SKILL.md with YAML frontmatter + XML tags), or
- a **standalone prompt** (copy/paste headings).

Format differences are just **rendering**. Canonical mapping is below (and in `../SKILL.md` `<output_schema>`).
The `workflows/build-skill.md` and `workflows/build-prompt.md` files are thin pointers that apply these rules.

---

## Format Mapping

| Concept | Skill format | Prompt format |
|---|---|---|
| Sections | XML tags (e.g. `<inputs_first>`) | Headings (e.g. `## Inputs First:`) |
| Output artifact | `SKILL.md` (YAML frontmatter + XML body) | A single prompt text |
| “No bloat” rule | No markdown headings in XML body | Keep headings compact |

---

## Pass 1: Example Anchor + Output Contract

**Goal:** lock a real example + what the artifact looks like.

Ask:
1) What should this do?
2) Show one concrete happy-path example (Input → Output → Success).

Then define the **Output Schema / Artifact Contract**:
- format (markdown/JSON/code)
- filename(s)
- required sections/fields
- forbidden sections (anti-bloat)
- validation: how to check it's correct

Keep schema minimal and focused.

Stop and confirm:
- “Is this the output you want?”

---

## Pass 2: Inputs First

Define a compact input schema:
- required
- optional + defaults
- validation rules
- missing-input behavior (ask vs safe default vs stop)

If any required input is missing, ask **up to ~5 intake questions** in one batch; if still blocked, make safe defaults and list assumptions once.

Confirm:
- “Are these the inputs you can provide?”

---

## Pass 3: Step Contract

Write a numbered step plan from inputs → output.

Constraints:
- each step must produce a **visible deliverable** (a list, a file section, a decision)
- steps must be checkable

See `shared-checklists.md` for recommended guidelines.

---

## Pass 4: Decision Points (+ minimal failure modes)

Identify where the process branches.

Format:
- If condition → action
- Else if …
- Else → fallback

See `shared-checklists.md` for recommended guidelines.

If external dependencies exist, define at least:
- primary approach
- one fallback
- terminal error surface

---

## Pass 5: Quality Gates

Define checkpoints that catch mistakes early.

See `shared-checklists.md` for recommended guidelines.

Each gate should say:
- when (after which step)
- what to check (one line)
- what to do if it fails (revise step X, ask user, etc.)

---

## Pass 6: Logging & Traceability (Optional)

Add logging only if:
- workflow is long (10+ steps)
- debugging/audit matters

If adding logging:
- step ledger (done/next/decisions/open issues)
- assumption registry (only if needed)

---

## Pass 7: Assemble + Validate

Assemble the final artifact (skill or prompt) in the required structure.

Validation checklist:
- output matches the output schema
- all required sections present
- no forbidden sections
- run edge-case tests (minimum: 2 cases)

Stop conditions:
- done when deliverables complete AND gates pass
- don’t expand scope
