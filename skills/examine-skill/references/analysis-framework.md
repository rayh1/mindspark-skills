# Analysis Framework

Detailed 12-section framework for examining skills systematically.

## Table of Contents

1. [Structure Analysis](#1-structure-analysis) - Lines 30-90
2. [Trigger & Description Examination](#2-trigger--description-examination) - Lines 92-215
3. [Directive Extraction](#3-directive-extraction) - Lines 217-230
4. [Contradiction Detection](#4-contradiction-detection) - Lines 232-250
5. [Redundancy Analysis](#5-redundancy-analysis) - Lines 252-380
6. [Temporal Analysis](#6-temporal-analysis) - Lines 382-400
7. [Flow Mapping](#7-flow-mapping) - Lines 402-425
8. [Freedom Calibration](#8-freedom-calibration) - Lines 427-455
9. [Progressive Disclosure Check](#9-progressive-disclosure-check) - Lines 457-640
10. [Dead Code Identification](#10-dead-code-identification) - Lines 642-665
11. [Writing Voice](#11-writing-voice) - Lines 667-720
12. [Context Efficiency](#12-context-efficiency) - Lines 722-790

---

## Note

For detailed quality criteria and assessment standards, consult **skill-quality-standards.md**.
The framework below provides the structure; the standards provide the criteria.

---

## Work Through Each Section Systematically

**1. STRUCTURE ANALYSIS**

- [D-59] **Correct anatomy?**
  - Required: SKILL.md with YAML frontmatter
  - Optional: scripts/, references/, assets/
  - **CRITICAL** if SKILL.md missing or lacks YAML frontmatter

- [D-60] **YAML frontmatter complete?**
  - Required fields: name, description
  - **CRITICAL** if either field missing
  - **HIGH** if description is empty or placeholder

- [D-61] **SKILL.md size check** (500-line guideline):
  - Count lines in SKILL.md (excluding frontmatter)
  - **Severity based on size:**
    - <400 lines → ✓ Good
    - 400-500 lines → **LOW** (start planning split strategy)
    - 500-700 lines → **MEDIUM** (should actively split)
    - 700+ lines → **HIGH** (urgently needs restructuring)
  - If over 500 lines, identify splittable content:
    - Domain-specific details → references/{domain}.md
    - Framework-specific patterns → references/{framework}.md
    - Advanced features → references/advanced.md
    - Detailed examples → references/examples.md
    - API documentation → references/api-reference.md
  - See § Split Strategy in skill-quality-standards.md

- [D-62] **Reference files properly organized and clearly named?**
  - Check organization pattern (domain/framework/feature-specific)
  - Check file naming (descriptive vs vague)
  - Check nesting depth (should be ≤1 level)
  - **MEDIUM** if references nested >1 level deep
  - **LOW** if naming could be clearer (e.g., "misc.md", "other.md")

- [D-63] **Unnecessary documentation present?**
  - Check for: README.md, CHANGELOG.md, INSTALLATION_GUIDE.md, CONTRIBUTING.md
  - **MEDIUM** severity if found (skills are for AI agents, not users)
  - Recommendation: Remove these files

---

**2. TRIGGER & DESCRIPTION EXAMINATION**

(See references/skill-quality-standards.md § Description Field Excellence for detailed criteria)

Critical checks with severity guidance:

- [D-64] **Description size:** Target ~50-150 words
  - Count words in description field
  - **CRITICAL** if <20 words (too vague to trigger correctly)
  - **HIGH** if <50 words (insufficient detail for reliable triggering)
  - **MEDIUM** if >200 words (bloated metadata, hurts context efficiency)
  - **LOW** if 30-49 words (functional but could be more comprehensive)

- [D-65] **Description content requirements** (check all):
  - [ ] What the skill does (capability summary)
  - [ ] When to use it (trigger conditions)
  - [ ] Specific file types/formats (if applicable)
  - [ ] Enumerated use cases (concrete examples: "Use when: (1)..., (2)..., (3)...")
  - [ ] Trigger keywords (terms that should activate the skill)
  - **CRITICAL** if missing "when to use" (skill won't trigger appropriately)
  - **HIGH** if missing enumerated use cases or file types (when applicable)
  - **MEDIUM** if missing trigger keywords

- [D-66] **"When to Use" sections in body** (anti-pattern):
  - Search SKILL.md body for "When to Use", "Use this skill when", etc.
  - **CRITICAL** issue if found (body loads AFTER triggering decision)
  - Recommendation: Move to description field

- [D-67] **Trigger clarity test** (distinguishability):
  - Could another skill have this same description? (If yes: too vague)
  - Would Claude know when to use THIS skill vs similar skills?
  - Compare to similar skills if they exist
  - **HIGH** if description is too generic to distinguish
  - **MEDIUM** if description could be more specific

**Description Quality Depth:**

Beyond structural checks, assess description excellence:

- [D-101] **Compare to standard examples:**
  - Read § Description Field Excellence examples (Excellent/Good/Mediocre/Bad)
  - Which category does this description match?
  - Excellent: Comprehensive, specific, enumerated, with keywords
  - Good: Covers essentials, room for improvement
  - Mediocre: Vague, missing key elements
  - Bad: Minimal, no triggers
  - **HIGH** if matches "Mediocre" or "Bad" examples

- [D-102] **Specificity assessment:**
  - Generic verbs present? ("process", "handle", "manage", "work with")
  - Specific verbs present? ("extract", "rotate", "merge", "convert", "validate")
  - Generic verbs without specifics → vague triggering
  - **MEDIUM** if primarily generic verbs, no specific operations listed

- [D-103] **Distinguishability from similar skills:**
  - List skills with similar capabilities (if any exist)
  - Would this description trigger correctly vs those skills?
  - Does it specify what makes THIS skill different?
  - Example: If multiple document skills exist, does description specify .docx vs .pdf?
  - **HIGH** if description doesn't distinguish from similar skills
  - **MEDIUM** if distinction exists but could be clearer

---

**3. DIRECTIVE EXTRACTION**

Create numbered list of EVERY instruction/directive:
- [D-68] Extract each "do this" or "don't do this" statement
- [D-69] Note file and line number for each
- [D-70] Flag vague or ambiguous directives

---

**4. CONTRADICTION DETECTION**

Compare all extracted directives:
- [D-71] Identify conflicting pairs
- [D-72] Check if same topic addressed differently in multiple places
- [D-73] Check if reference files contradict SKILL.md
- [D-74] List each contradiction with specific quotes and locations

---

**5. REDUNDANCY ANALYSIS**

(See references/skill-quality-standards.md § Context Window Philosophy and § Duplication Rule)

Apply the Challenge Test to all content:

- [D-75] **Cross-file duplication** (SKILL.md ↔ references/):
  - Extract key concepts from SKILL.md
  - Search references/ for same concepts
  - Compare content:
    - Identical content in both → **HIGH** severity (duplication rule violation)
    - Summary in SKILL.md + details in references/ → Acceptable (proper pattern)
    - Different aspects of same topic → Acceptable
  - Check specifically for:
    - Same examples in both places → **HIGH**
    - Same code snippets duplicated → **HIGH**
    - Same explanation with minor wording changes → **HIGH**
    - Same table/list duplicated → **MEDIUM**

- [D-76] **Explanations duplicating Claude's base knowledge:**
  - Look for: basic programming concepts, common patterns, general knowledge
  - Examples of unnecessary explanations:
    - "A function is a reusable block of code..."
    - "JSON is a data format..."
    - "Use try/catch for error handling..." (unless skill-specific nuance)
  - **MEDIUM** severity if significant base-knowledge duplication found
  - Apply test: "Would Claude already know this from training?"

- [D-77] **Verbose sections that could be condensed:**
  - Look for prose paragraphs that could be:
    - Bulleted lists (more scannable)
    - Code examples (show don't tell)
    - Tables (structured data)
  - Count: explanations >50 words where example would suffice
  - **MEDIUM** severity if 3+ instances found
  - **LOW** severity if 1-2 instances

- [D-78] **Token cost justification** (The Challenge Test):
  - For each section >100 words, ask:
    - "Does Claude really need this explanation?"
    - "Does this paragraph justify its token cost?"
    - "Is this information Claude can already infer from context?"
  - Look for warning signs:
    - Introductory fluff ("As you know...", "It's important to understand...")
    - Obvious statements that add no value
    - Repetition of the same point in different words
  - **MEDIUM** severity if significant unjustified verbosity

**Systematic Cross-File Duplication Check:**

Specific check for SKILL.md ↔ references/ duplication (beyond general redundancy):

- [D-104] **Systematic cross-file duplication check:**
  - **Step 1:** Extract distinct examples from SKILL.md (code snippets, workflows, patterns)
  - **Step 2:** For each example, search all references/ files for same/similar content
  - **Step 3:** Classify duplication:
    - **Identical:** Same example, code, or explanation in both
      - **HIGH** severity (violates duplication rule)
      - Recommendation: Keep in ONE location only
    - **Summary + Detail:** Brief mention in SKILL.md, full detail in references/
      - Acceptable pattern (verify summary is truly brief)
      - **MEDIUM** if "summary" is actually detailed (should remove from SKILL.md)
    - **Different aspects:** Different information about same topic
      - Acceptable (not duplication)
  - **Step 4:** Count violations
    - 1-2 identical duplications → **HIGH**
    - 3+ identical duplications → **CRITICAL** (systemic issue)
  - **Step 5:** Recommend consolidation strategy
    - If core workflow → keep in SKILL.md, remove from references/
    - If detailed/optional → keep in references/, remove from SKILL.md

---

**6. TEMPORAL ANALYSIS (Outdated Content)**

- [D-79] References to specific dates, versions, or "new" features
- [D-80] Language suggesting currency that may be old ("recently", "now", "the latest")
- [D-81] Deprecated tools, APIs, or approaches
- [D-82] Instructions referencing features/behaviors that may have changed

---

**7. FLOW MAPPING**

For each major user scenario:
- [D-83] Trace complete decision path through all files
- [D-84] Identify dead ends or unclear branches
- [D-85] Find circular references or infinite loops
- [D-86] Map where user/Claude might get confused

---

**8. FREEDOM CALIBRATION**

For each instruction, assess specificity level:
- [D-87] **HIGH freedom** (text guidance) — appropriate when multiple approaches valid
- [D-88] **MEDIUM freedom** (pseudocode/parameters) — appropriate when preferred pattern exists
- [D-89] **LOW freedom** (specific scripts) — appropriate when operations fragile

Flag mismatches:
- [D-90] Overly rigid instructions limiting valid approaches
- [D-91] Overly loose instructions leaving fragile operations undefined

---

**9. PROGRESSIVE DISCLOSURE CHECK**

(See references/skill-quality-standards.md § Progressive Disclosure and § Resource Type Selection)

Verify information is at the right level (3-level loading model):

**Level 1 - Metadata (description field):**
- Target: ~100 words (~600-800 tokens)
- Should contain: triggers, not detailed instructions
- Already checked in Section 2 (D-64 to D-67)

**Level 2 - SKILL.md body:**
- Target: <500 lines, <5k words
- Should contain: core workflow, navigation logic, essential patterns
- Already checked in Section 1 (D-61)

**Level 3 - Bundled resources:**
- Target: Unlimited (loaded as needed)
- Should contain: details, examples, framework-specific content

**Content placement verification:**

- [D-92] **Essential workflow in SKILL.md?**
  - Core step sequence should be in SKILL.md, not references/
  - **CRITICAL** if essential workflow buried in references/
  - **HIGH** if navigation logic missing (when to read which reference)

- [D-93] **Detailed references in references/?**
  - Check for detailed content in SKILL.md that should move:
    - Complete API documentation → references/api-reference.md
    - Framework-specific patterns → references/{framework}.md
    - Domain-specific knowledge → references/{domain}.md
  - **MEDIUM** if significant details in SKILL.md (should be in references/)

- [D-94] **Executable code in scripts/?**
  - Check if code should be in scripts/:
    - Repeatedly rewritten code → scripts/
    - Deterministic reliability needed → scripts/
    - Complex/error-prone operations → scripts/
  - **LOW** if appropriate code inline (could be in scripts/)
  - **MEDIUM** if complex code repeated in SKILL.md

- [D-95] **Output resources in assets/?**
  - Check for resources in wrong category:
    - Templates in references/ → **HIGH** severity (should be assets/)
    - Documentation in assets/ → **HIGH** severity (should be references/)
    - Fonts/images/boilerplate in references/ → **MEDIUM** (should be assets/)

- [D-96] **SKILL.md overload check:**
  - Is SKILL.md trying to do too much? Signs:
    - Multiple framework sections (AWS + GCP + Azure all in SKILL.md)
    - Complete API docs inline
    - Exhaustive examples (>5 examples for same pattern)
  - **MEDIUM** severity if SKILL.md is overloaded
  - Are reference files clearly signposted?
    - Does SKILL.md explain when to read each reference?
    - "See references/X for..." statements present?
  - **HIGH** if references exist but no navigation logic in SKILL.md

**Reference Quality Assessment:**

Detailed assessment of references/ organization and quality:

- [D-105] **Table of contents check for large references:**
  - For each file in references/, count lines
  - Files >100 lines should have table of contents at top
  - Check for TOC presence:
    - "## Table of Contents" or similar section
    - Links to major sections
  - **MEDIUM** severity per file >100 lines missing TOC
  - Example acceptable TOC format:
    ```markdown
    ## Table of Contents
    - [Section 1](#section-1) - Lines 10-50
    - [Section 2](#section-2) - Lines 51-100
    ```

- [D-106] **Nesting depth check:**
  - Map directory structure under references/
  - Count nesting levels:
    - Level 0: references/file.md ✓ (acceptable)
    - Level 1: references/category/file.md ⚠️ (flag for review)
    - Level 2+: references/cat/subcat/file.md ❌ (too deep)
  - **MEDIUM** severity if any files nested >1 level deep
  - Rationale: Each level adds cognitive overhead
  - Recommendation: Flatten structure, use descriptive filenames instead

- [D-107] **Resources in wrong category:**
  - **Check scripts/:**
    - Simple one-liners that could be inline? → **LOW** (over-engineering)
    - Scripts never referenced in SKILL.md? → **MEDIUM** (dead code)
    - Scripts that are actually documentation? → **HIGH** (should be references/)

  - **Check references/:**
    - Template files (.pptx, .docx, .html boilerplate)? → **HIGH** (should be assets/)
    - Binary files (images, fonts)? → **MEDIUM** (should be assets/)
    - Executable scripts (.py, .sh)? → **MEDIUM** (should be scripts/)

  - **Check assets/:**
    - Documentation files (.md)? → **HIGH** (should be references/)
    - Executable scripts? → **HIGH** (should be scripts/)
    - Files that Claude should read for context? → **MEDIUM** (should be references/)

  - **Detection method:**
    - List all files in each directory with extensions
    - Apply decision tree from § Resource Type Selection
    - Flag mismatches

---

**10. DEAD CODE IDENTIFICATION**

- [D-97] Instructions that can never be triggered
- [D-98] Conditional branches with impossible conditions
- [D-99] References to files or functions that don't exist
- [D-100] Parameters or options never used

---

**11. WRITING VOICE**

Check consistency of voice and style (polish issue, generally LOW severity):

- [D-108] **Section headings voice check:**
  - Extract all section headings (##, ###, etc.) from SKILL.md
  - Check form:
    - ✓ Imperative: "Extract Text", "Rotate Pages", "Fill Forms"
    - ❌ Gerund: "Extracting Text", "Rotating Pages", "Filling Forms"
    - ❌ Noun: "Text Extraction", "Page Rotation", "Form Filling"
  - Count violations (non-imperative headings)
  - **LOW** severity if 3+ violations (consistency issue)
  - Recommendation: Convert to imperative form
  - See § Writing Voice Guidelines for rationale

- [D-109] **Instruction steps voice check:**
  - Extract numbered step sequences from SKILL.md
  - Check form for each step:
    - ✓ Imperative: "1. Open the PDF", "2. Extract tables", "3. Export to CSV"
    - ❌ Gerund: "1. Opening the PDF", "2. Extracting tables", "3. Exporting to CSV"
  - Count violations
  - **LOW** severity if 5+ violations
  - Note: This is polish, not functional, but improves clarity and scannability

---

**12. CONTEXT EFFICIENCY**

Assess whether skill uses context efficiently (examples vs prose):

- [D-110] **Verbose explanations vs examples:**
  - Scan SKILL.md for explanation blocks >50 words
  - For each block, ask: "Could this be replaced with a code example?"
  - Look for patterns:
    - Multi-paragraph explanation of how to use a library
    - Detailed prose describing a function call
    - Long explanation where 5-line example would suffice
  - Count instances where example would be more efficient
  - **MEDIUM** severity if 5+ instances found
  - **LOW** severity if 2-4 instances
  - Apply principle from § Context Window Philosophy: "Prefer concise examples over verbose explanations"

- [D-111] **Prose paragraphs vs bulleted lists:**
  - Scan for prose paragraphs that list multiple items/steps
  - Example anti-pattern:
    ```
    The skill supports multiple operations. You can extract text from PDFs,
    rotate pages, merge multiple documents, split documents into parts, and
    fill PDF forms. Each operation has specific requirements.
    ```
  - Should be:
    ```
    The skill supports:
    - Extract text from PDFs
    - Rotate pages
    - Merge multiple documents
    - Split documents
    - Fill PDF forms
    ```
  - Count instances where bullets would improve scannability
  - **LOW** severity if 3+ instances (scannability improvement)
  - Note: Bulleted lists are more scannable and context-efficient
