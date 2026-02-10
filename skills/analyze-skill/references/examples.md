# Examples

## Table of Contents

- [Example 1: Text Description - Skip Verdict](#example-1-text-description---skip-verdict)
- [Example 2: Existing Skill - Build Verdict](#example-2-existing-skill---build-verdict)
- [Example 3: Text Description - Consider Verdict with Full Flow](#example-3-text-description---consider-verdict-with-full-flow)
- [Example 4: Existing Skill - Enhancements Only Mode](#example-4-existing-skill---enhancements-only-mode)

---

## Example 1: Text Description - Skip Verdict

**User input:** `analyze-skill "A skill that helps users organize their morning routine"`

### Step 1-2: Skill Understanding (wait for confirmation)

```
## Skill Understanding

Based on your description, I understand this proposed skill as:

**Name:** morning-routine-organizer
**Purpose:** Helps users create, structure, and optimize their morning routines
**Likely features:**
- Suggests routine activities based on user goals
- Sequences activities in optimal order
- Provides time estimates
- May include habit-building elements

**Is this understanding correct?** Please confirm or clarify.
```

**User confirms:** "Yes, that's correct"

### Steps 3-7: Evaluation Report

```
## Evaluation Report: morning-routine-organizer

### Scoring Table

| Dimension | Score | Rationale |
|-----------|-------|-----------|
| [D-1] Knowledge Gap | 0/2 | General knowledge about routines; no specialized data |
| [D-2] Structural Process | 1/2 | Provides some structure but Claude can do this casually |
| [D-3] Tool Integration | 0/2 | No external tools mentioned |
| [D-4] Consistency & Guardrails | 0/2 | No enforcement mechanisms |
| [D-5] Complexity Management | 0/2 | Simple one-shot analysis |
| [D-6] User Experience | 1/2 | Saves a few messages but minimal UX improvement |
| [D-7] Specialization Depth | 0/2 | Same depth as general Claude advice |

### Total Score: 2/14

### Verdict: Skip

**Score Range:** 0-5 points = Likely redundant
**Recommendation:** Do not build. Claude already provides morning routine advice. Users can simply ask "Help me organize my morning routine" and get similar results.

### Initial Improvements

To make this skill worth building:

[I-1] Calendar API Integration (V-2) [HIGH]
- What: Integrate with calendar APIs to auto-schedule routines
- Impact: (↑ D-3 by +2, ↑ D-6 by +1)
- Effort: High
- Rationale: Transforms advice into actionable scheduling

[I-2] Chronobiology Knowledge Base (V-5) [HIGH]
- What: Include research on peak performance times, sleep science
- Impact: (↑ D-1 by +2, ↑ D-7 by +1)
- Effort: Medium
- Rationale: Adds specialized knowledge Claude doesn't have

[I-3] Habit Tracking Integration (V-2) [MEDIUM]
- What: Connect to habit-tracking apps to monitor completion
- Impact: (↑ D-3 by +1, ↑ D-4 by +1)
- Effort: High
- Rationale: Adds accountability and follow-through

**Potential improved score:** 9/14 (Consider) if I-1 and I-2 implemented

### Strategic Considerations
- **Acid test result:** 90%+ of value available by asking Claude directly
- **Low ecosystem value:** Overlaps with general productivity advice
- **No multiplier effect:** Doesn't enable other skills
```

*Steps 8-9 skipped because verdict = Skip*

---

## Example 2: Existing Skill - Build Verdict

**User input:** `analyze-skill pdf`

### Steps 1-7: Evaluation (no confirmation needed)

```
## Evaluation Report: pdf

### Scoring Table

| Dimension | Score | Rationale |
|-----------|-------|-----------|
| [D-1] Knowledge Gap | 1/2 | Synthesizes PDF manipulation knowledge systematically |
| [D-2] Structural Process | 2/2 | Enforces systematic PDF processing workflows |
| [D-3] Tool Integration | 2/2 | Orchestrates pdfplumber, PyPDF2, and file operations |
| [D-4] Consistency & Guardrails | 2/2 | Mandatory validation of PDF operations |
| [D-5] Complexity Management | 2/2 | Manages multi-page, multi-format complexity |
| [D-6] User Experience | 2/2 | Transforms complex PDF tasks into simple invocations |
| [D-7] Specialization Depth | 2/2 | Deep PDF manipulation beyond general knowledge |

### Total Score: 13/14

### Verdict: Build

**Score Range:** 10-14 points = Significant value add
**Recommendation:** This skill provides genuine new capabilities. PDF manipulation requires specialized tools and systematic processes that Claude cannot reliably perform without a dedicated skill.

### Strategic Considerations
- **Acid test result:** <20% of value available by asking Claude directly
- **High ecosystem value:** Enables document workflows across many domains
- **Multiplier effect:** Used by other skills and workflows
```

### Step 8: Enhancement Opportunities

```
## Enhancement Opportunities (5-Vector Exploration)

### Current Capability Map
- Inputs: Single PDF files
- Outputs: Text, tables, new PDFs, merged/split documents
- Tools: pdfplumber, PyPDF2, file system
- Validation: Format checking, page validation
- UX: Single-invocation commands

### HIGH Priority

[I-1] OCR Text Extraction (V-2) [HIGH]
- What: Add optical character recognition for scanned PDFs via Tesseract MCP
- Impact: (↑ D-1 by +1) → 14/14
- Effort: Medium
- Rationale: 30% of PDFs are scanned images; unlocks major use case

[I-2] Batch Processing Mode (V-1) [HIGH]
- What: Process multiple PDFs with glob patterns
- Impact: (↑ D-5 by +0, score already 2)
- Effort: Low
- Rationale: Power users process document batches; convenience feature

### MEDIUM Priority

[I-3] PDF/A Compliance Check (V-5) [MEDIUM]
- What: Validate and convert to PDF/A for archival
- Impact: (↑ D-7 by +0, score already 2)
- Effort: Medium
- Rationale: Professional/legal document requirements

[I-4] Cloud Upload (V-2) [MEDIUM]
- What: Direct upload to S3, GCS, or cloud storage
- Impact: (↑ D-3 by +0, score already 2)
- Effort: Medium
- Rationale: Integration with cloud workflows

### LOW Priority

[I-5] Progress Tracking (V-4) [LOW]
- What: Show progress for large batch operations
- Impact: (↑ D-6 by +0, score already 2)
- Effort: Low
- Rationale: Nice-to-have for long operations

## Quick Wins
- [I-2] Batch Processing Mode (Low effort, high convenience)

## Potential Improved Score: 14/14 with I-1
```

### Step 9: Specification Offer

```
Would you like me to generate a specification for enhancing this skill?

Options:
1. Generate spec with selected improvements (e.g., "I-1, I-2")
2. Generate spec with all improvements
3. Skip specification generation

Which improvements should be included?
```

---

## Example 3: Text Description - Consider Verdict with Full Flow

**User input:** `analyze-skill "A skill that analyzes git commit history and suggests code review priorities"`

### After Steps 1-7 (abbreviated)

```
### Total Score: 8/14
### Verdict: Consider

**Score Range:** 6-9 points = Moderate value
**Recommendation:** Moderate value. Consider building if code review efficiency is a strategic priority.
```

### Step 8: Enhancement Opportunities

```
## Enhancement Opportunities (5-Vector Exploration)

### HIGH Priority

[I-1] Issue Tracking Integration (V-2) [HIGH]
- What: Connect to Jira/GitHub Issues to correlate commits with bugs
- Impact: (↑ D-3 by +1, ↑ D-5 by +1)
- Effort: Medium
- Rationale: Shows which code areas have highest issue density

[I-2] Code Quality Analysis (V-5) [HIGH]
- What: Integrate pre-trained models for code smell detection
- Impact: (↑ D-1 by +2, ↑ D-7 by +1)
- Effort: High
- Rationale: Adds specialized knowledge beyond git metadata

### MEDIUM Priority

[I-3] Multi-Stage Workflow (V-3) [MEDIUM]
- What: File-level → Function-level → Risk scoring pipeline
- Impact: (↑ D-5 by +1)
- Effort: Medium
- Rationale: Better handles large codebases

[I-4] Team Velocity Insights (V-5) [MEDIUM]
- What: Analyze commit patterns for productivity bottlenecks
- Impact: (↑ D-7 by +1)
- Effort: Medium
- Rationale: Adds management-level insights

### LOW Priority

[I-5] Interactive Priority Adjustment (V-4) [LOW]
- What: Let users adjust and save priority profiles
- Impact: (↑ D-6 by +1)
- Effort: Medium
- Rationale: Customization for repeat users

## Quick Wins
None identified (all require medium+ effort)

## Potential Improved Score
- With I-1 + I-2: 13/14 (Strong Build)
- With all improvements: 14/14
```

### Step 9: Specification Offer

```
Would you like me to generate a specification for this skill?

This specification will include:
- Goal and objective
- Happy path example
- Input/output contracts
- Process workflow
- Quality gates
- Selected enhancements integrated

Options:
1. Generate spec with improvements: specify IDs (e.g., "I-1, I-2")
2. Generate spec without improvements (base concept only)
3. Skip specification

Which option?
```

**User responds:** "1 with I-1 and I-2"

**Output:**
```
Specification saved to: commit-review-prioritizer.skill-design.md

The specification includes:
- Complete objective with issue tracking and code quality analysis
- Inputs: repository path, issue tracker credentials, analysis depth
- Outputs: prioritized review list, risk scores, issue correlation
- 5-stage workflow with quality gates
- Implementation notes for MCP integrations

Ready for use with build-with-patterns.
```

---

## Example 4: Existing Skill - Enhancements Only Mode

**User input:** `analyze-skill pdf --enhancements-only`

```
## Enhancement Analysis: pdf

### Current Capability Map

**Inputs:**
- Single PDF file paths
- Operation type (extract, create, merge, split, fill)

**Outputs:**
- Extracted text, tables
- New PDF files
- Merged/split documents

**Tools:**
- pdfplumber (extraction)
- PyPDF2 (manipulation)
- File system operations

**Validation:**
- PDF format verification
- Page range validation

**UX Features:**
- Single-command invocations
- Operation-specific flags

### Enhancement Opportunities (5-Vector Exploration)

[I-1] OCR Support (V-2) [HIGH]
- What: Handle scanned PDFs via Tesseract MCP
- Why: 30% of PDFs are images, currently unsupported
- Effort: Medium
- Dependencies: tesseract-mcp server

[I-2] Batch Processing (V-1) [HIGH]
- What: Process multiple PDFs with glob patterns
- Why: Users repeatedly invoke for document sets
- Effort: Low
- Dependencies: None

[I-3] Joplin Export (V-2) [MEDIUM]
- What: Save extracted text directly to Joplin notes
- Why: Integrates into knowledge management workflow
- Effort: Low
- Dependencies: joplink MCP server

[I-4] PDF/A Validation (V-5) [MEDIUM]
- What: Check and convert to archival format
- Why: Legal/compliance requirements
- Effort: Medium
- Dependencies: None

[I-5] Cloud Upload (V-2) [MEDIUM]
- What: Direct upload to S3/GCS after processing
- Why: Cloud workflow integration
- Effort: Medium
- Dependencies: Cloud credentials

## Quick Wins
- [I-2] Batch Processing
- [I-3] Joplin Export

Would you like a specification for any of these enhancements?
```
