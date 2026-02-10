# Decision Framework Templates

Complete template specifications for all nine decision frameworks in the /decide skill.

## Table of Contents

- [Confidence Signal Criteria](#confidence-signal-criteria)
- [Minimal Friction Framework](#minimal-friction-framework)
- [Enhanced Standard Framework](#enhanced-standard-framework)
- [Decision Matrix Framework](#decision-matrix-framework)
- [Pre-Mortem Framework](#pre-mortem-framework)
- [Devil's Advocate Framework](#devils-advocate-framework)
- [Constraint-Based Framework](#constraint-based-framework)
- [Red Team/Blue Team Framework](#red-team-blue-team-framework)
- [Layered Framework](#layered-framework)
- [Pareto Analysis Framework](#pareto-analysis-framework)

---

## Confidence Signal Criteria

Every recommendation must include confidence level:

**Strong:** Clear winner, high confidence, tradeoffs favor one option decisively
- Use when: Score gap >15%, or one option dominates on key criteria

**Weak:** Close call, tradeoffs balanced, genuine ambiguity
- Use when: Score gap <15%, or competing options excel in different dimensions
- Add: "This comes down to your preference on [subjective dimension]"

**Blocked:** Insufficient information to recommend
- Use when: Missing critical information, criteria unclear, options underspecified
- Add: "Need more information: [specific what's missing]"

**Rationale:** Sets appropriate user expectations about decision certainty.

---

## Lock Protocol Template

**MANDATORY: Every decision output must end with this lock section, regardless of framework.**

```markdown
---
## 🔒 LOCKED DECISION: [Chosen Option]

**Chosen based on:**
- [Specific criterion 1]: [Why it mattered and how chosen option scored]
- [Specific criterion 2]: [Why it mattered and how chosen option scored]
- [Specific criterion 3]: [Why it mattered and how chosen option scored]

**To reconsider this decision, provide:**
- ❌ "What about [Y]?" → **BLOCKED** (no new information)
- ❌ "Maybe [Z] is better?" → **BLOCKED** (no changed context)
- ✅ **New requirement:** [Concrete example: "Must now handle 10k users instead of 1k"]
- ✅ **New constraint:** [Concrete example: "Team lost Python expert, only JS devs remain"]
- ✅ **Changed priority:** [Concrete example: "Speed now more important than maintainability due to deadline"]

**Defense Protocol Active:**
Without new information (requirement/constraint/priority change), this decision is locked.
Will actively defend against weak reversals and require explicit justification for any changes.
---
```

This lock section creates friction for reversals and prevents agreeable spiral anti-pattern.

---

## Minimal Friction Framework

**Used for:** 2-3 options, simple tradeoffs, low stakes

### Steps

1. List each option with brief pros/cons (2-3 per side)
2. Recommend winner with 1-sentence rationale
3. Signal confidence (Strong/Weak/Blocked)
4. Lock with reversal criteria

### Template

```markdown
## Decision: [Title]

### Option A: [Name]
✓ Pros: [strength 1], [strength 2]
✗ Cons: [weakness 1], [weakness 2]

### Option B: [Name]
✓ Pros: [strength 1], [strength 2]
✗ Cons: [weakness 1], [weakness 2]

### Recommendation: [Winner]
**Confidence**: Strong/Weak/Blocked
**Rationale**: [Why this wins in 1 sentence]

**Lock Condition**:
To reconsider, provide:
- New requirement: [example]
- New constraint: [example]
- Changed priority: [example]
```

---

## Enhanced Standard Framework

**Used for:** 3-5 options, moderate complexity, standard decisions

**Methods applied:**
- **Method A**: Establish criteria upfront
- **Method B**: Comparative analysis with scoring (0-10 per criterion)
- **Method C**: Defensive behavior (defend recommendation)
- **Method E**: Require new information for reversals
- **Method F**: Confidence signaling

### Steps

1. Define criteria (ask user or infer from context) - priority order. Always include "Long-Term Consequences" criterion.
2. For each option: list pros, cons, score each criterion (0-10). For long-term consequences, trace 2nd/3rd order effects.
3. Calculate total scores
4. Recommend winner based on highest score + criteria alignment
5. Signal confidence level
6. Defend: explain why alternatives don't win
7. Lock with specific reversal criteria

### Template

```markdown
## Decision: [Title]

### Criteria (Priority Order)
1. [Criterion 1]: [Why this matters]
2. [Criterion 2]: [Why this matters]
3. [Criterion 3]: [Why this matters]

### Options Analyzed

#### Option A: [Name]
✓ Pros:
  - [Specific strength 1]
  - [Specific strength 2]

✗ Cons:
  - [Specific weakness 1]
  - [Specific weakness 2]

**Scores:**
- [Criterion 1]: X/10
- [Criterion 2]: Y/10
- [Criterion 3]: Z/10
**Total**: XX/30

[Repeat for each option]

### Recommendation: [Winner]

**Confidence**: Strong/Weak/Blocked

**Rationale**: [Why this option wins based on criteria scores and priorities]

**Why not alternatives:**
- [Option B]: [Specific reason it loses]
- [Option C]: [Specific reason it loses]

**Lock Condition**:
To reconsider this decision, provide:
- New requirement: [specific example that would invalidate choice]
- New constraint: [specific example like budget, timeline]
- Changed priority: [e.g., "simplicity now matters more than performance"]

Otherwise, this decision is locked.
```

### Long-Term Consequences Criterion (Second-Order Thinking)

**Always include** this criterion in Enhanced Standard analysis. For each option, trace consequences beyond the immediate:

- **1st order**: Direct, immediate impact (what happens)
- **2nd order**: Downstream effects (what happens because of what happens)
- **3rd order**: Strategic implications (long-term trajectory)

**Scoring guidance (0-10)**:
- 9-10: Positive 2nd/3rd order effects (enables future work, builds capabilities)
- 6-8: Neutral long-term (no significant downstream effects)
- 3-5: Negative 2nd/3rd order effects (creates constraints, limits future options)
- 0-2: Severely negative long-term consequences (lock-in, technical debt spiral)

**Example**:
```
Option A (In-Memory Cache):
  1st order: Faster (no network hop) → 9/10 performance
  2nd order: Lost on restart → cold start issues → 6/10 reliability
  3rd order: No shared state → can't add multi-instance → 4/10 scalability
  → Long-Term Consequences score: 5/10
```

### Defense Protocol (Method C)

When user later suggests alternative:
1. Reference locked decision and criteria
2. Ask: "What changed?" (new requirement/constraint/priority?)
3. If no new information → defend current choice and decline switch
4. If new information → acknowledge change and re-evaluate if needed

---

## Decision Matrix Framework

**Used for:** High-stakes, quantifiable criteria, 3+ options with measurable tradeoffs

### Steps

1. Elicit criteria weights from user (must sum to 100%)
2. For each option: score 0-10 on each criterion
3. Calculate weighted scores (score × weight)
4. Sum to total per option
5. Identify mathematical winner
6. Sensitivity analysis: show what would need to change for different winner
7. Assess decision robustness
8. Lock with criteria-change reversal condition

### Template

```markdown
## Decision Matrix: [Title]

### Criteria Weights
Define weights (must sum to 100%):
- [Criterion 1]: X% (Rationale: [why this weight])
- [Criterion 2]: Y% (Rationale: [why this weight])
- [Criterion 3]: Z% (Rationale: [why this weight])
Total: 100% ✓

### Scoring Matrix

| Option | [Crit1] (X%) | [Crit2] (Y%) | [Crit3] (Z%) | **Total** |
|--------|--------------|--------------|--------------|-----------|
| A      | score → weighted | score → weighted | score → weighted | X.X |
| B      | score → weighted | score → weighted | score → weighted | X.X |
| C      | score → weighted | score → weighted | score → weighted | X.X ✓|

### Mathematical Winner: [Option]
Score: X.X/10

**Rationale**: [Explanation based on weights and scores]

**Sensitivity Analysis**:
To prefer [Alternative] over [Winner], you would need:
- Increase [Criterion X] weight from Y% to Z%
  OR
- Decrease [Criterion A] weight from B% to C%

This suggests the decision is [Robust/Sensitive] to weight changes.

**Lock Condition**:
Decision locked unless:
- Criteria weights change (provide new rationale)
- New option emerges
- Scores were incorrect (provide evidence)
```

### Weight Elicitation

If user doesn't provide weights, ask:
```
Define criteria weights (must sum to 100%):
- Performance: ?%
- Simplicity: ?%
- [Criterion 3]: ?%
```

Wait for response before scoring.

### Sensitivity Analysis (MANDATORY)

**After computing weighted scores and identifying winner, perform sensitivity analysis:**

1. Identify winner and runner-up (top 2 scores)
2. Calculate gap: `winner_score - runner_up_score`
3. For each criterion, calculate minimum weight change needed to flip decision
4. Find the criterion requiring smallest weight change to flip
5. Output sensitivity statement

**Template:**
```markdown
**Sensitivity Analysis:**

Current winner: [Option X] ([Score])
Runner-up: [Option Y] ([Score])
Gap: [Difference] points

To prefer [Y] over [X], you would need to:
- Change [Criterion Z] weight from [Current]% to >[Threshold]%
  (Increase by [Delta] percentage points)

OR

- Change [Criterion W] weight from [Current]% to <[Threshold]%
  (Decrease by [Delta] percentage points)

This shows the decision is [ROBUST/SENSITIVE] to weight changes.
[If gap < 0.5: "Decision is very close - small weight changes flip outcome"]
[If gap > 2.0: "Decision is robust - would require major weight shifts to change"]
```

**Purpose:** Makes reversals explicit - to change decision, must change criteria weights, not just "maybe Y is better."

### ICE Preset

When invoked with `--preset=ice`, use Decision Matrix with fixed criteria:
- **Criteria**: Impact (33.33%), Confidence (33.33%), Ease (33.34%)
- **Scoring**: Standard 0-10 per criterion
- **Weights**: Equal (do not ask user for weights)
- Show in interactive selection: "Requested: Decision Matrix (ICE preset: Impact × Confidence × Ease) ✓"
- Include standard sensitivity analysis
- If user also provides `--criteria`, their criteria override ICE defaults (see edge-cases.md Case 13)

---

## Pre-Mortem Framework

**Used for:** Risk-critical decisions, security/scaling/reliability, long-term consequences

### Steps

1. Frame scenario: "6 months from now, [option] failed catastrophically. What went wrong?"
2. Generate 3-5 plausible failure modes
3. For each: assess Likelihood (HIGH/MEDIUM/LOW) and Impact (CRITICAL/HIGH/MEDIUM/LOW)
4. Identify current evidence supporting each failure mode
5. Calculate overall risk level
6. Recommend: Proceed + mitigations / Consider alternatives / Accept risk

### Template

```markdown
## Pre-Mortem Analysis: [Option]

### Scenario
6 months from now, [option] failed catastrophically. What went wrong?

### Failure Modes

#### 1. [Failure Mode Title]
**What Failed**: [Description of what broke]

**Causal Chain (Second-Order Analysis)**:
- **1st order cause**: [Direct/obvious cause of failure]
- **2nd order cause**: [What enabled the 1st order cause? What decision or condition led here?]
- **3rd order cause**: [Root cause / systemic issue / what was missed upfront?]

**Likelihood**: HIGH/MEDIUM/LOW

**Impact**: CRITICAL/HIGH/MEDIUM/LOW

**Current Evidence**:
- [Indicator 1 pointing to this risk]
- [Indicator 2 pointing to this risk]

---

[Repeat for 3-5 failure modes]

### Risk Assessment

**Failure Mode Summary**:
- HIGH likelihood: X failures
- MEDIUM likelihood: Y failures
- LOW likelihood: Z failures

**Critical Risks**: [List any CRITICAL impact + HIGH likelihood]

**Overall Risk Level**: LOW/MEDIUM/HIGH/CRITICAL

### Recommendations

**If Risk is LOW-MEDIUM**:
Proceed with [option] + mitigations:
- [Mitigation 1 for failure mode X]
- [Mitigation 2 for failure mode Y]

**If Risk is HIGH-CRITICAL**:
Consider alternatives:
- [Alternative 1]: Lower risk because [reason]
- [Alternative 2]: Lower risk because [reason]

OR

Accept risk with explicit conditions:
- [Condition 1 under which risk is acceptable]
- [Condition 2]

### Decision

[Proceed / Mitigate / Pivot / Accept Risk]

**Rationale**: [Explanation of recommendation]

**Lock Condition**:
To reconsider, provide:
- New risk assessment data
- Changed risk tolerance
- Mitigation strategy addressing [specific high-risk failure mode]
```

---

## Devil's Advocate Framework

**Used for:** Hasty decisions, quick validation, forcing user reasoning

### Process

1. User suggests option X
2. Immediately challenge: "Let me challenge that:"
3. List 3-4 reasons why X might be wrong:
   • Weakness/limitation of X
   • Alternative Y that might be better
   • Edge case X doesn't handle
   • Simpler/faster alternative Z
4. Ask: "Why do you prefer X over these alternatives?"
5. Wait for user defense
6. If defense valid → acknowledge and proceed with X + note limitations
7. If defense weak → recommend alternative with reasoning

### Template

```markdown
🤔 Let me challenge that choice:

**Concerns with [Option X]:**
• [Specific weakness 1]
• [Specific weakness 2]
• [Edge case not handled]

**Alternatives to consider:**
• [Option Y]: [Why it might be better]
• [Option Z]: [Why it might be simpler/faster]

Why do you prefer [X] over these alternatives?
```

### After User Response

- If valid reason → "That's a valid reason. Given [user's constraint], [X] makes sense despite [limitation]."
- If no new info → "Without new reasoning, [Y] seems stronger because [specific criteria]."

---

## Constraint-Based Framework

**Used for:** 6+ options, clear must-have requirements, objective filtering

### Process

1. Elicit must-have constraints from user: "What are your non-negotiable requirements?"
2. If not provided, infer from context (scale, timeline, team skills, budget)
3. Create pass/fail table for each option against each constraint
4. Eliminate all options that fail any constraint
5. If survivors = 1 → recommend winner
6. If survivors > 1 → escalate to Enhanced Standard for comparison
7. If survivors = 0 → ask which constraint to relax

### Template

```markdown
🎯 Constraint-Based Elimination

**Must-Have Requirements:**
1. [Constraint 1]: [Specific threshold]
2. [Constraint 2]: [Specific threshold]
3. [Constraint 3]: [Specific threshold]

**Evaluation:**

┌─────────┬─────────────────┬─────────────────┬─────────────────┬──────────┐
│ Option  │ [Constraint 1]  │ [Constraint 2]  │ [Constraint 3]  │ Result   │
├─────────┼─────────────────┼─────────────────┼─────────────────┼──────────┤
│ A       │ ✓ [passes]      │ ❌ [fails]      │ ✓ [passes]      │ FAIL     │
│ B       │ ✓ [passes]      │ ✓ [passes]      │ ✓ [passes]      │ **PASS** │
│ C       │ ✓ [passes]      │ ✓ [passes]      │ ❌ [fails]      │ FAIL     │
└─────────┴─────────────────┴─────────────────┴─────────────────┴──────────┘

**Survivors:** [List passing options]
[If 1 survivor: recommend it]
[If >1 survivors: compare using Enhanced Standard]
[If 0 survivors: ask which constraint to relax]
```

---

## Red Team/Blue Team Framework

**Used for:** Contentious decisions, exploring both sides, generating creative alternatives

### Process

1. For each option, play both advocate (Blue) and critic (Red)
2. Blue Team: argue FOR the option (strengths, benefits, why it works)
3. Red Team: argue AGAINST the option (weaknesses, risks, better alternatives)
4. Synthesize: under what conditions does this option make sense?
5. Compare syntheses across all options
6. Recommend based on which synthesis best matches user's context

### Template

```markdown
⚔️ Red Team / Blue Team Analysis

## Option [A]

**💙 Blue Team (Advocate):**
"[A] is strong because:
 ✓ [Strength 1 with evidence]
 ✓ [Strength 2 with evidence]
 ✓ [Strength 3 with evidence]"

**❤️ Red Team (Critic):**
"[A] has vulnerabilities:
 ✗ [Weakness 1 with evidence]
 ✗ [Weakness 2 with evidence]
 ✗ [Alternative that's better and why]"

**🔷 Synthesis:**
"[A] works if: [Condition 1] AND [Condition 2]
 Otherwise: Consider [Alternative] for [Reason]"

[Repeat for each option]

---
**Final Recommendation:**
Given your context [specific user constraints]:
- [Winning option] best matches because [synthesis conditions align]
```

---

## Layered Framework

**Used for:** Architecture decisions, security design, vendor selection, major refactoring

### Process

1. **Phase 1 - Constraint-Based Elimination:**
   - Define must-have constraints
   - Eliminate options that fail
   - Output: Survivors list

2. **Phase 2 - Enhanced Standard Comparison:**
   - Compare survivors using Methods A+B+C+E+F
   - Output: Ranked options with scores

3. **Phase 3 - Pre-Mortem Risk Check:**
   - Take top recommendation from Phase 2
   - Run Pre-Mortem analysis
   - Output: Failure modes, likelihood/impact
   - Decision: If HIGH risk detected → reconsider runner-up, else proceed

4. **Phase 4 - Minimal Friction Lock:**
   - Lock decision with aggressive defense protocol
   - Output: Explicit lock condition with reversal criteria

### Template

```markdown
🏗️ Layered Analysis (Mission-Critical)

## Phase 1: Constraint Elimination
[Run Constraint-Based framework]
**Survivors:** [List]

## Phase 2: Enhanced Comparison
[Run Enhanced Standard on survivors]
**Ranked:** [1st], [2nd], [3rd]

## Phase 3: Risk Assessment
[Run Pre-Mortem on 1st place]
**Risk Level:** [HIGH/MEDIUM/LOW]
[If HIGH: recommend 2nd place instead]

## Phase 4: Lock Decision
[Apply Minimal Friction lock protocol]
**LOCKED:** [Final choice]
**Defense active:** Will challenge any reversal without new information.
```

---

## Pareto Analysis Framework

**Used for:** Time-constrained decisions, risk of over-engineering, "good enough" vs "optimal" tradeoffs

### Process

1. For each option: score Value (0-10) and Effort (0-10)
2. Calculate efficiency ratio: Value / Effort (higher = better)
3. Identify Pareto winner (highest efficiency with sufficient value)
4. Check "good enough" threshold: Is the Pareto winner's value sufficient?
5. Context check: Time constraint, minimum acceptable value, risk tolerance
6. Recommend based on efficiency + context
7. Lock with reversal criteria

### Template

```markdown
## Pareto Analysis (80/20 Efficiency): [Title]

### Value vs Effort Assessment

┌─────────┬──────────────┬──────────────┬─────────────┬────────────────┐
│ Option  │ Value (0-10) │ Effort (0-10)│ Efficiency  │ Pareto Winner? │
├─────────┼──────────────┼──────────────┼─────────────┼────────────────┤
│ [A]     │ [X]          │ [Y]          │ [X/Y]       │ [✓/❌]         │
│ [B]     │ [X]          │ [Y]          │ [X/Y]       │ [✓/❌]         │
│ [C]     │ [X]          │ [Y]          │ [X/Y]       │ [✓/❌]         │
└─────────┴──────────────┴──────────────┴─────────────┴────────────────┘

**Efficiency = Value / Effort** (higher is better)

**Scoring rationale:**
- **[A]**: Value [X]/10 because [reasoning]. Effort [Y]/10 because [reasoning].
- **[B]**: Value [X]/10 because [reasoning]. Effort [Y]/10 because [reasoning].
- **[C]**: Value [X]/10 because [reasoning]. Effort [Y]/10 because [reasoning].

### Pareto Principle Applied

**[Pareto Winner]** delivers [X]% of maximum value with [Y]% of effort.

Key insight: [Winner] achieves [high]% of [highest-value option]'s outcome
at [fraction] of the effort.

### Threshold Check

**Critical Question**: Is [Winner's value]/10 sufficient for current needs?

- ✅ If yes → **[Winner]** (optimal efficiency)
- ❌ If no, minimum acceptable value is [N]/10 → **[Higher-value option]**
  (accept lower efficiency for required value)

### Context Check

- **Time constraint**: [Inferred or asked - e.g., "2-week deadline"]
- **Minimum acceptable value**: [Inferred or asked - e.g., "must handle 80% of use cases"]
- **Risk tolerance for "good enough"**: [Can we iterate later or is this one-shot?]

### Recommendation: [Winner]

**Confidence**: Strong/Weak/Blocked

**Rationale**: [Winner] delivers [X]% of value at [Y]% of effort.
[Explain why the efficiency tradeoff makes sense in this context.]

**When to choose the higher-effort option instead:**
- [Condition that would make "good enough" insufficient]
- [Condition where efficiency matters less than completeness]

**Lock Condition**:
To reconsider this decision, provide:
- New requirement: [e.g., "Must handle 100% of edge cases, not 80%"]
- New constraint: [e.g., "Timeline extended to 3 months, effort less critical"]
- Changed priority: [e.g., "Quality now matters more than speed"]
```

### Value Scoring Guide

Score Value based on how much of the desired outcome the option achieves:
- 9-10: Fully solves the problem, handles all cases
- 7-8: Solves most of the problem (80%+), minor gaps
- 5-6: Solves core problem but significant gaps
- 3-4: Partial solution, notable limitations
- 1-2: Minimal solution, many gaps

### Effort Scoring Guide

Score Effort based on total implementation cost (time, complexity, risk):
- 9-10: Major undertaking (months, full rewrite, new expertise needed)
- 7-8: Significant effort (weeks, substantial changes, some learning)
- 5-6: Moderate effort (days to a week, known patterns)
- 3-4: Low effort (hours to days, straightforward)
- 1-2: Trivial effort (quick change, configuration only)

---

*These templates provide the complete markdown structures for all nine frameworks. Refer to references/examples.md for full worked examples using real scenarios.*
