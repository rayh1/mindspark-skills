# Complete Pattern Scaffold: All 22 Patterns

This reference documents how all 22 reliability patterns map to the 7-pass creation methodology.

**Key principle:** Required patterns build core structure. Optional patterns solve specific problems.

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

**Note on tiers:** Tier assignments (which patterns are "core" vs "optional") vary by context. When creating new skills (this document), see the pass-by-pass guidance below. When retrofitting existing skills, see `apply-patterns/references/minimal-templates.md`.

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
**When:** Output has multiple items needing follow-up discussion
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

**When to add:**
- Reviews/audits with multiple findings
- Analysis tasks with recommendations
- Error reports with multiple issues

#### 4. Mode Selection
**When:** Skill supports multiple distinct workflows
**Purpose:** Present workflow choices upfront
**Tier:** 3 (Situational)
**Examples:** build vs debug, lite vs full, create vs modify

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

**When to add:**
- Multiple workflows (not just variations)
- Different use cases need different steps
- User needs to choose approach upfront

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
**When:** Skill needs to ask user when blocked or uncertain
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

**When to add:**
- Requirements often ambiguous
- High-stakes decisions need confirmation
- Skill serves non-technical users

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
**When:** Scope could expand beyond intended boundaries
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

**When to add:**
- File editing/modification tasks
- Refactoring work
- Any task where scope creep is likely

#### 10. User Approval Gate
**When:** Irreversible actions or significant changes
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

**When to add:**
- Destructive operations
- Production deployments
- Data modifications
- Any irreversible action

#### 11. Confidence Signal
**When:** High-stakes decisions where uncertainty should be surfaced
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

**When to add:**
- Security-sensitive decisions
- Business logic choices
- Complex technical trade-offs
- Medical/legal/financial domains

#### 12. Fallback Chain
**When:** External dependencies that might fail
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

**When to add:**
- External API calls
- Tool dependencies
- Network operations
- File system operations that might fail

#### 13. Error Handling
**When:** Failures need taxonomy or sensitive data redaction
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

**When to add:**
- Handles sensitive data
- Compliance/audit requirements
- Complex error scenarios
- Needs error categorization

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
**When:** Output complex enough to need holistic coherence check
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

**When to add:**
- Complex multi-part outputs
- Generated code that must compile
- Documents needing coherence
- Outputs with cross-references

#### 16. Lens
**When:** Need multi-perspective analysis to avoid tunnel vision
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

**When to add:**
- Review/audit tasks
- Security analysis
- Design reviews
- Risk of missing important aspects

---

## Pass 6: Logging & Traceability

### All Patterns Optional

**When to add any logging:** Only if workflow is long/complex (10+ steps), debugging needed, or audit trail required.

#### 17. Step Ledger
**When:** Long/complex workflows need running state
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

**When to add:**
- Workflows long enough to drift
- Need to resume after interruption
- Complex dependencies between steps

#### 18. Observability
**When:** Need debugging/comparison of runs
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

**When to add:**
- Debugging complex workflows
- Comparing different runs
- Production monitoring
- Performance analysis

#### 19. Assumption Registry
**When:** Need to track assumptions for audit/compliance
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

**When to add:**
- Regulated domains
- Audit requirements
- High-stakes decisions
- Need assumption traceability

#### 20. Context Window
**When:** Very long tasks with shifting focus
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

**When to add:**
- Multi-hour tasks
- Working across many files
- Context switching between domains
- Memory management critical

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
**When:** Task complex/ambiguous enough to need pre-flight verification
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

**When to add:**
- Vague requirements
- Multiple interpretations possible
- High-stakes work
- First time working with user

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
