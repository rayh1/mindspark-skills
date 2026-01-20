# Core Passes (Shared): Spec-by-Tightening

This file is the **canonical, shared workflow logic** for building either:
- a **Claude Code skill** (SKILL.md with YAML frontmatter + XML tags), or
- a **standalone prompt** (copy/paste headings).

The per-format differences live in:
- workflows/build-skill.md
- workflows/build-prompt.md

---

## Format Mapping

| Concept | Skill format | Prompt format |
|---|---|---|
| Sections | XML tags (e.g. `<inputs_first>`) | Headings (e.g. `## Inputs First:`) |
| Output artifact | `SKILL.md` (YAML frontmatter + XML body) | A single prompt text |
| “No bloat” rule | No markdown headings in XML body | Keep headings compact |

---

## Mode: lite vs full

### Lite mode (default)
Lite mode is designed to be **fast and non-bloated**.

**Hard caps (enforce these):**
- **Steps:** 4–6 total in Step Contract
- **Quality gates:** **max 3** (each 1 line)
- **Decision points:** **max 3** branches
- **Per section length:** aim ≤ ~15 lines; if you exceed, **summarize** or ask to switch to full
- **Edge cases to test:** 2 (one missing input, one conflicting/ambiguous)
- **Skip Pass 6** (logging/traceability)

**If the user asks for more:** either (a) switch to **full**, or (b) agree a scoped expansion (e.g. “add 2 more steps only”).

### Full mode
Full mode allows optional patterns when justified, and may include Pass 6.

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
- validation: how to check it’s correct

**Lite guidance:** keep schema minimal (3–7 required sections).

Stop and confirm:
- “Is this the output you want?”

---

## Pass 2: Inputs First

Define a compact input schema:
- required
- optional + defaults
- validation rules
- missing-input behavior (ask vs safe default vs stop)

**Lite guidance:** if any required input is missing, prefer **ask 1–3 questions** and proceed.

Confirm:
- “Are these the inputs you can provide?”

---

## Pass 3: Step Contract

Write a numbered step plan from inputs → output.

Constraints:
- each step must produce a **visible deliverable** (a list, a file section, a decision)
- steps must be checkable

**Lite hard cap:** 4–6 steps.

---

## Pass 4: Decision Points (+ minimal failure modes)

Identify where the process branches.

Format:
- If condition → action
- Else if …
- Else → fallback

**Lite hard cap:** max 3 decision points.

If external dependencies exist, define at least:
- primary approach
- one fallback
- terminal error surface

---

## Pass 5: Quality Gates

Define 3–7 checkpoints that catch mistakes early.

**Lite hard cap:** max 3 gates.

Each gate should say:
- when (after which step)
- what to check (one line)
- what to do if it fails (revise step X, ask user, etc.)

---

## Pass 6: Logging & Traceability (Full mode only)

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
- run edge-case tests (lite: 2 cases)

Stop conditions:
- done when deliverables complete AND gates pass
- don’t expand scope
