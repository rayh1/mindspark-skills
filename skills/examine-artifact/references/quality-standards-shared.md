# Shared Quality Standards

Universal quality standards applicable to both skills and prompts.

## Table of Contents

1. [Context Window Philosophy](#context-window-philosophy) - Lines 15-85
2. [Duplication Rule](#duplication-rule) - Lines 87-150
3. [Writing Voice Guidelines](#writing-voice-guidelines) - Lines 152-210
4. [Quality Assessment Principles](#quality-assessment-principles) - Lines 212-280

---

## Context Window Philosophy

### Core Principle: Context Window as Public Good

**Foundational assumption: Claude is already very smart.**

Only add context Claude doesn't already have. Artifacts share the context window with:
- System prompt
- Conversation history
- Other skills' metadata
- User requests
- Files being worked on

Every token in an artifact reduces space available for actual work.

### The Challenge Test

For every piece of content, ask:
1. **"Does Claude really need this explanation?"**
2. **"Does this paragraph justify its token cost?"**
3. **"Is this information Claude can already infer from context?"**

If the answer to #1 or #2 is "no," or #3 is "yes" → remove it.

### Preference Hierarchy

**Prefer (in order):**
1. **Concise examples** → Show, don't tell
2. **Bulleted lists** → Scannable, compact
3. **Short declarative sentences** → Clear, minimal
4. **Verbose explanations** → Last resort, only when essential

### Good vs Bad

**GOOD - Concise example:**
```xml
<example>
Extract tables:
```python
import pdfplumber
with pdfplumber.open('doc.pdf') as pdf:
    tables = pdf.pages[0].extract_tables()
```
</example>
```
→ 30 tokens, shows exactly what to do

**BAD - Verbose explanation:**
```xml
<explanation>
When you need to extract tables from a PDF document, you should use the
pdfplumber library. First, you'll want to import the library at the top
of your file. Then, open the PDF file using pdfplumber.open() with a
context manager to ensure proper cleanup. Once you have the PDF object,
you can access individual pages and call the extract_tables() method on
each page to retrieve the table data in a structured format.
</explanation>
```
→ 90 tokens, says the same thing with unnecessary elaboration

### Assessment Criteria

When examining any artifact:
- ✓ Does it prefer examples over explanations?
- ✓ Are explanations justified by complexity, or just verbose?
- ✓ Could any paragraph be replaced with a code snippet or bullet list?
- ✓ Is there "teaching" content Claude doesn't need?

---

## Duplication Rule

**Core rule:** Information should appear in ONE place only.

### Why Duplication is Harmful

1. **Token waste:** Same info consumes context twice
2. **Maintenance burden:** Updates must happen in multiple places
3. **Inconsistency risk:** Copies drift apart over time
4. **Confusion:** Which version is authoritative?

### Acceptable Patterns

**Very limited exceptions:**
- ✓ Brief overview + full details elsewhere
- ✓ One example inline + more examples in dedicated section
- ✓ Principle stated once + application details separate

**Key:** High-level summary in one place, details in another. Not identical content.

### Unacceptable Duplication

**Never duplicate:**
- ❌ Same example in multiple locations
- ❌ Same instructions repeated
- ❌ Same explanation with minor wording changes
- ❌ Same code snippet in multiple places

### Detection During Examination

**How to detect duplication:**

1. **Extract key concepts** from the artifact
2. **Search for same concepts** appearing multiple times
3. **Compare content:**
   - Identical → duplication (bad)
   - Summary vs detail → acceptable
   - Different aspects → acceptable

4. **Check for:**
   - Same example in multiple places
   - Same code snippet repeated
   - Same explanation with minor wording changes
   - Same list duplicated

### Remediation

**When duplication found:**

**Option 1: Keep in primary location**
- Identify which location is most logical
- Remove from other locations
- Add reference if needed ("See section X for...")

**Option 2: Consolidate**
- Merge duplicated content into one comprehensive section
- Remove all other instances
- Update references to point to consolidated section

---

## Writing Voice Guidelines

Consistent voice improves clarity and scannability.

### Imperative/Infinitive Form

**Rule:** Always use imperative or infinitive form for instructions.

**Examples:**

| ✓ Imperative | ❌ Gerund | ❌ Noun form |
|--------------|-----------|--------------|
| Extract text | Extracting text | Extraction of text |
| Rotate PDF | Rotating PDF | PDF rotation |
| Parse tables | Parsing tables | Table parsing |
| Create document | Creating document | Document creation |
| Validate schema | Validating schema | Schema validation |

### Section Headings

**Prefer:**
```markdown
## Extract Text from PDF
## Rotate Pages
## Fill Forms
```

**Avoid:**
```markdown
## Extracting Text from PDF
## Page Rotation
## Form Filling
```

**Exception: Pattern Section Labels**
Pattern section labels are intentionally noun phrases and should NOT be converted to imperative:
```markdown
## Inputs First        ✓ Correct (canonical pattern name)
## Quality Gates       ✓ Correct (canonical pattern name)
## User Approval Gate  ✓ Correct (canonical pattern name)
```

These follow the canonical pattern naming conventions defined in apply-patterns and build-with-patterns skills.

### Instructions

**Prefer:**
```markdown
1. Open the PDF file
2. Extract tables from pages 1-5
3. Export to CSV format
```

**Avoid:**
```markdown
1. Opening the PDF file
2. Extracting tables from pages 1-5
3. Exporting to CSV format
```

### Why This Matters

**Imperative form is:**
- More direct and actionable
- Easier to scan
- Standard for instructions/commands
- Consistent with CLI conventions

---

## Quality Assessment Principles

### Using Standards During Examination

**When examining any artifact:**

1. **Read the relevant standard** for the section you're analyzing
2. **Apply the criteria** from the standard to the artifact
3. **Identify gaps** between current state and standard
4. **Provide specific recommendations** based on the standard
5. **Use examples** from the standard to illustrate good/bad

### In Examination Reports

- Reference specific standards when identifying issues
- Quote criteria from standards to justify recommendations
- Use good/bad examples from standards to clarify issues
- Provide concrete remediation steps based on standards

**Example - Instead of:**
```
Issue: Content is too verbose
Recommendation: Make it shorter
```

**Use standards to provide depth:**
```
Issue: Explanation block (lines 45-65) is 150 words but describes a
simple concept Claude already knows. Fails the Challenge Test: "Does
Claude really need this explanation?" (§ Context Window Philosophy)

Current: [quote the verbose content]

Recommendation:
1. Replace with 3-line code example
2. Or condense to single sentence + bullet list
3. Estimated token savings: ~100 tokens
```

### Assessment Checklist

**Context Efficiency:**
- [ ] Prefers examples over explanations
- [ ] No verbose content Claude can infer
- [ ] Every paragraph justifies its token cost
- [ ] Bulleted lists used instead of prose where applicable

**Duplication:**
- [ ] No identical content appears multiple times
- [ ] If same topic in multiple places, one is summary and one is detail
- [ ] No repeated examples
- [ ] No repeated code snippets

**Writing Voice:**
- [ ] Section headings use imperative form (except canonical pattern labels)
- [ ] Instructions use imperative form
- [ ] Consistent voice throughout

### Severity Assignment Principles

**Assign severity based on impact:**

| Severity | Impact Type | Example |
|----------|-------------|---------|
| CRITICAL | Blocks core function | Missing purpose statement |
| HIGH | Significantly degrades usability | No structure, >500 lines |
| MEDIUM | Quality improvement needed | Inconsistent formatting, verbose |
| LOW | Polish issue | Voice inconsistency, minor redundancy |

**Key principle:** Severity reflects user/Claude impact, not how easy the fix is.
