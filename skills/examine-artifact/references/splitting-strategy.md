# Splitting Strategy for examine-artifact

## Current Status
- **Current SKILL.md size:** 310 lines (as of examination)
- **Guideline threshold:** 500 lines
- **Status:** Within acceptable range, but planning needed for future growth

## Split Recommendations (when approaching 400+ lines)

### Priority 1: Move to References
**Candidates for references/detailed-analysis-guide.md:**
- Extended framework explanations that go beyond essential workflow
- Detailed examples of analysis techniques
- Comprehensive guidance on specific check types
- Implementation details that support but don't drive core workflow

### Priority 2: Consolidate Verbose Content
**Current verbose sections to condense:**
- Severity framework details (currently condensed in main edit)
- Lens analysis comprehensive explanations
- Extended decision point explanations

### Content That Must Stay in SKILL.md
**Essential workflow elements:**
- All pattern section structures (canonical)
- Core step contract and decision points
- Quality gates and stop conditions
- Essential framework references
- Navigation logic for when to read references

### Proposed Split Structure (if needed)

```
examine-artifact/
├── SKILL.md (200-300 lines target)
│   ├── Essential workflow only
│   ├── Core patterns and gates
│   └── Clear reference navigation
└── references/
    ├── [existing files] ✓
    └── detailed-analysis-guide.md (new)
        ├── Extended framework explanations
        ├── Advanced analysis techniques
        └── Comprehensive examples
```

## Splitting Triggers
- **400+ lines:** Start planning split
- **500+ lines:** Execute split
- **References growing:** Consider sub-categorization

## Quality Preservation
- Maintain all canonical pattern section labels
- Preserve complete workflow logic in SKILL.md
- Ensure clear navigation to detailed references
- Keep essential analysis capabilities accessible