---
name: examine-skill
description: Comprehensive SKILL.md examination for contradictions, redundancies, structural issues, outdated content, and quality standards compliance. Analyzes skill anatomy, description field quality, progressive disclosure, reference organization, and context efficiency. Use when (1) auditing skill files before deployment, (2) identifying improvement opportunities in existing skills, (3) validating SKILL.md structure against quality standards, (4) preparing skills for refactoring or enhancement, (5) reviewing skill quality for production readiness, or any other skill examination tasks. Produces detailed reports with severity-tagged issues and actionable recommendations.
---

<interpretation_check>
Before starting:
- Restate the target skill(s) and scope (single vs multiple) in your own words
- List key assumptions (if used)
- If anything is ambiguous, ask the user to confirm before proceeding
</interpretation_check>

<output_schema>
**Write the full report to a file, not to chat.**

Artifact:
- Report file: `{skill-directory}/EXAMINATION-REPORT.md`

In chat, provide:
- Confirmation that examination is complete
- File path where report was written
- Health score with emoticon and severity breakdown:
  - "Health: Good 🟢 (0 Critical, 1 High, 3 Medium, 2 Low)"
  - Use emoticons: Critical 🔴, Needs Work 🟡, Good 🟢, Excellent ✨
  - Calculate using severity_framework formula
- The prioritized action list from the report (with severity tags)
- Prompt: "What actions do you want to execute?"
- Then use AskUserQuestion to let the user select which actions to execute
- Execute the selected actions

Validation:
- Report file written for each examined skill
- All issues tagged with [CRITICAL], [HIGH], [MEDIUM], or [LOW]
- Chat summary includes file path, health score with severity breakdown, and action list
- User is prompted to select actions to execute
</output_schema>

<inputs_first>
**Required inputs:**
- Target skill(s) to examine: file paths or folder path containing SKILL.md files
- Scope: single skill or multiple skills

**Assumptions (list explicitly if used):**
- If given folder path, examine all SKILL.md files under it
- If multiple skills provided, produce one report per skill

**Validation:**
- Provided paths must exist and be readable
- If a folder path is provided, it must contain at least one `SKILL.md`
</inputs_first>

<clarifying_questions>
triggers:
- Required inputs missing or ambiguous
constraints:
- max_per_phase: 5
- batch_related: true
fallback:
- If still missing after questions: state what is needed and stop
</clarifying_questions>

<user_approval_gate>
Before writing: if `{skill-directory}/EXAMINATION-REPORT.md` already exists, ask whether to overwrite.

- If user approves overwrite: overwrite the existing report.
- If user rejects overwrite or does not respond: do not overwrite; write to `{skill-directory}/EXAMINATION-REPORT.new.md` instead.

If `{skill-directory}/EXAMINATION-REPORT.md` does not exist: proceed.

**When examining multiple skills:** Check each report file independently; mix of overwrites and new files is acceptable.
</user_approval_gate>

<scope_fence>
**In scope:**
- Analyzing skill files for structural and content issues
- Producing examination reports (EXAMINATION-REPORT.md)
- Identifying contradictions, redundancies, and outdated content
- Providing actionable recommendations and outlines
- Executing user-approved fixes from the prioritized action list

**Permitted execution actions:**
- Removing redundant sections from SKILL.md
- Consolidating duplicate content within files
- Fixing contradictions by aligning conflicting directives
- Restructuring sections for clarity (merging, splitting, reordering)
- Updating references between files
- Deleting unnecessary files (README.md, CHANGELOG.md)

**Execution guardrails:**
- NEVER execute without explicit user approval via AskUserQuestion
- NEVER add new features or capabilities beyond fixing identified issues
- NEVER modify the core functionality or purpose of the skill
- NEVER execute actions on files outside the examined skill directory
- ALWAYS preserve all reference files, scripts, and assets unless explicitly approved for deletion

**Out of scope:**
- Creating entirely new skills or adding unrelated features
- Executing or testing the skill being examined (only fix structure/content)
- Making changes to skills other than the one being examined
- Modifying user code or project files outside skill directories
</scope_fence>

<step_contract>
Follow this exact sequence:

1. Read inputs and enumerate target files
2. For each skill, run Analysis Framework sections 1-12
   - If multiple skills: iterate sequentially, produce complete report per skill before proceeding to next
3. Write full report to `{skill-directory}/EXAMINATION-REPORT.md` (see references/output-format.md)
4. Include Final Deliverable items in report (action list, revised SKILL.md outline, migration notes)
5. In chat, provide: confirmation, file path, health score with severity breakdown, and prioritized action list
6. Ask user which actions to execute using AskUserQuestion
7. Execute the selected actions (if any; if none selected, complete successfully)
</step_contract>

<decision_points>
- **File cannot be read or doesn't exist** → Record as critical issue under "Dead code identification", continue with remaining files
- **Skill exceeds 500 lines** → Propose split into references/, keep SKILL.md as executive workflow
- **Multiple skills examined** → Keep findings separated by skill name/path; do not merge directives across skills
- **Directive is ambiguous** → Include in Directive Extraction with concrete clarification suggestion
</decision_points>

<quality_gates>
Before finalizing the report and chat summary, verify all gates:

**Structural Gates:**
- **G1 Coverage Gate:** All sections in Output Format present (even if empty with "None found")
- **G2 Traceability Gate:** Every extracted directive includes file + line number
- **G3 Evidence Gate:** Contradictions cite specific quotes and locations from both sides
- **G4 Scope Gate:** No recommendations beyond skill/prompt structure improvements unless directly required by identified issue

**Standards Compliance Gates:**
- **G5 Severity Gate:** Every identified issue has assigned severity [CRITICAL], [HIGH], [MEDIUM], or [LOW]
  - Check: All issues in Critical Issues, Contradictions, Redundancies, etc. are tagged
  - Verify: Health score calculation uses severity counts correctly

- **G6 Criteria Gate:** Assessments reference specific standard criteria, not just opinions
  - Check: Issues cite relevant sections from skill-quality-standards.md when applicable
  - Example: "Description is 15 words [CRITICAL] - below ~50-150 word target (§ Description Field Excellence)"
  - Verify: Not just "description is bad" but WHY based on standards

- **G7 Examples Gate:** Critical/High issues include guidance from standards
  - Check: For CRITICAL/HIGH issues, provide:
    - What's wrong (specific)
    - Why it's wrong (reference to standard)
    - How to fix (concrete recommendation)
  - Example pattern shown when helpful (good vs bad from standards)

Evidence capture guidance (for G2/G3):
- Use `nl -ba {file}` to capture stable line numbers
- Quote the exact relevant lines in the report (copy/paste), alongside file + line range

**Verification process (authoritative - referenced in output-format.md):**
1. After writing the report, verify G1-G7
2. If any gate fails: fix once and re-check
3. If still fails after one fix: stop with `ERROR: GATE_FAIL <GateName>`
4. If issues found but fixable: fix once → re-check
5. If issues persist: report remaining issues and stop
</quality_gates>

<lens>
Analyze findings from multiple perspectives:

- **Correctness lens:** Does this pattern/directive prevent errors or ambiguity?
  - Report: findings + severity (critical/major/minor)

- **Integration lens:** Conflicts with existing structure? Duplicates content?
  - Report: findings + severity (critical/major/minor)

- **ROI lens:** Does value (reliability gain) exceed overhead (token cost)?
  - Report: findings + severity (critical/major/minor)

**Synthesis:** Merge findings across lenses, flag conflicts (e.g., high correctness but low ROI)

**Coverage requirement:** Each lens must report for critical issues and contradictions
</lens>

<addressable_output>
Assign unique IDs to output items for follow-up reference:
- Format: `[{prefix}-{number}]`
- Prefixes: Action items = A, Issues = I, Recommendations = R
- Presentation: inline in report tables and action lists
- Usage: User can reference specific items ("apply A-1 and A-3" or "explain I-2")
</addressable_output>

<severity_framework>
Assign severity to every identified issue based on impact.

**This table is the authoritative source for severity assignments** (referenced throughout analysis-framework.md):

| Severity | Impact | Key Examples |
|----------|--------|--------------|
| **CRITICAL** | Blocks correctness or triggering | Description missing "when to use" (D-65); Essential workflow in references/ not SKILL.md (D-92); Dead references (D-99); 3+ identical duplications (D-104) |
| **HIGH** | Significantly degrades quality/usability | SKILL.md >700 lines (D-61); Description <20 words (D-64); No navigation logic for references/ (D-96); Description matches "Bad" examples (D-101) |
| **MEDIUM** | Quality improvement needed | SKILL.md 500-700 lines (D-61); Description <50 or >200 words (D-64); Missing TOC in reference >100 lines (D-105); References nested >1 level (D-106); Details in SKILL.md should be in references/ (D-93) |
| **LOW** | Polish/consistency | Voice inconsistency (D-108, D-109); Vague file naming (D-62); Minor redundancy (D-75); Prose where bullets would work (D-111) |

**Health Score Calculation:**
- 2+ CRITICAL → Critical 🔴
- 0-1 CRITICAL, 3+ HIGH → Needs Work 🟡
- 0 CRITICAL, 0-2 HIGH, any MEDIUM → Good 🟢
- 0 CRITICAL, 0 HIGH, 0-2 MEDIUM → Excellent ✨

**In reports:** Tag every issue with severity [CRITICAL], [HIGH], [MEDIUM], or [LOW]
</severity_framework>

<analysis_framework>
Work through each section systematically using the 12-part framework: Structure Analysis, Trigger & Description Examination, Description Quality Depth, Directive Extraction, Contradiction Detection, Redundancy Analysis, Cross-File Duplication, Temporal Analysis, Flow Mapping, Freedom Calibration, Progressive Disclosure Check, Reference Quality, Dead Code Identification, Writing Voice, and Context Efficiency.

For detailed analysis criteria, section-by-section checks (D-59 through D-111), and quality standards, see:
- **references/analysis-framework.md** (detailed 12-section examination framework)
- **references/skill-quality-standards.md** (quality criteria and examples)
</analysis_framework>

<stop_conditions>
**Done when:**
- Step Contract complete for all targets
- All targets analyzed through the 12-part framework (sections 1-12)
- Output artifacts + chat summary produced per `output_schema`
- All Quality Gates pass (G1-G7)
- Final Deliverable section produced
- User has been asked which actions to execute
- Selected actions have been executed (if any)

**Don't:**
- Rewrite unrelated documentation
- Add new features or "nice-to-have" content not justified by examination
- Change required output headings or omit sections
</stop_conditions>

<reference_index>
**Analysis framework:** references/analysis-framework.md (detailed 12-section examination framework)
**Output format:** references/output-format.md (report structure and formatting)
**Quality standards:** references/skill-quality-standards.md (domain expertise for what makes skills excellent)
</reference_index>
