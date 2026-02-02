---
name: examine-artifact
description: Examines SKILL.md files and prompts for contradictions, redundancies, structural issues, and outdated content. Automatically detects skill vs prompt type via YAML frontmatter. Use when (1) auditing skills before deployment, (2) identifying improvement opportunities, (3) validating structure against quality standards, (4) reviewing prompt clarity and efficiency, (5) preparing artifacts for refactoring. Produces detailed reports with severity-tagged issues and prioritized actionable recommendations.
---

<interpretation_check>
Before starting:
- Restate the target artifact(s) and scope (single vs multiple) in your own words
- Identify the artifact type (skill or prompt) or note if detection will be automatic
- List key assumptions (if used)
- If anything is ambiguous, ask the user to confirm before proceeding
</interpretation_check>

<type_detection>
1. Read first 10 lines of target file
2. Check for YAML frontmatter (starts with `---` on line 1)
3. If frontmatter present with both `name:` AND `description:` fields → **skill**, otherwise → **prompt**
4. If no frontmatter → **prompt**
5. Report detected type in interpretation check
6. Apply appropriate framework:
   - skill → skill-specific checks + shared checks
   - prompt → prompt-specific checks + shared checks
</type_detection>

<output_schema>
**Write the full report to a file, not to chat.**

Artifact:
- Report file: `{artifact-directory}/EXAMINATION-REPORT.md`

In chat, provide:
- Confirmation that examination is complete
- Detected artifact type (skill or prompt)
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
- Report file written for each examined artifact
- All issues tagged with [CRITICAL], [HIGH], [MEDIUM], or [LOW]
- Chat summary includes artifact type, file path, health score with severity breakdown, and action list
- User is prompted to select actions to execute
</output_schema>

<inputs_first>
**Required inputs:**
- Target artifact(s) to examine: file paths or folder path containing SKILL.md or prompt files
- Scope: single artifact or multiple artifacts

**Derived inputs (automatic):**
- `artifact_type`: Determined by type_detection logic (skill or prompt)

**Assumptions (list explicitly if used):**
- If given folder path, examine all SKILL.md files under it (for skills)
- If given a single .md file without YAML frontmatter with name/description, treat as prompt
- If multiple artifacts provided, produce one report per artifact

**Validation:**
- Provided paths must exist and be readable
- If a folder path is provided, it must contain at least one examinable file
</inputs_first>

<clarifying_questions>
triggers:
- Required inputs missing or ambiguous
- Cannot determine artifact type automatically (edge case)
constraints:
- max_per_phase: 5
- batch_related: true
fallback:
- If still missing after questions: state what is needed and stop
</clarifying_questions>

<user_approval_gate>
Before writing: if `{artifact-directory}/EXAMINATION-REPORT.md` already exists, ask whether to overwrite.

- If user approves overwrite: overwrite the existing report.
- If user rejects overwrite or does not respond: do not overwrite; write to `{artifact-directory}/EXAMINATION-REPORT.new.md` instead.

If `{artifact-directory}/EXAMINATION-REPORT.md` does not exist: proceed.

**When examining multiple artifacts:** Check each report file independently; mix of overwrites and new files is acceptable.
</user_approval_gate>

<scope_fence>
**In scope:**
- Analyze skill files for structural and content issues
- Analyze prompt files for clarity, organization, and efficiency issues
- Produce examination reports (EXAMINATION-REPORT.md)
- Identify contradictions, redundancies, and outdated content
- Provide actionable recommendations and outlines
- Execute user-approved fixes from the prioritized action list

**Permitted execution actions:**
- Remove redundant sections
- Consolidate duplicate content within files
- Fix contradictions by aligning conflicting directives
- Restructure sections for clarity (merge, split, reorder)
- Update references between files (skills only)
- Delete unnecessary files (README.md, CHANGELOG.md)

**Execution guardrails:**
- NEVER execute without explicit user approval via AskUserQuestion
- NEVER add new features or capabilities beyond fixing identified issues
- NEVER modify the core functionality or purpose of the artifact
- NEVER execute actions on files outside the examined artifact directory
- NEVER modify canonical pattern section labels (preserve exact names from apply-patterns/build-with-patterns)
- ALWAYS preserve all reference files, scripts, and assets unless explicitly approved for deletion

**Out of scope:**
- Create entirely new skills/prompts or add unrelated features
- Execute or test the artifact being examined (only fix structure/content)
- Make changes to artifacts other than the one being examined
- Modify user code or project files outside artifact directories

**Note:** This skill does not require scripts/ or assets/ directories. It operates purely through analysis and produces markdown reports. Examined artifacts may have these directories, but this skill itself does not.
</scope_fence>

<step_contract>
Follow this exact sequence:

1. Read inputs and enumerate target files
2. For each artifact:
   a. Detect artifact type using type_detection logic
   b. Run appropriate analysis framework:
      - If skill: shared checks (sections 3-4, 6-8, 10-12) + skill-specific checks (sections 1-2, 5, 9)
      - If prompt: shared checks (sections 3-4, 6-8, 10-12) + prompt-specific checks (P-01 to P-20)
   - If multiple artifacts: iterate sequentially, produce complete report per artifact before proceeding to next
3. Write full report to `{artifact-directory}/EXAMINATION-REPORT.md` (see references/output-format.md)
4. Include Final Deliverable items in report (action list, revised outline, migration notes)
5. In chat, provide: confirmation, artifact type, file path, health score with severity breakdown, and prioritized action list
6. Ask user which actions to execute using AskUserQuestion
7. Execute the selected actions (if any; if none selected, complete successfully)
</step_contract>

<decision_points>
**Common (both types):**
- **File cannot be read or doesn't exist** → Record as critical issue under "Dead code identification", continue with remaining files
- **Directive is ambiguous** → Include in Directive Extraction with concrete clarification suggestion

**Skill-specific:**
- **Skill exceeds 500 lines** → Propose split into references/, keep SKILL.md as executive workflow
- **Multiple skills examined** → Keep findings separated by skill name/path; do not merge directives across skills

**Prompt-specific:**
- **Prompt exceeds 300 lines** → Flag as MEDIUM, suggest condensing or restructuring
- **Prompt exceeds 500 lines** → Flag as HIGH, strongly recommend splitting or significant revision
- **No clear section structure** → Flag as HIGH, suggest adding headings
</decision_points>

<quality_gates>
Before finalizing the report and chat summary, verify all gates:

**Structural Gates:**
- **G1 Coverage Gate:** All sections in Output Format present (even if empty with "None found")
- **G2 Traceability Gate:** Every extracted directive includes file + line number
- **G3 Evidence Gate:** Contradictions cite specific quotes and locations from both sides
- **G4 Scope Gate:** No recommendations beyond artifact structure improvements unless directly required by identified issue

**Standards Compliance Gates:**
- **G5 Severity Gate:** Every identified issue has assigned severity [CRITICAL], [HIGH], [MEDIUM], or [LOW]
  - Check: All issues in Critical Issues, Contradictions, Redundancies, etc. are tagged
  - Verify: Health score calculation uses severity counts correctly

- **G6 Criteria Gate:** Assessments reference specific standard criteria, not just opinions
  - Check: Issues cite relevant sections from quality standards when applicable
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

<review_step>
After completing all steps, perform holistic review:

**Criteria:**
- All steps in step_contract completed
- Report file(s) written to correct location(s)
- All quality gates (G1-G7) passed
- All identified issues tagged with severity ([CRITICAL], [HIGH], [MEDIUM], [LOW])
- Health score calculated correctly using severity_framework formula
- Chat summary includes: artifact type, file path, health score, action list
- If actions were selected: verify execution completed successfully

**Max cycles:** 1

**Per cycle:**
1. Identify issues against criteria
2. Fix issues
3. Re-check criteria

**Exit conditions:**
- No issues found, OR
- Max cycles reached

**If issues remain at max_cycles:** Report them in chat output and note in final summary
</review_step>

<severity_framework>
Assign severity to every identified issue based on impact.

**Authoritative severity assignments** (see references/quality-standards-shared.md for details):

- **CRITICAL:** Blocks correctness/triggering (missing "when to use", dead references, 3+ duplications)
- **HIGH:** Significantly degrades quality (>700 lines, <20 words description, no structure)  
- **MEDIUM:** Quality improvement needed (500-700 lines, <50/>200 words, missing TOC)
- **LOW:** Polish/consistency (voice issues, vague naming, minor redundancy)

**Health Score:** 2+ CRITICAL → Critical 🔴; 0-1 CRITICAL + 3+ HIGH → Needs Work 🟡; 0 CRITICAL + 0-2 HIGH → Good 🟢; 0 CRITICAL + 0 HIGH + 0-2 MEDIUM → Excellent ✨

**In reports:** Tag every issue with severity [CRITICAL], [HIGH], [MEDIUM], or [LOW]
</severity_framework>

<analysis_framework>
Load and apply checks systematically based on detected artifact type.

**Skills:**
- Shared: references/analysis-framework-shared.md (sections 3-4, 6-8, 10-12)
- Skill-specific: references/skill-specific-checks.md (sections 1-2, 5, 9)
- Standards: references/skill-quality-standards.md

**Prompts:**
- Shared: references/analysis-framework-shared.md (sections 3-4, 6-8, 10-12)
- Prompt-specific: references/prompt-specific-checks.md (P-01 to P-20)
- Standards: references/prompt-quality-standards.md

**All artifacts:** Apply references/quality-standards-shared.md (universal criteria)

Work through frameworks systematically, documenting findings per references/output-format.md structure.
</analysis_framework>

<stop_conditions>
**Done when:**
- Step Contract complete for all targets
- All targets analyzed through the appropriate framework
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
**Analysis frameworks:**
- references/analysis-framework-shared.md (shared checks for all artifact types)
- references/skill-specific-checks.md (skill-only checks)
- references/prompt-specific-checks.md (prompt-only checks)

**Quality standards:**
- references/quality-standards-shared.md (universal quality criteria)
- references/skill-quality-standards.md (skill-specific quality criteria)
- references/prompt-quality-standards.md (prompt-specific quality criteria)

**Pattern detection:**
- references/pattern-label-registry.md (canonical pattern names to preserve during voice checking)

**Output format:**
- references/output-format.md (report structure and formatting)
</reference_index>
