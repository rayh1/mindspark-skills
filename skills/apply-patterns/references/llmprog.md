# LLM Programming: Patterns for Reliable AI Workflows

## Table of Contents

- [Pattern Overview](#pattern-overview) - Lines 17-53
- **Tier 1 - Core Patterns** - Lines 56-274
  - [1) Step Contract](#1-step-contract) - Line 58
  - [2) Inputs First](#2-inputs-first) - Line 85
  - [3) Decision Points](#3-decision-points) - Line 110
  - [4) Quality Gates](#4-quality-gates) - Line 135
  - [5) Stop Conditions](#5-stop-conditions) - Line 162
  - [6) Scope Fence](#6-scope-fence) - Line 186
  - [7) Interpretation Check](#7-interpretation-check) - Line 216
  - [8) Output Schema](#8-output-schema) - Line 247
- **Tier 2 - Recommended Patterns** - Lines 276-540
  - [9) User Approval Gate](#9-user-approval-gate) - Line 278
  - [10) Clarifying Questions](#10-clarifying-questions) - Line 312
  - [11) Confidence Signal](#11-confidence-signal) - Line 343
  - [12) Fallback Chain](#12-fallback-chain) - Line 373
  - [13) Lens](#13-lens) - Line 401
  - [14) Addressable Output](#14-addressable-output) - Line 447
  - [15) Review Step](#15-review-step) - Line 479
  - [16) Step Ledger](#16-step-ledger) - Line 515
- **Tier 3 - Situational Patterns** - Lines 542-762
  - [17) Mode Selection](#17-mode-selection) - Line 544
  - [18) Example Anchor](#18-example-anchor) - Line 578
  - [19) Assumption Registry](#19-assumption-registry) - Line 627
  - [20) Context Window](#20-context-window) - Line 655
  - [21) Observability](#21-observability) - Line 681
  - [22) Error Handling](#22-error-handling) - Line 722
- [Applying Patterns: Quick Guide](#applying-patterns-quick-guide) - Lines 764-791
- [Example: Hardened Prompt](#example-hardened-prompt) - Line 793

---

LLM programming treats a language model as a **runtime** for **structured instructions**. You write "programs" in natural language that specify inputs, steps, decisions, checks, and outputs. The goal: produce results that are **repeatable enough**, **correct enough**, and **maintainable enough** to rely on—despite inherent probabilistic behavior.

This catalog organizes 22 patterns into three tiers:

- **Tier 1 - Core (8 patterns):** High-yield, low-overhead. Apply to any non-trivial workflow.
- **Tier 2 - Recommended (8 patterns):** Valuable when the situation warrants. Check applicability.
- **Tier 3 - Situational (6 patterns):** Specific use cases. Don't apply by default.

**Warning:** An LLM is not a deterministic interpreter. Even well-structured prompts can fail under long context, ambiguous inputs, or conflicting constraints. Treat outputs as **provisional**—add gates, keep logs, and expect iteration.

---

## Pattern Overview

### Tier 1 - Core

| # | Pattern | Purpose |
|---|---------|---------|
| 1 | Step Contract | Make step sequence explicit |
| 2 | Inputs First | Declare required inputs before work |
| 3 | Decision Points | Make branching explicit |
| 4 | Quality Gates | Validation checkpoints between steps |
| 5 | Stop Conditions | Define "done" precisely |
| 6 | Scope Fence | Bound what's in/out of scope |
| 7 | Interpretation Check | Verify understanding before starting |
| 8 | Output Schema | Define required output structure |

### Tier 2 - Recommended

| # | Pattern | Purpose |
|---|---------|---------|
| 9 | User Approval Gate | Human oversight at checkpoints |
| 10 | Clarifying Questions | Protocol for asking user |
| 11 | Confidence Signal | Surface uncertainty explicitly |
| 12 | Fallback Chain | Recovery when primary approach fails |
| 13 | Lens | Multi-perspective analysis |
| 14 | Addressable Output | Unique IDs for output items |
| 15 | Review Step | Holistic post-completion check |
| 16 | Step Ledger | Running execution state |

### Tier 3 - Situational

| # | Pattern | Purpose |
|---|---------|---------|
| 17 | Mode Selection | Present workflow choices upfront |
| 18 | Example Anchor | Ground behavior with examples |
| 19 | Assumption Registry | Log assumptions as made |
| 20 | Context Window | Working memory policy |
| 21 | Observability | Run metadata + step traces + artifacts |
| 22 | Error Handling | Error classification + redaction |

---

# Tier 1 - Core Patterns

## 1) Step Contract

**Purpose:** Make the step sequence explicit so the model follows a clear workflow.

**When to use:**
- Multi-step tasks
- Work that could be done in wrong order
- When you need predictable execution flow

**Template:**

```text
Step Contract:
1) Parse inputs → extract {goal, constraints}
2) Plan → produce step plan
3) Execute → produce artifacts
4) Verify → run checks
5) Finalize → deliver output
```

**Anti-patterns:**
- Steps too vague ("do the work")
- Steps too granular (20+ micro-steps)
- Missing input/output expectations per step

---

## 2) Inputs First

**Purpose:** Force all required inputs to be declared before work starts. Surface missing information early.

**When to use:**
- Any task that takes inputs
- When hidden assumptions cause late-stage rework
- Tasks with optional parameters that need defaults

**Template:**

```text
Inputs First:
required: repo_url, target_branch, success_criteria
provided: repo_url ✓, target_branch ✓
missing: success_criteria → assume: "tests pass + lint clean"
```

**Anti-patterns:**
- Starting work before confirming inputs exist
- Silent defaults without documenting them
- Asking for inputs one at a time (batch upfront)

---

## 3) Decision Points

**Purpose:** Make branching explicit so the model doesn't silently improvise.

**When to use:**
- Workflow can branch based on conditions
- Different inputs require different approaches
- When you need to audit why a path was chosen

**Template:**

```text
Decision Points:
If task_type == "bugfix" → prioritize minimal change + regression tests
Else if "feature" → add spec + acceptance tests
Chosen: bugfix
```

**Anti-patterns:**
- Implicit branching (model decides without recording)
- Too many branches (decision tree becomes unmanageable)
- Branches without clear conditions

---

## 4) Quality Gates

**Purpose:** Add validation checks between steps to catch errors early.

**When to use:**
- Correctness matters
- Errors in early steps propagate to later steps
- When you need pass/fail checkpoints

**Template:**

```text
Quality Gates:
G1 (after Parse): inputs validated?
G2 (after Execute): output matches schema?
G3 (after Verify): all tests pass?

If gate fails: fix once → re-check; else stop with ERROR: GATE_FAIL
```

**Anti-patterns:**
- Gates without clear pass/fail criteria
- Too many gates (overhead exceeds value)
- No recovery action defined for failures

---

## 5) Stop Conditions

**Purpose:** Define "done" precisely to prevent endless elaboration.

**When to use:**
- Scope could creep
- Task could expand indefinitely
- When you need explicit boundaries on what NOT to do

**Template:**

```text
Stop Conditions:
done when: all steps complete, all gates pass, open_issues empty
don't: add new features, change unrelated code, refactor for style
```

**Anti-patterns:**
- Vague completion criteria ("when it's good")
- No explicit "don't" list
- Done conditions that can't be objectively verified

---

## 6) Scope Fence

**Purpose:** Bound what's in-scope and out-of-scope before work begins.

**When to use:**
- Task could expand beyond intended boundaries
- Working in sensitive areas (auth, payments)
- When others own adjacent code

**Template:**

```text
Scope Fence:
in_scope:
  - src/auth/login.ts
  - src/auth/session.ts
  - tests/auth/*.test.ts
out_of_scope:
  - database migrations
  - UI components
boundary_violation: note desire, ask before proceeding
```

**Anti-patterns:**
- Vague boundaries ("just the auth stuff")
- No violation handling defined
- Scope fence that's too restrictive to do the work

---

## 7) Interpretation Check

**Purpose:** Verify understanding before starting work, even when confident.

**When to use:**
- Complex or multi-part requests
- Ambiguous language
- High-stakes tasks where misunderstanding is costly
- Requests from unfamiliar domains

**Template:**

```text
Interpretation Check:
restatement: |
  You want me to add input validation to the registration form,
  specifically checking email format and password strength.
key_interpretations:
  - scope: registration form only (not login)
  - approach: client-side with server-side backup
  - success: invalid inputs show inline errors
confirmation: "Is this correct, or should I adjust?"
```

**Anti-patterns:**
- Echoing verbatim instead of paraphrasing
- Skipping when confident (confidence ≠ correctness)
- Asking for confirmation on trivial requests

---

## 8) Output Schema

**Purpose:** Define required output structure and format for consistency.

**When to use:**
- Outputs need to be parsed by other systems
- Format consistency matters across runs
- Outputs have multiple required sections

**Template:**

```text
Output Schema:
format: markdown
required_sections: [Summary, Changes, Test Plan]
constraints:
  Summary: 2-3 sentences, no jargon
  Changes: bullet list with file:line refs
  Test Plan: numbered steps with expected results
validation: all sections present before output
```

**Anti-patterns:**
- Schema without validation step
- Over-specified schema that's hard to satisfy
- No constraints on section content

---

# Tier 2 - Recommended Patterns

## 9) User Approval Gate

**Purpose:** Pause for human approval before proceeding at critical moments.

**When to use:**
- Before irreversible actions (delete, deploy, publish)
- After completing significant phases
- When confidence is low on high-impact decisions

**Template:**

```text
User Approval Gate:
triggers:
  - before: delete, deploy, publish
  - after: implementation phase complete
  - when: confidence < 70% on high-impact decision

presentation:
  summary: what was done
  changes: list of modifications
  risks: concerns or uncertainties

options: [approve] [modify] [reject]
on_reject: preserve work, discuss alternatives
```

**Anti-patterns:**
- Approval for every small change (fatigue)
- Vague presentations ("made some changes")
- Proceeding without approval on triggered items

---

## 10) Clarifying Questions

**Purpose:** Define when and how to ask the user for missing information.

**When to use:**
- Requirements are incomplete
- Consequential decisions with missing info
- Interactive workflows where user is available

**Template:**

```text
Clarifying Questions:
triggers:
  - confidence < 60% on consequential decision
  - ambiguous requirement with multiple interpretations
constraints:
  max_per_phase: 3
  batch_related: true
  ask_before: irreversible changes
format: context + specific question + options + default
fallback: use safe default, document in assumptions
```

**Anti-patterns:**
- Asking without searching first (answer may exist)
- Vague questions ("what should I do?")
- Asking too many at once (>3 feels like interrogation)

---

## 11) Confidence Signal

**Purpose:** Surface uncertainty explicitly with action thresholds.

**When to use:**
- Decisions with significant consequences
- Information is incomplete or ambiguous
- Human judgment would be valuable

**Template:**

```text
Confidence Signal:
assessment: medium (70%)
reason: "Multiple valid approaches; chose X based on convention"
thresholds:
  high (>85%): proceed
  medium (60-85%): proceed, flag for review
  low (<60%): ask before proceeding
increase_if: user confirms approach
decrease_if: find contradicting evidence
```

**Anti-patterns:**
- Confidence without reasoning
- Binary confident/not-confident (use spectrum)
- No action thresholds defined

---

## 12) Fallback Chain

**Purpose:** Define recovery strategies when the primary approach fails.

**When to use:**
- External dependencies (APIs, tools, files)
- Partial success is acceptable
- "Best effort" is better than complete failure

**Template:**

```text
Fallback Chain:
primary: call external API for live data
fallback_1: use cached response (if <1hr old)
fallback_2: use default values + flag as degraded
terminal: ERROR: DATA_UNAVAILABLE - surface to user

trigger: timeout >5s, rate limit (429), server error (5xx)
```

**Anti-patterns:**
- No terminal state (infinite fallback)
- Fallbacks that hide failures silently
- Not flagging degraded responses

---

## 13) Lens

**Purpose:** Analyze from multiple explicit perspectives for comprehensive coverage.

**When to use:**
- Code review (security, performance, maintainability)
- Architecture analysis (scalability, cost, complexity)
- Bug investigation (data flow, concurrency, timing)
- Any task where tunnel vision is a risk

**Template:**

```text
Lens:
perspectives:
  security: [input validation, auth boundaries, injection vectors]
  performance: [complexity, I/O patterns, caching]
  maintainability: [coupling, naming, test coverage]

order: security → performance → maintainability
per_lens: findings + severity + confidence
synthesis: merge findings, flag conflicts, resolve
coverage: each lens must report (even "no issues")
```

**Example (code review):**

```text
Lens Log:
  [security] 1 finding: SQL injection in searchUsers() (critical)
  [performance] 1 finding: N+1 query in getUserOrders() (major)
  [maintainability] 0 findings: no issues

  Synthesis: 2 issues, 1 critical
  Conflicts: none
  Recommendation: block on security fix
```

**Anti-patterns:**
- Too many lenses (3-5 is sufficient)
- Lenses with significant overlap
- Skipping synthesis (just listing findings)
- Unequal depth across lenses

---

## 14) Addressable Output

**Purpose:** Assign unique IDs to output items for easy reference in follow-up.

**When to use:**
- Output contains multiple discrete items
- Follow-up discussion or action expected
- Items need individual status tracking

**Template:**

```text
Addressable Output:
id_format: "[{prefix}-{number}]"
prefixes: { finding: F, recommendation: R, question: Q }
presentation: inline prefix

Example output:
[F-1] SQL injection in searchUsers() - critical
[F-2] N+1 query in getUserOrders() - major
[R-1] Use parameterized queries (addresses F-1)

Follow-up: "Fix F-1 first, defer F-2"
```

**Anti-patterns:**
- IDs too long (keep short: F-1 not FINDING-2024-001)
- IDs that change between conversations
- Assigning IDs to items that won't be referenced

---

## 15) Review Step

**Purpose:** Perform holistic review after completing main work, with bounded revision.

**When to use:**
- Multi-step workflows where outputs must fit together
- Tasks where coherence matters (documents, plans)
- Workflows prone to accumulated errors

**Template:**

```text
Review Step:
criteria:
  - all required sections present
  - no contradictions between sections
  - examples match described behavior
max_cycles: 2
exit_when: no issues found OR max_cycles reached

Cycle 1:
  issues: ["section 3 contradicts section 5"]
  action: revise section 3 → re-review

Cycle 2:
  issues: []
  status: PASS
```

**Anti-patterns:**
- Unbounded cycles ("keep reviewing until perfect")
- Vague criteria ("is it good?")
- Reviewing too early (that's what Quality Gates are for)

---

## 16) Step Ledger

**Purpose:** Maintain running state to prevent drift and track progress.

**When to use:**
- Long-running tasks
- Complex multi-step work
- When you need to audit what was decided and when

**Template:**

```text
Step Ledger:
done: [Parse inputs, Plan]
next: Execute
decisions: [use schema v2, skip rate limiting for MVP]
assumptions: [Node.js ≥18, PostgreSQL database]
open_issues: [missing error handling spec]
```

**Anti-patterns:**
- Ledger that's not updated after each step
- Too much detail (keep it concise)
- Duplicating information from other patterns

---

# Tier 3 - Situational Patterns

## 17) Mode Selection

**Purpose:** Present workflow or input choices upfront for user to select.

**When to use:**
- Skill supports multiple workflows (create/edit/review)
- Multiple input sources valid (file/paste/clipboard)
- User preference should drive the choice

**Template:**

```text
Mode Selection:
workflow_options:
  - create: "Start from scratch"
  - revise: "Improve existing"
  - review: "Analyze without changes"

input_options:
  - default: "Use ./config.json"
  - path: "Enter file path"
  - paste: "Paste content"

skip_when: request explicitly specifies mode
default: infer from context, else ask
```

**Anti-patterns:**
- Too many options (decision paralysis)
- Always asking (even when obvious from context)
- Options that aren't actually supported

---

## 18) Example Anchor

**Purpose:** Ground behavior with concrete good/bad examples.

**When to use:**
- Output format/style is hard to describe abstractly
- Consistency with existing patterns matters
- Avoiding specific anti-patterns

**Template:**

````text
Example Anchor:

GOOD:
```typescript
async function fetchUser(id: string): Promise<User> {
  const response = await api.get(`/users/${id}`);
  if (!response.ok) {
    throw new ApiError(`Failed to fetch user ${id}`, response.status);
  }
  return response.json();
}
// Why good: explicit error type, informative message, typed
```

BAD:
```typescript
async function fetchUser(id) {
  try {
    return (await fetch(`/users/${id}`)).json();
  } catch (e) {
    console.log(e);
    return null;
  }
}
// Why bad: swallows errors, returns null ambiguously, no types
```

Instruction: Match error handling style of GOOD example.
````

**Anti-patterns:**
- Examples without annotation of why good/bad
- Too many examples (1-2 per concern is enough)
- Examples that don't match the target context

---

## 19) Assumption Registry

**Purpose:** Track assumptions made during execution for auditability.

**When to use:**
- Tasks with incomplete information
- When "why did it do that?" needs answering
- Debugging where root cause is unclear

**Template:**

```text
Assumption Registry:
[Step 1] assumed: "project uses ESM" | source: package.json | confidence: high
[Step 2] assumed: "auth uses JWT" | source: imports | confidence: high
[Step 3] assumed: "user wants minimal changes" | source: not specified | confidence: medium

High-impact + low-confidence (validate before proceeding):
- [Step 3] "user wants minimal changes" → asked user → confirmed
```

**Anti-patterns:**
- Logging every trivial assumption
- No confidence assessment
- Not validating high-impact/low-confidence assumptions

---

## 20) Context Window

**Purpose:** Define policy for managing working memory in long tasks.

**When to use:**
- Long-running tasks with many steps
- Tasks that read many files
- When stale context causes confusion

**Template:**

```text
Context Window:
always_include: [requirements, current step, error messages, decisions]
summarize_after_use: [file contents → keep only relevant functions]
drop_after_step: [rejected alternatives, superseded plans]
refresh_trigger: if referencing file changed >2 steps ago, re-read
```

**Anti-patterns:**
- Keeping everything (context pollution)
- Dropping too aggressively (losing needed info)
- No refresh strategy for stale data

---

## 21) Observability

**Purpose:** Make runs inspectable and debuggable with metadata, traces, and artifacts.

**When to use:**
- Runs need to be compared or debugged
- Reproducibility matters
- When you need to see "where did it go wrong?"

**Components:**

**Run Header:**
```text
run_id: 2026-01-18T14:30Z-a3f2
skill: apply-patterns-v2
mode: lite
inputs: { target: "skill.md" }
```

**Step Trace:**
```text
[Step 1: Parse] status=ok outcome="detected skill type"
[Step 2: Analyze] status=ok outcome="3 patterns applicable"
[Step 3: Apply] status=fail outcome="merge conflict in inputs section"
```

**Artifact Snapshots:**
```text
Artifacts:
- analysis.md (from Step 2)
- proposed_changes.diff (from Step 3)
- error_log.txt (from Step 3)
```

**Anti-patterns:**
- Logging without structure (can't parse later)
- Missing run identifiers (can't correlate)
- Artifacts without stable names

---

## 22) Error Handling

**Purpose:** Classify failures systematically and handle sensitive data in logs.

**When to use:**
- Failures need classification for systematic improvement
- Logs may contain sensitive data
- When you need to know "what type of error was this?"

**Components:**

**Error Taxonomy:**
```text
Error codes:
- INPUT_MISSING: required input not provided
- INPUT_INVALID: input fails validation
- TOOL_TIMEOUT: external tool didn't respond
- GATE_FAIL: quality gate failed after retry
- CONSTRAINT_VIOLATION: output violates requirements

Format: ERROR: {code} - {context} - {suggested action}
Example: ERROR: GATE_FAIL - tests failing after fix - review test assumptions
```

**Redaction Rules:**
```text
Redaction:
- api_key → "[REDACTED]"
- password → "[REDACTED]"
- email → hash(email)
- request_bodies → schema + length only

Apply to: all logs, artifacts, error messages
```

**Anti-patterns:**
- Unclassified errors ("something went wrong")
- Sensitive data in logs
- Error codes without suggested actions

---

# Applying Patterns: Quick Guide

## Lite Mode (2-4 patterns)

Start with these, pick what applies:

1. **Inputs First** - almost always
2. **Step Contract** - if multi-step
3. **Scope Fence** - if boundaries matter
4. **Output Schema** - if output structure matters

## Standard Mode (3-6 patterns)

Add from Tier 2 based on situation:

- **User Approval Gate** - if irreversible actions
- **Lens** - if analysis/review task
- **Confidence Signal** - if consequential decisions
- **Review Step** - if output coherence matters

## Full Mode (comprehensive)

Audit against all 22 patterns. Prioritize:
- Must-have: blocks correctness if missing
- Should-have: improves reliability significantly
- Optional: nice to have, lower ROI

---

# Example: Hardened Prompt

**Before (weak):**
```text
Build a REST endpoint for creating invoices. Make it secure and well-tested.
```

**After (with Tier 1 patterns):**
```text
Inputs First:
required: language, framework, auth_method, db_type, invoice_schema
optional: endpoint_path (default: POST /invoices)
missing: ask up to 3 questions, then use safe defaults

Step Contract:
1) Clarify requirements → confirm assumptions
2) Design endpoint contract → request/response/errors
3) Implement → code + validation
4) Test → unit + integration
5) Document → API docs + deploy notes

Scope Fence:
in: invoice creation endpoint + tests + docs
out: billing, payments, email notifications, schema migrations
boundary: note desire, ask before crossing

Quality Gates:
G1: contract explicitly defined?
G2: auth/authz enforced?
G3: server-side validation complete?
G4: tests cover happy path + key failures?
If fail: fix once, re-check; else ERROR: GATE_FAIL

Stop Conditions:
done: steps 1-5 complete, all gates pass
don't: add features beyond invoice creation, refactor unrelated code

Output Schema:
format: markdown
sections: [Contract, Implementation Notes, Test Plan, Docs]

Task: Build the invoice creation endpoint.
```

This version is explicit about inputs, steps, boundaries, validation, and completion—reducing drift and improving reliability.
