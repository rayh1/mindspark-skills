# Skill Quality Standards

Quality standards specific to SKILL.md files. For universal standards, see quality-standards-shared.md.

## Table of Contents

1. [Progressive Disclosure: 3-Level Loading](#progressive-disclosure-3-level-loading) - Lines 20-120
2. [Resource Type Selection Criteria](#resource-type-selection-criteria) - Lines 122-230
3. [Reference Organization Patterns](#reference-organization-patterns) - Lines 232-350
4. [Description Field Excellence](#description-field-excellence) - Lines 352-480
5. [Split Strategy (500-Line Guideline)](#split-strategy-500-line-guideline) - Lines 482-600
6. [Frontmatter Complete Schema](#frontmatter-complete-schema)
7. [MCP Skill Quality Standards](#mcp-skill-quality-standards)
8. [Skill Quality Checklist](#skill-quality-checklist)

---

## Progressive Disclosure: 3-Level Loading

Skills use a three-level loading system. Understanding this is essential for evaluating architecture quality.

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

**Red flags:**
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

**Red flags:**
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

**Red flags:**
- ❌ Essential workflow instructions buried in references/
- ❌ SKILL.md doesn't explain when to read each reference
- ❌ References are deeply nested (>1 level deep)
- ❌ Large reference files (>100 lines) lack table of contents

### Loading Model Assessment

When reviewing a skill, evaluate:

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

Understanding WHEN to use scripts/ vs references/ vs assets/ is critical.

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
- `aws.md` - AWS-specific deployment patterns

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

**Red flags:**
- ❌ Documentation in assets/ (should be references/)
- ❌ Template in references/ (should be assets/)
- ❌ Script in assets/ (should be scripts/)
- ❌ Asset used in SKILL.md examples but doesn't exist

### Selection Decision Tree

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

---

## Reference Organization Patterns

When skills grow complex, references/ organization becomes critical.

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
```

**Quality markers:**
- ✓ Clear signposting: "See [file] for..."
- ✓ Self-contained references (readable independently)
- ✓ Quick start in SKILL.md covers 80% of use cases

### Pattern 2: Domain-Specific Organization

**Use when:** Skill supports multiple domains/categories

**Structure:**
```
bigquery-skill/
├── SKILL.md (overview + domain selection logic)
└── references/
    ├── finance.md (revenue, billing metrics)
    ├── sales.md (opportunities, pipeline)
    └── product.md (API usage, features)
```

**Quality markers:**
- ✓ Domains are mutually exclusive
- ✓ SKILL.md explains how to choose domain
- ✓ Each reference is self-contained for its domain

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

**Quality markers:**
- ✓ SKILL.md contains framework-agnostic workflow
- ✓ Variant files contain ONLY variant-specific details
- ✓ Clear selection criteria in SKILL.md

### Organization Best Practices

**Nesting depth:**
- ✓ One level deep: `references/file.md`
- ❌ Two+ levels: `references/advanced/feature/details.md`

**Table of contents:**
- Required for references/ files >100 lines
- Place at top of file
- Include line numbers or section links

**File naming:**
- ✓ Descriptive: `authentication-guide.md`, `aws-deployment.md`
- ❌ Vague: `advanced.md`, `misc.md`, `other.md`

---

## Description Field Excellence

The description field is the MOST IMPORTANT part of a skill. It determines when the skill gets used.

### The Loading Model Reality

**Critical insight:** The description is loaded at Level 1 (metadata), BEFORE the skill body.

This means:
- ✓ Description determines whether skill triggers
- ❌ Body is useless for triggering (loads AFTER decision)
- ❌ "When to Use This Skill" sections in body are worthless for triggering

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

### Assessment Criteria

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

**Common mistakes:**
- ❌ "When to Use" section in body (useless, should be in description)
- ❌ Description only says what, not when
- ❌ Generic verbs (process, handle, manage) without specifics
- ❌ No file types mentioned when skill is format-specific

---

## Split Strategy (500-Line Guideline)

**Target: Keep SKILL.md under 500 lines**

### Why 500 Lines?

Not a hard limit, but a forcing function:
- Forces good information architecture
- Encourages proper use of references/
- Balances comprehensiveness with context efficiency

**Red flag zones:**
- 400-500 lines: Start planning split strategy
- 500-700 lines: Should actively split
- 700+ lines: Urgently needs restructuring

### When Approaching 500 Lines

**Candidates for references/:**
- Domain-specific details (finance, sales, marketing)
- Framework-specific patterns (AWS, GCP, Azure)
- Advanced features (loaded only when needed)
- Detailed examples (keep 1-2 in SKILL.md, rest in references/)
- API documentation (complete reference)

**Must stay in SKILL.md:**
- Core workflow and step sequence
- When to read which reference file (navigation logic)
- Essential patterns and principles
- High-level decision points

**Size target after split:** 100-300 lines for SKILL.md

### Common Split Mistakes

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

---

---

## Frontmatter Complete Schema

### Required Fields

```yaml
---
name: skill-name-in-kebab-case   # required; kebab-case only, no spaces/capitals
description: What it does and when to use it. Include specific trigger phrases.  # required; ~50-150 words
---
```

### Optional Fields

```yaml
license: MIT                      # open-source skills; e.g. MIT, Apache-2.0
compatibility: Requires Python 3.9+, network access, Claude Code  # environment requirements; 1-500 chars
allowed-tools: "Bash(python:*) WebFetch"  # restrict tool access for scoping/security
metadata:
  author: Company Name
  version: 1.0.0
  mcp-server: server-name         # associate skill with a specific MCP server
  category: productivity
  tags: [project-management, automation]
  documentation: https://example.com/docs
  support: support@example.com
```

### Security Restrictions

**Forbidden in frontmatter — these are security rules, not style guidelines:**

| Forbidden | Why |
|---|---|
| XML angle brackets (`<` or `>`) anywhere in frontmatter | Frontmatter is always loaded into Claude's system prompt. Angle brackets can inject instructions into that context, altering Claude's behavior in every conversation where the skill is active. |
| `name` starting with "claude" or "anthropic" | Reserved prefixes; violates Anthropic naming policy |

### When to Use Optional Fields

- `compatibility`: Use whenever the skill has environment requirements users may not meet (packages, network, specific Claude surface, MCP server). Prevents silent failures.
- `allowed-tools`: Use when the skill should not invoke all available tools. Provides security scoping and makes skill intent explicit.
- `metadata.mcp-server`: Use for Category 3 (MCP Enhancement) skills to document the associated server.

### Name-to-Folder Consistency

The `name` value in frontmatter should match the skill's folder name exactly. Mismatches break install conventions and can cause skills to fail to load correctly.

```
✓ Folder: notion-project-setup/  →  name: notion-project-setup
✗ Folder: notion-project-setup/  →  name: notion_project_setup
✗ Folder: NotionProjectSetup/    →  name: notion-project-setup
```

---

## MCP Skill Quality Standards

Applies to **Category 3 (MCP Enhancement)** skills and partially to **Category 2 (Workflow Automation)** skills that make MCP calls.

### What Good MCP Error Handling Looks Like

A well-written MCP skill includes a troubleshooting section that addresses the three most common failure modes:

```markdown
## Troubleshooting

### MCP Connection Failed
If you see "Connection refused":
1. Verify MCP server is running: Settings > Extensions > [Service]
2. Confirm API key is valid and not expired
3. Try reconnecting: Settings > Extensions > [Service] > Reconnect

### Authentication Errors
- OAuth: tokens may need refresh — disconnect and reconnect the integration
- API keys: verify key has correct permissions/scopes for required operations

### Tool Call Failures
- MCP tool names are case-sensitive: verify exact names in MCP server documentation
- To isolate whether failure is in the skill or the MCP connection, test the MCP tool directly: "Use [Service] MCP to [action]" — if this also fails, the issue is the MCP connection, not the skill
```

### Tool Name Discipline

- Always reference MCP tool names using exact casing (e.g., `create_customer`, not `Create_Customer`)
- Note case-sensitivity in the skill when referencing tools
- List the MCP tools used in the skill's instructions so users know what to expect

### MCP + `metadata.mcp-server`

Category 3 skills should declare their associated MCP server in frontmatter:

```yaml
metadata:
  mcp-server: notion          # or "linear", "sentry", etc.
```

This helps users understand the dependency before installing the skill.

---

## Skill Quality Checklist

### Progressive Disclosure
- [ ] Description ~50-150 words
- [ ] Description includes what AND when
- [ ] Specific file types/formats mentioned (if applicable)
- [ ] Use cases enumerated
- [ ] Contains trigger keywords
- [ ] SKILL.md under 500 lines (or has split plan)
- [ ] Contains core workflow
- [ ] Navigation logic for references/ present
- [ ] No "When to Use" sections in body

### Resource Organization
- [ ] Correct anatomy (SKILL.md + optional resources)
- [ ] No unnecessary files (README, CHANGELOG, etc.)
- [ ] Resources in correct categories
- [ ] References one level deep
- [ ] Files >100 lines have TOC
- [ ] Clear, descriptive file names

### Description Quality
- [ ] Matches "Excellent" or "Good" examples
- [ ] Uses specific verbs, not generic
- [ ] Distinguishes from similar skills
- [ ] Appropriate length

### Duplication (see quality-standards-shared.md)
- [ ] No identical content in SKILL.md and references/
- [ ] Summary in SKILL.md, detail in references/ (if both)
- [ ] No repeated examples
- [ ] No repeated code snippets

### Frontmatter Security & Completeness
- [ ] No XML angle brackets in frontmatter (injection risk)
- [ ] name does not start with "claude" or "anthropic"
- [ ] name matches folder name
- [ ] `compatibility` field present if skill has external dependencies
- [ ] `allowed-tools` field present if tool scoping is appropriate
- [ ] `metadata.mcp-server` declared for Category 3 skills

### MCP Quality (Category 2/3 Only)
- [ ] MCP connection failure handling documented
- [ ] Authentication failure handling documented
- [ ] Tool names referenced with exact casing
- [ ] Independent MCP testing guidance present
- [ ] Negative trigger present if sibling skill covers overlapping domain
