# Edge Cases for /decide Skill

Complete reference for handling edge cases across all 9 frameworks.

---

## Case 1: Only 1 Option Provided

**Response:**
Decline: "This appears to be a single option, not a decision between alternatives. If you want analysis of this option, use /decide --framework=pre-mortem to assess risks and failure modes."

---

## Case 2: Options Are Identical or Trivial Differences

**Response:**
Signal Blocked confidence: "These options are too similar to differentiate meaningfully. Can you clarify the key differences or provide more distinct alternatives?"

---

## Case 3: User Provides Conflicting Flags

**Example:** `--tier=minimal --framework=pre-mortem`

**Resolution:**
- --framework takes precedence (explicit framework > tier)
- Show in interactive selection: "Requested: Pre-Mortem (via --framework flag)"

---

## Case 4: Missing Criteria for Matrix Framework

**Response:**
Prompt: "Decision Matrix requires weighted criteria. Define weights (must sum to 100%): [list common criteria like performance, cost, simplicity, maintainability]"

Wait for user response before proceeding.

---

## Case 5: All Options Score Equally in Enhanced/Matrix

**Response:**
Signal Weak confidence: "Options A and B are tied ([score]). This comes down to: [subjective tiebreaker like team preference, gut feel, or minor factors]"

In lock section, note: "Decision is weak - new information could easily flip this"

---

## Case 6: Constraint-Based Eliminates All Options

**Response:**
- Output: "All options fail at least one must-have constraint."
- Show failure table
- Ask: "Which constraint should we relax? Or do you need to find new options?"
- Wait for user decision

---

## Case 7: Constraint-Based Results in Only 1 Survivor

**Response:**
- Output: "Only [Option X] passes all constraints."
- Skip comparison, recommend X immediately
- Lock decision with note: "Chosen by elimination - only option meeting all must-haves"

---

## Case 8: Devil's Advocate - User Provides No Defense

**Scenario:** User says "just do it" without reasoning

**Response:**
"Without reasoning, I recommend [alternative] instead because [specific advantages]"

Force user to either accept alternative OR provide actual reasoning for original choice.

---

## Case 9: User Requests Same Framework Twice in Multi-Framework

**Response:**
Block: "[Framework] already used. Choose a different framework from the available list or select 'Done'."

---

## Case 10: Layered Framework - High Risk Detected in Phase 3

**Scenario:** After Pre-Mortem shows HIGH risk in Phase 3

**Response:**
- Output: "⚠️ Phase 3 Risk Alert: [Winner] has HIGH risk of [failure mode]"
- Recommend: "Switching to runner-up [Option Y] from Phase 2 due to risk concerns"
- Lock the runner-up instead of original winner

---

## Case 11: Red Team/Blue Team - Synthesis Conditions Conflict

**Scenario:** Option A works under conditions X, but Option B works under conditions NOT-X

**Response:**
Ask user: "Which conditions match your context better: [X] or [NOT-X]?"

Use answer to pick recommendation.

---

## Case 12: Pareto Analysis - All Options Have Same Efficiency

**Scenario:** Value/Effort ratios are identical or nearly identical across options

**Response:**
Signal Weak confidence: "All options have similar efficiency ([ratio]). Pareto Analysis can't differentiate them."

Suggest escalation: "Consider Enhanced Standard for deeper criterion-based comparison, or Decision Matrix for weighted scoring."

---

## Case 13: ICE Preset with Explicit --criteria Flag

**Scenario:** User provides both `--preset=ice` and `--criteria="custom1,custom2"`

**Resolution:**
- `--criteria` overrides ICE preset criteria (explicit > preset)
- Keep equal weights from ICE preset (33.33% each) if 3 criteria provided
- If different number of criteria, distribute weights equally across provided criteria
- Show in interactive selection: "ICE preset modified: using custom criteria with equal weights"

---
