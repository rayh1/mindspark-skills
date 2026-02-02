# Shared Checklists

Use these to keep outputs small, consistent, and testable.

---

## Recommended Guidelines
- Step Contract: appropriate steps as needed
- Quality Gates: as needed for the specific artifact
- Decision Points: as needed for the specific artifact
- Edge-case tests: at minimum 2 (missing input + conflicting/ambiguous)

If scope expands significantly beyond initial requirements, get explicit user approval for the expansion.

---

## Minimal "Done" Definition
A build is done when:
- output matches the output schema
- required sections exist and are non-empty
- all gates pass
- stop conditions are satisfied
