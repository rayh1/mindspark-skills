---
name: examine-skill
description: Analyze skill files for problems, contradictions, redundancies, outdated content, and structural issues. Use when auditing skills, reviewing skill quality, or preparing skills for improvement.
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
- Health score with emoticon: "Health: Good 🟢" (use Critical 🔴, Needs Work 🟡, Good 🟢, or Excellent ✨)
- The prioritized action list from the report
- Prompt: "What actions do you want to execute?"
- Then use AskUserQuestion to let the user select which actions to execute
- Execute the selected actions

Validation:
- Report file written for each examined skill
- Chat summary includes file path, health score with emoticon, and action list
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
2. For each skill, run Analysis Framework sections 1-10
3. Write full report to `{skill-directory}/EXAMINATION-REPORT.md` (see references/output-format.md)
4. Include Final Deliverable items in report (action list, revised SKILL.md outline, migration notes)
5. In chat, provide: confirmation, file path, health score with emoticon, and prioritized action list
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
Before finalizing the report and chat summary, verify:

- **G1 Coverage Gate:** All sections in Output Format present (even if empty with "None found")
- **G2 Traceability Gate:** Every extracted directive includes file + line number
- **G3 Evidence Gate:** Contradictions cite specific quotes and locations from both sides
- **G4 Scope Gate:** No recommendations beyond skill/prompt structure improvements unless directly required by identified issue

Evidence capture guidance (for G2/G3):
- Use `nl -ba {file}` to capture stable line numbers
- Quote the exact relevant lines in the report (copy/paste), alongside file + line range

**Verification process:**
1. After writing the report, verify all gates above
2. If any gate fails: fix once and re-check
3. If still fails after one fix: stop with `ERROR: GATE_FAIL <GateName>`
4. If issues found but fixable: fix once → re-check
5. If issues persist: report remaining issues and stop
</quality_gates>

<lens>
Analyze findings from multiple perspectives:
- **Correctness lens:** Does this pattern/directive prevent errors or ambiguity?
- **Integration lens:** Conflicts with existing structure? Duplicates content?
- **ROI lens:** Does value (reliability gain) exceed overhead (token cost)?

Per lens: findings + severity (critical/major/minor)
Synthesis: merge findings, flag conflicts (e.g., high correctness but low ROI)
Coverage: each lens must report for critical issues and contradictions
</lens>

<addressable_output>
Assign unique IDs to output items for follow-up reference:
- Format: `[{prefix}-{number}]`
- Prefixes: Action items = A, Issues = I, Recommendations = R
- Presentation: inline in report tables and action lists
- Usage: User can reference specific items ("apply A-1 and A-3" or "explain I-2")
</addressable_output>

<analysis_framework>
Work through each section systematically:

**1. STRUCTURE ANALYSIS**
- [D-59] Correct anatomy? (SKILL.md with YAML frontmatter + optional scripts/, references/, assets/)
- [D-60] YAML frontmatter complete? (requires: name, description)
- [D-61] SKILL.md under 500 lines? If over, what should split into reference files?
- [D-62] Reference files properly organized and clearly named?
- [D-63] Unnecessary documentation (README.md, CHANGELOG.md) that should be removed?

**2. TRIGGER & DESCRIPTION EXAMINATION**
- [D-64] Is `description` field comprehensive enough to trigger correctly?
- [D-65] Does it include BOTH what the skill does AND when to use it?
- [D-66] Are "When to Use" sections buried in body that should be in description? (Body loads AFTER triggering)
- [D-67] Would Claude know when to activate based solely on description?

**3. DIRECTIVE EXTRACTION**
Create numbered list of EVERY instruction/directive:
- [D-68] Extract each "do this" or "don't do this" statement
- [D-69] Note file and line number for each
- [D-70] Flag vague or ambiguous directives

**4. CONTRADICTION DETECTION**
Compare all extracted directives:
- [D-71] Identify conflicting pairs
- [D-72] Check if same topic addressed differently in multiple places
- [D-73] Check if reference files contradict SKILL.md
- [D-74] List each contradiction with specific quotes and locations

**5. REDUNDANCY ANALYSIS**
- [D-75] Information appearing in multiple places
- [D-76] Explanations duplicating what Claude already knows
- [D-77] Verbose sections that could be condensed
- [D-78] Challenge each paragraph: "Does this justify its token cost?"

**6. TEMPORAL ANALYSIS (Outdated Content)**
- [D-79] References to specific dates, versions, or "new" features
- [D-80] Language suggesting currency that may be old ("recently", "now", "the latest")
- [D-81] Deprecated tools, APIs, or approaches
- [D-82] Instructions referencing features/behaviors that may have changed

**7. FLOW MAPPING**
For each major user scenario:
- [D-83] Trace complete decision path through all files
- [D-84] Identify dead ends or unclear branches
- [D-85] Find circular references or infinite loops
- [D-86] Map where user/Claude might get confused

**8. FREEDOM CALIBRATION**
For each instruction, assess specificity level:
- [D-87] **HIGH freedom** (text guidance) — appropriate when multiple approaches valid
- [D-88] **MEDIUM freedom** (pseudocode/parameters) — appropriate when preferred pattern exists
- [D-89] **LOW freedom** (specific scripts) — appropriate when operations fragile

Flag mismatches:
- [D-90] Overly rigid instructions limiting valid approaches
- [D-91] Overly loose instructions leaving fragile operations undefined

**9. PROGRESSIVE DISCLOSURE CHECK**
Is information at right level?
- [D-92] Essential workflow → SKILL.md
- [D-93] Detailed references → references/
- [D-94] Executable code → scripts/
- [D-95] Output resources → assets/

[D-96] Is SKILL.md trying to do too much? Are reference files clearly signposted?

**10. DEAD CODE IDENTIFICATION**
- [D-97] Instructions that can never be triggered
- [D-98] Conditional branches with impossible conditions
- [D-99] References to files or functions that don't exist
- [D-100] Parameters or options never used
</analysis_framework>

<stop_conditions>
**Done when:**
- Step Contract complete for all targets
- All targets analyzed through the 10-part framework
- Output artifacts + chat summary produced per `output_schema`
- All Quality Gates pass (G1-G4)
- Final Deliverable section produced
- User has been asked which actions to execute
- Selected actions have been executed (if any)

**Don't:**
- Rewrite unrelated documentation
- Add new features or "nice-to-have" content not justified by examination
- Change required output headings or omit sections
</stop_conditions>

<reference_index>
**Output format:** references/output-format.md
</reference_index>
