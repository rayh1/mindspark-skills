# Shared Analysis Framework

Universal checks applicable to both skills and prompts.

## Table of Contents

1. [Directive Extraction](#1-directive-extraction)
2. [Contradiction Detection](#2-contradiction-detection)
3. [Temporal Analysis](#3-temporal-analysis)
4. [Flow Mapping](#4-flow-mapping)
5. [Freedom Calibration](#5-freedom-calibration)
6. [Dead Code Identification](#6-dead-code-identification)
7. [Writing Voice](#7-writing-voice)
8. [Context Efficiency](#8-context-efficiency)
9. [Domain Correctness](#9-domain-correctness)

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

- [D-73b] **Check intra-file contradictions:**
  - Does any file contradict its own content?
  - Section ordering in the file vs ordering described/specified within that file
  - TOC line numbers match actual heading locations
  - Rule A within a file conflicting with Rule B in the same file
  - Examples or templates contradicting the prose that describes them

- [D-73b-ii] **Template value completeness:**
  - When a template or example shows enumerated options (e.g., severity levels, status values, field choices), cross-check against all documented valid values elsewhere in the artifact
  - Flag: a template listing "A / B / C" when the prose also documents "D" as a valid value (omission from template)
  - Example: severity template showing "Critical / Major / Minor / Info" but omitting "—" (dash for no concerns), which is a documented valid value
  - **MEDIUM** severity if omitted value is commonly used; **LOW** if rare edge case

- [D-73c] **Check cross-rule strictness consistency:**
  - When two directives address the same behavior, do they agree on strictness?
  - One rule requiring X (e.g., "section must be present") while a related rule allows omitting X (e.g., "omit section if empty")
  - Quality gates expecting content that other directives say to skip
  - Catch pattern: extract each "must/required/always" directive and each "omit/skip/optional" directive, cross-reference for same-topic conflicts

- [D-73d] **Check vocabulary/terminology consistency:**
  - When the same concept (enumerated reasons, status values, section names) appears in multiple locations, is the full vocabulary consistent?
  - Example: if skip reasons are enumerated in file A, do all skip reasons used in files B and C also appear in A's list?
  - Example: if section ordering is stated in one place, do all other mentions of section order match?
  - Flag: a value introduced in one file (e.g., a new skip reason in a fallback chain) but absent from the canonical list or example table elsewhere

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

- [D-99b] **Section-level cross-references:**
  - When an artifact uses `§ Section Name`, `see file.md § Heading`, or similar notation referencing a specific section within a file, verify the target heading exists as an actual `##`/`###` heading in the referenced file
  - Also check: inline references like "see document-format.md § When no concerns" — does that exact heading exist?
  - **MEDIUM** severity for section references that don't resolve to actual headings (misleading navigation)

- [D-100] **Unused parameters/options:**
  - Documented options that are never referenced
  - Parameters defined but never used
  - Configuration that has no effect

---

## 7. WRITING VOICE

Check consistency of voice and style (generally LOW severity, polish issue):

- [D-108] **Section headings voice check:**
  - Extract all section headings (##, ###, etc.)
  - **Exclude pattern section labels** (these are intentionally noun phrases):
    - Load references/pattern-label-registry.md for canonical pattern names
    - Skip headings matching any pattern from the registry (both XML tags and markdown headings)
    - Skip headings ending with `:` that match pattern names (prompt format)
  - Check form for remaining headings:
    - ✓ Imperative: "Extract Text", "Rotate Pages", "Fill Forms"
    - ❌ Gerund: "Extracting Text", "Rotating Pages", "Filling Forms"
    - ❌ Noun: "Text Extraction", "Page Rotation", "Form Filling"
  - Count violations (non-imperative headings, excluding pattern labels)
  - **LOW** severity if 3+ violations (consistency issue)
  - Recommendation: Convert to imperative form (but preserve canonical pattern labels per registry)

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

**The Challenge Test:** (see quality-standards-shared.md § The Challenge Test for full definition)
Apply the three questions (need? justify? inferable?). If content fails → flag for removal/condensing.

---

## 9. DOMAIN CORRECTNESS

Verify the artifact's instructions are semantically correct for its stated purpose:

- [D-112] **Semantic correctness of directives:**
  - For each major workflow step, ask: "Is this instruction logically correct for what the artifact claims to do?"
  - Check for instructions that contradict the artifact's stated scope or purpose
  - Example: a "staged changes" scope that includes untracked files (untracked = not staged)
  - Example: a "skip all markdown" rule that makes a later classification layer for markdown vestigial
  - **HIGH** severity if an instruction is semantically wrong for its stated purpose
  - **MEDIUM** severity if an instruction is misleading or could cause confusion

- [D-113] **Unspecified edge cases:**
  - For each branching point or enumerated category, ask: "What happens in the case not listed?"
  - Look for:
    - Display/output formats for error states (e.g., what text goes in a table cell when a value is empty?)
    - Boundary conditions not addressed (e.g., what if a directory contains files that don't match expected patterns?)
    - Ambiguous specifications (e.g., "line range" without specifying old file vs new file vs diff output)
  - **Systematic enumeration method** (apply all four):
    1. **Output fields/columns:** List every output field, table column, and template placeholder in the artifact. For each, verify the spec covers: what value when empty, what value on error, what value at boundary conditions.
    2. **User-facing text:** List every user-facing text string or message. Verify each is fully specified (no vague words like "stats", "info", "details" without definition).
    3. **Parse/find operations:** List every operation that reads or searches input (file finding, pattern matching, parsing). Verify behavior is specified for: malformed input, unexpected format, no matches, partial matches.
    4. **Enumerated states/values:** For each set of enumerated values (statuses, severity levels, skip reasons), verify all valid values are documented and no implicit "other" case is unhandled.
  - **MEDIUM** severity per unspecified edge case that a reasonable implementer would need to resolve
  - **LOW** severity for minor ambiguities

- [D-114] **Implicit dependencies:**
  - Check if instructions rely on external behavior not documented in the artifact
  - Examples:
    - Git default flags (e.g., `-M` for rename detection) relied upon without mention
    - Tool version behavior assumed without stating version requirements
    - Example lists presented ambiguously — could be read as exhaustive when they're illustrative, or vice versa
  - **MEDIUM** severity if implicit dependency could cause silent failure
  - **LOW** severity if implicit dependency is a reasonable default

- [D-115] **Logical preemption (dead content via other rules):**
  - Check if any rule/pattern renders another rule/pattern logically dead
  - Different from D-97 (unreachable instructions): logical preemption involves two valid rules where one makes the other vestigial in practice
  - Example: a pre-classification skip rule catching all files that a later classification category would match
  - **LOW** severity (content waste, not functional failure)
  - Recommendation: note the preemption, suggest consolidation or removal of vestigial content

- [D-112b] **Scope claim verification:**
  - For each assertion in scope_fence, description, or purpose statement, verify the claim against actual implementation
  - Check patterns:
    - "Nothing is X" → confirm nothing is X (e.g., "nothing is skipped" but skill has skip rules → contradiction)
    - "Includes Y fields" → confirm those fields exist in the actual specification (e.g., scope says "failure modes" but no such annotation field exists)
    - "All Z are reviewed" → confirm no Z is silently excluded
  - **MEDIUM** severity if scope claim contradicts implementation (misleading to both users and the executing LLM)
  - **LOW** severity if scope claim is imprecise but not outright wrong
