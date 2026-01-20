# Pattern Integration Guide

See [minimal-templates.md](minimal-templates.md) for canonical tag names and minimal insertion templates.

## Format Adaptation

### For skills (YAML frontmatter detected)

- Use XML tags: `<inputs_first>`, `<step_contract>`, `<decision_points>`, etc.
- Place patterns after frontmatter, before `<objective>` or main content
- Preserve existing skill structure

Merge rule:
- If a section already exists (same tag), edit it in place. **Do not duplicate pattern sections.**

### For raw prompts

- Use text block format matching llmprog.md examples
- Place patterns at top of prompt
- Preserve task description at end

Merge rule:
- If a heading already exists (e.g., `Inputs First:`), update it instead of adding another.

## Pattern Ordering

When adding multiple patterns, follow this order:

1. **Tier order:** Tier 1 → Tier 2 → Tier 3
2. **Within Tier 1:** Inputs First → Step Contract → Scope Fence → Output Schema → Stop Conditions → Quality Gates → Interpretation Check → Decision Points
3. **Within Tier 2/3:** follow the order in pattern-catalog.md

This ordering ensures dependencies flow correctly:
- Inputs declared before steps reference them
- Gates reference defined steps
- Stop conditions reference defined states

## Canonical naming

- For skills: use canonical XML tags from [minimal-templates.md](minimal-templates.md).
- For prompts: use canonical headings from [minimal-templates.md](minimal-templates.md).
