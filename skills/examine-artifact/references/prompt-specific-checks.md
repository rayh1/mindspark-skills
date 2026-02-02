# Prompt-Specific Checks

Checks that apply only to standalone prompt files (no YAML frontmatter with name + description).

## Table of Contents

1. [Structure Analysis (P-01 to P-05)](#1-structure-analysis) - Lines 20-100
2. [Heading Quality (P-06 to P-10)](#2-heading-quality) - Lines 102-170
3. [Content Efficiency (P-11 to P-15)](#3-content-efficiency) - Lines 172-250
4. [Progressive Complexity (P-16 to P-20)](#4-progressive-complexity) - Lines 252-330

---

## 1. STRUCTURE ANALYSIS

Basic structural checks for prompt files:

- [P-01] **Heading structure consistency:**
  - Extract all headings (# through ####)
  - Check hierarchy:
    - H1 (#) used for title only, appears once at top
    - H2 (##) used for major sections
    - H3 (###) used for subsections
    - H4+ rarely needed (flag if overused)
  - **HIGH** if no headings at all (unstructured)
  - **MEDIUM** if heading levels are inconsistent (e.g., H1 → H3, skipping H2)
  - **LOW** if H4+ used more than 3 times (over-nested)

- [P-02] **Section ordering:**
  - Check for logical flow:
    - Context/background comes early
    - Inputs/requirements defined before steps
    - Steps in executable order
    - Output specification near the end
  - Recommended order:
    1. Purpose/Goal
    2. Inputs/Context
    3. Steps/Instructions
    4. Output format
    5. Examples (optional)
    6. Constraints/Edge cases (optional)
  - **MEDIUM** if outputs specified before inputs
  - **MEDIUM** if steps are not in logical sequence
  - **LOW** if section order could be improved for clarity

- [P-03] **Length check:**
  - Count total lines in prompt
  - **Severity based on length:**
    - <100 lines → ✓ Concise (check it's not too sparse)
    - 100-200 lines → ✓ Ideal range
    - 200-300 lines → **LOW** (consider if all content necessary)
    - 300-500 lines → **MEDIUM** (should review for condensing)
    - 500+ lines → **HIGH** (likely needs significant restructuring)
  - For prompts >300 lines:
    - Identify content that could be removed
    - Consider if examples are excessive
    - Check for unnecessary explanations

- [P-04] **Format consistency:**
  - Check list formatting:
    - Bulleted lists use consistent markers (-, *, or numbers)
    - Numbered lists for sequential steps
    - Bullets for non-sequential items
  - **LOW** if mixed list styles without reason
  - **MEDIUM** if numbered lists used for non-sequential content (confusing)
  - Check code block consistency:
    - Language specified for code blocks
    - Consistent use of inline code vs code blocks

- [P-05] **Self-containment check:**
  - Does prompt work standalone?
  - Check for:
    - Undefined terms or acronyms
    - References to external context not provided
    - Assumptions about reader knowledge that aren't stated
  - **MEDIUM** if prompt relies on undefined external context
  - **HIGH** if prompt references files/resources that don't exist

---

## 2. HEADING QUALITY

Assess the quality and usefulness of headings:

- [P-06] **Descriptive headings:**
  - Headings should describe content, not just label it
  - Check for vague headings:
    - ❌ "Details", "More Info", "Notes", "Misc"
    - ❌ "Step 1", "Step 2" (without description)
    - ✓ "Extract User Input", "Validate Response Format"
    - ✓ "Step 1: Parse Input", "Step 2: Generate Response"
  - **LOW** if 2+ vague headings
  - **MEDIUM** if 5+ vague headings

- [P-07] **Heading duplication:**
  - Check for duplicate or near-duplicate headings
  - Same heading appearing multiple times → confusing
  - **MEDIUM** if duplicate headings exist

- [P-08] **Heading granularity:**
  - Check section sizes:
    - Sections <5 lines may be too granular
    - Sections >100 lines may need subdivision
  - **LOW** if many tiny sections (over-fragmented)
  - **MEDIUM** if sections >100 lines without subheadings

- [P-09] **Heading hierarchy depth:**
  - Count deepest heading level used
  - Prompts rarely need H4 (####) or deeper
  - **LOW** if H4+ used extensively (>5 times)
  - Consider flattening structure

- [P-10] **Table of contents (for long prompts):**
  - For prompts >200 lines, check for TOC
  - **LOW** if >200 lines without TOC
  - **MEDIUM** if >400 lines without TOC

---

## 3. CONTENT EFFICIENCY

Assess whether content is efficiently expressed:

- [P-11] **Internal redundancy:**
  - Same instruction repeated in multiple places
  - Same concept explained multiple ways
  - **HIGH** if same instruction appears 3+ times
  - **MEDIUM** if same instruction appears twice
  - Recommendation: State once, reference if needed

- [P-12] **Verbose instructions:**
  - Look for instructions that could be shorter
  - Example anti-pattern:
    ```
    When you receive the user's input, you should first carefully read through
    all of the content they have provided. After reading, take a moment to
    consider what they are asking for. Then, formulate your response...
    ```
  - Could be:
    ```
    1. Read user input
    2. Identify the request
    3. Formulate response
    ```
  - **MEDIUM** if 3+ verbose instruction blocks (>50 words each)
  - **LOW** if 1-2 verbose blocks

- [P-13] **Excessive examples:**
  - Count examples for each concept
  - Rule of thumb: 1-2 examples per concept usually sufficient
  - **LOW** if 4+ examples for same concept
  - **MEDIUM** if 6+ examples for same concept (likely padding)
  - Exception: Complex patterns may warrant more examples

- [P-14] **Explanations of obvious concepts:**
  - Check for explanations Claude doesn't need:
    - How to write markdown
    - What JSON is
    - Basic programming concepts
    - Common acronyms
  - **MEDIUM** if significant obvious explanations found
  - Apply test: "Would Claude already know this?"

- [P-15] **Filler content:**
  - Look for content that adds no value:
    - "Please note that..."
    - "It's important to remember..."
    - "As mentioned above..."
    - "In conclusion..."
  - **LOW** if 3+ filler phrases
  - Recommendation: Remove and let content speak for itself

---

## 4. PROGRESSIVE COMPLEXITY

Assess whether the prompt is organized for optimal comprehension:

- [P-16] **Essential information first:**
  - Is the core purpose stated early?
  - Are critical constraints front-loaded?
  - Check first 20 lines:
    - Should contain: purpose, main task
    - Should not contain: edge cases, exceptions, detailed examples
  - **MEDIUM** if essential info buried after line 50
  - **HIGH** if purpose/goal unclear in first 30 lines

- [P-17] **Exception handling placement:**
  - Edge cases and exceptions should come AFTER main flow
  - Anti-pattern: Starting with "Don't do X, Y, Z..."
  - Pattern: Main flow first, then "Edge cases: ..."
  - **MEDIUM** if exceptions precede main instructions

- [P-18] **Example placement:**
  - Examples should follow the instructions they illustrate
  - Not: Example → then explanation
  - Yes: Instruction → supporting example
  - **LOW** if examples precede their instructions

- [P-19] **Complexity gradient:**
  - Simple concepts should come before complex ones
  - Basic cases before edge cases
  - Required elements before optional enhancements
  - **MEDIUM** if complex content precedes foundational content

- [P-20] **Scannability check:**
  - Can a reader quickly find what they need?
  - Check for:
    - Clear section markers
    - Consistent formatting
    - Logical grouping
    - Visual breaks between sections
  - **LOW** if prompt is a wall of text (no visual structure)
  - **MEDIUM** if key sections hard to locate

---

## Severity Summary for Prompt Checks

| Check | Critical | High | Medium | Low |
|-------|----------|------|--------|-----|
| P-01 Heading structure | - | No headings | Inconsistent levels | Over-nested |
| P-02 Section ordering | - | - | Illogical order | Could be improved |
| P-03 Length | - | >500 lines | 300-500 lines | 200-300 lines |
| P-04 Format consistency | - | - | Wrong list type | Mixed styles |
| P-05 Self-containment | - | Missing resources | Undefined context | - |
| P-06 Descriptive headings | - | - | 5+ vague | 2+ vague |
| P-07 Heading duplication | - | - | Duplicates exist | - |
| P-08 Heading granularity | - | - | >100 line sections | Over-fragmented |
| P-09 Hierarchy depth | - | - | - | Excessive H4+ |
| P-10 TOC for long prompts | - | - | >400 lines no TOC | >200 lines no TOC |
| P-11 Internal redundancy | - | 3+ repeats | 2 repeats | - |
| P-12 Verbose instructions | - | - | 3+ verbose blocks | 1-2 verbose |
| P-13 Excessive examples | - | - | 6+ examples | 4+ examples |
| P-14 Obvious explanations | - | - | Significant | - |
| P-15 Filler content | - | - | - | 3+ filler phrases |
| P-16 Essential info first | - | Purpose unclear | Essential buried | - |
| P-17 Exception placement | - | - | Exceptions first | - |
| P-18 Example placement | - | - | - | Examples before instructions |
| P-19 Complexity gradient | - | - | Complex before basic | - |
| P-20 Scannability | - | - | Key sections hard to find | Wall of text |
