# Specification Template

**Step 9: Specification Document Offer (if verdict ≠ skip)**

After presenting additional improvements, ask the user:

**Prompt format:**
```
Would you like me to create a complete specification document for this skill?

This specification will include:
- Overview and objective
- Key capabilities
- Inputs (required and optional)
- Outputs and format
- High-level process flow
- Quality gates
- Features (including selected improvements)
- Edge cases and error handling
- Example invocations
- Constraints and limitations
- Implementation notes

Please let me know:
1. Should I generate this specification?
2. Which improvements from the list above should be included? (List numbers or "all" or "none")
```

**Specification structure:**

When user confirms, generate a markdown document with these sections (only include relevant sections):

```markdown
# [Skill Name] - Specification

## Overview
[2-3 sentence summary of what this skill does and why it exists]

## Objective
[Clear statement of the skill's purpose and success criteria]

## Key Capabilities
- [Bullet list of core capabilities]
- [Focus on what, not how]

## Inputs

### Required
- `input_name`: [description, type, validation rules]

### Optional
- `input_name`: [description, default value]

## Outputs
[Describe output format, structure, and what user receives]

## Process (High-Level)
1. [Step name] → [outcome]
2. [Step name] → [outcome]
[Keep at workflow level, not implementation details]

## Quality Gates
- G1 (after Step X): [what must be true]
- G2 (after Step Y): [what must be true]

## Features
- [Core feature 1]
- [Core feature 2]
- [Selected improvements from additional improvements list]

## Edge Cases & Error Handling
- [Scenario]: [expected behavior]
- [Error condition]: [recovery strategy]

## Example Invocations
```
[Example 1: simple case]
[Example 2: complex case]
[Example 3: edge case]
```

## Constraints and Limitations
- [What the skill does NOT do]
- [Known limitations or boundaries]

## Implementation Notes
- [Architecture suggestions]
- [Tool/library recommendations]
- [Integration points with other skills]
- [Performance considerations]
```

**Specification principles:**
- Focus on WHAT the skill does, not HOW to implement it
- Provide enough detail for a skill builder to implement it
- Include selected improvements as features
- Keep it concise and actionable
- No actual code or detailed implementation steps
- Suitable as input to build-with-patterns or skill-creator

**File naming:**
- Save to: `/tmp/claude/-workspace/[session-id]/scratchpad/[skill-name]-specification.md`
- Inform user of the file path when complete
