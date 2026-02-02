# The "Spec by Tightening" Methodology

**Note:** This is a non-authoritative narrative deep-dive. The canonical, normative workflow is in `core-passes.md`.

This reference documents the 9-step methodology for writing complex, executable specifications that stay understandable while becoming precise.

The core insight: **build in layers** so structure emerges gradually, not all at once.

---

## Table of Contents

- [The 9 Steps](#the-9-steps) - Line 11
  - [1) Start with One Concrete "Happy Path"](#1-start-with-one-concrete-happy-path) - Line 13
  - [2) Define the Artifact Contract Early (and Keep It Small)](#2-define-the-artifact-contract-early-and-keep-it-small) - Line 31
  - [3) Write "Inputs First" Like a Form, Not Prose](#3-write-inputs-first-like-a-form-not-prose) - Line 49
  - [4) Add a Step Contract That Is Short and Enforceable](#4-add-a-step-contract-that-is-short-and-enforceable) - Line 68
  - [5) Encode Decision Points as Explicit Branches](#5-encode-decision-points-as-explicit-branches) - Line 86
  - [6) Introduce "Quality Gates" Sparingly](#6-introduce-quality-gates-sparingly) - Line 103
  - [7) Add Logging Only After the Workflow Is Stable](#7-add-logging-only-after-the-workflow-is-stable) - Line 120
  - [8) Stress-Test the Spec with Adversarial Examples](#8-stress-test-the-spec-with-adversarial-examples) - Line 137
  - [9) Refactor for Readability (The Underrated Part)](#9-refactor-for-readability-the-underrated-part) - Line 155
- [A Human-Friendly Outline You Can Reuse](#a-human-friendly-outline-you-can-reuse) - Line 170
- [A Practical Writing Tactic: Build in Passes](#a-practical-writing-tactic-build-in-passes) - Line 197
- [Why This Works](#why-this-works) - Line 213

---

## The 9 Steps

### 1) Start with One Concrete "Happy Path"

Write a **single end-to-end example** of what you want the skill/prompt to do.

**What to capture:**
- Input: what the user gives
- Output: exact file(s) / sections produced
- Quality bar: what "good" looks like
- One example run (even if rough)

**Why this works:**
This anchors everything and prevents abstract overengineering. You can't write a good spec for something you haven't seen work once.

**Anti-pattern:**
Starting with "this skill will handle X, Y, and Z cases" before showing even one case.

---

### 2) Define the Artifact Contract Early (and Keep It Small)

Before rules, decide what the skill/prompt **delivers**.

**Lock down:**
- One artifact (e.g., `OUTPUT.md`) or a small fixed set
- Required sections
- Forbidden sections (to prevent bloat)
- Formatting rules (headings, checklists, code fences, etc.)

**Why this works:**
Once the output format is stable, everything else becomes easier to specify.

**Anti-pattern:**
Saying "it produces a report" without specifying what sections that report must have.

---

### 3) Write "Inputs First" Like a Form, Not Prose

Make inputs a compact schema:

**What to specify:**
- Required vs optional
- Defaults
- Validation rules
- Missing-input handling
- How assumptions must be recorded

**Think of it like:**
CLI flags or an API request schema—boring is good.

**Anti-pattern:**
Scattered references to inputs throughout the spec. Inputs should be declared once, upfront.

---

### 4) Add a Step Contract That Is Short and Enforceable

Turn "what it should do" into **numbered steps** that can be checked.

**Good step contracts are:**
- **Linear** (mostly)
- **Each step produces a visible result**
- **Fail-fast rules are explicit**
- **No step has ambiguous completion criteria**

**If a step can't be verified, rewrite it.**

**Example:**
- ✅ "1) Parse inputs → extract goal and constraints"
- ❌ "1) Understand the requirements"

---

### 5) Encode Decision Points as Explicit Branches

Where people normally hand-wave ("depends on complexity"), do this instead:

**Structure:**
- If condition → do A
- Else → do B
- Record which branch was chosen (and why) in a "ledger" section

**Why this works:**
This is what makes a complex doc **executable**. No ambiguity about what happens when.

**Anti-pattern:**
"Handle edge cases appropriately" (what does that mean??)

---

### 6) Introduce "Quality Gates" Sparingly

Complex specs often die from too many rules. Pick only a few gates that matter:

**Common gates:**
- **Completeness gate:** all required sections exist and are non-empty
- **Consistency gate:** terms match, no contradictions
- **Testability gate:** each requirement has a verification method
- **Scope gate:** no "future work" sprawl

**Each gate should have a checklist with 3–7 items max.**

**Why sparingly:**
Too many gates → spec becomes unreadable and brittle.

---

### 7) Add Logging Only After the Workflow Is Stable

Logging is easy to bloat. Add it after the process works:

**What to log:**
- Run header (run_id, mode)
- Step ledger (what happened per step)
- Assumptions list (what was inferred)
- Open questions (if any)

**Keep it mechanical.**

**Anti-pattern:**
Over-logging every tiny decision. Log only what helps debugging or auditing.

---

### 8) Stress-Test the Spec with Adversarial Examples

Run your own spec against:

**Edge cases:**
- Missing required input
- Conflicting inputs
- Extremely small task
- Extremely large task
- User asks to skip steps

**Every time it breaks, you patch the doc with ONE new rule (not ten).**

**Why this works:**
Real-world testing finds the gaps that armchair design misses.

---

### 9) Refactor for Readability (The Underrated Part)

After it's correct, make it readable:

**Organize into sections:**
- Move philosophy/rationale to a short **"Principles"** section
- Keep procedural rules in **"Execution"**
- Keep examples in **"Examples"**
- Keep edge cases in **"Appendix: Exceptions"**

**Why this matters:**
Most "AI-looking" specs are hard to read because everything is mixed together. Separate concerns.

---

## A Human-Friendly Outline You Can Reuse

Use this structure for any skill or prompt:

1. **Purpose**
2. **Non-goals**
3. **Artifact Contract**
   - Filename(s)
   - Required sections
   - Output format rules
4. **Inputs**
   - Required / optional / defaults
   - Validation + missing-input behavior
5. **Workflow**
   - Step contract (numbered)
   - Decision points (branch rules)
6. **Quality Gates**
7. **Logging / Traceability**
8. **Examples**
   - Happy path
   - Edge case
9. **Appendix**
   - Glossary
   - Templates / snippets

---

## A Practical Writing Tactic: Build in Passes

Don't try to write everything at once. Tighten in passes:

**Pass 1:** Happy path + artifact contract
**Pass 2:** Inputs schema
**Pass 3:** Step contract
**Pass 4:** Branches + failure modes
**Pass 5:** Quality gates
**Pass 6:** Logging
**Pass 7:** Refactor + examples

**Each pass should add structure, not length.**

---

## Why This Works

This methodology comes from **how humans produce complex, executable internal specs** without getting lost:

- **Anchor on examples** (not abstractions)
- **Lock the output contract** (so you know what success looks like)
- **Formalize steps/branches/gates** (so it's verifiable)
- **Iterate in passes** (so complexity stays manageable)

This is a pragmatic approach that tends to work well for complex specs—treat it as a starting point and adapt it to your context.
