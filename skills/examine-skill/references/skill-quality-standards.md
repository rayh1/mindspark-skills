# Skill Quality Standards

Domain expertise for evaluating skill quality during examination. Use this reference to understand what makes a skill excellent, not just structurally correct.

These standards provide the **rationale** behind structural checks in the analysis framework. When examining a skill, use these standards to assess quality and provide informed recommendations.

---

## Table of Contents

1. [Context Window Philosophy](#context-window-philosophy)
2. [Progressive Disclosure: 3-Level Loading](#progressive-disclosure-3-level-loading)
3. [Resource Type Selection Criteria](#resource-type-selection-criteria)
4. [Reference Organization Patterns](#reference-organization-patterns)
5. [Description Field Excellence](#description-field-excellence)
6. [Split Strategy (500-Line Guideline)](#split-strategy-500-line-guideline)
7. [Duplication Rule](#duplication-rule)
8. [Writing Voice Guidelines](#writing-voice-guidelines)
9. [Quality Assessment Checklist](#quality-assessment-checklist)

---

## Context Window Philosophy

### Core Principle: Context Window as Public Good

**Foundational assumption: Claude is already very smart.**

Only add context Claude doesn't already have. Skills share the context window with:
- System prompt
- Conversation history
- Other skills' metadata
- User requests
- Files being worked on

Every token in a skill reduces space available for actual work.

### The Challenge Test

For every piece of content in a skill, ask:
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

**Assessment criteria when examining:**
- ✓ Does the skill prefer examples over explanations?
- ✓ Are explanations justified by complexity, or just verbose?
- ✓ Could any paragraph be replaced with a code snippet or bullet list?
- ✓ Is there "teaching" content Claude doesn't need?

---

## Progressive Disclosure: 3-Level Loading

Skills use a three-level loading system. Understanding the loading model is essential for evaluating architecture quality.

### Level 1: Metadata (Always Loaded)

**What:** YAML frontmatter (`name` + `description`)
**When loaded:** Always, for ALL skills in the system
**Purpose:** Determine which skill to trigger
**Target size:** ~100 words (~600-800 tokens)
**Critical:** This is the ONLY part Claude sees before triggering

**Quality standards:**
- Description must be comprehensive enough to trigger correctly
- Must include BOTH what the skill does AND when to use it
- Should include specific triggers: file types, use cases, keywords
- Should enumerate use cases when possible

**Red flag if:**
- ❌ Description exceeds 150 words (too bloated for metadata)
- ❌ Description under 20 words (too vague to trigger correctly)
- ❌ Description only says what, not when

### Level 2: SKILL.md Body (Loaded When Triggered)

**What:** XML-tagged sections with instructions
**When loaded:** Only AFTER skill triggers
**Purpose:** Guide Claude through the skill's workflow
**Target size:** <500 lines, <5k words (~30k-40k tokens max)
**Critical:** This loads every time the skill is used

**Quality standards:**
- Keep core workflow and essential instructions
- Reference bundled resources clearly
- Provide navigation logic for when to read references
- Split into references/ before exceeding 500 lines

**Red flag if:**
- ❌ Exceeds 500 lines without clear split strategy
- ❌ Contains detailed API documentation (should be in references/)
- ❌ Contains multiple framework-specific sections (split by framework)
- ❌ Contains "When to Use" sections (belongs in description, not body)

### Level 3: Bundled Resources (Loaded As Needed)

**What:** scripts/, references/, assets/
**When loaded:** Claude decides based on need, OR scripts execute without loading
**Purpose:** Provide detailed information without bloating SKILL.md
**Target size:** Unlimited
**Critical:** Only loaded when explicitly needed or executed

**Quality standards:**
- scripts/ can execute without reading into context
- references/ loaded only when Claude determines they're needed
- assets/ never loaded (used in output only)
- Clear guidance in SKILL.md about when to read each reference

**Red flag if:**
- ❌ Essential workflow instructions buried in references/
- ❌ SKILL.md doesn't explain when to read each reference
- ❌ References are deeply nested (>1 level deep)
- ❌ Large reference files (>100 lines) lack table of contents

### Loading Model Assessment

When examining a skill, evaluate:

1. **Is critical information at the right level?**
   - Triggers in description (Level 1)
   - Workflow in SKILL.md (Level 2)
   - Details in references/ (Level 3)

2. **Are size targets respected?**
   - Description ~100 words
   - SKILL.md <500 lines
   - No artificial limits on references/

3. **Is the loading model efficient?**
   - Claude doesn't load references/ until needed
   - Scripts can execute without context load
   - Assets never consume context

---

## Resource Type Selection Criteria

Understanding WHEN to use scripts/ vs references/ vs assets/ is critical for evaluating skill architecture.

### scripts/ - Executable Code

**When to use:**
- Code is being rewritten repeatedly by Claude
- Deterministic reliability is required
- Operation is complex and error-prone
- Same script used across multiple skill invocations

**Examples:**
- `rotate_pdf.py` - PDF rotation logic
- `extract_tables.py` - Table extraction with edge case handling
- `validate_schema.py` - Data validation with complex rules
- `init_project.py` - Project scaffolding

**Benefits:**
- Token efficient (execute without loading to context)
- Deterministic behavior (tested, versioned)
- Reusable across invocations

**Red flags:**
- ❌ Script in scripts/ but never referenced in SKILL.md
- ❌ Simple one-liner that could be inline (over-engineering)
- ❌ Script requires heavy modification every time (defeats purpose)
- ❌ Untested script (should be validated before inclusion)

### references/ - Documentation Loaded As Needed

**When to use:**
- Documentation Claude should reference while working
- Domain knowledge not in base model training
- Information too detailed for SKILL.md
- Multiple variants/frameworks supported (split by variant)

**Examples:**
- `schema.md` - Database schemas, table relationships
- `api_docs.md` - Complete API specifications
- `finance.md` - Company-specific financial terminology
- `aws.md` - AWS-specific deployment patterns (when skill supports multiple clouds)
- `advanced-features.md` - Optional advanced functionality

**Benefits:**
- Keeps SKILL.md lean
- Loaded only when Claude determines it's needed
- Can be very large without bloating core skill

**Best practices:**
- Include table of contents for files >100 lines
- Provide grep search patterns in SKILL.md for very large files (>10k words)
- Avoid duplication between SKILL.md and references/
- Keep one level deep (no deeply nested structure)

**Red flags:**
- ❌ Essential workflow instructions in references/ (should be in SKILL.md)
- ❌ Information duplicated in both SKILL.md and references/
- ❌ Template files in references/ (should be assets/)
- ❌ No guidance in SKILL.md about when to read the reference
- ❌ References nested 2-3 levels deep
- ❌ Large reference (>100 lines) without table of contents

### assets/ - Files Used in Output

**When to use:**
- Files that will be used in the final output
- Templates that get copied or modified
- Resources needed by generated code
- Brand assets, fonts, configuration templates

**Examples:**
- `template.pptx` - PowerPoint template
- `logo.png` - Brand logo
- `frontend-template/` - HTML/React boilerplate directory
- `config-template.yaml` - Configuration file template
- `font.ttf` - Typography file

**Key difference:**
- assets/ are NEVER loaded into context
- They are used/copied/modified in the output
- references/ are loaded to inform Claude's thinking
- assets/ are raw materials for the deliverable

**Red flags:**
- ❌ Documentation in assets/ (should be references/)
- ❌ Template in references/ (should be assets/)
- ❌ Script in assets/ (should be scripts/)
- ❌ Asset used in SKILL.md examples but doesn't exist

### Selection Decision Tree

When examining resource placement:

```
Is this executable code?
├─ YES: Should it be in scripts/
└─ NO: ↓

Is this documentation Claude reads to inform decisions?
├─ YES: Should it be in references/
└─ NO: ↓

Is this a file used in the output Claude produces?
├─ YES: Should it be in assets/
└─ NO: Should it be in SKILL.md or removed
```

### Duplication Rule (Cross-Resource)

**Critical rule:** Information should live in EITHER SKILL.md OR references/, not both.

**Prefer references/ when:**
- Information is detailed/comprehensive
- Multiple variants/options exist
- Information is optional/advanced
- File-specific or framework-specific

**Keep in SKILL.md when:**
- Core workflow/essential steps
- Navigation logic (when to read references/)
- High-level principles
- Common patterns everyone needs

---

## Reference Organization Patterns

When skills grow complex, references/ organization becomes critical. Three proven patterns:

### Pattern 1: High-Level Guide with References

**Use when:** Skill has basic workflow + advanced features

**Structure:**
```markdown
# SKILL.md

## Quick start
[Basic workflow + example]

## Advanced features
- **Feature A**: See [feature-a.md](references/feature-a.md)
- **Feature B**: See [feature-b.md](references/feature-b.md)
- **API reference**: See [api-reference.md](references/api-reference.md)
```

**SKILL.md:** 100-200 lines, core workflow + pointers
**references/:** Detailed guides loaded as needed

**Quality markers:**
- ✓ Clear signposting: "See [file] for..."
- ✓ Self-contained references (readable independently)
- ✓ Quick start in SKILL.md covers 80% of use cases

**Red flags:**
- ❌ "See [file]" but no description of what's in it
- ❌ References assume you read SKILL.md first (not self-contained)

### Pattern 2: Domain-Specific Organization

**Use when:** Skill supports multiple domains/categories

**Structure:**
```
bigquery-skill/
├── SKILL.md (overview + domain selection logic)
└── references/
    ├── finance.md (revenue, billing metrics)
    ├── sales.md (opportunities, pipeline)
    ├── product.md (API usage, features)
    └── marketing.md (campaigns, attribution)
```

**Workflow:**
1. User asks about sales metrics
2. Claude reads SKILL.md, sees domain-specific organization
3. Claude loads ONLY `references/sales.md`

**Quality markers:**
- ✓ Domains are mutually exclusive (finance ≠ sales)
- ✓ SKILL.md explains how to choose domain
- ✓ Each reference is self-contained for its domain

**Red flags:**
- ❌ Information spans multiple domains (unclear where to look)
- ❌ No selection logic in SKILL.md
- ❌ Domains overlap significantly

### Pattern 3: Framework/Variant-Specific Organization

**Use when:** Skill supports multiple frameworks or platforms

**Structure:**
```
cloud-deploy/
├── SKILL.md (deployment workflow + provider selection)
└── references/
    ├── aws.md (AWS-specific patterns)
    ├── gcp.md (GCP-specific patterns)
    └── azure.md (Azure-specific patterns)
```

**Workflow:**
1. User chooses AWS
2. Claude reads SKILL.md for core workflow
3. Claude loads ONLY `references/aws.md` for AWS specifics

**Quality markers:**
- ✓ SKILL.md contains framework-agnostic workflow
- ✓ Variant files contain ONLY variant-specific details
- ✓ Clear selection criteria in SKILL.md

**Red flags:**
- ❌ Core workflow duplicated across variant files
- ❌ Variant selection logic unclear
- ❌ Variants not truly independent (cross-referencing)

### Organization Best Practices

**Nesting depth:**
- ✓ One level deep: `references/file.md`
- ❌ Two+ levels: `references/advanced/feature/details.md`

**Why:** Each level adds cognitive overhead. Keep flat.

**Table of contents:**
- Required for references/ files >100 lines
- Place at top of file
- Include line numbers or section links

**Example:**
```markdown
# API Reference

## Table of Contents
- [Authentication](#authentication) - Lines 15-45
- [Endpoints](#endpoints) - Lines 46-120
- [Error Codes](#error-codes) - Lines 121-150
```

**File naming:**
- ✓ Descriptive: `authentication-guide.md`, `aws-deployment.md`
- ❌ Vague: `advanced.md`, `misc.md`, `other.md`

**Grep patterns:**
For very large reference files (>10k words), include grep guidance in SKILL.md:

```markdown
For complete API reference, see references/api-docs.md
Search patterns:
- Authentication: grep "^## Auth" references/api-docs.md
- Rate limits: grep "rate.limit" references/api-docs.md
```

---

## Description Field Excellence

The description field is the MOST IMPORTANT part of a skill. It determines when the skill gets used.

### The Loading Model Reality

**Critical insight:** The description is loaded at Level 1 (metadata), BEFORE the skill body.

This means:
- ✓ Description determines whether skill triggers
- ❌ Body is useless for triggering (loads AFTER decision)
- ❌ "When to Use This Skill" sections in body are worthless

### Description Structure

**Formula:** `[What the skill does] + [When to use it with specific examples]`

**Components:**
1. **Capability summary** (what it does)
2. **Trigger conditions** (when to use)
3. **Specific file types/formats** (if applicable)
4. **Enumerated use cases** (concrete examples)
5. **Keywords** that should trigger the skill

### Excellent Example

```yaml
name: docx
description: Comprehensive document creation, editing, and analysis with
  support for tracked changes, comments, formatting preservation, and text
  extraction. Use when Claude needs to work with professional documents
  (.docx files) for: (1) Creating new documents, (2) Modifying or editing
  content, (3) Working with tracked changes, (4) Adding comments, or any
  other document tasks
```

**Why excellent:**
- ✓ Specific file type: `.docx files`
- ✓ Enumerated use cases: (1), (2), (3)
- ✓ Keywords: tracked changes, comments, formatting, extraction
- ✓ Comprehensive scope: "any other document tasks"
- ✓ ~70 words (appropriate size)

### Good Example

```yaml
name: pdf
description: Comprehensive PDF manipulation including text extraction, form
  filling, page operations, and table parsing. Use when Claude needs to:
  (1) extract text or tables from PDFs, (2) fill PDF forms, (3) rotate,
  merge, or split PDF pages, (4) work with PDF metadata, or any other PDF
  processing tasks.
```

**Why good:**
- ✓ Specific format: PDF
- ✓ Enumerated use cases
- ✓ Keywords: extraction, forms, tables, metadata
- ✓ Clear triggers

### Mediocre Example

```yaml
name: document-processor
description: Process documents by reading, editing, and analyzing their
  content. Use for document-related tasks.
```

**Why mediocre:**
- △ No specific file types (documents? which formats?)
- △ Vague triggers ("document-related tasks")
- △ No enumerated use cases
- △ Generic keywords (process, edit, analyze)
- ❌ Too short (~20 words)

### Bad Example

```yaml
name: docx
description: Work with DOCX files
```

**Why bad:**
- ❌ No "when to use" information
- ❌ No enumerated use cases
- ❌ No keywords
- ❌ Way too short (5 words)
- ❌ When does this trigger vs a generic file handler?

### Assessment Criteria

When examining a description field:

**Required elements:**
- [ ] Includes what the skill does
- [ ] Includes when to use it
- [ ] Specifies file types/formats (if applicable)
- [ ] Enumerates use cases or provides examples
- [ ] Contains keywords that would trigger the skill

**Size guidelines:**
- [ ] ~50-150 words (rough target)
- [ ] Not too short (<20 words → too vague)
- [ ] Not too long (>200 words → bloated metadata)

**Specificity check:**
- [ ] Could another skill have the same description? (If yes → too vague)
- [ ] Would Claude know when to use this vs similar skills? (If no → needs more specificity)

**Common mistakes to flag:**
- ❌ "When to Use" section in body (useless, should be in description)
- ❌ Description only says what, not when
- ❌ Generic verbs (process, handle, manage) without specifics
- ❌ No file types mentioned when skill is format-specific
- ❌ Missing enumeration when multiple use cases exist

---

## Split Strategy (500-Line Guideline)

**Target: Keep SKILL.md under 500 lines**

### Why 500 Lines?

Not a hard limit, but a forcing function:
- Forces good information architecture
- Encourages proper use of references/
- Balances comprehensiveness with context efficiency
- Typical sweet spot: core workflow fits comfortably

**Red flag zones:**
- 400-500 lines: Start planning split strategy
- 500-700 lines: Should actively split
- 700+ lines: Urgently needs restructuring

### When Approaching 500 Lines: Decision Framework

**Step 1: Identify splittable content**

**Candidates for references/:**
- Domain-specific details (finance, sales, marketing)
- Framework-specific patterns (AWS, GCP, Azure)
- Advanced features (loaded only when needed)
- Detailed examples (keep 1-2 in SKILL.md, rest in references/)
- API documentation (complete reference)
- Variant-specific instructions

**Must stay in SKILL.md:**
- Core workflow and step sequence
- When to read which reference file (navigation logic)
- Essential patterns and principles
- High-level decision points

**Step 2: Choose organization pattern**

Match the pattern to your content:

| Content Type | Pattern | Example |
|--------------|---------|---------|
| Multiple domains | Domain-specific | references/finance.md, references/sales.md |
| Multiple frameworks | Framework-specific | references/aws.md, references/gcp.md |
| Basic + advanced | High-level guide | SKILL.md (basic), references/advanced.md |
| Multiple features | Feature-specific | references/feature-a.md, references/feature-b.md |

**Step 3: Execute split**

**Before:**
```markdown
# SKILL.md (650 lines)

## Core workflow
[50 lines]

## AWS deployment
[150 lines of AWS-specific details]

## GCP deployment
[150 lines of GCP-specific details]

## Azure deployment
[150 lines of Azure-specific details]

## Advanced features
[150 lines]
```

**After:**
```markdown
# SKILL.md (120 lines)

## Core workflow
[50 lines]

## Cloud provider deployment
Choose your provider:
- AWS: See references/aws.md
- GCP: See references/gcp.md
- Azure: See references/azure.md

[20 lines of provider-agnostic logic]

## Advanced features
For advanced features, see references/advanced.md
[20 lines of overview + signposting]

---

# references/aws.md (150 lines)
[AWS-specific deployment details]

# references/gcp.md (150 lines)
[GCP-specific deployment details]

# references/azure.md (150 lines)
[Azure-specific deployment details]

# references/advanced.md (150 lines)
[Advanced features details]
```

**Result:**
- SKILL.md: 650 → 120 lines (81% reduction)
- Core workflow preserved in SKILL.md
- Details moved to references/ (loaded only when needed)
- Clear navigation logic

### What to Keep in SKILL.md

**Core elements (never split out):**
1. **Workflow and step sequence**
   - The "how" of using the skill
   - Step-by-step instructions for common path

2. **Navigation logic**
   - When to read each reference file
   - How to choose between variants/options

3. **Essential patterns**
   - High-level principles that apply across all scenarios
   - Must-know concepts

4. **Decision points**
   - "If X then Y" logic for choosing paths
   - Branching criteria

**Size target after split:** 100-300 lines for SKILL.md

### What to Move to references/

**Detail-heavy content:**
1. **Complete API documentation** → `references/api-reference.md`
2. **Framework-specific patterns** → `references/{framework}.md`
3. **Domain-specific knowledge** → `references/{domain}.md`
4. **Advanced/optional features** → `references/advanced.md`
5. **Exhaustive examples** → `references/examples.md` (keep 1-2 in SKILL.md)
6. **Troubleshooting guides** → `references/troubleshooting.md`
7. **Edge case handling** → `references/edge-cases.md`

### Assessment Questions

When examining a skill approaching or exceeding 500 lines:

1. **Is there domain-specific content that could split?**
   - Look for sections specific to finance, sales, marketing, etc.
   - Each domain → separate reference file

2. **Is there framework-specific content?**
   - Look for AWS vs GCP vs Azure
   - Look for React vs Vue vs Angular
   - Each framework → separate reference file

3. **Are there detailed examples that could move?**
   - Keep 1-2 examples in SKILL.md
   - Move rest to references/examples.md

4. **Is there advanced/optional content?**
   - Content not needed for basic usage
   - Move to references/advanced.md

5. **Is there API documentation?**
   - Complete method/endpoint lists
   - Move to references/api-reference.md

### Common Mistakes

**Mistake 1: Moving workflow to references/**
```
❌ references/workflow.md (essential steps)
✓ Keep workflow in SKILL.md
```

**Mistake 2: No navigation logic**
```
❌ SKILL.md just says "see references/"
✓ SKILL.md explains when to read each reference
```

**Mistake 3: Splitting too aggressively**
```
❌ SKILL.md reduced to 20 lines, everything in references/
✓ SKILL.md contains complete workflow (100-300 lines)
```

**Mistake 4: Not splitting at all**
```
❌ SKILL.md at 800 lines, no references/
✓ Split before 500 lines, use references/ appropriately
```

---

## Duplication Rule

**Core rule:** Information should live in EITHER SKILL.md OR references/, not both.

### Why Duplication is Harmful

1. **Token waste:** Same info consumes context twice
2. **Maintenance burden:** Updates must happen in two places
3. **Inconsistency risk:** Two copies drift apart over time
4. **Confusion:** Which version is authoritative?

### Acceptable Duplication

**Very limited exceptions:**
- ✓ Brief overview in SKILL.md + full details in references/
- ✓ Example in SKILL.md + more examples in references/examples.md
- ✓ Principle stated in SKILL.md + application details in references/

**Key:** SKILL.md has high-level/summary, references/ have details. Not identical content.

### Unacceptable Duplication

**Never duplicate:**
- ❌ Same example in both SKILL.md and references/
- ❌ Same API documentation in both
- ❌ Same detailed instructions in both
- ❌ Same code snippet in both

### Where Information Should Live

**Prefer SKILL.md when:**
- Core workflow (80% of use cases)
- Essential instructions everyone needs
- High-level principles
- Navigation logic (when to read references/)

**Prefer references/ when:**
- Detailed information (full API docs)
- Domain-specific content
- Framework-specific patterns
- Advanced/optional features
- Exhaustive examples
- Edge cases and troubleshooting

### Detection During Examination

**How to detect duplication:**

1. **Extract key concepts** from SKILL.md
2. **Search references/** for same concepts
3. **Compare content:**
   - Identical → duplication (bad)
   - Summary vs detail → acceptable
   - Different aspects → acceptable

4. **Check for:**
   - Same example in multiple places
   - Same code snippet repeated
   - Same explanation with minor wording changes
   - Same table/list duplicated

**Assessment questions:**
- [ ] Is any example present in both SKILL.md and references/?
- [ ] Is any code snippet duplicated?
- [ ] Is any explanation repeated with minor variations?
- [ ] If the same topic appears in both, is SKILL.md high-level and references/ detailed?

### Remediation

**When duplication found:**

**Option 1: Keep in SKILL.md (if core workflow)**
```markdown
# Before
SKILL.md: [detailed explanation, 100 lines]
references/advanced.md: [same explanation, 100 lines]

# After
SKILL.md: [detailed explanation, 100 lines]
references/advanced.md: [removed or references back to SKILL.md]
```

**Option 2: Move to references/ (if detailed)**
```markdown
# Before
SKILL.md: [detailed API docs, 100 lines]
references/api-reference.md: [same API docs, 100 lines]

# After
SKILL.md: "See references/api-reference.md for complete API documentation"
references/api-reference.md: [detailed API docs, 100 lines]
```

**Option 3: Summary + detail pattern (if both needed)**
```markdown
# Before
SKILL.md: [detailed explanation, 100 lines]
references/advanced.md: [detailed explanation, 100 lines]

# After
SKILL.md: [2-3 sentence summary + "See references/advanced.md for details"]
references/advanced.md: [detailed explanation, 100 lines]
```

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

**Assessment:**
- [ ] Section headings use imperative form
- [ ] Step sequences use imperative form
- [ ] Instructions are direct commands, not descriptions

---

## Quality Assessment Checklist

Use this checklist when examining any skill.

### 1. Context Efficiency

- [ ] Prefer examples over explanations
- [ ] No verbose content that Claude can infer
- [ ] Every paragraph justifies its token cost
- [ ] Bulleted lists used instead of prose where applicable

### 2. Progressive Disclosure

**Level 1 - Metadata:**
- [ ] Description ~50-150 words
- [ ] Description includes what AND when
- [ ] Specific file types/formats mentioned
- [ ] Use cases enumerated or exemplified
- [ ] Contains trigger keywords

**Level 2 - SKILL.md Body:**
- [ ] Under 500 lines (or has clear split plan)
- [ ] Contains core workflow
- [ ] Navigation logic for references/ present
- [ ] No "When to Use" sections (belongs in description)
- [ ] Essential patterns included

**Level 3 - Bundled Resources:**
- [ ] scripts/ for executable code
- [ ] references/ for documentation
- [ ] assets/ for output files
- [ ] Clear guidance on when to read each reference

### 3. Resource Organization

**Structure:**
- [ ] Correct anatomy (SKILL.md + optional bundled resources)
- [ ] No unnecessary files (README, CHANGELOG, etc.)
- [ ] Resources in correct categories (scripts/ vs references/ vs assets/)

**References organization:**
- [ ] Pattern chosen appropriately (domain/framework/feature-specific)
- [ ] One level deep (not deeply nested)
- [ ] Files >100 lines have table of contents
- [ ] Clear, descriptive file names

### 4. Description Quality

- [ ] Includes what the skill does
- [ ] Includes when to use it
- [ ] Specifies file types/formats (if applicable)
- [ ] Enumerates use cases or provides examples
- [ ] Contains trigger keywords
- [ ] ~50-150 words (not too short or too long)
- [ ] Distinguishes this skill from similar skills

### 5. Duplication

- [ ] No identical content in SKILL.md and references/
- [ ] If same topic in both, SKILL.md is summary and references/ is detail
- [ ] No repeated examples across files
- [ ] No repeated code snippets

### 6. Writing Voice

- [ ] Section headings use imperative form
- [ ] Instructions use imperative form
- [ ] Consistent voice throughout

### 7. Split Strategy (if approaching 500 lines)

- [ ] Domain-specific content identified for split
- [ ] Framework-specific content identified for split
- [ ] Advanced features identified for split
- [ ] Core workflow identified to stay in SKILL.md
- [ ] Navigation logic planned for post-split SKILL.md

---

## Using These Standards During Examination

### Integration with Analysis Framework

The analysis framework (in main SKILL.md) provides the **structure** for examination. These standards provide the **criteria** for assessment.

**Mapping:**

| Analysis Framework Section | Relevant Standards |
|----------------------------|-------------------|
| 1. Structure Analysis | Progressive Disclosure (3-level), Resource Organization |
| 2. Trigger & Description | Description Field Excellence |
| 3. Directive Extraction | (structural, no specific standard) |
| 4. Contradiction Detection | (structural, no specific standard) |
| 5. Redundancy Analysis | Context Window Philosophy, Duplication Rule |
| 6. Temporal Analysis | (structural, no specific standard) |
| 7. Flow Mapping | (structural, no specific standard) |
| 8. Freedom Calibration | (already in framework, no additional standard needed) |
| 9. Progressive Disclosure Check | Progressive Disclosure, Resource Type Selection, Reference Organization |
| 10. Dead Code Identification | (structural, no specific standard) |

### How to Apply Standards

**During examination:**

1. **Read the standard** relevant to the section you're analyzing
2. **Apply the criteria** from the standard to the skill
3. **Identify gaps** between current state and standard
4. **Provide specific recommendations** based on the standard
5. **Use examples** from the standard to illustrate good/bad

**In the examination report:**

- Reference specific standards when identifying issues
- Quote criteria from standards to justify recommendations
- Use good/bad examples from standards to clarify issues
- Provide concrete remediation steps based on standards

**Example:**

Instead of:
```
Issue: Description is too short
Recommendation: Make it longer
```

Use standards to provide depth:
```
Issue: Description is 15 words, below the ~50-150 word target (see
"Description Field Excellence" standard). Missing "when to use"
information and enumerated use cases.

Current: "Process PDF files for various tasks"

Standard example: "Comprehensive PDF manipulation including text
extraction, form filling, page operations, and table parsing. Use when
Claude needs to: (1) extract text or tables from PDFs, (2) fill PDF
forms, (3) rotate, merge, or split PDF pages, (4) work with PDF
metadata, or any other PDF processing tasks."

Recommendation: Expand description to include:
1. Specific capabilities (what operations)
2. When to use (enumerated use cases)
3. Trigger keywords (extraction, forms, tables, etc.)
4. Target ~70-100 words
```

---

## Summary

These standards provide the domain expertise to evaluate skill quality beyond structural correctness.

**Key principles:**
1. **Context window is a public good** - justify every token
2. **Progressive disclosure** - right information at right level
3. **Resource type selection** - scripts/ vs references/ vs assets/ based on use
4. **Organization patterns** - domain/framework/feature-specific as appropriate
5. **Description excellence** - comprehensive triggers at metadata level
6. **Split strategy** - keep under 500 lines, split intelligently
7. **No duplication** - info lives in SKILL.md OR references/, not both
8. **Imperative voice** - direct, actionable instructions

Apply these standards during examination to produce insightful, actionable reports that improve skill quality.
