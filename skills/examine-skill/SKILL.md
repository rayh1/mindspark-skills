---
name: examine-skill
description: Analyze skill files for problems, contradictions, redundancies, outdated content, and structural issues. Use when auditing skills, reviewing skill quality, or preparing skills for improvement.
---

<objective>
Examine skill files thoroughly and produce a structured examination report. Identifies problems, contradictions, redundancies, outdated content, and structural issues that make skills confusing or ineffective.
</objective>

<quick_start>
1. Get target skill path(s) from user
2. Read all skill files (SKILL.md + references/, scripts/, assets/)
3. Run the 10-part analysis framework
4. Write report to file: `{skill-directory}/EXAMINATION-REPORT.md`
5. Summarize key findings in chat (top 3 issues only)
</quick_start>

<output_handling>
**Write the full report to a file, not to chat.**

- Single skill: Write to `{skill-directory}/EXAMINATION-REPORT.md`
- Multiple skills: Write to `{skill-directory}/EXAMINATION-REPORT.md` for each skill

**In chat, provide only:**
- Confirmation that examination is complete
- File path where report was written
- Top 3 most urgent issues (one line each)
- Prompt: "Read the full report for details and action items."
</output_handling>

<inputs>
**Required inputs:**
- Target skill(s) to examine: file paths or folder path containing SKILL.md files
- Scope: single skill or multiple skills

**If inputs missing:** Ask up to 5 clarification questions. If still missing, state what is needed.

**Assumptions (list explicitly if used):**
- If given folder path, examine all SKILL.md files under it
- If multiple skills provided, produce one report per skill
</inputs>

<scope_fence>
**In scope:**
- Analyzing skill files for structural and content issues
- Producing examination reports (EXAMINATION-REPORT.md)
- Identifying contradictions, redundancies, and outdated content
- Providing actionable recommendations and outlines

**Out of scope:**
- Fixing or implementing recommended changes (analysis only)
- Rewriting SKILL.md files (report includes outline, user applies)
- Creating new skills or adding features
- Modifying reference files, scripts, or assets
- Executing or testing the skill being examined
</scope_fence>

<step_contract>
Follow this exact sequence:

1. Read inputs and enumerate target files
2. For each skill, run Analysis Framework sections 1-10
3. Write full report to `{skill-directory}/EXAMINATION-REPORT.md` (see references/output-format.md)
4. Include Final Deliverable items in report (action list, rewritten SKILL.md or outline, migration notes)
5. Summarize top 3 issues in chat with file path
</step_contract>

<step_ledger>
Maintain and update after each major step:

```text
Step Ledger:
done: [ ... ]
next: <step>
targets: [ ... ]
assumptions: [ ... ]
open_questions: [ ... ]
```
</step_ledger>

<decision_points>
- **File cannot be read or doesn't exist** → Record as critical issue under "Dead code identification", continue with remaining files
- **Skill exceeds 500 lines** → Propose split into references/, keep SKILL.md as executive workflow
- **Multiple skills examined** → Keep findings separated by skill name/path; do not merge directives across skills
- **Directive is ambiguous** → Include in Directive Extraction with concrete clarification suggestion
</decision_points>

<quality_gates>
Before finalizing, verify:

- **G1 Coverage Gate:** All sections in Output Format present (even if empty with "None found")
- **G2 Traceability Gate:** Every extracted directive includes file + line number
- **G3 Evidence Gate:** Contradictions cite specific quotes and locations from both sides
- **G4 Scope Gate:** No recommendations beyond skill/prompt structure improvements unless directly required by identified issue

**If any gate fails:** Fix once and re-check. If still fails, stop with: `ERROR: GATE_FAIL <GateName>`
</quality_gates>

<analysis_framework>
Work through each section systematically:

**1. STRUCTURE ANALYSIS**
- Correct anatomy? (SKILL.md with YAML frontmatter + optional scripts/, references/, assets/)
- YAML frontmatter complete? (requires: name, description)
- SKILL.md under 500 lines? If over, what should split into reference files?
- Reference files properly organized and clearly named?
- Unnecessary documentation (README.md, CHANGELOG.md) that should be removed?

**2. TRIGGER & DESCRIPTION EXAMINATION**
- Is `description` field comprehensive enough to trigger correctly?
- Does it include BOTH what the skill does AND when to use it?
- Are "When to Use" sections buried in body that should be in description? (Body loads AFTER triggering)
- Would Claude know when to activate based solely on description?

**3. DIRECTIVE EXTRACTION**
Create numbered list of EVERY instruction/directive:
- Extract each "do this" or "don't do this" statement
- Note file and line number for each
- Flag vague or ambiguous directives

**4. CONTRADICTION DETECTION**
Compare all extracted directives:
- Identify conflicting pairs
- Check if same topic addressed differently in multiple places
- Check if reference files contradict SKILL.md
- List each contradiction with specific quotes and locations

**5. REDUNDANCY ANALYSIS**
- Information appearing in multiple places
- Explanations duplicating what Claude already knows
- Verbose sections that could be condensed
- Challenge each paragraph: "Does this justify its token cost?"

**6. TEMPORAL ANALYSIS (Outdated Content)**
- References to specific dates, versions, or "new" features
- Language suggesting currency that may be old ("recently", "now", "the latest")
- Deprecated tools, APIs, or approaches
- Instructions referencing features/behaviors that may have changed

**7. FLOW MAPPING**
For each major user scenario:
- Trace complete decision path through all files
- Identify dead ends or unclear branches
- Find circular references or infinite loops
- Map where user/Claude might get confused

**8. FREEDOM CALIBRATION**
For each instruction, assess specificity level:
- **HIGH freedom** (text guidance) — appropriate when multiple approaches valid
- **MEDIUM freedom** (pseudocode/parameters) — appropriate when preferred pattern exists
- **LOW freedom** (specific scripts) — appropriate when operations fragile

Flag mismatches:
- Overly rigid instructions limiting valid approaches
- Overly loose instructions leaving fragile operations undefined

**9. PROGRESSIVE DISCLOSURE CHECK**
Is information at right level?
- Essential workflow → SKILL.md
- Detailed references → references/
- Executable code → scripts/
- Output resources → assets/

Is SKILL.md trying to do too much? Are reference files clearly signposted?

**10. DEAD CODE IDENTIFICATION**
- Instructions that can never be triggered
- Conditional branches with impossible conditions
- References to files or functions that don't exist
- Parameters or options never used
</analysis_framework>

<stop_conditions>
**Done when:**
- Step Contract complete for all targets
- All Quality Gates pass
- Final Deliverable section produced

**Don't:**
- Rewrite unrelated documentation
- Add new features or "nice-to-have" content not justified by examination
- Change required output headings or omit sections
</stop_conditions>

<reference_index>
**Output format:** references/output-format.md
</reference_index>

<success_criteria>
Examination complete when:
- All target skills analyzed through 10-part framework
- Full report written to `{skill-directory}/EXAMINATION-REPORT.md`
- All quality gates pass (G1-G4)
- Chat summary provided with file path and top 3 issues
</success_criteria>
