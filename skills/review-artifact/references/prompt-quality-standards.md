# Prompt Quality Standards

Quality standards specific to standalone prompt files. For universal standards, see quality-standards-shared.md.

## Table of Contents

1. [Prompts vs Skills: Key Differences](#prompts-vs-skills-key-differences) - Lines 18-70
2. [Heading Structure Excellence](#heading-structure-excellence) - Lines 72-140
3. [Section Organization](#section-organization) - Lines 142-210
4. [Content Efficiency](#content-efficiency) - Lines 212-280
5. [Progressive Complexity](#progressive-complexity) - Lines 282-350
6. [Length Guidelines](#length-guidelines) - Lines 352-410
7. [Prompt Quality Checklist](#prompt-quality-checklist) - Lines 412-480

---

## Prompts vs Skills: Key Differences

Understanding when to use a prompt vs a skill helps evaluate which standards apply.

### When to Use a Prompt

Prompts are standalone instruction files without the skill infrastructure:

- **One-off tasks**: Instructions for a specific task that won't be reused
- **Simple workflows**: Linear instructions without conditional branching
- **Context-specific**: Instructions that don't need to be triggered automatically
- **No resource dependencies**: No need for scripts/, references/, or assets/

### When to Use a Skill

Skills have YAML frontmatter and may include bundled resources:

- **Reusable capabilities**: Instructions meant to be triggered repeatedly
- **Automatic triggering**: Needs to be discovered and activated by Claude
- **Complex workflows**: Multiple paths, conditions, or decision points
- **Resource dependencies**: Needs scripts, references, or template assets

### Key Structural Differences

| Aspect | Prompt | Skill |
|--------|--------|-------|
| Frontmatter | None or minimal | Required (name + description) |
| Triggering | Manual/context | Automatic via description |
| Resources | Self-contained | Can bundle scripts/, references/, assets/ |
| Length | Typically <300 lines | Can be larger with references/ |
| Structure | Headings + content | XML tags + headings |

### Review Implications

When reviewing a prompt:
- Focus on clarity, organization, and self-containment
- No description field to analyze (no triggering concerns)
- No reference files to check for duplication
- Length limits are stricter (no offloading to references/)
- Structure is simpler (no XML tags expected)

---

## Heading Structure Excellence

Prompts rely on headings for organization (unlike skills which use XML tags).

### Heading Hierarchy

**Recommended structure:**

```markdown
# Prompt Title                    ← H1: One per file, at top
## Major Section                  ← H2: Main organizational divisions
### Subsection                    ← H3: Details within sections
#### Rarely needed                ← H4: Usually indicates over-nesting
```

### Heading Rules

**Use H1 (#) for:**
- The prompt title only
- Should appear exactly once, at the top

**Use H2 (##) for:**
- Major sections: Purpose, Inputs, Steps, Output, Examples
- Should divide the prompt into distinct logical parts

**Use H3 (###) for:**
- Subsections within H2 sections
- Breaking down complex steps
- Categorizing related items

**Avoid H4+ (####) unless:**
- Truly necessary for clarity
- Content is deeply nested by nature
- Using more than 5 times is a warning sign

### Descriptive vs Generic Headings

**Good headings describe content:**
```markdown
## Parse User Input
## Generate Response
## Handle Edge Cases
```

**Bad headings are vague:**
```markdown
## Details
## Notes
## Misc
## Step 1
```

**Numbered steps with description are acceptable:**
```markdown
## Step 1: Validate Input
## Step 2: Process Data
## Step 3: Format Output
```

### Heading Quality Checklist

- [ ] H1 appears exactly once at top
- [ ] H2 divides prompt into major sections
- [ ] H3 used for subsections (not major divisions)
- [ ] H4+ used sparingly (<5 times)
- [ ] All headings are descriptive (not just "Notes", "Details")
- [ ] No duplicate headings

---

## Section Organization

Prompts should follow a logical flow that aids comprehension.

### Recommended Section Order

**Standard prompt structure:**

1. **Purpose/Goal** (optional if obvious)
   - What this prompt accomplishes
   - When to use it

2. **Inputs/Context**
   - What information is needed
   - Expected format of inputs
   - Required context

3. **Steps/Instructions**
   - The actual work to be done
   - In logical/chronological order

4. **Output Format**
   - Expected structure of response
   - Examples of correct output

5. **Examples** (if helpful)
   - Input/output pairs
   - Edge case demonstrations

6. **Constraints/Edge Cases** (if applicable)
   - What NOT to do
   - Special cases to handle

### Why This Order?

- **Purpose first**: Establishes context for everything that follows
- **Inputs before steps**: Can't explain how to process without knowing what
- **Steps in sequence**: Natural execution order
- **Output near end**: After understanding the process
- **Examples after rules**: Illustrate, don't establish
- **Constraints last**: Exceptions after main flow

### Common Ordering Problems

**Anti-pattern: Outputs before inputs**
```markdown
❌ Here's the format to use: [detailed output spec]
   Now, given [inputs]...
```

**Anti-pattern: Constraints before instructions**
```markdown
❌ Never do X, Y, or Z.
   Here's what to do: [main instructions]
```

**Better: Positive instructions first**
```markdown
✓ Here's what to do: [main instructions]
   Note: Avoid X, Y, Z in these cases...
```

---

## Content Efficiency

Prompts should be concise while remaining clear.

### Efficiency Principles

1. **One instruction per concept**: Don't repeat the same instruction
2. **Examples over explanations**: Show, don't tell
3. **Lists over prose**: Scannable > readable
4. **Essential only**: Remove content that doesn't change behavior

### Verbose vs Efficient

**Verbose (avoid):**
```markdown
When you receive input from the user, you should first take the time to
carefully read through all of the content that has been provided. After
you have finished reading, you should consider what the user is actually
asking for. Think about the context and any implicit requirements that
might not have been explicitly stated.
```

**Efficient (prefer):**
```markdown
1. Read user input
2. Identify explicit and implicit requirements
3. Consider context
```

### When Explanation is Justified

**Keep explanations when:**
- The "why" affects the "how"
- Claude would otherwise choose wrong approach
- Domain-specific nuance is critical

**Remove explanations when:**
- Claude would know this from training
- It's general good practice
- It's obvious from context

### The Token Budget Test

For any content block >30 words, ask:
1. Does this change Claude's behavior?
2. Would Claude do the wrong thing without this?
3. Is this the most efficient way to communicate this?

If any answer is "no," the content is a candidate for removal or condensing.

---

## Progressive Complexity

Organize content from simple to complex, essential to optional.

### Front-Load Essentials

**First 20 lines should contain:**
- The primary purpose/goal
- The main task to accomplish
- Critical constraints (if any)

**First 20 lines should NOT contain:**
- Edge cases and exceptions
- Detailed examples
- Optional enhancements
- Long explanations

### Complexity Gradient

**Structure content as:**
```
1. Core task (what to do most of the time)
2. Main variations (common alternatives)
3. Edge cases (unusual situations)
4. Advanced options (optional enhancements)
```

**Not:**
```
1. All the ways things can go wrong
2. Exceptions and edge cases
3. Oh, and here's the main task
4. More edge cases
```

### Exception Handling Placement

**After main flow:**
```markdown
## Main Process
[Core instructions]

## Edge Cases
- If X occurs, do Y
- If Z occurs, do W
```

**Not before:**
```markdown
## Important Exceptions
- Never do X when Y
- Always check for Z before...

## Main Process
[Core instructions]
```

### Example Placement

**Examples follow instructions:**
```markdown
## Parse Input
Extract the name and email from the input.

**Example:**
Input: "John Smith <john@example.com>"
Output: {"name": "John Smith", "email": "john@example.com"}
```

**Not examples first:**
```markdown
**Example:**
Input: "John Smith <john@example.com>"
Output: {"name": "John Smith", "email": "john@example.com"}

## Parse Input
Extract the name and email from the input.
```

---

## Length Guidelines

Prompts should be as long as necessary, but no longer.

### Length Targets

| Length | Assessment | Action |
|--------|------------|--------|
| <100 lines | Concise | Check completeness |
| 100-200 lines | Ideal | Likely appropriate |
| 200-300 lines | Long | Review for trimming |
| 300-500 lines | Too long | Needs condensing |
| 500+ lines | Excessive | Major restructuring needed |

### Why Shorter is Better

- **Faster to read**: Claude processes faster
- **Less noise**: Signal-to-noise ratio improves
- **Fewer contradictions**: Less content = fewer conflicts
- **Easier to maintain**: Updates are simpler

### When Length is Justified

**Acceptable reasons for longer prompts:**
- Complex domain with many edge cases
- Multiple distinct workflows
- Extensive examples required for pattern learning
- Detailed output format specifications

**Not acceptable reasons:**
- Padding with obvious instructions
- Explaining concepts Claude knows
- Excessive examples for simple patterns
- Verbose prose that could be lists

### Reducing Length

**Strategies:**
1. **Convert prose to lists**: 50 words → 5 bullets
2. **Replace explanations with examples**: 100 words → 10-line example
3. **Remove obvious content**: Claude knows basic patterns
4. **Consolidate duplicates**: Say it once
5. **Remove filler**: "Please note", "It's important to remember"

---

## Prompt Quality Checklist

Use this checklist when reviewing any prompt.

### Structure
- [ ] H1 appears exactly once at top
- [ ] H2 divides into major sections
- [ ] Heading levels are consistent (no skipping)
- [ ] H4+ used sparingly
- [ ] All headings are descriptive

### Organization
- [ ] Purpose/goal stated early
- [ ] Inputs defined before steps
- [ ] Steps in logical order
- [ ] Output format specified
- [ ] Exceptions after main flow
- [ ] Examples follow instructions

### Content Efficiency
- [ ] No repeated instructions
- [ ] Examples preferred over explanations
- [ ] Lists preferred over prose
- [ ] No obvious/Claude-knows content
- [ ] No filler phrases

### Length
- [ ] Under 300 lines (or justified)
- [ ] Under 500 lines (mandatory)
- [ ] Every section earns its place
- [ ] No padding

### Progressive Complexity
- [ ] Essential info in first 20 lines
- [ ] Simple before complex
- [ ] Main flow before exceptions
- [ ] Required before optional

### Self-Containment
- [ ] Works without external context
- [ ] All terms defined or obvious
- [ ] No broken references
- [ ] Complete instructions

### Writing Voice (see quality-standards-shared.md)
- [ ] Imperative form for instructions
- [ ] Consistent style throughout
- [ ] Direct and actionable language
