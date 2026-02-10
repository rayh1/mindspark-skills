# Assumption Registry Guide

Log assumptions during auto-detection and execution for transparency and debuggability.

---

## Framework Detection Assumptions

**Format:**
- [Auto-detect] assumed: "{framework} based on {signals}" | source: {context analysis} | confidence: {high|medium|low}

**Examples:**
- "Escalated to Enhanced Standard based on: 4 options detected + user said 'compare'" | source: decision_points logic | confidence: high
- "Defaulted to Minimal Friction (no escalation signals detected)" | source: fallback rule | confidence: medium

---

## Input Assumptions

**Format:**
- [Inputs] assumed: "{what}" | source: {context inference} | confidence: {level}

**Examples:**
- "Criteria for Enhanced Standard: performance, simplicity, maintainability" | source: inferred from software decision context | confidence: medium
- "User wants quick analysis (Devil's Advocate triggered by 'just use X')" | source: hasty language detection | confidence: medium

---

## Validation Priority

- **High-impact + low-confidence** → ask before proceeding (via clarifying_questions)
- **High-impact + high-confidence** → proceed, show assumption in interactive selection
- **Low-impact** → proceed, log for transparency

---

## Output in Interactive Selection

Show key assumptions to user:

```
Detected: 4 options, user asked "compare"
Auto-selected: Enhanced Standard ✓
Assumptions: Will compare on performance, simplicity, maintainability (inferred from context)
```

---

## Purpose

- **Transparency:** User sees why framework was chosen
- **Debuggability:** Can trace auto-detection logic
- **Correction:** User can override if assumption wrong
- **Auditability:** Track what skill assumed vs. what user explicitly stated

---
