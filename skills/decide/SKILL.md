---
name: decide
description: "Structured decision-making with 9 frameworks preventing agreeable LLM spirals. Use when comparing options, evaluating alternatives, choosing between solutions, selecting approaches, or making architectural decisions requiring structured analysis. Supports: (1) comparing 2+ alternatives, (2) high-stakes decisions (architecture, security, vendor selection), (3) analyzing risks/failure modes, (4) rigorous comparison with explicit criteria, (5) efficiency-focused 80/20 analysis. Interactive framework selection with locked recommendations and reversal guards requiring new information. Do not use for single-option decisions."
---

<objective>
Provide structured decision-making frameworks that prevent "agreeable LLM" anti-patterns by analyzing options systematically, locking decisions with clear criteria, and requiring new information for reversals.
</objective>

<problem_context>
Common failure mode:
```
User: Do A → LLM: (does A)
User: Maybe B? → LLM: You're right! (does B)
User: Or C? → LLM: Excellent! (does C)
[endless spiral, no convergence]
```

Solution: Structured analysis with explicit criteria, locked decisions, and reversal guards.
</problem_context>

<interactive_selection>
**MANDATORY: Show framework selection at start**

Before executing analysis, present interactive selection menu:

```
📋 Decision Analysis: [brief context summary]

[If auto-detected]
Detected: [X options], [stakes level], [other signals]
Auto-selected: [Framework Name] ✓

[If explicit flag provided]
Requested: [Framework Name] (via --framework or --tier flag) ✓

Available frameworks:
  1. Minimal Friction (quick defense, low ceremony) [← SELECTED if current]
  2. Enhanced Standard (balanced comparison, 5 methods)
  3. Decision Matrix (quantitative scoring, weighted criteria)
  4. Pre-Mortem (risk analysis, failure modes)
  5. Devil's Advocate (quick challenge, force defense)
  6. Constraint-Based (filter options, objective elimination)
  7. Red Team/Blue Team (adversarial analysis, both sides)
  8. Layered (mission-critical, 4-phase comprehensive)
  9. Pareto Analysis (80/20 efficiency, value vs effort)

Proceed with [Selected Framework], or choose different number (1-9)?
```

**Wait for user confirmation or override choice.**

If user provides number 1-9, use that framework.
If user says "proceed" / "yes" / "ok", use selected framework.
If user provides context suggesting different framework, switch to that.

Track chosen framework for multi-framework analysis at end.
</interactive_selection>

<interpretation_check>
Before starting framework selection, verify understanding of decision scope:

**Protocol:**
1. Restate decision context in own words
2. Confirm number of options being compared
3. Clarify if decision is single-phase (A vs B) or multi-phase (IF buy THEN which type)
4. If context ambiguous or missing key information, use AskUserQuestion to gather:
   - Primary use case or goal
   - Budget or resource constraints
   - Current state (existing solution, prior attempts)
5. Output verification to user before proceeding to framework selection

**Purpose:** Prevents misaligned analysis by catching scope ambiguity early.

**Triggers:** Decision context unclear, multiple valid interpretations, missing critical context for framework selection.
</interpretation_check>

<inputs_first>
**Required:**
- `decision_context`: What decision needs to be made (provided after /decide command)
- `options`: List of alternatives (extracted from context or asked explicitly)

**Optional flags:**
- `--tier=minimal|enhanced|matrix`: Force specific tier (legacy, maps to framework)
- `--framework=minimal|enhanced|matrix|pre-mortem|devil|constraints|red-blue|layered|pareto`: Force specific framework
- `--preset=ice`: Use Decision Matrix with Impact/Confidence/Ease criteria at equal weights (33.33% each)
- `--criteria="criterion1,criterion2,..."`: Hint at evaluation dimensions (useful for Enhanced/Matrix)
- `--context="previous_choice"`: Signal that this is a reversal/reconsideration (escalates to Enhanced)

**Auto-detection inputs:**
- Number of options, stakes keywords, risk keywords, challenge keywords, quantitative hints, contentious language
- See <decision_points> for complete auto-detection logic across all 9 frameworks

**Missing input handling:**
- If options unclear: extract from context or ask "What are you comparing?"
- If criteria unclear for Enhanced/Matrix: infer from context (performance, simplicity, cost, maintainability) or ask
- If framework unclear: use auto-detection from <decision_points> (default: Minimal Friction)
- If constraints unclear for Constraint-Based: ask "What are your must-have requirements?"
- If weights unclear for Matrix: ask user to provide weights summing to 100%

**Validation:**
- At least 2 options must exist (if only 1, decline and suggest Pre-Mortem for risk analysis)
  - **Exception:** If `--framework=pre-mortem` is explicitly provided, single-option analysis is allowed (risk assessment mode)
- Context must describe the decision domain clearly
- For Matrix: criteria weights must sum to 100% (validate and reject if not)
- For Constraint-Based: at least 1 constraint must be defined
</inputs_first>

<clarifying_questions>
**Triggers:**
- Confidence < 60% on framework selection (ambiguous context, multiple frameworks seem equally applicable)
- Missing required inputs after parsing (options unclear, criteria unclear for Matrix, constraints unclear for Constraint-Based)
- Conflicting signals (user mentions both "quick" and "rigorous" in same request)

**Constraints:**
- max_per_phase: 3 questions (batch related questions together)
- batch_related: true (combine "options unclear + criteria unclear" into one question)
- ask_before: proposing uncertain framework escalation (confidence < 60%)

**Fallback:**
- If can't ask: use default per `<decision_points>` fallback rule, document assumption in output
- If blocked: decline gracefully with explanation of what's needed

**Protocol:**
- Present questions clearly with context: "To choose the right framework, I need to know: [questions]"
- Wait for user response before proceeding
- Update inputs/context based on response
</clarifying_questions>

<step_contract>
**Core workflow (progressive with interactive selection):**

1. **Parse invocation** → Extract context, flags, options from user input
2. **Auto-detect framework** → Apply detection rules from <decision_points> OR use explicit flag (log assumptions per references/assumption-registry.md)
3. **Interactive selection** → Present all 9 frameworks, show auto-selected/requested one, wait for user confirmation or override
4. **Execute chosen framework**:
   - Minimal Friction → Quick pros/cons + defense
   - Enhanced Standard → Methods A+B+C+E+F with scores (include long-term consequences criterion)
   - Decision Matrix → Weighted scoring + sensitivity analysis (MANDATORY — see references/frameworks.md § Sensitivity Analysis)
   - Pre-Mortem → Failure modes + likelihood/impact (include 2nd/3rd order causal chains)
   - Devil's Advocate → Challenge + force defense
   - Constraint-Based → Elimination table + survivors
   - Red Team/Blue Team → Advocate + Critic + Synthesis
   - Layered → 4-phase (Constraints → Enhanced → Pre-Mortem → Lock)
   - Pareto Analysis → Value/effort scoring + efficiency ratio + "good enough" threshold
5. **Output structured analysis** → Format per framework template (track progress per references/step-ledger.md)
6. **Add mandatory lock section** → Explicit reversal criteria per <lock_protocol> (include confidence signal: Strong/Weak/Blocked per references/frameworks.md)
7. **Offer multi-framework analysis** → Present option to re-analyze with different framework

**Checkpoints:**
- After step 1: Options identified? At least 2? (else ask or decline)
- After step 2: Framework auto-detected? (else default Minimal Friction)
- After step 3: User confirmed framework choice? (else wait)
- After step 5: Output matches template for chosen framework? (else fix)
- After step 6: Lock section present and explicit? (else add)
- After step 7: If user chooses another framework → loop back to step 4 with same context

**Loop handling:**
- Track used frameworks in session (per references/step-ledger.md)
- When user selects new framework in step 7 → re-execute steps 4-7 with new framework
- Exit loop when user says "Done" in step 7
</step_contract>

<decision_points>
**Explicit routing (takes precedence over auto-detection):**
- `--framework=<name>` flag → use specified framework (shown as "Requested" in interactive selection)
- `--tier=minimal|enhanced|matrix` flag → map to framework (legacy support)
- `--preset=ice` flag → use Decision Matrix with Impact/Confidence/Ease at equal weights
- User can override any selection at interactive menu (step 3)

**Auto-detection (if no explicit --framework, --tier, or --preset flag):**

```
IF explicit --framework flag provided:
  → Use that framework (skip auto-detection and reversibility triage)

IF explicit --preset=ice flag provided:
  → Use Decision Matrix with criteria=Impact,Confidence,Ease at equal weights (33.33% each)
  → Skip auto-detection

ELSE (auto-detect):

  # Step 1: Reversibility Triage (for high-stakes keywords only)
  IF context contains (architecture|vendor|database|framework|platform|API contract):
    → Assess reversibility:
    → IF Type 1 (one-way door: vendor lock-in, data model, public API, core architecture)
      → ESCALATE to Decision Matrix or Layered
    → IF Type 2 (two-way door: internal impl, swappable components, config-driven, dev tooling)
      → DEFAULT to Minimal Friction (move fast, iterate)
    → IF Uncertain → Use Enhanced Standard (balanced analysis)
    → Show in selection: "Reversibility: [One-Way Door | Two-Way Door | Uncertain]"

  # Step 2: Standard auto-detection
  DEFAULT: Minimal Friction (lowest ceremony, appropriate for most decisions)

  ESCALATE to Pareto Analysis IF:
    → User mentions "efficient" / "quick win" / "80/20" / "good enough" / "fastest path"
    → Context suggests time/resource constraint ("need fast" / "deadline" / "limited time")
    → Options have obvious effort differences (rewrite vs refactor vs patch)
    → Risk of over-engineering detected (perfect solution exists but expensive)

  ESCALATE to Devil's Advocate IF:
    → Decision seems hasty (user says "just use X" without reasoning)
    → Context suggests quick validation needed

  ESCALATE to Constraint-Based IF:
    → 6+ options exist (need filtering first)
    → User mentions "requirements" or "must-have"
    → Clear pass/fail criteria evident

  ESCALATE to Enhanced Standard IF:
    → User asks "what's best?" / "analyze options" / "compare"
    → 4-5 options exist
    → 3 options AND (user explicitly says "compare" OR high-stakes keywords detected)
    → Context indicates previous reversal (--context="tried X before")

  ESCALATE to Decision Matrix IF:
    → User requests "rigorous" / "quantitative" / "thorough" analysis
    → Regulatory/compliance keywords mentioned
    → User provides numerical criteria or weights
    → Vendor/architecture/major technology decision

  ESCALATE to Pre-Mortem IF:
    → User mentions "risks" / "what could fail" / "failure modes"
    → Security/scaling/reliability critical

  ESCALATE to Red Team/Blue Team IF:
    → User asks to "see both sides"
    → Decision is contentious (context suggests debate)
    → User wants creative alternatives explored

  ESCALATE to Layered IF:
    → User explicitly mentions "comprehensive" + high-stakes keywords
    → Mission-critical keywords: architecture + security/scaling/irreversible
```

**Reversibility Indicators:**

Type 1 (One-Way Door - hard to reverse):
- Vendor/platform choice (switching cost: months + $$$)
- Database/data model design (migration complexity: HIGH)
- Public API contracts (breaking changes affect users)
- Core architecture patterns (rewrite required to change)

Type 2 (Two-Way Door - easy to reverse):
- Internal implementation details (no external impact)
- Swappable components behind interfaces
- Configuration-driven behavior (change via config, not code)
- Development tooling (affects dev workflow only)
- Feature-flagged functionality

**High-stakes keywords:** architecture, security, vendor, scaling, mission-critical, irreversible, compliance, regulatory

**Efficiency keywords:** efficient, quick win, 80/20, good enough, fastest path, time constraint, deadline, over-engineering

**Quantitative hints:** User mentions "weights", "%", "score", or provides numerical criteria

**Fallback:** If detection fails or ambiguous, default to **Minimal Friction** (not Enhanced).
</decision_points>

<scope_fence>
**In scope:**
- Structured analysis of 2+ options using 9 decision frameworks
- Interactive framework selection with auto-detection
- Multi-framework analysis (re-analyzing same decision with different frameworks)
- Risk analysis (Pre-Mortem) and failure mode identification
- Locking decisions with explicit reversal criteria
- Confidence signaling (Strong/Weak/Blocked)
- Sensitivity analysis for quantitative frameworks (Matrix)
- Adversarial analysis (Red Team/Blue Team, Devil's Advocate)
- Constraint-based elimination for filtering many options
- Pareto (80/20) efficiency analysis for value vs effort optimization
- ICE preset for Decision Matrix (Impact/Confidence/Ease)
- Reversibility triage for routing decisions by reversibility

**Out of scope:**
- Single-option decisions (decline, suggest normal conversation or Pre-Mortem for risk analysis)
- Implementing the chosen solution (separate step after decision is locked)
- Cross-session persistence or decision history tracking
- Multi-stakeholder voting or consensus mechanisms
- Decision templates or saved custom frameworks
- Real-time collaboration or shared decision workspaces
</scope_fence>

<frameworks>
9 frameworks available: Minimal Friction, Enhanced Standard, Decision Matrix, Pre-Mortem, Devil's Advocate, Constraint-Based, Red Team/Blue Team, Layered, Pareto Analysis. Auto-detected via `<decision_points>` or explicitly requested via `--framework` flag. ICE preset available via `--preset=ice`.

See references/frameworks.md for complete templates. See references/examples.md for worked examples.
</frameworks>

<lock_protocol>
**MANDATORY: Every decision output must end with explicit lock section.**

Apply the full lock template from references/frameworks.md § Lock Protocol. The lock must include: 🔒 header, "Chosen based on" criteria, ✅/❌ reversal examples, and "Defense Protocol Active" statement.
</lock_protocol>

<output_schema>
**Format:** Markdown with consistent structure across all frameworks

**Required sections (every framework execution must include):**
1. **Framework header** - Identifies which framework was used
2. **Analysis body** - Framework-specific content (comparison table, failure modes, etc.)
3. **Recommendation** - Clear decision with rationale
4. **Confidence signal** - Strong/Weak/Blocked with justification
5. **Lock section** - 🔒 LOCKED DECISION with reversal criteria (per lock_protocol)
6. **Multi-framework menu** - Option to re-analyze (per multi_framework_analysis)

**Section order:**
```markdown
# [Framework Name] Analysis

[Framework-specific content - varies by framework]
- Minimal Friction: pros/cons comparison
- Enhanced Standard: criteria + comparison table + scores (including long-term consequences)
- Decision Matrix: weighted matrix + sensitivity analysis
- Pre-Mortem: failure modes + likelihood/impact (including 2nd/3rd order causal chains)
- Devil's Advocate: challenge + alternatives
- Constraint-Based: elimination table + survivors
- Red Team/Blue Team: advocate + critic + synthesis
- Layered: 4-phase output
- Pareto Analysis: value/effort table + efficiency ratio + threshold check

## Recommendation
[Direct statement of chosen option with rationale anchored to analysis]

**Confidence:** [Strong/Weak/Blocked] - [Justification]

---
## 🔒 LOCKED DECISION: [Option]
[Complete lock section per lock_protocol template]
---

## 📊 Multi-Framework Analysis Option
[Multi-framework menu per multi_framework_analysis template]
```

**Validation before output:**
- All 6 required sections present?
- Framework-specific content matches template for chosen framework?
- Lock section has all 3 subsections (chosen based on, reconsider criteria, defense protocol)?
- Multi-framework menu lists only unused frameworks?

**Constraints:**
- Recommendation must be single clear option (not "A or B")
- Lock section must include concrete examples (✅/❌ format)
- Multi-framework menu must track used_frameworks from step_ledger

**Purpose:**
- Ensures consistency across 9 different framework outputs
- Prevents missing required sections (lock, confidence, multi-framework)
- Makes skill output predictable and parseable
</output_schema>

<multi_framework_analysis>
**MANDATORY: Offer re-analysis with different framework at end**

After completing decision analysis and lock section, present option to analyze same decision with different framework.

Track which frameworks have been used in this session (start with empty list, add current framework after execution).

**Template:**
```markdown
---
## 📊 Multi-Framework Analysis Option

Want to see this decision analyzed through a different lens?

**Already used:** [List of frameworks used so far]

**Available frameworks:**
[Only list frameworks NOT yet used, with brief description]
  2. [Framework Name] - [One-line description of what it offers]
  3. [Framework Name] - [One-line description]
  ...
  Done (finish analysis)

Note: Framework numbers preserve original IDs (1-9) for consistency across iterations.

Choose framework number (1-9) to re-analyze, or 'Done' to finish:
```

**Wait for user choice:**
- If user selects number (1-9) → Add current framework to "used" list, execute selected framework on SAME decision context, then offer multi-framework option again (with updated "already used" list)
- If user says "Done" / "finish" / "that's enough" → Exit, don't offer again
- Preserve original decision context across all framework executions

**Benefits:**
- Triangulation: See same decision through multiple analytical lenses
- Higher confidence: Matrix + Pre-Mortem might reveal risks quantitative analysis misses
- Comparison: Red Team/Blue Team might surface alternatives Enhanced Standard overlooked
- Learning: User sees how different frameworks approach same problem

**Limit:** After 4 frameworks used, suggest "Done" more explicitly: "You've analyzed this with 4 frameworks. Recommend finishing unless you want additional perspective."
</multi_framework_analysis>

<quality_gates>
**Gate 1** (after parsing invocation):
- Options identified? At least 2? (else ask via clarifying_questions or decline)
- Decision context clear? (else ask for clarification via clarifying_questions)
- Step ledger initialized? (framework_iteration=0, used_frameworks=[])

**Gate 2** (after auto-detection):
- Framework auto-detected OR explicit flag provided? (else default Minimal Friction)
- Auto-detection assumptions logged in assumption_registry?
- Step ledger updated with framework choice?

**Gate 3** (after interactive selection):
- User confirmed framework choice? (else wait for response)
- Framework valid (1-9)? (else ask again)

**Gate 4** (after framework execution):
- Output matches output_schema structure? (6 required sections present)
- Output matches template for chosen framework? (else fix format)
- Scores calculated correctly? (check math for Enhanced/Matrix)
- All framework-specific sections present? (else add missing)
- For Matrix: Criteria weights sum to 100%? (else recalculate)
- For Matrix: Sensitivity analysis included? (else add)
- For Pareto: Value and Effort scores present for all options? (else add)
- For Pareto: Efficiency ratio calculated (Value/Effort)? (else calculate)
- For Pareto: "Good enough" threshold check present? (else add)
- Step ledger updated with framework completion?

**Gate 5** (after analysis, before lock):
- Confidence signal present (Strong/Weak/Blocked)? (else add)
- Recommendation anchored to criteria/scores? (else add rationale)

**Gate 6** (after adding lock section):
- Lock section present with 🔒 header? (else add)
- "Chosen based on" lists specific criteria? (else make explicit)
- Reversal criteria has concrete examples (✅/❌)? (else add examples)
- Defense protocol statement present? (else add)

**Gate 7** (before multi-framework offer):
- All previous gates passed? (else fix issues first)
- Multi-framework menu presented? (else add)
- Step ledger updated with used_frameworks list? (else update)
- Multi-framework menu excludes already-used frameworks? (cross-check with step_ledger)

**Failure handling:**
- Gate fails → fix once → re-check
- If still fails after fix → report what's missing and ask user for clarification
- Critical failures (no options, invalid context) → decline gracefully with explanation
</quality_gates>

<review_step>
After framework execution, perform holistic review:

**Criteria (apply to all frameworks):**
- Output matches output_schema structure (6 required sections)
- Output matches template for selected framework (Minimal Friction/Enhanced Standard/Decision Matrix/Pre-Mortem/Devil's Advocate/Constraint-Based/Red Team Blue Team/Layered/Pareto Analysis)
- All required sections present per framework template
- Confidence signal present (Strong/Weak/Blocked) with justification
- Lock section present with 🔒 header and complete structure
- Lock section has "Chosen based on" with specific criteria
- Lock section has concrete reversal examples (✅/❌ format)
- Lock section has "Defense Protocol Active" statement
- Recommendation anchored to criteria/scores/analysis
- Step ledger updated throughout execution (current step, framework_iteration, used_frameworks, decisions)
- Assumption registry shows auto-detection logic (if auto-detected framework)

**Framework-specific criteria:**
- **Enhanced Standard:** Criterion scores (0-10) calculated correctly, comparison table complete, Methods A+B+C+E+F applied, long-term consequences (2nd/3rd order) criterion included
- **Decision Matrix:** Criteria weights sum to 100%, weighted scores computed accurately, sensitivity analysis included (winner vs runner-up, weight change threshold)
- **Pre-Mortem:** 3-5 failure modes identified, likelihood/impact assessment (HIGH/MEDIUM/LOW) present, mitigations suggested, 2nd/3rd order causal chains traced for each failure mode
- **Devil's Advocate:** Challenge presented with 3+ concerns, alternatives suggested, user defense solicited
- **Constraint-Based:** Pass/fail table complete, elimination logic clear, survivors identified or constraint relaxation requested
- **Red Team/Blue Team:** Both advocate and critic perspectives for each option, synthesis conditions clear
- **Layered:** All 4 phases executed in order, phase outputs present, final lock from Phase 4
- **Minimal Friction:** Quick pros/cons, direct recommendation with minimal ceremony
- **Pareto Analysis:** Value (0-10) and Effort (0-10) scored for all options, efficiency ratio (Value/Effort) calculated, Pareto winner identified, "good enough" threshold check present, context check (time constraint, minimum acceptable value) included

**Max_cycles:** 1 (default)

**Per cycle:**
1. Identify issues:
   - Format mismatches (template not followed)
   - Missing sections (lock, confidence, sensitivity for Matrix, etc.)
   - Math errors (scores, weights, calculations)
   - Vague lock conditions (no concrete examples)
   - Missing framework-specific elements
2. Revise to fix issues
3. Re-check against criteria

**Exit conditions:**
- No issues found → proceed to multi-framework offer
- Max_cycles reached → report remaining issues, note what couldn't be auto-corrected, ask user if they want to proceed anyway

**If issues remain at max_cycles:**
Output format:
```
⚠️ Review Issues Detected:
- [Issue 1]
- [Issue 2]

Attempted auto-correction but [reason why couldn't fix].
Proceed with current output or revise manually?
```
</review_step>

<edge_cases>
Handle special scenarios across all 9 frameworks including single option, tied scores, conflicting flags, missing criteria, and more. See references/edge-cases.md for complete reference (13 cases).
</edge_cases>

<examples>
See references/examples.md for worked examples across all 9 frameworks, including Pareto Analysis and ICE preset examples. See references/frameworks.md for complete template specifications.
</examples>

<stop_conditions>
**Per-framework iteration complete when:** All quality gates (1-7) pass.

**Skill exits when:**
- User chooses "Done" / "finish" / "that's enough" in multi-framework menu
- OR user has used 4+ frameworks and chooses to stop
- OR critical error prevents continuation (no options, invalid context)

**Do NOT exit after first framework** — always offer multi-framework analysis option unless user explicitly chooses "Done."
</stop_conditions>
