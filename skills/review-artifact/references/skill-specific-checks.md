# Skill-Specific Checks

Checks that apply only to SKILL.md files (artifacts with YAML frontmatter containing name + description).

## Table of Contents

1. [Structure Analysis](#1-structure-analysis) - Lines 20-90
2. [Trigger & Description Review](#2-trigger--description-review) - Lines 92-180
3. [Cross-File Redundancy](#3-cross-file-redundancy) - Lines 182-260
4. [Progressive Disclosure Check](#4-progressive-disclosure-check) - Lines 262-400
5. [Skill Category & MCP-Specific Checks](#5-skill-category--mcp-specific-checks)

---

## 1. STRUCTURE ANALYSIS

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
    - Reference documentation → references/
  - **Note:** Cognitive anchors should stay inline (see § Exception: Cognitive Anchors in quality-standards-shared.md)
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

- [D-108] **Frontmatter security restrictions:**
  - Check `name` field: must NOT begin with "claude" or "anthropic" (reserved prefixes)
  - Check entire frontmatter block for XML angle brackets (`<` or `>`)
  - **CRITICAL** if name starts with "claude" or "anthropic" (reserved; violates Anthropic naming policy)
  - **CRITICAL** if frontmatter contains XML angle brackets (prompt injection risk: frontmatter is always loaded into Claude's system prompt — malicious or accidentally injected content can interfere with Claude's behavior in every conversation where the skill is active)
  - Security note: Unlike the SKILL.md body (loaded on demand), frontmatter is always in context. These are security restrictions, not style guidelines.

- [D-109] **Skill name-to-folder consistency:**
  - Read `name:` value from frontmatter
  - Compare against the actual folder/directory name containing SKILL.md
  - **HIGH** if they do not match (breaks install conventions and discoverability; skill may not install correctly)
  - Note: If reviewing a standalone SKILL.md without folder context available, skip and note as unchecked

---

## 2. TRIGGER & DESCRIPTION REVIEW

(See references/skill-quality-standards.md § Description Field Excellence for detailed criteria)

**Critical checks with severity guidance:**

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

- [D-68] **Negative trigger presence** (overtriggering guard):
  - When a skill is domain-scoped or has a sibling skill covering overlapping territory, check if the description contains explicit exclusions
  - Canonical pattern: `"... Do NOT use for [X] (use [other-skill] instead)."`
  - Example: `"Advanced statistical analysis for CSV. Do NOT use for simple data exploration (use data-viz skill instead)."`
  - **MEDIUM** if skill operates in a domain shared by another skill, but no negative trigger is present to prevent overlap
  - Note: Negative triggers are the primary prescribed fix for overtriggering. Absence when there is a clear sibling skill is a reliability risk.

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

## 3. CROSS-FILE REDUNDANCY

(See references/quality-standards-shared.md § Duplication Rule)

Apply the Challenge Test to all content:

- [D-75] **Cross-file duplication** (SKILL.md ↔ references/):
  - Extract key concepts from SKILL.md
  - Search references/ for same concepts
  - Compare content:
    - Identical content with no cognitive function → **HIGH** severity (redundant duplication)
    - Cognitive anchors (see § Exception: Cognitive Anchors in quality-standards-shared.md) → Acceptable even if identical
    - Summary in SKILL.md + details in references/ → Acceptable (proper pattern)
    - Different aspects of same topic → Acceptable
  - Before flagging identical content, check if it's a cognitive anchor:
    - [ ] Would removing it force reference lookup during execution?
    - [ ] Does it show non-obvious domain behavior?
    - [ ] Does it anchor pattern recognition for data structures?
    - [ ] Is it a high-frequency operation?
    - If YES to any → acceptable functional duplication
    - If NO to all → flag as redundant duplication
  - Check specifically for redundant duplication (not cognitive anchors):
    - Same examples in both places (no cognitive function) → **HIGH**
    - Same code snippets duplicated (no cognitive function) → **HIGH**
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

- [D-104] **Systematic cross-file duplication check:**
  - **Step 1:** Extract distinct examples from SKILL.md (code snippets, workflows, patterns)
  - **Step 2:** For each example, search all references/ files for same/similar content
  - **Step 3:** Classify duplication:
    - **Identical - Cognitive Anchor:** Same example in both, but serves cognitive function
      - Apply cognitive anchor test (see § Exception: Cognitive Anchors):
        - Shows non-obvious domain behavior?
        - Anchors pattern recognition?
        - High-frequency operation?
        - Guides decision-making?
      - If YES to any → Acceptable (functional duplication)
      - If NO to all → proceed to classify as redundant
    - **Identical - Redundant:** Same example, code, or explanation with no cognitive function
      - **HIGH** severity (violates duplication rule)
      - Recommendation: Keep in ONE location only
    - **Summary + Detail:** Brief mention in SKILL.md, full detail in references/
      - Acceptable pattern (verify summary is truly brief)
      - **MEDIUM** if "summary" is actually detailed (should remove from SKILL.md)
    - **Different aspects:** Different information about same topic
      - Acceptable (not duplication)
  - **Step 4:** Count violations (redundant duplications only, exclude cognitive anchors)
    - 1-2 redundant duplications → **HIGH**
    - 3+ redundant duplications → **CRITICAL** (systemic issue)
  - **Step 5:** Recommend consolidation strategy
    - If cognitive anchor → keep in both locations (functional duplication)
    - If core workflow → keep in SKILL.md, remove from references/
    - If detailed/optional → keep in references/, remove from SKILL.md

---

## 4. PROGRESSIVE DISCLOSURE CHECK

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
    - Complete reference documentation → references/
    - Framework-specific patterns (not high-frequency) → references/{framework}.md
    - Domain-specific knowledge (not cognitive anchors) → references/{domain}.md
  - **Before flagging:** Check if content is cognitive anchor (see § Exception: Cognitive Anchors in quality-standards-shared.md)
    - If YES → acceptable inline (functional placement)
    - If NO → should move to references/
  - **MEDIUM** if significant non-anchor details in SKILL.md

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
    - Complete reference documentation inline (excluding cognitive anchors)
    - Exhaustive examples (>5 examples for same pattern, excluding cognitive anchor variations)
  - **Before flagging overload:** Distinguish cognitive anchors from bloat (see § Exception: Cognitive Anchors in quality-standards-shared.md)
  - **MEDIUM** severity if SKILL.md is overloaded with non-anchor content
  - Are reference files clearly signposted?
    - Does SKILL.md explain when to read each reference?
    - "See references/X for..." statements present?
  - **HIGH** if references exist but no navigation logic in SKILL.md

**Reference Quality Assessment:**

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

## 5. SKILL CATEGORY & MCP-SPECIFIC CHECKS

Classify the skill type first, then apply category-appropriate checks.

### Category Classification

- [D-111] **Classify skill category:**
  - Read SKILL.md and identify which category best fits:
    - **Category 1 (Document & Asset Creation)**: Creates documents, designs, code artifacts; no external tools required beyond Claude's built-in capabilities
    - **Category 2 (Workflow Automation)**: Multi-step processes with validation gates; may use MCP but focus is on repeatable methodology
    - **Category 3 (MCP Enhancement)**: Explicitly orchestrates MCP server tools; adds workflow/knowledge layer on top of MCP access
  - Record category in review report (e.g., "Detected: Category 3 - MCP Enhancement")
  - Apply D-112/D-113 only for Category 2/3 skills

### MCP-Specific Instruction Quality (Category 2/3 Only)

- [D-112] **MCP error handling coverage:**
  - Check if skill instructions address:
    - [ ] Connection failures (e.g., "Connection refused", server not running — verify in Settings > Extensions)
    - [ ] Authentication failures (invalid/expired API keys, OAuth token refresh)
    - [ ] Tool name case-sensitivity (MCP tool names are case-sensitive; skill must reference exact names)
    - [ ] Fallback or retry guidance when MCP calls fail
  - **HIGH** if no MCP error handling present in a skill that makes MCP calls
  - **MEDIUM** if error handling present but incomplete (covers connection but not auth, or vice versa)
  - Best practice that should be present: instructions to test MCP independently ("Ask Claude to call the MCP tool directly without the skill to isolate whether failure is in the skill or the MCP connection itself")

- [D-113] **MCP tool name references:**
  - Identify all MCP tool names explicitly mentioned in SKILL.md (e.g., `create_customer`, `setup_payment_method`)
  - Flag names that appear malformed (spaces, inconsistent casing)
  - **MEDIUM** if tool names are used without noting they are case-sensitive
  - **LOW** if no tool names are mentioned but skill claims to orchestrate MCP (tool names should be explicit)
  - Note: Static review cannot verify names against a live MCP server; flag for manual verification

