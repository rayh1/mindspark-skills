# Core Passes (Shared): Spec-by-Tightening

This file is the **canonical, shared workflow logic** for building either:
- a **Claude Code skill** (SKILL.md with YAML frontmatter + XML tags), or
- a **standalone prompt** (copy/paste headings).

Format differences are just **rendering**. Canonical mapping is below (and in `../SKILL.md` `<output_schema>`).
The `workflow-build-skill.md` and `workflow-build-prompt.md` files are thin pointers that apply these rules.

---

## Table of Contents

- [Format Mapping](#format-mapping) - Lines 24-32
- [Pass 0: Input Mode Detection](#pass-0-input-mode-detection) - Lines 34-112
- [Pass 1: Example Anchor + Output Contract](#pass-1-example-anchor--output-contract) - Lines 114-134
- [Pass 2: Inputs First](#pass-2-inputs-first) - Lines 136-150
- [Pass 3: Step Contract](#pass-3-step-contract) - Lines 152-162
- [Pass 4: Decision Points](#pass-4-decision-points--minimal-failure-modes) - Lines 164-180
- [Pass 5: Quality Gates](#pass-5-quality-gates) - Lines 182-193
- [Pass 6: Logging & Traceability](#pass-6-logging--traceability-optional) - Lines 195-204
- [Pass 7: Assemble + Validate](#pass-7-assemble--validate) - Lines 206-221

---

## Format Mapping

| Concept | Skill format | Prompt format |
|---|---|---|
| Sections | XML tags (e.g. `<inputs_first>`) | Headings (e.g. `## Inputs First:`) |
| Output artifact | `SKILL.md` (YAML frontmatter + XML body) | A single prompt text |
| “No bloat” rule | No markdown headings in XML body | Keep headings compact |

---

## Pass 0: Input Mode Detection

**Goal:** Determine how inputs will be provided (interactive or design file).

**Detection logic:**
- If `--from-design <file>` flag present → **Design file mode**
- Else → **Interactive mode** (default)

**Design file mode (forgiving extraction):**

1. **Read file:**
   - Read entire file content
   - File should be markdown format
   - If read fails: show error, exit

2. **Check for required information:**
   - Look for `## Specification` section
   - Look for required subsections:
     - `### Goal`
     - `### Happy Path Example`
     - `### Artifact Contract`
     - `### User Inputs`
   - Approach: Extract content, don't validate format strictly

3. **Handle missing information:**

   If `## Specification` section missing:
   ```
   I couldn't find a ## Specification section in this file.

   Would you like to:
   1. Provide specification details interactively
   2. Switch to full interactive mode

   Your choice (1/2):
   ```

   If specific subsections missing:
   ```
   I found the Specification section but couldn't extract: {missing_items}

   Please provide:
   - {missing_item_1}: {what it should contain}
   - {missing_item_2}: {what it should contain}

   Or type 'interactive' to switch to full interactive mode.
   ```

   If information is unclear or ambiguous:
   ```
   I found a Goal section but it's unclear: "{extracted_text}"

   Please clarify: What should this skill do? (1-2 sentences)
   ```

4. **Confirm understanding:**
   - Extract skill name from first heading (if present)
   - Extract goal (first paragraph of Goal section)
   - If metadata present, extract verdict
   - Show summary:
     ```
     Reading design from {filename}...

     Skill: {skill_name}
     Goal: {goal}
     {Verdict: {verdict} (score {score}/14) if metadata present}

     Is this understanding correct? (yes/no)
     ```
   - If yes: proceed to Pass 1 with extracted content
   - If no: ask for corrections or offer interactive mode

5. **Make extracted content available:**
   - Store extracted sections for use in subsequent passes
   - Treat as context, not strict schema
   - When drafting, reference Goal, Example, Contract, Inputs naturally

**Interactive mode:**
- Proceed directly to Pass 1 (ask questions)

**Error handling:**
- Design file not found → `File not found: {path}. Please check the path.`, exit
- File not readable → `Could not read file: {error}. Please check permissions.`, exit
- Completely unparseable → `Could not extract specification from this file. Switch to interactive mode? (yes/no)`
- Missing pieces → Ask for them interactively, continue with design file mode for rest
- Format variations → Tolerate and extract content anyway

**Key principle:** Extract what you can, ask for what you need, tolerate format variations.

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

See `validation-checklists.md` for recommended guidelines.

---

## Pass 4: Decision Points (+ minimal failure modes)

Identify where the process branches.

Format:
- If condition → action
- Else if …
- Else → fallback

See `validation-checklists.md` for recommended guidelines.

If external dependencies exist, define at least:
- primary approach
- one fallback
- terminal error surface

---

## Pass 5: Quality Gates

Define checkpoints that catch mistakes early.

See `validation-checklists.md` for recommended guidelines.

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
