# Pattern Label Registry

**Purpose:** Canonical pattern names that should be excluded from voice checking and preserved during examination.

These labels are standardized across the apply-patterns and build-with-patterns skills and should NOT be converted to imperative form.

## Skill Format (XML Tags)

**Tier 1 - Core:**
- `<inputs_first>`
- `<step_contract>`
- `<decision_points>`
- `<quality_gates>`
- `<stop_conditions>`
- `<scope_fence>`
- `<interpretation_check>`
- `<output_schema>`

**Tier 2 - Recommended:**
- `<user_approval_gate>`
- `<clarifying_questions>`
- `<confidence_signal>`
- `<fallback_chain>`
- `<lens>`
- `<addressable_output>`
- `<review_step>`
- `<step_ledger>`

**Tier 3 - Situational:**
- `<mode_selection>`
- `<example_anchor>`
- `<assumption_registry>`
- `<context_window>`
- `<observability>`
- `<error_handling>`

## Prompt Format (Markdown Headings)

**Tier 1 - Core:**
- `## Inputs First:`
- `## Step Contract:`
- `## Decision Points:`
- `## Quality Gates:`
- `## Stop Conditions:`
- `## Scope Fence:`
- `## Interpretation Check:`
- `## Output Schema:`

**Tier 2 - Recommended:**
- `## User Approval Gate:`
- `## Clarifying Questions:`
- `## Confidence Signal:`
- `## Fallback Chain:`
- `## Lens:`
- `## Addressable Output:`
- `## Review Step:`
- `## Step Ledger:`

**Tier 3 - Situational:**
- `## Mode Selection:`
- `## Example Anchor:`
- `## Assumption Registry:`
- `## Context Window:`
- `## Observability:`
- `## Error Handling:`

## Detection Logic

When performing voice checking:

1. **Extract heading text** (remove ## and any trailing :)
2. **Check against this registry** (case-insensitive match)
3. **If match found**: Skip voice analysis for this heading
4. **If no match**: Apply normal imperative form checking

## Examples

**✓ Correctly preserved:**
```markdown
## Inputs First:           # Pattern label - preserve
## Quality Gates:          # Pattern label - preserve
## User Approval Gate:     # Pattern label - preserve
```

**✓ Correctly flagged for voice improvement:**
```markdown
## Extracting Data        # Should be "Extract Data"
## File Processing        # Should be "Process Files"
## Error Handling Setup   # Should be "Set Up Error Handling"
```

**❌ Incorrectly modified (avoid this):**
```markdown
## Input First            # Wrong - corrupts canonical pattern name
## Gate Quality            # Wrong - corrupts canonical pattern name  
## Gate User Approval      # Wrong - corrupts canonical pattern name
```