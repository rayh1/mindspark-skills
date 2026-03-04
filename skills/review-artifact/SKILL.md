---
name: review-artifact
description: "Reviews SKILL.md files and prompts for contradictions, redundancies, structural issues, and outdated content. Automatically detects skill vs prompt type via YAML frontmatter. Use when (1) auditing skills before deployment, (2) identifying improvement opportunities, (3) validating structure against quality standards, (4) reviewing prompt clarity and efficiency, (5) preparing artifacts for refactoring. Produces detailed reports with severity-tagged issues and prioritized actionable recommendations. Do NOT use for evolving artifacts after execution (use evolve-artifact instead) or for adding reliability patterns (use apply-patterns instead)."
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
3. If frontmatter present:
   a. If both `name:` AND `description:` fields → **skill**
   b. If `name:` present but `description:` missing → **skill** (D-60 will flag missing description as CRITICAL)
   c. Otherwise → **prompt**
4. If no frontmatter → **prompt**
5. Report detected type in interpretation check
6. Apply appropriate framework:
   - skill → skill-specific checks + shared checks
   - prompt → prompt-specific checks + shared checks
</type_detection>

<output_schema>
**Write the full report to a file, not to chat.**

Artifact:
- Report file: `{artifact-directory}/REVIEW-REPORT.md`

In chat, provide:
- Confirmation that review is complete
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
- Verify execution results and report status

Validation:
- Report file written for each reviewed artifact
- All issues tagged with [CRITICAL], [HIGH], [MEDIUM], or [LOW]
- Consequential actions include confidence assessment (per <confidence_signal>)
- Chat summary includes artifact type, file path, health score with severity breakdown, and action list
- User is prompted to select actions to execute
- If actions executed: verification results reported
</output_schema>

<inputs_first>
**Required inputs:**
- Target artifact(s) to review: file paths or folder path containing SKILL.md or prompt files
- Scope: single artifact or multiple artifacts

**Derived inputs (automatic):**
- `artifact_type`: Determined by type_detection logic (skill or prompt)

**Optional inputs:**
- `max_cycles`: Maximum post-fix review-fix cycles (default: 2, upper bound: 5)

**Assumptions (list explicitly if used):**
- If given folder path, review all SKILL.md files under it (for skills)
- If given a single .md file without YAML frontmatter with name/description, treat as prompt
- If multiple artifacts provided, produce one report per artifact

**Validation:**
- Provided paths must exist and be readable
- If a folder path is provided, it must contain at least one reviewable file
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
Before writing: if `{artifact-directory}/REVIEW-REPORT.md` already exists, ask whether to overwrite.

- If user approves overwrite: overwrite the existing report.
- If user rejects overwrite or does not respond: do not overwrite; write to `{artifact-directory}/REVIEW-REPORT.new.md` instead.

If `{artifact-directory}/REVIEW-REPORT.md` does not exist: proceed.

**When reviewing multiple artifacts:** Check each report file independently; mix of overwrites and new files is acceptable.
</user_approval_gate>

<scope_fence>
**In scope:**
- Analyze skill files for structural and content issues
- Analyze prompt files for clarity, organization, and efficiency issues
- Produce review reports (REVIEW-REPORT.md)
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
- NEVER execute actions on files outside the reviewed artifact directory
- NEVER modify canonical pattern section labels (preserve exact names from apply-patterns/build-with-patterns)
- ALWAYS preserve all reference files, scripts, and assets unless explicitly approved for deletion

**Out of scope:**
- Create entirely new skills/prompts or add unrelated features
- Execute or test the artifact being reviewed (only fix structure/content)
- Make changes to artifacts other than the one being reviewed
- Modify user code or project files outside artifact directories

**Note:** This skill does not require scripts/ or assets/ directories. It operates purely through analysis and produces markdown reports. Reviewed artifacts may have these directories, but this skill itself does not.
</scope_fence>

<step_contract>
Follow this exact sequence:

1. Read inputs and enumerate target files
2. For each artifact:
   a. Detect artifact type using type_detection logic
   b. Run appropriate analysis framework:
      - All types: run all 11 shared checks from references/analysis-framework-shared.md (Directive Extraction, Contradiction Detection, Temporal Analysis, Flow Mapping, Freedom Calibration, Dead Code Identification, Writing Voice, Context Efficiency, Domain Correctness, Model Agnosticism, Determinism Analysis)
      - If skill: also run skill-specific checks from references/skill-specific-checks.md (Structure Analysis, Trigger & Description Review, Cross-File Redundancy, Progressive Disclosure Check, Skill Category & MCP-Specific Checks)
      - If prompt: also run prompt-specific checks from references/prompt-specific-checks.md (P-01 to P-20)
      - All types: run [D-126] tag validation on the main artifact file — check for orphaned/stray XML tags. **Mandatory boundary verification applies** (see D-126 in step 2c for full procedure).
   c. For skills with reference files, analyze each reference file individually (**per-file tracking required**):
      - Enumerate all reference files. For EACH file, report results for:
        - [D-126] Tag validation: check for orphaned/stray XML tags (opening without closing, closing without opening). **Mandatory boundary verification:** the Read tool wraps its output in `</output>` — this appears after the last line of every file and is NOT file content. Before reporting ANY suspected orphaned tag on the last line of a file: (1) run `wc -l` to get actual line count, (2) run `tail -3` to see actual final lines. Only report if the tag appears in the verified output. Tags not on the last line do not require this verification.
        - [D-127] Internal consistency: verify TOC line numbers match actual heading locations, section ordering within the file matches any ordering the file describes
        - Contradiction Detection (D-73b intra-file) applied to this file
        - Domain Correctness (D-115 logical preemption) across this file and SKILL.md
      - Verify: number of files analyzed matches total reference file count (G10)
   - If multiple artifacts: iterate sequentially, produce complete report per artifact before proceeding to next
3. Write full report to `{artifact-directory}/REVIEW-REPORT.md` (see references/output-format.md)
   - For consequential actions (moderate/breaking risk), include confidence assessment per <confidence_signal>
4. Include Final Deliverable items in report (action list, revised outline, migration notes)
5. In chat, provide: confirmation, artifact type, file path, health score with severity breakdown, and prioritized action list
6. Ask user which actions to execute using AskUserQuestion
7. Execute the selected actions (if any; if none selected, complete successfully)
8. Post-fix review-fix cycle (bounded by `max_cycles`, default 2):
   a. Re-read modified file(s) fully
   b. Confirm all selected actions applied correctly
   c. Run regression checks:
      - Structural: YAML frontmatter valid, XML tags balanced, markdown structure intact
      - Semantic: no new contradictions introduced, content coherence preserved
      - Integration: fixes don't conflict with unchanged sections
      - Echo propagation: for each applied fix, search the full artifact for the same pattern (term, label, formulation) in other locations not covered by the fix. If the same pattern appears elsewhere, propagate the fix to all instances. Report each as a separate finding.
      - Example staleness: check if examples (quick_start, inline templates, code blocks) still demonstrate old behavior after a fix changed the underlying functionality. Distinct from echo — catches illustrative content using different wording.
      - Ordering disruption: if a fix restructured, merged, split, or reordered sections, trace the dependency chain — does every step/section still have its prerequisites before it? Especially relevant for step_contract and flow-dependent sections.
      - Reference integrity: verify all cross-references still resolve — section links, file paths in reference_index, IDs in addressable_output, line numbers in reports. Check both intra-file and inter-file (SKILL.md ↔ reference files) references.
      - Constraint satisfaction: re-read the artifact's own stated constraints (line limits, required sections, naming rules, format specifications) and verify each still holds after fixes.
   d. If issues found and cycle < max_cycles → fix issues, increment cycle, go to (a)
   e. If no issues found → report PASS with cycle count
   f. If max_cycles reached with issues remaining → report remaining issues in summary
</step_contract>

<decision_points>
**Common (both types):**
- **File cannot be read or doesn't exist** → Record as critical issue under "Dead code identification", continue with remaining files
- **Directive is ambiguous** → Include in Directive Extraction with concrete clarification suggestion

**Skill-specific:**
- **Skill exceeds 500 lines** → Propose split into references/, keep SKILL.md as executive workflow
- **Multiple skills reviewed** → Keep findings separated by skill name/path; do not merge directives across skills
- **Implicit contract between skills** → When reviewing a skill, check if its output format (headings, data shape, section structure) is consumed by other skills without explicit documentation. If found, flag as HIGH: undocumented contracts break silently when either side changes. Recommend documenting the contract in both skills.

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

**Execution Verification Gate:**
- **G8 Execution Gate:** If actions were executed (step 7), verify per cycle in step 8:
  - All selected action IDs successfully applied
  - Modified files readable and valid (check YAML frontmatter, XML tags)
  - No syntax breaks introduced
  - Changes match intended action descriptions
  - No regressions: applied fixes don't introduce new contradictions, break cross-references, or conflict with unchanged sections
  - Report verification results per cycle (cycle number, issues found, fixes applied)
  - Final status: PASS (all clean) or REMAINING_ISSUES (at max_cycles)

**Domain Correctness Application Gate:**
- **G9 Domain Correctness Gate:** After running Domain Correctness checks (D-112 through D-115), verify:
  - Each D-112 example pattern was explicitly cross-checked against the target artifact (report: "checked — applies/does not apply" per example)
  - D-113 was applied systematically: all output fields, table columns, user-facing text strings, and parse/find operations enumerated and checked for specification completeness
  - D-115 was applied across all classification/ordering/skip rules for logical preemption
  - If any D-112/D-113 example is not reported on → gate fails

**Reference File Analysis Gate:**
- **G10 Reference File Coverage Gate:** After step 2c (reference file content analysis), verify:
  - Every reference file was individually analyzed (enumerate: filename + result for each check)
  - File count analyzed must match total reference file count
  - Per-file results reported: tag validation, TOC accuracy (if applicable), intra-file contradictions
  - If any reference file was not analyzed → gate fails
  - If the artifact has no reference files, G10 is automatically satisfied

**Tag Validation Accuracy Gate:**
- **G11 Tag Boundary Gate:** No orphaned-tag finding referencing the last line of a file may appear in the report without independent verification (`wc -l` + `tail`). If any such unverified finding exists → gate fails.

Evidence capture guidance (for G2/G3):
- Capture file + line number for each directive and quoted evidence
- Quote the exact relevant lines in the report alongside file + line range
</quality_gates>

<lens>
Analyze findings from multiple perspectives:

- **Correctness lens:** Does this pattern/directive prevent errors or ambiguity?
  - Report: findings + severity (CRITICAL/HIGH/MEDIUM/LOW)

- **Integration lens:** Conflicts with existing structure? Duplicates content?
  - Report: findings + severity (CRITICAL/HIGH/MEDIUM/LOW)

- **ROI lens:** Does value (reliability gain) exceed overhead (token cost)?
  - Report: findings + severity (CRITICAL/HIGH/MEDIUM/LOW)

**Synthesis:** Merge findings across lenses, flag conflicts (e.g., high correctness but low ROI)

**Coverage requirement:** Each lens must report for critical issues and contradictions

**Report integration:** Lens findings are not a separate report section. Integrate them into Critical Issues and Contradictions Found sections, prefixing relevant findings with the lens label (e.g., "[ROI lens] This pattern adds ~200 tokens overhead for marginal reliability gain").
</lens>

<addressable_output>
Assign unique IDs to output items for follow-up reference:
- Format: `[{prefix}-{number}]`
- Prefixes: Action items = A, Issues = I, Recommendations = R
- Presentation: inline in report tables and action lists
- Usage: User can reference specific items ("apply A-1 and A-3" or "explain I-2")
</addressable_output>

<confidence_signal>
For consequential recommendations (moderate/breaking risk), include confidence assessment:

**Risk definitions:**
- **Moderate risk:** Changes existing behavior or structure but is reversible (e.g., restructuring sections, consolidating content, updating references)
- **Breaking risk:** Changes that could alter the artifact's core functionality or invalidate existing workflows (e.g., removing sections other artifacts depend on, changing execution order, modifying scope)

**Thresholds:**
- High (>85%): Recommendation is well-supported, low ambiguity
- Medium (60-85%): Recommendation is reasonable but has uncertainty
- Low (<60%): Recommendation needs user judgment

**Format in recommendations:**
- "confidence: high (90%) - {reason}"
- "confidence: medium (70%) - {reason}; would increase with {condition}"

**Apply to:**
- Actions with moderate/breaking risk (modifies existing behavior)
- Actions addressing CRITICAL/HIGH severity issues
- Structural changes that affect multiple sections

**What to include:**
- Percentage estimate
- Reason for confidence level
- What would increase confidence (if medium/low)

**Example:**
```
[A-1] [HIGH] Move workflow to SKILL.md from references/workflow.md
- confidence: high (90%) - workflow clearly belongs in main file per § Progressive Disclosure; references/workflow.md violates pattern
```
</confidence_signal>

<review_step>
After completing all steps, perform holistic validation:

**Criteria:**
- All steps in step_contract completed
- Report file(s) written to correct location(s)
- All quality gates (G1-G11) passed
- All identified issues tagged with severity ([CRITICAL], [HIGH], [MEDIUM], [LOW])
- Health score calculated correctly using severity_framework formula
- Chat summary includes: artifact type, file path, health score, action list
- If actions were selected: post-fix review-fix cycles completed (step 8) with results reported

**Max validation passes:** 2

**Per pass:**
1. Identify issues against criteria
2. Fix issues
3. Re-check criteria

**Exit conditions:**
- No issues found, OR
- Max passes reached

**If issues remain at max passes:** Report them in chat output and note in final summary

**Note:** These validation passes check the overall process output (report completeness, gate compliance). The post-fix review-fix cycles in step 8 (bounded by `max_cycles`) handle artifact-level regression detection separately.
</review_step>

<severity_framework>
Assign severity to every identified issue based on impact.

**Authoritative severity assignments** (see references/quality-standards-shared.md for details):

- **CRITICAL:** Blocks correctness/triggering or poses security risk (missing "when to use", dead references, 3+ duplications, XML in frontmatter, reserved name prefix)
- **HIGH:** Significantly degrades quality (>700 lines, <20 words description, no structure)  
- **MEDIUM:** Quality improvement needed (500-700 lines, <50/>200 words, missing TOC)
- **LOW:** Polish/consistency (voice issues, vague naming, minor redundancy)

**Health Score (evaluate in order, first match wins):** 2+ CRITICAL → Critical 🔴; 1 CRITICAL or 3+ HIGH → Needs Work 🟡; 0 CRITICAL + 1-2 HIGH → Good 🟢; 0 CRITICAL + 0 HIGH + 3+ MEDIUM → Good 🟢; 0 CRITICAL + 0 HIGH + 0-2 MEDIUM → Excellent ✨

**In reports:** Tag every issue with severity [CRITICAL], [HIGH], [MEDIUM], or [LOW]
</severity_framework>

<analysis_framework>
Load and apply checks systematically based on detected artifact type.

**Skills:**
- Shared: references/analysis-framework-shared.md (all 11 sections: Directive Extraction, Contradiction Detection, Temporal Analysis, Flow Mapping, Freedom Calibration, Dead Code Identification, Writing Voice, Context Efficiency, Domain Correctness, Model Agnosticism, Determinism Analysis)
- Skill-specific: references/skill-specific-checks.md (Structure Analysis, Trigger & Description Review, Cross-File Redundancy, Progressive Disclosure Check, Skill Category & MCP-Specific Checks)
- Standards: references/skill-quality-standards.md

**Prompts:**
- Shared: references/analysis-framework-shared.md (all 11 sections)
- Prompt-specific: references/prompt-specific-checks.md (P-01 to P-20)
- Standards: references/prompt-quality-standards.md

**All artifacts:** Apply references/quality-standards-shared.md (universal criteria)

**Check-to-report mapping:**

| Check(s) | Report Section |
|----------|---------------|
| Directive Extraction, Contradiction Detection | Critical Issues + Contradictions Found |
| Cross-File Redundancy, Context Efficiency | Redundancies |
| Temporal Analysis | Outdated Content |
| Flow Mapping, Freedom Calibration | Unclear Flows |
| Structure Analysis, Trigger & Description Review, Progressive Disclosure, Skill Category & MCP-Specific Checks | Type-Specific Issues |
| Dead Code Identification, Writing Voice, Model Agnosticism | Line-by-Line Issues |
| Domain Correctness | Critical Issues + Unclear Flows |
| Determinism Analysis | Type-Specific Issues + Recommendations |

Work through frameworks systematically, documenting findings per references/output-format.md structure.
</analysis_framework>

<stop_conditions>
**Done when:**
- Step Contract complete for all targets
- All targets analyzed through the appropriate framework
- Output artifacts + chat summary produced per `output_schema`
- All Quality Gates pass (G1-G11)
- Final Deliverable section produced
- User has been asked which actions to execute
- Selected actions have been executed and verified (if any)

**Don't:**
- Rewrite unrelated documentation
- Add new features or "nice-to-have" content not justified by review
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
