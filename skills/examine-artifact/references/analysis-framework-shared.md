# Shared Analysis Framework

Universal checks applicable to both skills and prompts.

## Table of Contents

1. [Directive Extraction](#1-directive-extraction) - Lines 20-45
2. [Contradiction Detection](#2-contradiction-detection) - Lines 47-70
3. [Temporal Analysis](#3-temporal-analysis) - Lines 72-95
4. [Flow Mapping](#4-flow-mapping) - Lines 97-120
5. [Freedom Calibration](#5-freedom-calibration) - Lines 122-155
6. [Dead Code Identification](#6-dead-code-identification) - Lines 157-180
7. [Writing Voice](#7-writing-voice) - Lines 182-225
8. [Context Efficiency](#8-context-efficiency) - Lines 227-280

---

## 1. DIRECTIVE EXTRACTION

Create numbered list of EVERY instruction/directive in the artifact:

- [D-68] **Extract each directive:**
  - Find all "do this" or "don't do this" statements
  - Include explicit commands, constraints, and requirements
  - Note conditional directives ("if X, then Y")

- [D-69] **Document location for each:**
  - Record file name
  - Record line number (use `nl -ba {file}` for stable numbering)
  - Quote the exact text

- [D-70] **Flag ambiguous directives:**
  - Identify vague language ("should", "might", "consider")
  - Note directives lacking specific criteria
  - Suggest concrete clarification for each ambiguous directive

**Output format:**
```
| ID | Directive | File | Line | Ambiguous? | Notes |
|----|-----------|------|------|------------|-------|
| 1  | "Always validate input" | SKILL.md | 45 | No | Clear |
| 2  | "Consider using caching" | SKILL.md | 78 | Yes | What triggers caching? |
```

---

## 2. CONTRADICTION DETECTION

Compare all extracted directives for conflicts:

- [D-71] **Identify conflicting pairs:**
  - Direct contradictions ("do X" vs "don't do X")
  - Implicit conflicts (mutually exclusive conditions)
  - Scope conflicts (global rule vs local exception)

- [D-72] **Check topic consistency:**
  - Same topic addressed differently in multiple places?
  - Same concept with different terminology?
  - Conflicting default values or thresholds?

- [D-73] **Check cross-file consistency (skills only):**
  - Reference files contradict main file?
  - Different reference files contradict each other?

- [D-74] **Document each contradiction:**
  - Quote both conflicting directives
  - Include file and line for each
  - Propose resolution (which should take precedence and why)

**Output format:**
```
| Contradiction | Directive A | Location A | Directive B | Location B | Resolution |
|---------------|-------------|------------|-------------|------------|------------|
| Scope conflict | "Always use UTC" | L:45 | "Use local time for display" | L:120 | Clarify: UTC for storage, local for display |
```

---

## 3. TEMPORAL ANALYSIS (Outdated Content)

Identify content that may be outdated or time-sensitive:

- [D-79] **Explicit temporal references:**
  - Specific dates ("as of January 2024")
  - Version numbers ("v2.3.1", "Python 3.9")
  - "New" or "recent" features
  - "Deprecated" or "legacy" mentions

- [D-80] **Implicit temporal language:**
  - "Recently", "now", "the latest"
  - "Currently", "at this time"
  - "Modern" or "up-to-date"

- [D-81] **Technology references:**
  - APIs or libraries that may have changed
  - Framework versions
  - Tool references that may be outdated

- [D-82] **Behavioral assumptions:**
  - Instructions referencing features that may have changed
  - Assumptions about tool behavior
  - References to external services

**Severity guidance:**
- **HIGH**: Instructions depend on specific version that may be outdated
- **MEDIUM**: General temporal language that may age poorly
- **LOW**: Minor date references that don't affect functionality

---

## 4. FLOW MAPPING

For each major user scenario, trace the execution path:

- [D-83] **Map complete decision paths:**
  - Start from entry point
  - Follow all branches and conditions
  - Note where Claude should read additional files (skills)
  - Track state changes through the flow

- [D-84] **Identify dead ends:**
  - Branches that don't lead anywhere
  - Missing instructions for edge cases
  - Incomplete conditional handling

- [D-85] **Find circular references:**
  - Loops without exit conditions
  - Mutual dependencies that could cause infinite loops
  - Recursive references without base case

- [D-86] **Map confusion points:**
  - Where user/Claude might get confused
  - Ambiguous branch conditions
  - Missing context for decisions

**Output format:**
```
Flow: [User scenario]
1. Entry → [section/line]
2. Decision: [condition] → Branch A / Branch B
3. Branch A → [action] → Exit
4. Branch B → [action] → ??? (dead end)
```

---

## 5. FREEDOM CALIBRATION

For each instruction, assess if the specificity level is appropriate:

- [D-87] **HIGH freedom (text guidance):**
  - Appropriate when: Multiple valid approaches exist
  - Example: "Use appropriate error handling"
  - Check: Is this intentionally flexible, or just vague?

- [D-88] **MEDIUM freedom (pseudocode/parameters):**
  - Appropriate when: Preferred pattern exists but variations acceptable
  - Example: "Parse JSON using standard library, handle errors gracefully"
  - Check: Are the key constraints specified?

- [D-89] **LOW freedom (specific scripts/commands):**
  - Appropriate when: Operations are fragile, exact steps required
  - Example: "Run `npm install --save-exact package@1.2.3`"
  - Check: Is this precision necessary?

**Flag mismatches:**

- [D-90] **Overly rigid instructions:**
  - Specific commands where flexibility would be better
  - Hard-coded values that should be parameters
  - Exact steps for operations that have valid alternatives
  - **MEDIUM** severity if limits valid approaches unnecessarily

- [D-91] **Overly loose instructions:**
  - Vague guidance for fragile operations
  - Missing specifics for error-prone steps
  - No constraints on operations that need them
  - **HIGH** severity if could cause errors

---

## 6. DEAD CODE IDENTIFICATION

Find instructions or content that will never be executed/used:

- [D-97] **Unreachable instructions:**
  - Instructions behind impossible conditions
  - Steps that can never be triggered
  - Branches that are always skipped

- [D-98] **Impossible conditions:**
  - Conditional branches with contradictory requirements
  - Mutually exclusive conditions combined with AND
  - Checks for states that can't occur

- [D-99] **Dead references:**
  - References to files that don't exist
  - Links to sections that were removed
  - Calls to functions/scripts that don't exist
  - **CRITICAL** severity for missing referenced files

- [D-100] **Unused parameters/options:**
  - Documented options that are never referenced
  - Parameters defined but never used
  - Configuration that has no effect

---

## 7. WRITING VOICE

Check consistency of voice and style (generally LOW severity, polish issue):

- [D-108] **Section headings voice check:**
  - Extract all section headings (##, ###, etc.)
  - Check form:
    - ✓ Imperative: "Extract Text", "Rotate Pages", "Fill Forms"
    - ❌ Gerund: "Extracting Text", "Rotating Pages", "Filling Forms"
    - ❌ Noun: "Text Extraction", "Page Rotation", "Form Filling"
  - Count violations (non-imperative headings)
  - **LOW** severity if 3+ violations (consistency issue)
  - Recommendation: Convert to imperative form

- [D-109] **Instruction steps voice check:**
  - Extract numbered step sequences
  - Check form for each step:
    - ✓ Imperative: "1. Open the PDF", "2. Extract tables", "3. Export to CSV"
    - ❌ Gerund: "1. Opening the PDF", "2. Extracting tables", "3. Exporting to CSV"
  - Count violations
  - **LOW** severity if 5+ violations
  - Note: This is polish, not functional, but improves clarity and scannability

**Why imperative form matters:**
- More direct and actionable
- Easier to scan
- Standard for instructions/commands
- Consistent with CLI conventions

---

## 8. CONTEXT EFFICIENCY

Assess whether the artifact uses context efficiently:

- [D-110] **Verbose explanations vs examples:**
  - Scan for explanation blocks >50 words
  - For each block, ask: "Could this be replaced with a code example?"
  - Look for patterns:
    - Multi-paragraph explanation of how to use a library
    - Detailed prose describing a function call
    - Long explanation where 5-line example would suffice
  - Count instances where example would be more efficient
  - **MEDIUM** severity if 5+ instances found
  - **LOW** severity if 2-4 instances
  - Apply principle: "Prefer concise examples over verbose explanations"

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

**The Challenge Test:**
For every piece of content, ask:
1. "Does Claude really need this explanation?"
2. "Does this paragraph justify its token cost?"
3. "Is this information Claude can already infer from context?"

If #1 or #2 is "no," or #3 is "yes" → flag for removal/condensing.
