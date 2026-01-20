# Shared Checklists

Use these to keep outputs small, consistent, and testable.

---

## Lite Mode Hard Caps (enforce)
- Step Contract: 4–6 steps
- Quality Gates: max 3
- Decision Points: max 3
- Skip Pass 6 (logging)
- Edge-case tests: 2 (missing input + conflicting/ambiguous)

If caps are exceeded, either:
- switch to full mode, or
- get explicit user approval for the scope expansion.

---

## Minimal "Done" Definition
A build is done when:
- output matches the output schema
- required sections exist and are non-empty
- all gates pass
- stop conditions are satisfied
