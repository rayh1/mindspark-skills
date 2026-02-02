# Minimal Templates + Canonical Tags

## Table of Contents

- [Canonical Tag Names (skills)](#canonical-tag-names-skills) - Line 15
- [Canonical Headings (raw prompts)](#canonical-headings-raw-prompts) - Line 53
- [Merge Strategy](#merge-strategy) - Line 65
- **Tier 1 Templates (Core)** - Lines 78-277
  - [Inputs First](#inputs-first) - Line 80
  - [Step Contract](#step-contract) - Line 108
  - [Decision Points](#decision-points) - Line 133
  - [Quality Gates](#quality-gates) - Line 156
  - [Stop Conditions](#stop-conditions) - Line 179
  - [Scope Fence](#scope-fence) - Line 204
  - [Interpretation Check](#interpretation-check) - Line 229
  - [Output Schema](#output-schema) - Line 255
- **Tier 2 Templates (Recommended)** - Lines 281-475
  - [User Approval Gate](#user-approval-gate) - Line 283
  - [Clarifying Questions](#clarifying-questions) - Line 305
  - [Confidence Signal](#confidence-signal) - Line 332
  - [Fallback Chain](#fallback-chain) - Line 357
  - [Lens](#lens) - Line 382
  - [Addressable Output](#addressable-output) - Line 409
  - [Review Step](#review-step) - Line 431
  - [Step Ledger](#step-ledger) - Line 453
- **Tier 3 Templates (Situational)** - Lines 479-575
  - [Mode Selection](#mode-selection) - Line 481
  - [Example Anchor](#example-anchor) - Line 497
  - [Assumption Registry](#assumption-registry) - Line 519
  - [Context Window](#context-window) - Line 533
  - [Observability](#observability) - Line 548
  - [Error Handling](#error-handling) - Line 562
- [Notes on "Minimal"](#notes-on-minimal) - Line 579

---

This page standardizes **how** patterns are written so that `apply-patterns` produces consistent, minimal, repeatable structure.

Use this as the **default insertion form**.

- **Ceiling, not floor:** templates are intentionally minimal; only expand when the target truly needs it.
- **Never duplicate sections:** if a pattern section already exists, **edit it in place**.
- **Prefer matching existing style:** if a target already uses a compatible structure, keep it and normalize only the minimum needed.

---

## Canonical Tag Names (skills)

For skills (YAML frontmatter detected), use these XML tags exactly:

**Tier 1:**
- `<inputs_first>`
- `<step_contract>`
- `<decision_points>`
- `<quality_gates>`
- `<stop_conditions>`
- `<scope_fence>`
- `<interpretation_check>`
- `<output_schema>`

**Tier 2:**
- `<user_approval_gate>`
- `<clarifying_questions>`
- `<confidence_signal>`
- `<fallback_chain>`
- `<lens>`
- `<addressable_output>`
- `<review_step>`
- `<step_ledger>`

**Tier 3:**
- `<mode_selection>`
- `<example_anchor>`
- `<assumption_registry>`
- `<context_window>`
- `<observability>`
- `<error_handling>`

Notes:
- Use **snake_case** tag names only.
- Keep tags at the same "level" (no nested pattern tags).

---

## Canonical Headings (raw prompts)

For raw prompts, use these markdown H2 headings (exact spelling):

**Tier 1:** `## Inputs First:` | `## Step Contract:` | `## Decision Points:` | `## Quality Gates:` | `## Stop Conditions:` | `## Scope Fence:` | `## Interpretation Check:` | `## Output Schema:`

**Tier 2:** `## User Approval Gate:` | `## Clarifying Questions:` | `## Confidence Signal:` | `## Fallback Chain:` | `## Lens:` | `## Addressable Output:` | `## Review Step:` | `## Step Ledger:`

**Tier 3:** `## Mode Selection:` | `## Example Anchor:` | `## Assumption Registry:` | `## Context Window:` | `## Observability:` | `## Error Handling:`

Note: Pattern sections in prompts are treated as H2 sections for organizational consistency with overall prompt structure.

---

## Merge Strategy

When applying patterns to an existing target:

1. **If the canonical section exists** (same tag/heading): update its contents.
2. **If a near-equivalent exists** (different name, same concept): rename/normalize to canonical form *only if low-risk*; otherwise keep the existing name but align the internal structure.
3. **If content is scattered** (e.g., inputs mentioned across multiple sections): create the canonical section and move only the minimum necessary content into it.
4. **If the target already has a stronger version** of a pattern: keep it; do not downgrade to minimal.

Hard rule: **Never add a second section for the same pattern.**

---

# Tier 1 Templates (Core)

## Inputs First

**Skill (XML):**

```xml
<inputs_first>
**Required inputs:**
- `target_path`: ...

**Optional inputs:**
- `mode`: ... (default: lite)

**Validation:**
- `target_path` must exist and be readable
</inputs_first>
```

**Prompt (markdown):**

```markdown
## Inputs First:
required: target_path
optional: mode=lite
validation: target_path exists and readable
```

---

## Step Contract

**Skill (XML):**

```xml
<step_contract>
1. Read inputs → confirm assumptions
2. Do the work → produce artifacts
3. Verify → run checks
4. Report → summarize results + next actions
</step_contract>
```

**Prompt (markdown):**

```markdown
## Step Contract:
1) Read inputs
2) Do the work
3) Verify
4) Report
```

---

## Decision Points

**Skill (XML):**

```xml
<decision_points>
- If {condition_a} → {action_a}
- Else if {condition_b} → {action_b}
- Record chosen branch in output
</decision_points>
```

**Prompt (markdown):**

```markdown
## Decision Points:
If condition_a → action_a
Else if condition_b → action_b
Record: chosen branch
```

---

## Quality Gates

**Skill (XML):**

```xml
<quality_gates>
G1 (after Step 1): inputs validated?
G2 (after Step 2): output matches requested format?
If a gate fails: fix once → re-check; else stop and report.
</quality_gates>
```

**Prompt (markdown):**

```markdown
## Quality Gates:
G1 after step 1: inputs validated?
G2 after step 2: output matches format?
fail → retry once; else stop and report
```

---

## Stop Conditions

**Skill (XML):**

```xml
<stop_conditions>
Done when:
- requested deliverables are produced
- required gates pass
Don't:
- expand scope
- refactor unrelated code
</stop_conditions>
```

**Prompt (markdown):**

```markdown
## Stop Conditions:
done when: deliverables complete + gates pass
don't: expand scope or refactor unrelated code
```

---

## Scope Fence

**Skill (XML):**

```xml
<scope_fence>
**In scope:**
- {files/areas}
**Out of scope:**
- {explicit exclusions}
If boundary is crossed: ask before proceeding.
</scope_fence>
```

**Prompt (markdown):**

```markdown
## Scope Fence:
in: {what to touch}
out: {what not to touch}
if out-of-scope: ask first
```

---

## Interpretation Check

**Skill (XML):**

```xml
<interpretation_check>
Before starting work:
- Restate understanding in own words
- List key assumptions about scope/approach
- Confirm with user before proceeding
If corrected: update understanding, re-confirm if significant.
</interpretation_check>
```

**Prompt (markdown):**

```markdown
## Interpretation Check:
restatement: {paraphrase the request}
assumptions: {key interpretations}
confirm: "Is this correct?"
if corrected: update and re-confirm
```

---

## Output Schema

**Skill (XML):**

```xml
<output_schema>
format: markdown
sections: [Summary, Changes, Notes]
constraints:
  Summary: 2-3 sentences
  Changes: bullet list
validation: all sections present before output
</output_schema>
```

**Prompt (markdown):**

```markdown
## Output Schema:
format: markdown
sections: Summary, Changes, Notes
validation: all sections present
```

---

# Tier 2 Templates (Recommended)

## User Approval Gate

**Skill (XML):**

```xml
<user_approval_gate>
Before edits: present proposal + options.
On approval: apply changes.
On rejection: analysis-only; no edits.
</user_approval_gate>
```

**Prompt (markdown):**

```markdown
## User Approval Gate:
pause before edits; show proposal
options: approve / select subset / cancel
```

---

## Clarifying Questions

**Skill (XML):**

```xml
<clarifying_questions>
triggers:
  - confidence < 60% on consequential decision
  - ambiguous requirement
constraints:
  max_per_phase: 3
  batch_related: true
fallback: use safe default, document assumption
</clarifying_questions>
```

**Prompt (markdown):**

```markdown
## Clarifying Questions:
trigger: low confidence or ambiguity
max: 3 per phase, batch related
fallback: safe default + document
```

---

## Confidence Signal

**Skill (XML):**

```xml
<confidence_signal>
Thresholds:
- high (>85%): proceed
- medium (60-85%): proceed, flag for review
- low (<60%): ask before proceeding
Include: assessment + reason + what would change it
</confidence_signal>
```

**Prompt (markdown):**

```markdown
## Confidence Signal:
high (>85%): proceed
medium (60-85%): proceed + flag
low (<60%): ask first
```

---

## Fallback Chain

**Skill (XML):**

```xml
<fallback_chain>
primary: {preferred approach}
fallback_1: {if primary fails}
fallback_2: {if fallback_1 fails}
terminal: ERROR: {code} - surface to user
trigger: {conditions that activate fallback}
</fallback_chain>
```

**Prompt (markdown):**

```markdown
## Fallback Chain:
primary: preferred approach
fallback_1: if primary fails
terminal: ERROR - surface to user
```

---

## Lens

**Skill (XML):**

```xml
<lens>
perspectives:
  - {lens_1}: [focus areas]
  - {lens_2}: [focus areas]
per_lens: findings + severity
synthesis: merge findings, flag conflicts
coverage: each lens must report
</lens>
```

**Prompt (markdown):**

```markdown
## Lens:
perspectives: [lens_1, lens_2, lens_3]
per_lens: findings + severity
synthesis: merge, flag conflicts
coverage: all must report
```

---

## Addressable Output

**Skill (XML):**

```xml
<addressable_output>
id_format: "[{prefix}-{number}]"
prefixes: { finding: F, recommendation: R }
Use IDs in follow-up: "address F-1", "defer R-2"
</addressable_output>
```

**Prompt (markdown):**

```markdown
## Addressable Output:
format: [F-1], [R-1], etc.
use in follow-up: "fix F-1", "defer R-2"
```

---

## Review Step

**Skill (XML):**

```xml
<review_step>
criteria: completeness, consistency, format
max_cycles: 2
If issues: revise once per cycle; stop at max_cycles.
</review_step>
```

**Prompt (markdown):**

```markdown
## Review Step:
criteria: completeness + consistency + format
max_cycles: 2
```

---

## Step Ledger

**Skill (XML):**

```xml
<step_ledger>
Update after each step:
- done: [completed steps]
- next: {current step}
- decisions: [choices made]
- open_issues: [blockers]
</step_ledger>
```

**Prompt (markdown):**

```markdown
## Step Ledger:
done: [...]
next: {step}
decisions: [...]
open_issues: [...]
```

---

# Tier 3 Templates (Situational)

## Mode Selection

**Skill (XML):**

```xml
<mode_selection>
workflow_options:
  - {mode_a}: "{description}"
  - {mode_b}: "{description}"
skip_when: request explicitly specifies mode
default: infer from context, else ask
</mode_selection>
```

---

## Example Anchor

**Skill (XML):**

```xml
<example_anchor>
GOOD:
```
{good example with annotation}
```

BAD:
```
{bad example with annotation}
```

Instruction: match style of GOOD example
</example_anchor>
```

---

## Assumption Registry

**Skill (XML):**

```xml
<assumption_registry>
Log assumptions as made:
- [Step N] assumed: "{what}" | source: {why} | confidence: {level}
Validate high-impact + low-confidence before proceeding.
</assumption_registry>
```

---

## Context Window

**Skill (XML):**

```xml
<context_window>
always_include: [requirements, current step, errors]
summarize_after_use: [large file contents]
drop_after_step: [rejected alternatives]
refresh_trigger: re-read if stale (>2 steps)
</context_window>
```

---

## Observability

**Skill (XML):**

```xml
<observability>
run_header: id, skill, mode, inputs
step_trace: [Step N] status=ok|fail outcome="..."
artifacts: named outputs per step
</observability>
```

---

## Error Handling

**Skill (XML):**

```xml
<error_handling>
taxonomy:
  - INPUT_MISSING: required input not provided
  - GATE_FAIL: quality gate failed
  - TOOL_TIMEOUT: external tool didn't respond
format: ERROR: {code} - {context} - {action}
redaction: api_key, password → "[REDACTED]"
</error_handling>
```

---

## Notes on "Minimal"

Minimal means:
- Prefer **bullets over prose**.
- Prefer **one validation rule** over a paragraph.
- Prefer **2 gates** over 6.
- Prefer **one approval checkpoint** over many.

If adding a pattern would exceed ~10-15 lines, reconsider whether it's actually needed for the target.

**Tier priority:** When in doubt, prefer Tier 1 patterns over Tier 2, and Tier 2 over Tier 3.
