# Flow Diagram: assess-skill-value

## Table of Contents

- [Flow Diagram](#flow-diagram-assess-skill-value) - Lines 1-163
- [Common Flow Paths](#common-flow-paths) - Lines 184-206
- [Clarifying Questions Triggers](#clarifying-questions-triggers) - Lines 208-214
- [Notes](#notes) - Lines 216-221

---

This diagram shows the complete execution flow from input to final output, including all decision points.

```
┌─────────────────────────────────────────────────────────────────┐
│                         START: User Input                        │
│                  (description / name / path)                     │
└──────────────────────────────┬──────────────────────────────────┘
                               │
                               ▼
┌─────────────────────────────────────────────────────────────────┐
│ STEP 1: Parse Input                                             │
│ ┌───────────────────────────────────────────────────────────┐   │
│ │ D1: Input Type Routing                                    │   │
│ │ • Contains "/" or ".md"? → File path (Read tool)         │   │
│ │ • Single word/hyphenated? → Skill name (Glob tool)       │   │
│ │   - Not found? → Ask user for clarification              │   │
│ │ • Otherwise → Text description                            │   │
│ └───────────────────────────────────────────────────────────┘   │
└──────────────────────────────┬──────────────────────────────────┘
                               │
                               ▼
┌─────────────────────────────────────────────────────────────────┐
│ STEP 2: Validate Completeness (Text Descriptions Only)         │
│ ┌───────────────────────────────────────────────────────────┐   │
│ │ D2: Completeness Validation                               │   │
│ │ • Text description AND lacks key details?                 │   │
│ │   → Ask user for more detail (what/who/inputs)           │   │
│ │ • Text description AND complete?                          │   │
│ │   → Reformulate understanding → Show user → Wait for ✓   │   │
│ │ • Existing skill file/name?                               │   │
│ │   → Skip interpretation check, proceed to Step 3          │   │
│ └───────────────────────────────────────────────────────────┘   │
└──────────────────────────────┬──────────────────────────────────┘
                               │
                               ▼
┌─────────────────────────────────────────────────────────────────┐
│ STEP 3: Evaluate All 7 Dimensions                              │
│ • Assign 0/1/2 score + rationale for each dimension            │
│ • Use clarifying_questions if confidence < 60%                  │
│ ┌───────────────────────────────────────────────────────────┐   │
│ │ G1: Quality Gate (after Step 3)                          │   │
│ │ ✓ All 7 dimensions scored 0/1/2 with rationales          │   │
│ │ ✗ Fail → Re-evaluate missing/invalid dimensions          │   │
│ └───────────────────────────────────────────────────────────┘   │
└──────────────────────────────┬──────────────────────────────────┘
                               │
                               ▼
┌─────────────────────────────────────────────────────────────────┐
│ STEP 4: Calculate Total & Interpret                            │
│ • Sum scores (0-14)                                             │
│ • Determine verdict:                                            │
│   - 10-14 points = Build ✅                                     │
│   - 6-9 points = Consider ⚠️                                    │
│   - 0-5 points = Skip ❌                                        │
│ ┌───────────────────────────────────────────────────────────┐   │
│ │ G2: Quality Gate (after Step 4)                          │   │
│ │ ✓ Total = sum of 7 dimensions                            │   │
│ │ ✓ Verdict matches evaluation_framework rules             │   │
│ │ ✗ Fail → Recalculate                                     │   │
│ └───────────────────────────────────────────────────────────┘   │
└──────────────────────────────┬──────────────────────────────────┘
                               │
                               ▼
┌─────────────────────────────────────────────────────────────────┐
│ STEP 5: Generate Improvements                                   │
│ ┌───────────────────────────────────────────────────────────┐   │
│ │ D3: Improvement Section Inclusion                         │   │
│ │ • Total score < 12?                                       │   │
│ │   → Generate concrete improvement suggestions [I-N]      │   │
│ │ • Score 12-14?                                            │   │
│ │   → Include note: "Score already high; only minor        │   │
│ │     refinements possible"                                 │   │
│ └───────────────────────────────────────────────────────────┘   │
└──────────────────────────────┬──────────────────────────────────┘
                               │
                               ▼
┌─────────────────────────────────────────────────────────────────┐
│ STEP 6: Compile Report                                          │
│ • Produce final output with all required sections               │
│ • Format: Scoring table, verdict, recommendations, etc.         │
│ ┌───────────────────────────────────────────────────────────┐   │
│ │ G3: Quality Gate (after Step 6)                          │   │
│ │ ✓ All required output_schema sections present            │   │
│ │ ✓ Scoring table includes all 7 dimensions                │   │
│ │ ✓ All improvement suggestions use [I-N] format           │   │
│ │ ✗ Fail → Add missing sections                            │   │
│ └───────────────────────────────────────────────────────────┘   │
└──────────────────────────────┬──────────────────────────────────┘
                               │
                               ▼
┌─────────────────────────────────────────────────────────────────┐
│ STEP 7: Review                                                   │
│ • Check consistency, arithmetic, alignment before delivery       │
└──────────────────────────────┬──────────────────────────────────┘
                               │
                               ▼
                    ┌──────────┴──────────┐
                    │   D4: Decision      │
                    │   Verdict = skip?   │
                    └──────────┬──────────┘
                               │
                ┌──────────────┴──────────────┐
                │                             │
                ▼ YES (skip)                  ▼ NO (build/consider)
    ┌───────────────────────┐     ┌───────────────────────────────┐
    │ STOP after Step 7     │     │ Continue to Steps 8-9         │
    │ • Include initial     │     │                               │
    │   improvements in     │     │                               │
    │   report              │     │                               │
    │ • Skip Steps 8-9      │     │                               │
    └───────────────────────┘     └───────────┬───────────────────┘
                                              │
                                              ▼
                            ┌─────────────────────────────────────┐
                            │ STEP 8: Show Additional            │
                            │         Improvements               │
                            │ • Ranked by value impact           │
                            │ • Use [I-N] format                 │
                            │ • Include estimated score impact   │
                            │ ┌─────────────────────────────┐   │
                            │ │ G4: Quality Gate (Step 8)   │   │
                            │ │ ✓ Additional improvements   │   │
                            │ │   section present with ≥1   │   │
                            │ │   ranked improvement        │   │
                            │ │ ✓ All use [I-N] format      │   │
                            │ │ ✗ Fail → Generate missing   │   │
                            │ └─────────────────────────────┘   │
                            └────────────┬────────────────────────┘
                                        │
                                        ▼
                            ┌─────────────────────────────────────┐
                            │ STEP 9: Offer Specification        │
                            │         Generation                 │
                            │ • Ask user if they want complete   │
                            │   specification document           │
                            │ • Ask which improvements to        │
                            │   include (by ID, "all", or       │
                            │   "none")                          │
                            │ • Wait for user response           │
                            └────────────┬────────────────────────┘
                                        │
                                        ▼
                            ┌─────────────────────────────────────┐
                            │ User requests specification?        │
                            └────────────┬────────────────────────┘
                                        │
                        ┌───────────────┴───────────────┐
                        │                               │
                        ▼ YES                           ▼ NO
        ┌───────────────────────────┐   ┌─────────────────────────┐
        │ Generate specification    │   │ DONE                    │
        │ • Include selected        │   │                         │
        │   improvements            │   │                         │
        │ • Save to scratchpad      │   │                         │
        │ • Return file path        │   │                         │
        └───────────┬───────────────┘   └─────────────────────────┘
                    │
                    ▼
        ┌───────────────────────────┐
        │ DONE                      │
        └───────────────────────────┘
```

## Common Flow Paths

(See `decision_points` and `quality_gates` sections in SKILL.md for authoritative definitions of D1-D4 and G1-G4.)

### Path 1: High-Scoring Existing Skill (e.g., pdf skill)
```
Input → Parse (file) → Evaluate → Calculate (13/14, Build) →
High score note → Compile → Review → Additional improvements →
Specification offer → DONE
```

### Path 2: Low-Scoring New Description (e.g., morning routine)
```
Input → Parse (text) → Validate → Reformulate → Confirm →
Evaluate → Calculate (2/14, Skip) → Generate improvements →
Compile → Review → STOP (no Steps 8-9)
```

### Path 3: Medium-Scoring Description with Spec Generation
```
Input → Parse (text) → Validate → Reformulate → Confirm →
Evaluate → Calculate (8/14, Consider) → Generate improvements →
Compile → Review → Additional improvements → Specification offer →
User says yes → Generate spec → DONE
```

## Clarifying Questions Triggers

Questions may be asked at various points when:
- Confidence < 60% on dimension scoring that would change verdict boundary
- Text description is underspecified (who, what inputs/outputs, what tools)
- Multiple plausible interpretations exist
- Maximum 3 questions per phase

## Notes

- **verdict_not_skip** = (verdict = "build" OR verdict = "consider")
- Steps 8-9 only execute if verdict_not_skip
- All improvement suggestions must use [I-N] addressable format
- Specification documents saved to scratchpad directory
