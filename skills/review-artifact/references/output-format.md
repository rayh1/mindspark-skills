<output_format>
Produce review report in this structure:

## Table of Contents

1. [Executive Summary](#executive_summary) - Lines 4-12
2. [Critical Issues](#critical_issues) - Lines 14-22
3. [Contradictions Found](#contradictions_found) - Lines 24-32
4. [Redundancies](#redundancies) - Lines 34-42
5. [Outdated Content](#outdated_content) - Lines 44-52
6. [Unclear Flows](#unclear_flows) - Lines 54-63
7. [Type-Specific Issues](#type_specific_issues) - Lines 65-81
8. [Structural Recommendations](#structural_recommendations) - Lines 83-98
9. [Proposed New Structure](#proposed_new_structure) - Lines 100-123
10. [Line-by-Line Issues](#line_by_line_issues) - Lines 125-132
11. [Final Deliverable](#final_deliverable) - Lines 134-153
12. [Severity Usage](#severity_usage) - Lines 155-185
13. [Empty Section Handling](#empty_section_handling) - Lines 187-196
14. [Addressable IDs](#addressable_ids) - Lines 198-211

---


<executive_summary>
**EXECUTIVE SUMMARY**

- Artifact type: [Skill / Prompt]
- Artifact path: [path/to/artifact]
- Overall health score: [Critical 🔴 / Needs Work 🟡 / Good 🟢 / Excellent ✨]
- Severity breakdown: [X Critical, X High, X Medium, X Low issues]
- Top 3 most urgent issues (with severity tags)
</executive_summary>

<critical_issues>
**CRITICAL ISSUES (Must Fix)**

| ID | Severity | Issue | Location | Description | Suggested Fix |
|----|----------|-------|----------|-------------|---------------|
| I-1 | [CRITICAL] | ... | ... | ... | ... |

Note: Tag every issue with [CRITICAL], [HIGH], [MEDIUM], or [LOW] based on severity_framework
</critical_issues>

<contradictions_found>
**CONTRADICTIONS FOUND**

| ID | Severity | Directive A | Location A | Directive B | Location B | Resolution |
|----|----------|-------------|------------|-------------|------------|------------|
| I-2 | [CRITICAL] | ... | ... | ... | ... | ... |

If none: "None found."
</contradictions_found>

<redundancies>
**REDUNDANCIES**

| ID | Severity | Content | Locations | Recommendation |
|----|----------|---------|-----------|----------------|
| I-3 | [HIGH] | ... | ... | Remove from X, keep in Y |

If none: "None found."
</redundancies>

<outdated_content>
**OUTDATED CONTENT**

| ID | Severity | Content | Location | Why Outdated | Update Needed |
|----|----------|---------|----------|--------------|---------------|
| I-4 | [MEDIUM] | ... | ... | ... | ... |

If none: "None found."
</outdated_content>

<unclear_flows>
**UNCLEAR FLOWS**

For each problematic flow:
- **Scenario:** [what the user is trying to do]
- **Where it breaks:** [specific point of confusion]
- **Suggested clarification:** [how to fix]

If none: "None found."
</unclear_flows>

<type_specific_issues>
**TYPE-SPECIFIC ISSUES**

For skills, include:
- Description field assessment (word count, components present/missing)
- Progressive disclosure evaluation
- Reference organization assessment
- Resource categorization issues

For prompts, include:
- Heading structure assessment
- Section ordering evaluation
- Length assessment
- Self-containment check

Format as table with severity tags.
</type_specific_issues>

<structural_recommendations>
**STRUCTURAL RECOMMENDATIONS**

For skills:
- Files to merge: ...
- Files to split: ...
- Content to relocate: ...
- Files to delete: ...
- Reference organization changes: ...

For prompts:
- Sections to merge: ...
- Sections to split: ...
- Content to relocate: ...
- Headings to rename: ... (exclude canonical pattern labels from pattern-label-registry.md)
</structural_recommendations>

<proposed_new_structure>
**PROPOSED NEW STRUCTURE**

For skills:
```
skill-name/
├── SKILL.md (revised outline)
├── references/
│   └── [suggested files]
├── scripts/
│   └── [suggested files]
└── assets/
    └── [suggested files]
```

For prompts:
```markdown
# [Title]
## [Section 1] - purpose
## [Section 2] - purpose
...
```
</proposed_new_structure>

<line_by_line_issues>
**LINE-BY-LINE ISSUES (Optional Detail)**

| ID | File | Line | Issue Type | Description |
|----|------|------|------------|-------------|
| I-5 | ... | ... | ... | ... |

Include only if detailed line-level issues found beyond those in other sections.
</line_by_line_issues>
</output_format>

<final_deliverable>
After the review report, provide:

1. **Prioritized action list** — numbered steps to fix the artifact, ordered by severity (CRITICAL first, then HIGH, MEDIUM, LOW)
   - Format: "[A-1] [CRITICAL] Fix description field: Add 'when to use' with enumerated cases"
   - Each action tagged with severity
   - Ordered by impact (severity + scope)
   - Include estimated token savings where applicable

2. **Revised artifact outline** — an outline describing how to restructure/fix the artifact
   - For skills: SKILL.md section outline + reference file recommendations
   - For prompts: Heading structure + section order recommendations
   - Do not provide full rewritten content

3. **Migration notes** — what to do with content being moved/deleted
   - Which content stays
   - Which content moves where
   - Which content gets removed
</final_deliverable>

<severity_usage>
**Using Severity Throughout the Report:**

Every identified issue must be tagged with severity based on the severity_framework:

- **[CRITICAL]** - Blocks correctness or triggering
- **[HIGH]** - Significantly degrades quality/usability
- **[MEDIUM]** - Quality improvement needed
- **[LOW]** - Polish/consistency

**In tables:** Add severity as first column
**In action lists:** Prefix each action with severity tag
**In executive summary:** Include severity breakdown count

**Example issue with severity:**
```
[HIGH] Description is 18 words, below ~50-150 word target. Missing enumerated
use cases and trigger keywords. See § Description Field Excellence for examples.

Current: "Process documents by reading and analyzing content"
Recommended: "Comprehensive document processing including text extraction,
format conversion, and metadata analysis. Use when Claude needs to: (1) extract
text from .docx/.pdf files, (2) convert between formats, (3) analyze document
structure, or other document processing tasks."
```

This approach ensures assessments are:
1. Objective (based on standards)
2. Actionable (severity guides priority)
3. Educational (references to standards provide learning)
</severity_usage>

<empty_section_handling>
If a section has no findings, include it with "None found" rather than omitting it. This satisfies Quality Gate G1 (Coverage).

Example:
```
**CONTRADICTIONS FOUND**

None found.
```
</empty_section_handling>

<addressable_ids>
**ID Assignment:**

All issues and actions must have unique IDs for follow-up reference:

- Issues: `I-{number}` (I-1, I-2, I-3, ...)
- Actions: `A-{number}` (A-1, A-2, A-3, ...)
- Recommendations: `R-{number}` (R-1, R-2, R-3, ...)

This allows the user to reference specific items:
- "Apply A-1 and A-3"
- "Explain I-2 in more detail"
- "Skip R-4"
</addressable_ids>
