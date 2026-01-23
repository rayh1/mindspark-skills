<output_format>
Produce examination report in this structure:

<executive_summary>
**EXECUTIVE SUMMARY**

- Overall health score: [Critical / Needs Work / Good / Excellent]
- Top 3 most urgent issues
- Estimated effort to fix: [Hours]
</executive_summary>

<critical_issues>
**CRITICAL ISSUES (Must Fix)**

| Issue | Location | Description | Suggested Fix |
|-------|----------|-------------|---------------|
| ... | ... | ... | ... |
</critical_issues>

<contradictions_found>
**CONTRADICTIONS FOUND**

| Directive A | Location A | Directive B | Location B | Resolution |
|-------------|------------|-------------|------------|------------|
| ... | ... | ... | ... | ... |
</contradictions_found>

<redundancies>
**REDUNDANCIES**

| Content | Locations | Recommendation |
|---------|-----------|----------------|
| ... | ... | Remove from X, keep in Y |
</redundancies>

<outdated_content>
**OUTDATED CONTENT**

| Content | Location | Why Outdated | Update Needed |
|---------|----------|--------------|---------------|
| ... | ... | ... | ... |
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

1. **Prioritized action list** — numbered steps to fix the skill, ordered by impact
2. **Revised SKILL.md outline** — an outline describing how to restructure/fix the skill (do not provide a full rewritten SKILL.md file)
3. **Migration notes** — what to do with content being moved/deleted
</final_deliverable>

<empty_section_handling>
If a section has no findings, include it with "None found" rather than omitting it. This satisfies Quality Gate G1 (Coverage).

Example:
```
**CONTRADICTIONS FOUND**

None found.
```
</empty_section_handling>
