# Skill Architecture Guide

Essential knowledge for building production-quality Claude Code skills that manage context efficiently and follow progressive disclosure principles.

---

## Table of Contents

- [Core Philosophy: Context Window as Public Good](#core-philosophy-context-window-as-public-good) - Line 7
- [Skill Anatomy](#skill-anatomy) - Line 27
- [Progressive Disclosure: 3-Level Loading](#progressive-disclosure-3-level-loading) - Line 46
- [YAML Frontmatter: The Trigger Mechanism](#yaml-frontmatter-the-trigger-mechanism) - Line 69
- [Bundled Resources: When and How](#bundled-resources-when-and-how) - Line 105
- [The 500-Line Guideline](#the-500-line-guideline) - Line 210
- [What NOT to Include](#what-not-to-include) - Line 252
- [Degrees of Freedom](#degrees-of-freedom) - Line 273
- [Integration with build-with-patterns](#integration-with-build-with-patterns) - Line 326
- [Examples: Good vs Bad](#examples-good-vs-bad) - Line 358
- [Quick Reference Checklist](#quick-reference-checklist) - Line 433
- [Summary](#summary) - Line 468

---

## Core Philosophy: Context Window as Public Good

**Default assumption: Claude is already very smart.**

Only add context Claude doesn't already have. Challenge each piece of information:
- "Does Claude really need this explanation?"
- "Does this paragraph justify its token cost?"

**Prefer concise examples over verbose explanations.**

Skills share the context window with:
- System prompt
- Conversation history
- Other skills' metadata
- User requests

Every token in a skill reduces space available for actual work.

---

## Skill Anatomy

Every skill consists of a required SKILL.md file and optional bundled resources:

```
skill-name/
├── SKILL.md (required)
│   ├── YAML frontmatter (required)
│   │   ├── name: (required)
│   │   └── description: (required)
│   └── XML-tagged sections (required)
└── Bundled Resources (optional)
    ├── scripts/          - Executable code
    ├── references/       - Documentation loaded as needed
    └── assets/           - Files used in output
```

---

## Progressive Disclosure: 3-Level Loading

Skills use a three-level loading system to manage context efficiently:

### Level 1: Metadata (always loaded, ~100 words)
- YAML frontmatter: `name` + `description`
- Loaded for ALL skills in the system
- Used to determine which skill to trigger

### Level 2: SKILL.md body (loaded when triggered, <5k words)
- XML-tagged sections with instructions
- **Target: under 500 lines**
- Only loaded AFTER skill triggers

### Level 3: Bundled resources (loaded as needed, unlimited)
- Claude decides when to read references/
- Scripts can execute without loading to context
- Assets never loaded (used in output only)

**Key principle:** When a skill supports multiple variations, keep only the core workflow in SKILL.md. Move variant-specific details into separate reference files.

---

## YAML Frontmatter: The Trigger Mechanism

### Description Field = Primary Trigger

The description field is THE MOST IMPORTANT part of your skill. It determines when the skill gets used.

**Structure:**
```yaml
description: [What the skill does] + [When to use it with specific examples]
```

**Good example:**
```yaml
---
name: docx
description: Comprehensive document creation, editing, and analysis with support for tracked changes, comments, formatting preservation, and text extraction. Use when Claude needs to work with professional documents (.docx files) for: (1) Creating new documents, (2) Modifying or editing content, (3) Working with tracked changes, (4) Adding comments, or any other document tasks
---
```

**Bad example:**
```yaml
---
name: docx
description: Work with DOCX files
---
```
→ Too vague. When should this trigger vs a generic file handler?

**Critical rules:**
- Include "when to use" information in description, NOT in body
- Body is only loaded AFTER triggering, so "When to Use This Skill" sections in the body are worthless
- Be specific about triggers (file types, use cases, keywords)
- Include examples of what should trigger the skill

---

## Bundled Resources: When and How

### scripts/ - Executable Code

**When to include:**
- Same code is being rewritten repeatedly
- Deterministic reliability is required
- Operation is complex and error-prone

**Examples:**
- `scripts/rotate_pdf.py` for PDF rotation
- `scripts/extract_tables.py` for table extraction
- `scripts/validate_schema.py` for data validation

**Benefits:**
- Token efficient (can execute without loading to context)
- Deterministic behavior
- Versioned and tested

**Note:** Scripts may still need to be read for patching or environment-specific adjustments.

---

### references/ - Documentation Loaded As Needed

**When to include:**
- Documentation Claude should reference while working
- Domain knowledge not in base model training
- Information too detailed for SKILL.md

**Examples:**
- `references/schema.md` - Database schemas, table relationships
- `references/api_docs.md` - API specifications
- `references/finance.md` - Financial domain knowledge for specific company
- `references/policies.md` - Company-specific policies
- `references/patterns.md` - Detailed workflow guides

**Best practices:**
- For files >100 lines: include table of contents at top
- For large files (>10k words): include grep search patterns in SKILL.md
- Avoid duplication: information lives in SKILL.md OR references/, not both
- Keep references one level deep (no deeply nested structure)

**Organization patterns:**

**Pattern 1: Domain-specific organization**
```
bigquery-skill/
├── SKILL.md (overview and navigation)
└── references/
    ├── finance.md (revenue, billing metrics)
    ├── sales.md (opportunities, pipeline)
    ├── product.md (API usage, features)
    └── marketing.md (campaigns, attribution)
```
When user asks about sales metrics, Claude only reads sales.md.

**Pattern 2: Framework-specific organization**
```
cloud-deploy/
├── SKILL.md (workflow + provider selection)
└── references/
    ├── aws.md (AWS deployment patterns)
    ├── gcp.md (GCP deployment patterns)
    └── azure.md (Azure deployment patterns)
```
When user chooses AWS, Claude only reads aws.md.

**Pattern 3: Conditional details**
```markdown
# SKILL.md

## Basic workflow
[core instructions]

## Advanced features
- **Feature A**: See [feature-a.md](references/feature-a.md)
- **Feature B**: See [feature-b.md](references/feature-b.md)
```
Claude loads feature files only when needed.

---

### assets/ - Files Used in Output

**When to include:**
- Files that will be used in the final output
- Templates that get copied or modified
- Resources needed by generated code

**Examples:**
- `assets/logo.png` - Brand assets
- `assets/template.pptx` - PowerPoint templates
- `assets/frontend-template/` - HTML/React boilerplate
- `assets/font.ttf` - Typography files
- `assets/config-template.yaml` - Configuration templates

**Key difference from references/:**
- assets/ are NEVER loaded into context
- They are used/copied/modified in the output
- references/ are loaded to inform Claude's thinking
- assets/ are raw materials for the deliverable

---

## The 500-Line Guideline

**Target: Keep SKILL.md under 500 lines**

Why 500 lines?
- Balances comprehensiveness with context efficiency
- Forces good information architecture
- Encourages proper use of references/

**When approaching 500 lines:**

1. **Identify splittable content:**
   - Domain-specific details → `references/domain.md`
   - Framework-specific patterns → `references/framework.md`
   - Advanced features → `references/advanced.md`
   - Examples → `references/examples.md`

2. **Keep in SKILL.md:**
   - Core workflow and step sequence
   - When to read which reference file
   - Essential patterns and principles
   - Navigation/selection logic

3. **Move to references/:**
   - Detailed examples
   - API documentation
   - Domain knowledge
   - Variant-specific instructions

**Example split:**
```markdown
# SKILL.md (120 lines)
Core workflow + which reference to read when

# references/
├── quickstart.md (80 lines) - Common patterns
├── advanced.md (200 lines) - Complex scenarios
└── api-reference.md (300 lines) - Complete API docs
```

---

## What NOT to Include

Skills should contain ONLY files essential for AI agent execution.

**Do NOT create:**
- ❌ README.md (users don't read skills; Claude does)
- ❌ INSTALLATION_GUIDE.md (not a software package)
- ❌ QUICK_REFERENCE.md (that's what SKILL.md is)
- ❌ CHANGELOG.md (use version control)
- ❌ CONTRIBUTING.md (not a community project)
- ❌ EXAMPLES.md in root (use references/ if needed)
- ❌ Any other auxiliary documentation

**Why:**
- These files add clutter and confusion
- Claude doesn't need setup procedures or user-facing docs
- All essential info belongs in SKILL.md or references/
- Skills are not user-facing software packages

---

## Degrees of Freedom

Match the level of specificity to the task's fragility and variability:

### High Freedom (text-based instructions)
**When to use:**
- Multiple approaches are valid
- Decisions depend on context
- Heuristics guide the approach

**Example:**
```markdown
Analyze the code for potential security issues. Focus on:
- Input validation
- Authentication/authorization
- Data exposure
Consider the severity and likelihood of each issue.
```

### Medium Freedom (pseudocode or scripts with parameters)
**When to use:**
- Preferred pattern exists
- Some variation is acceptable
- Configuration affects behavior

**Example:**
```markdown
Extract tables using this approach:
1. Detect table boundaries
2. Parse rows/columns
3. Handle merged cells (strategy depends on table structure)
4. Export to requested format

See references/table-patterns.md for common strategies.
```

### Low Freedom (specific scripts, few parameters)
**When to use:**
- Operations are fragile and error-prone
- Consistency is critical
- Specific sequence must be followed

**Example:**
```bash
python scripts/rotate_pdf.py --input {file} --degrees {90|180|270}
```

**Mental model:** Think of Claude exploring a path:
- Narrow bridge with cliffs → specific guardrails (low freedom)
- Open field → many valid routes (high freedom)

---

## Integration with build-with-patterns

When building skills with build-with-patterns:

### During intake (Step 1):
Ask about bundled resources:
- "Will this need any executable scripts?"
- "Is there domain-specific documentation to include?"
- "Are there template files or assets needed for output?"

### When defining output contract (Step 2):
Consider:
- Will SKILL.md exceed 500 lines? Plan to split.
- What goes in SKILL.md vs references/?
- What triggers loading of each reference file?

### When drafting (Step 4):
- Write comprehensive description with triggers
- Reference bundled resources clearly
- Add navigation logic for references/
- Keep core workflow in SKILL.md, details in references/

### During validation (Step 5):
Check:
- ✓ Description includes specific trigger conditions
- ✓ SKILL.md under 500 lines (or has clear split strategy)
- ✓ No README or auxiliary docs
- ✓ Bundled resources properly organized
- ✓ References/ files have clear "when to read" guidance

---

## Examples: Good vs Bad

### GOOD: Progressive Disclosure
```
pdf-skill/
├── SKILL.md (200 lines)
│   Core workflow + references to advanced features
├── scripts/
│   ├── rotate.py
│   └── extract_tables.py
├── references/
│   ├── forms.md (form filling guide)
│   └── api-reference.md (complete API docs)
└── assets/
    └── template.pdf

SKILL.md excerpt:
"For basic text extraction, use pdfplumber.open().
For form filling, see references/forms.md.
For complete API reference, see references/api-reference.md."
```
→ Clean separation, loads only what's needed

### BAD: Everything in SKILL.md
```
pdf-skill/
└── SKILL.md (1200 lines)
    All extraction code, all form filling code,
    complete API docs, every example, inline
```
→ Hogs context window, no progressive disclosure

### GOOD: Clear Resource Types
```
frontend-webapp/
├── SKILL.md (150 lines)
├── scripts/
│   └── init_project.py (creates boilerplate)
├── references/
│   ├── react-patterns.md (loaded when using React)
│   └── vue-patterns.md (loaded when using Vue)
└── assets/
    ├── react-template/ (copied to output)
    └── vue-template/ (copied to output)
```
→ Scripts execute, references inform, assets copied

### BAD: Confused Resource Types
```
frontend-webapp/
└── references/
    ├── react-template/ (should be assets/)
    ├── init-script.py (should be scripts/)
    └── README.md (shouldn't exist)
```
→ Wrong organization, unnecessary files

### GOOD: Trigger-Rich Description
```yaml
description: Comprehensive PDF manipulation including text extraction,
form filling, page operations, and table parsing. Use when Claude needs
to: (1) extract text or tables from PDFs, (2) fill PDF forms, (3) rotate,
merge, or split PDF pages, (4) work with PDF metadata, or any other PDF
processing tasks.
```
→ Clear triggers, comprehensive coverage

### BAD: Vague Description
```yaml
description: Work with PDF files
```
→ When does this trigger vs generic file handling?

---

## Quick Reference Checklist

When building a skill:

**YAML frontmatter:**
- [ ] Description includes "when to use" with specific examples
- [ ] Description mentions file types, use cases, or keywords

**SKILL.md body:**
- [ ] Under 500 lines (or has clear split strategy)
- [ ] Core workflow present
- [ ] Clear navigation to references/
- [ ] No "When to Use" section (that's in description)

**Bundled resources:**
- [ ] scripts/ for deterministic/repeated code
- [ ] references/ for documentation Claude reads
- [ ] assets/ for files used in output
- [ ] references/ files have TOC if >100 lines
- [ ] No deeply nested structure (one level)

**What NOT to include:**
- [ ] No README.md
- [ ] No INSTALLATION_GUIDE.md
- [ ] No CHANGELOG.md
- [ ] No auxiliary documentation

**Progressive disclosure:**
- [ ] Metadata always loaded (~100 words)
- [ ] SKILL.md loaded when triggered (<5k words)
- [ ] References loaded as needed (unlimited)
- [ ] Clear guidance on when to read each reference

---

## Summary

A well-architected skill:
1. **Triggers precisely** (description includes specific "when to use")
2. **Stays lean** (SKILL.md under 500 lines)
3. **Discloses progressively** (metadata → body → references → assets)
4. **Organizes resources** (scripts/ vs references/ vs assets/ used correctly)
5. **Respects context** (every token justified)
6. **Avoids bloat** (no README or auxiliary docs)

Apply these principles during all phases of build-with-patterns to create skills that are efficient, maintainable, and context-aware.
