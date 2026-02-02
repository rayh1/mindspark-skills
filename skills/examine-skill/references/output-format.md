<output_format>
Produce examination report in this structure:

<executive_summary>
**EXECUTIVE SUMMARY**

- Overall health score: [Critical 🔴 / Needs Work 🟡 / Good 🟢 / Excellent ✨]
- Severity breakdown: [X Critical, X High, X Medium, X Low issues]
- Top 3 most urgent issues (with severity tags)
- Estimated effort to fix: [Hours]
</executive_summary>

<critical_issues>
**CRITICAL ISSUES (Must Fix)**

| Severity | Issue | Location | Description | Suggested Fix |
|----------|-------|----------|-------------|---------------|
| [CRITICAL] | ... | ... | ... | ... |

Note: Tag every issue with [CRITICAL], [HIGH], [MEDIUM], or [LOW] based on severity_framework
</critical_issues>

<contradictions_found>
**CONTRADICTIONS FOUND**

| Severity | Directive A | Location A | Directive B | Location B | Resolution |
|----------|-------------|------------|-------------|------------|------------|
| [CRITICAL] | ... | ... | ... | ... | ... |
</contradictions_found>

<redundancies>
**REDUNDANCIES**

| Severity | Content | Locations | Recommendation |
|----------|---------|-----------|----------------|
| [HIGH] | ... | ... | Remove from X, keep in Y |
</redundancies>

<outdated_content>
**OUTDATED CONTENT**

| Severity | Content | Location | Why Outdated | Update Needed |
|----------|---------|----------|--------------|---------------|
| [MEDIUM] | ... | ... | ... | ... |
</outdated_content>

<unclear_flows>
**UNCLEAR FLOWS**

For each problematic flow:
- **Scenario:** [what the user is trying to do]
- **Where it breaks:** [specific point of confusion]
- **Suggested clarification:** [how to fix]
</unclear_flows>

<structural_recommendations>
**STRUCTURAL RECOMMENDATIONS**

- Files to merge: ...
- Files to split: ...
- Content to relocate: ...
- Files to delete: ...
</structural_recommendations>

<proposed_new_structure>
**PROPOSED NEW STRUCTURE**

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
</proposed_new_structure>

<line_by_line_issues>
**LINE-BY-LINE ISSUES (Optional Detail)**

| File | Line | Issue Type | Description |
|------|------|------------|-------------|
| ... | ... | ... | ... |
</line_by_line_issues>
</output_format>

<final_deliverable>
After the examination report, provide:

1. **Prioritized action list** — numbered steps to fix the skill, ordered by severity (CRITICAL first, then HIGH, MEDIUM, LOW)
   - Format: "[A-1] [CRITICAL] Fix description field: Add 'when to use' with enumerated cases"
   - Each action tagged with severity
   - Ordered by impact (severity + scope)

2. **Revised SKILL.md outline** — an outline describing how to restructure/fix the skill (do not provide a full rewritten SKILL.md file)

3. **Migration notes** — what to do with content being moved/deleted
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
