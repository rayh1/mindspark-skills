# Artifact Contracts: Common Output Schemas

This reference provides proven output schemas for common skill/prompt types. Use these as templates during Pass 1 (Artifact Contract definition).

An **artifact contract** specifies exactly what files/sections get produced, their format, and validation rules.

---

## Why Artifact Contracts Matter

**Problem:** "It produces a report" is too vague.

**Solution:** Lock down the exact structure:
- Filename(s)
- Required sections
- Forbidden sections (prevents bloat)
- Format rules
- Validation criteria

Once the output format is stable, everything else becomes easier to specify.

---

## Template 1: Analysis Report

**Use when:** Analyzing code, reviewing documents, auditing systems

**Contract:**

```xml
<output_schema>
format: markdown
filename: ANALYSIS.md

required_sections:
  - Summary (2-3 sentences)
  - Findings (numbered list with severity)
  - Recommendations (actionable items)
  - Details (supporting evidence)

optional_sections:
  - Metrics (if quantitative data exists)

forbidden_sections:
  - "Future Work" (prevents scope creep)
  - "Background" (keep focused on findings)

formatting_rules:
  - Findings use [F-1], [F-2] for addressability
  - Recommendations use [R-1], [R-2]
  - Severity: HIGH / MEDIUM / LOW
  - Code blocks use proper language tags

validation:
  - All required sections present and non-empty
  - At least 1 finding
  - Each finding has severity
  - Each recommendation is actionable
</output_schema>
```

---

## Template 2: Implementation Plan

**Use when:** Planning work, designing features, proposing changes

**Contract:**

```xml
<output_schema>
format: markdown
filename: PLAN.md

required_sections:
  - Goal (1 paragraph: what and why)
  - Stop Conditions (3-7 bullets)
  - Approach (high-level strategy)
  - Steps (numbered, verifiable)
  - Risks (what could go wrong)
  - Validation (how to verify success)

optional_sections:
  - Alternatives Considered
  - Dependencies
  - Timeline (if requested)

forbidden_sections:
  - "Nice to Have" (keep scope tight)

formatting_rules:
  - Steps numbered 1-N
  - Each step has clear completion criteria
  - Risks tagged as HIGH / MEDIUM / LOW
  - Stop conditions are checkboxes [ ]

validation:
  - Goal is 1 paragraph (not 3 pages)
  - 4-12 steps (not too vague, not too granular)
  - Each step is verifiable
  - At least 1 risk identified
</output_schema>
```

---

## Template 3: Configuration/Setup Output

**Use when:** Generating configs, setup scripts, boilerplate

**Contract:**

```xml
<output_schema>
format: depends on type (JSON, YAML, code, etc.)
filename: [varies by config type]

required_sections:
  - Header comment (what this is, how to use)
  - All required fields populated
  - Validation rules documented

optional_sections:
  - Examples of common customizations
  - Links to documentation

forbidden_sections:
  - Commented-out experimental options

formatting_rules:
  - Valid syntax for target format
  - Comments explain non-obvious choices
  - Defaults clearly marked
  - Secrets use placeholder values (never real keys)

validation:
  - File parses without errors
  - All required fields present
  - No actual secrets embedded
  - Can be used as-is or with minimal edits
</output_schema>
```

---

## Template 4: Code Generation

**Use when:** Generating functions, classes, boilerplate code

**Contract:**

```xml
<output_schema>
format: code (language-specific)
filename: [descriptive name with proper extension]

required_sections:
  - Docstring/comment header
  - Input validation (if applicable)
  - Main logic
  - Return value
  - Basic error handling

optional_sections:
  - Type hints/annotations
  - Inline comments for complex logic
  - Usage examples in docstring

forbidden_sections:
  - Dead code
  - Commented-out alternatives
  - TODO comments (finish it or don't add it)

formatting_rules:
  - Follows language conventions (PEP 8 for Python, etc.)
  - Consistent indentation
  - Descriptive variable names
  - No magic numbers

validation:
  - Code is syntactically valid
  - No unused imports
  - No obvious security issues (SQL injection, XSS, etc.)
  - Handles basic error cases
</output_schema>
```

---

## Template 5: Multi-File Output

**Use when:** Creating project structures, multiple related files

**Contract:**

```xml
<output_schema>
format: multiple files
structure:
  project-root/
    README.md (required)
    config.yaml (required)
    src/ (required)
      main.py (required)
      utils.py (optional)
    tests/ (optional)
      test_main.py (optional)

required_files:
  - README.md: purpose, usage, setup
  - config.yaml: configuration settings
  - src/main.py: main entry point

optional_files:
  - src/utils.py: helper functions
  - tests/*.py: test files

forbidden_files:
  - .env with real secrets
  - node_modules/ or __pycache__/
  - IDE-specific configs (.vscode, .idea)

formatting_rules:
  - Each file has header comment
  - Cross-references use relative paths
  - No absolute file paths
  - All imports are relative or from stdlib

validation:
  - Directory structure matches spec
  - All required files present
  - No files outside allowed structure
  - Files reference each other correctly
</output_schema>
```

---

## Template 6: Structured Data Output

**Use when:** Producing JSON, YAML, CSV for downstream tools

**Contract:**

```xml
<output_schema>
format: JSON
filename: output.json

schema:
{
  "run_id": "string (required)",
  "timestamp": "ISO 8601 datetime (required)",
  "results": [
    {
      "id": "string (required)",
      "status": "enum: pass|fail|skip (required)",
      "details": "string (optional)"
    }
  ],
  "summary": {
    "total": "integer (required)",
    "passed": "integer (required)",
    "failed": "integer (required)"
  }
}

required_fields: [all fields marked required above]

validation:
  - Valid JSON (parseable)
  - All required fields present
  - Enums match allowed values
  - run_id is unique
  - summary.total = passed + failed + skipped
  - No sensitive data (passwords, keys, PII)

constraints:
  - results array has at least 1 item
  - status values are lowercase
  - timestamp uses UTC
</output_schema>
```

---

## Template 7: Documentation Output

**Use when:** Generating docs, READMEs, guides

**Contract:**

```xml
<output_schema>
format: markdown
filename: [descriptive].md

required_sections:
  - Title (H1)
  - Purpose (what and why)
  - Quick Start (minimal working example)
  - Usage (common operations)
  - Examples (at least 1)

optional_sections:
  - Advanced Usage
  - Troubleshooting
  - API Reference
  - Contributing

forbidden_sections:
  - "Coming Soon" (finish it or omit it)
  - Empty placeholders

formatting_rules:
  - One H1 (title)
  - Consistent heading levels (no H4 under H2)
  - Code blocks have language tags
  - Links are valid (no broken references)
  - Examples are complete and runnable

validation:
  - Quick Start example is genuinely quick (<10 lines)
  - All code examples are syntactically valid
  - No "TODO" or "FIXME" comments
  - At least 1 complete example
</output_schema>
```

---

## How to Use These Templates

### During Pass 1 (Artifact Contract):

1. **Pick the closest template** for your use case
2. **Customize** required/optional/forbidden sections
3. **Add validation rules** specific to your domain
4. **Show it to the user** for confirmation

### During Pass 7 (Validation):

1. **Check output against contract**
2. **Verify all required sections present**
3. **Ensure no forbidden sections added**
4. **Run validation rules**

---

## Creating Custom Contracts

If none of the templates fit, build your own:

**Ask these questions:**

1. **Format:** What file type(s)? (markdown, JSON, code, etc.)
2. **Structure:** What sections/fields are required?
3. **Constraints:** What's forbidden? (prevents bloat)
4. **Validation:** How do you know output is valid?
5. **Defaults:** What happens if optional sections are omitted?

**Keep it minimal:**
- 3-7 required sections (not 20)
- Each section has clear purpose
- Validation is mechanical (checkable)
- Format rules are specific

---

## Anti-Patterns

**Too vague:**
```xml
<output_schema>
format: text
sections: whatever makes sense
</output_schema>
```
→ No way to verify this.

**Too rigid:**
```xml
<output_schema>
required_sections:
  - Executive Summary (exactly 150 words)
  - Background (3 paragraphs, each 4-5 sentences)
  - Analysis (organized by: alphabetical subsections)
  ...
</output_schema>
```
→ Overspecified, brittle, unreadable.

**Too loose:**
```xml
<output_schema>
format: markdown
sections: up to the model
</output_schema>
```
→ Output will vary wildly between runs.

---

## Summary

A good artifact contract:
- **Specific** enough to validate
- **Minimal** enough to understand
- **Flexible** enough to adapt
- **Enforceable** with simple rules

Use the templates as starting points, customize for your use case, and lock the contract in Pass 1 before adding other structure.
