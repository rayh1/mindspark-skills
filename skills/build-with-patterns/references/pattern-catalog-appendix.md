# Complete Pattern Scaffold: All 22 Patterns

This reference documents how all 22 reliability patterns map to the 7-pass creation methodology.

**Key principle:** Required patterns build core structure. Optional patterns solve specific problems.

**Trigger logic (authoritative):** See `patterns-when-needed.md`. This appendix focuses on templates and canonical tag names.

---

## Table of Contents

- [Formatting Standards](#formatting-standards) - Line 11
- [Quick Reference: All 22 Patterns by Pass](#quick-reference-all-22-patterns-by-pass) - Line 93
- [Pass 1: Happy Path + Artifact Contract](#pass-1-happy-path--artifact-contract) - Line 109
- [Pass 2: Inputs Schema](#pass-2-inputs-schema) - Line 191
- [Pass 3: Step Contract](#pass-3-step-contract) - Line 249
- [Pass 4: Decision Points + Failure Modes](#pass-4-decision-points--failure-modes) - Line 281
- [Pass 5: Quality Gates](#pass-5-quality-gates) - Line 399
- [Pass 6: Logging & Traceability](#pass-6-logging--traceability) - Line 469
- [Pass 7: Refactor & Validate](#pass-7-refactor--validate) - Line 540
- [Pattern Selection Decision Tree](#pattern-selection-decision-tree) - Line 586
- [Summary](#summary) - Line 613

---

## Formatting Standards

This section standardizes **how** patterns are written so that `build-with-patterns` produces consistent, minimal, repeatable structure.

### Minimalism Principles

- **Ceiling, not floor:** Templates are intentionally minimal; only expand when truly needed.
- **Never duplicate sections:** If a pattern section already exists, edit it in place.
- **Prefer bullets over prose:** Keep content scannable and concise.
- **Prefer one validation rule** over a paragraph.
- **Prefer 2-3 gates** over 6.
- **Prefer one approval checkpoint** over many.

If adding a pattern would exceed ~10-15 lines, reconsider whether it's actually needed.

**Tier priority:** When in doubt, prefer Tier 1 patterns over Tier 2, and Tier 2 over Tier 3.

### Canonical Tag Names (Skills)

For skills (YAML frontmatter), use these XML tags exactly:

**All 22 Pattern Tags (alphabetical):**
- `<addressable_output>`
- `<assumption_registry>`
- `<clarifying_questions>`
- `<confidence_signal>`
- `<context_window>`
- `<decision_points>`
- `<error_handling>`
- `<example_anchor>`
- `<fallback_chain>`
- `<inputs_first>`
- `<interpretation_check>`
- `<lens>`
- `<mode_selection>`
- `<observability>`
- `<output_schema>`
- `<quality_gates>`
- `<review_step>`
- `<scope_fence>`
- `<step_contract>`
- `<step_ledger>`
- `<stop_conditions>`
- `<user_approval_gate>`

**Formatting rules:**
- Use **snake_case** tag names only.
- Keep tags at the same "level" (no nested pattern tags).
- XML tags must be balanced (opening and closing tags match).

**Note on tiers:** Tier assignments (which patterns are "core" vs "optional") vary by context. When creating new skills (this document), see the pass-by-pass guidance below. When retrofitting existing skills, see `../apply-patterns/references/minimal-templates.md`.

### Canonical Headings (Prompts)

For standalone prompts (markdown), use these headings (exact spelling):

**All 22 Pattern Headings (alphabetical):**
- Addressable Output:
- Assumption Registry:
- Clarifying Questions:
- Confidence Signal:
- Context Window:
- Decision Points:
- Error Handling:
- Example Anchor:
- Fallback Chain:
- Inputs First:
- Interpretation Check:
- Lens:
- Mode Selection:
- Observability:
- Output Schema:
- Quality Gates:
- Review Step:
- Scope Fence:
- Step Contract:
- Step Ledger:
- Stop Conditions:
- User Approval Gate:

---

## Quick Reference: All 22 Patterns by Pass

| Pass | Required Patterns | Optional Patterns |
|------|-------------------|-------------------|
| **Pass 1** | Example Anchor, Output Schema | Addressable Output, Mode Selection |
| **Pass 2** | Inputs First | Clarifying Questions |
| **Pass 3** | Step Contract | - |
| **Pass 4** | Decision Points | Scope Fence, User Approval Gate, Confidence Signal, Fallback Chain, Error Handling |
| **Pass 5** | Quality Gates | Review Step, Lens |
| **Pass 6** | - | Step Ledger, Observability, Assumption Registry, Context Window |
| **Pass 7** | Stop Conditions | Interpretation Check |

**Total: 7 required (Tier 1 core) + 15 optional (contextual)**

---

## Pass 1: Happy Path + Artifact Contract

### Required Patterns

#### 1. Example Anchor
**When:** Always (required for grounding)
**Purpose:** Start with concrete reality, not abstractions
**Tier:** 1 (Core)

**What to build:**
```xml
<objective>
[Based on the example: what, why, value]
</objective>

<quick_start>
[Minimal working example from happy path]
</quick_start>
```

**Questions:**
- What's one concrete example of this working?
- Input → Output → Success?

#### 2. Output Schema
**When:** Always (required for contracts)
**Purpose:** Lock what gets delivered
**Tier:** 1 (Core)

**Template:**
```xml
<output_schema>
format: [markdown, JSON, code, etc.]
sections: [required sections]
constraints: [formatting rules]
validation: [how to verify output is valid]
</output_schema>
```

**Questions:**
- What files/sections does it produce?
- What makes output "valid"?
- What's forbidden (to prevent bloat)?

### Optional Patterns

#### 3. Addressable Output
**Triggers:** See `patterns-when-needed.md` (authoritative).
**Purpose:** Enable precise references to output elements
**Tier:** 2 (Recommended)
**Examples:** Findings list, recommendations, error list

**Template:**
```xml
<addressable_output>
id_format: "[{prefix}-{number}]"
prefixes: { finding: F, recommendation: R, issue: I }
Use IDs in follow-up: "address F-1", "defer R-2"
</addressable_output>
```


#### 4. Mode Selection
**Triggers:** See `patterns-when-needed.md` (authoritative).
**Purpose:** Present workflow choices upfront
**Tier:** 3 (Situational)
**Examples:** build vs debug, interactive vs automated, create vs modify

**Template:**
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

## Pass 2: Inputs Schema

### Required Patterns

#### 5. Inputs First
**When:** Always (any task with inputs)
**Purpose:** Declare inputs upfront, surface missing info early
**Tier:** 1 (Core)

**Template:**
```xml
<inputs_first>
**Required inputs:**
- `input_name`: description

**Optional inputs:**
- `input_name`: description (default: value)

**Validation:**
- Rule 1
- Rule 2

**Missing input handling:**
- If missing required → [action]
- Assumptions recorded in: [where]
</inputs_first>
```

**Questions:**
- What's absolutely required?
- What's optional?
- What happens if missing?
- How are assumptions documented?

### Optional Patterns

#### 6. Clarifying Questions
**Triggers:** See `patterns-when-needed.md` (authoritative).
**Purpose:** Define protocol for asking questions
**Tier:** 2 (Recommended)
**Examples:** Ambiguous requirements, low confidence decisions

**Template:**
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


---

## Pass 3: Step Contract

### Required Patterns

#### 7. Step Contract
**When:** Always (multi-step work)
**Purpose:** Make step sequence explicit and verifiable
**Tier:** 1 (Core)

**Template:**
```xml
<step_contract>
1. [Step name] → [what it produces]
2. [Step name] → [what it produces]
3. [Step name] → [what it produces]
4. [Step name] → [what it produces]
</step_contract>
```

**Quality criteria:**
- 4-8 steps (not too vague, not too granular)
- Each step is verifiable
- Clear completion criteria
- Mostly linear

**Questions:**
- What are the main steps from input to output?
- What does each step produce?
- Can you verify each step completed?

---

## Pass 4: Decision Points + Failure Modes

### Required Patterns

#### 8. Decision Points
**When:** Always (conditional logic exists)
**Purpose:** Make branching explicit, no hand-waving
**Tier:** 1 (Core)

**Template:**
```xml
<decision_points>
- If [condition_a] → [action_a]
- Else if [condition_b] → [action_b]
- Else → [fallback]
- Record chosen branch in [where]
</decision_points>
```

**Questions:**
- Where does the skill branch?
- What triggers each path?
- How is the choice recorded?

### Optional Patterns

#### 9. Scope Fence
**Triggers:** See `patterns-when-needed.md` (authoritative).
**Purpose:** Explicitly bound what's in/out of scope
**Tier:** 2 (Recommended)
**Examples:** Editing unrelated files, adding unrelated features

**Template:**
```xml
<scope_fence>
**In scope:**
- {files/areas to touch}
**Out of scope:**
- {explicit exclusions}
If boundary is crossed: ask before proceeding
</scope_fence>
```


#### 10. User Approval Gate
**Triggers:** See `patterns-when-needed.md` (authoritative).
**Purpose:** Human oversight at checkpoints
**Tier:** 2 (Recommended)
**Examples:** Deleting files, deploying to production, modifying data

**Template:**
```xml
<user_approval_gate>
Before [critical action]: present proposal + options
On approval: proceed
On rejection: [alternative action or stop]
Options: [approve / select subset / cancel]
</user_approval_gate>
```


#### 11. Confidence Signal
**Triggers:** See `patterns-when-needed.md` (authoritative).
**Purpose:** Surface uncertainty explicitly
**Tier:** 2 (Recommended)
**Examples:** Security choices, irreversible actions, business logic

**Template:**
```xml
<confidence_signal>
Thresholds:
- high (>85%): proceed
- medium (60-85%): proceed, flag for review
- low (<60%): ask before proceeding
Include: assessment + reason + what would change it
</confidence_signal>
```


#### 12. Fallback Chain
**Triggers:** See `patterns-when-needed.md` (authoritative).
**Purpose:** Recovery when primary approach fails
**Tier:** 2 (Recommended)
**Examples:** API calls, CLI tools, file dependencies

**Template:**
```xml
<fallback_chain>
primary: {preferred approach}
fallback_1: {if primary fails}
fallback_2: {if fallback_1 fails}
terminal: ERROR: {code} - surface to user
trigger: {conditions that activate fallback}
</fallback_chain>
```


#### 13. Error Handling
**Triggers:** See `patterns-when-needed.md` (authoritative).
**Purpose:** Structured error handling
**Tier:** 3 (Situational)
**Examples:** Logs contain secrets, compliance requirements

**Template:**
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

## Pass 5: Quality Gates

### Required Patterns

#### 14. Quality Gates
**When:** Always (correctness matters)
**Purpose:** Validation checkpoints between steps
**Tier:** 1 (Core)

**Template:**
```xml
<quality_gates>
G1 (after Step N): [check description]?
G2 (after Step N): [check description]?
G3 (after Step N): [check description]?
If a gate fails: [recovery action]
</quality_gates>
```

**Common gates:**
- Completeness: All required sections exist
- Consistency: No contradictions
- Format: Matches output schema
- Scope: No feature creep

**Questions:**
- What could go wrong at each step?
- What are 3-7 critical checks?
- When do you check?
- What happens on failure?

### Optional Patterns

#### 15. Review Step
**Triggers:** See `patterns-when-needed.md` (authoritative).
**Purpose:** Post-completion review for coherence
**Tier:** 2 (Recommended)
**Examples:** Multi-section documents, generated code with dependencies

**Template:**
```xml
<review_step>
criteria: completeness, consistency, format
max_cycles: 2
If issues: revise once per cycle; stop at max_cycles
</review_step>
```


#### 16. Lens
**Triggers:** See `patterns-when-needed.md` (authoritative).
**Purpose:** Analyze from multiple perspectives
**Tier:** 2 (Recommended)
**Examples:** Code review, security audit, design review

**Template:**
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


---

## Pass 6: Logging & Traceability

### All Patterns Optional

**Triggers:** See `patterns-when-needed.md` (authoritative).

#### 17. Step Ledger
**Triggers:** See `patterns-when-needed.md` (authoritative).
**Purpose:** Track execution state
**Tier:** 2 (Recommended for logging)

**Template:**
```xml
<step_ledger>
Update after each step:
- done: [completed steps]
- next: {current step}
- decisions: [choices made]
- open_issues: [blockers]
</step_ledger>
```


#### 18. Observability
**Triggers:** See `patterns-when-needed.md` (authoritative).
**Purpose:** Run metadata + step traces + artifacts
**Tier:** 3 (Situational)

**Template:**
```xml
<observability>
run_header: id, skill, mode, inputs
step_trace: [Step N] status=ok|fail outcome="..."
artifacts: named outputs per step
</observability>
```


#### 19. Assumption Registry
**Triggers:** See `patterns-when-needed.md` (authoritative).
**Purpose:** Log assumptions as made
**Tier:** 3 (Situational)

**Template:**
```xml
<assumption_registry>
Log assumptions as made:
- [Step N] assumed: "{what}" | source: {why} | confidence: {level}
Validate high-impact + low-confidence before proceeding
</assumption_registry>
```


#### 20. Context Window
**Triggers:** See `patterns-when-needed.md` (authoritative).
**Purpose:** Explicit working memory management
**Tier:** 3 (Situational)

**Template:**
```xml
<context_window>
always_include: [requirements, current step, errors]
summarize_after_use: [large file contents]
drop_after_step: [rejected alternatives]
refresh_trigger: re-read if stale (>2 steps)
</context_window>
```


---

## Pass 7: Refactor & Validate

### Required Patterns

#### 21. Stop Conditions
**When:** Always (scope could creep)
**Purpose:** Define "done" precisely
**Tier:** 1 (Core)

**Template:**
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

**Questions:**
- When is the skill completely done?
- What shouldn't it do?

### Optional Patterns

#### 22. Interpretation Check
**Triggers:** See `patterns-when-needed.md` (authoritative).
**Purpose:** Verify understanding before starting
**Tier:** 2 (Recommended)

**Template:**
```xml
<interpretation_check>
Before starting work:
- Restate understanding in own words
- List key assumptions about scope/approach
- Confirm with user before proceeding
If corrected: update understanding, re-confirm if significant
</interpretation_check>
```


---

## Pattern Selection Decision Tree

**Start here:**
1. Required patterns (7): Always include
   - Example Anchor, Output Schema, Inputs First, Step Contract, Decision Points, Quality Gates, Stop Conditions

**Then ask:**
2. Does output have multiple items? → Addressable Output
3. Multiple distinct workflows? → Mode Selection
4. Need to ask user questions? → Clarifying Questions
5. Could scope expand? → Scope Fence
6. Irreversible actions? → User Approval Gate
7. High-stakes decisions? → Confidence Signal
8. External dependencies? → Fallback Chain
9. Handle sensitive data? → Error Handling
10. Complex output needing review? → Review Step
11. Need multiple perspectives? → Lens
12. Long/complex workflow? → Step Ledger
13. Need debugging support? → Observability
14. Audit trail required? → Assumption Registry
15. Very long task? → Context Window
16. Complex/ambiguous task? → Interpretation Check

**Remember:** Every optional pattern should solve a **specific problem you've identified**. Don't add patterns "just in case."

---

## Summary

- **7 required patterns** build the core structure every skill needs
- **15 optional patterns** solve specific problems when they arise
- Build in 7 passes, adding patterns contextually
- Start minimal, add only what solves real problems
- Every pattern has a clear "when to add" criteria

The result: Skills that are executable, maintainable, and not over-engineered.
