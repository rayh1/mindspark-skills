# examine-skill

Analyze skill files for problems, contradictions, redundancies, outdated content, and structural issues. Produces detailed examination reports to guide improvements.

## 🎯 What It Does

Runs a comprehensive 10-part analysis on your skill files:
1. **Structural Issues** - Missing sections, poor organization
2. **Contradictions** - Conflicting instructions or information
3. **Redundancies** - Repeated content that should be consolidated
4. **Outdated Content** - References to deprecated patterns or old approaches
5. **Scope Problems** - Unclear or poorly defined boundaries
6. **Input/Output Issues** - Missing or ambiguous specifications
7. **Step Logic Problems** - Unclear, unordered, or missing steps
8. **Documentation Gaps** - Missing examples or unclear guidance
9. **Reference Issues** - Broken links, missing files
10. **Improvement Opportunities** - Actionable suggestions for enhancement

## 🚀 Quick Start

```bash
# Examine a single skill
examine-skill path/to/SKILL.md

# Examine all skills in a folder
examine-skill path/to/skills-folder/
```

## 📋 Output

### File Output
Writes detailed report to: `{skill-directory}/EXAMINATION-REPORT.md`

### Chat Output (Brief)
- Confirmation that examination is complete
- Report file path
- **Top 3 most urgent issues** (one line each)
- Prompt to read full report for details

## 📊 Report Structure

```markdown
# Examination Report: {skill-name}

## Executive Summary
- Overall assessment
- Critical issues count
- Priority recommendations

## Part 1: Structural Issues
[Findings...]

## Part 2: Contradictions
[Findings...]

... (10 parts total)

## Action Items
Prioritized list of fixes

## Improvement Outline
Step-by-step enhancement plan
```

## 💡 Example Usage

### Input
```bash
examine-skill ./my-skill/SKILL.md
```

### Chat Output
```
✓ Examination complete

Report: ./my-skill/EXAMINATION-REPORT.md

Top 3 issues:
1. [CRITICAL] Missing output contract in SKILL.md
2. [HIGH] Contradictory instructions in steps 3 and 7
3. [MEDIUM] Reference to deprecated pattern "Error Codes"

Read the full report for details and action items.
```

### Report Contents (Excerpt)
```markdown
## Part 1: Structural Issues

### Missing Sections
- No <output_contract> section found
- <scope_fence> present but out of scope section is empty

### Organization Problems
- Steps appear before inputs (recommend inputs → output → steps order)
- Quality gates scattered throughout, should be consolidated

## Part 2: Contradictions

### Instruction Conflicts
- Step 3 says "always validate inputs"
- Step 7 says "skip validation if mode=fast"
- **Impact:** Unclear when validation occurs
- **Recommendation:** Explicit validation rules per mode

...
```

## 🔍 What It Checks

### In SKILL.md
- Presence of essential sections
- YAML frontmatter correctness
- Instruction clarity and consistency
- Step logic and ordering
- Input/output specifications
- Scope definitions
- Error handling instructions

### In references/
- File existence and accessibility
- Content relevance and accuracy
- Broken internal links
- Outdated examples or patterns
- Duplicate information

### In workflows/ (if present)
- Workflow completeness
- Step-by-step clarity
- Integration with main skill
- Examples and edge cases

### Cross-File
- Reference consistency
- Version alignment
- Pattern usage correctness
- Documentation coverage

## ✅ Best Practices

- **Run before publishing** - Catch issues early
- **Run after major changes** - Ensure consistency
- **Read the full report** - Chat summary shows only top 3 issues
- **Prioritize by severity** - Start with CRITICAL and HIGH
- **Use with apply-patterns** - Fix issues, then add reliability patterns

## 🔧 Understanding Severity

| Severity | Meaning | Action |
|----------|---------|--------|
| **CRITICAL** | Blocks basic functionality | Fix immediately |
| **HIGH** | Causes significant problems | Fix before publication |
| **MEDIUM** | Reduces quality or clarity | Fix when possible |
| **LOW** | Minor improvements | Nice to have |
| **INFO** | Suggestions only | Optional |

## 🎓 Common Issues Found

### Structural
- Missing output contracts
- Steps before inputs
- No scope fence
- Incomplete error handling

### Contradictions
- Conflicting mode instructions
- Incompatible pattern combinations
- Steps that contradict inputs

### Redundancies
- Same instructions in multiple places
- Duplicated reference content
- Repeated examples

### Outdated
- Old pattern names
- Deprecated tool references
- Obsolete examples

## 🔍 When To Use

✅ **Use examine-skill when:**
- Preparing skills for publication
- After major refactoring
- Skill behavior seems inconsistent
- Before applying patterns (clean first)
- Conducting quality audits
- Onboarding to unfamiliar skills

❌ **Don't use when:**
- Skill is brand new (nothing to examine)
- You need functional testing (this checks structure, not execution)
- Issues are already obvious (just fix them)

## 📚 Documentation

- **[output-format.md](references/output-format.md)** - Report format specification

## 🔗 Related Skills

- **[build-with-patterns](../build-with-patterns/)** - Build new skills properly from start
- **[apply-patterns](../apply-patterns/)** - Add patterns after fixing issues

## 🎯 Workflow Integration

### Recommended Flow
```
1. Build skill (build-with-patterns)
   ↓
2. Examine for issues (examine-skill) ← You are here
   ↓
3. Fix critical/high issues
   ↓
4. Apply patterns (apply-patterns)
   ↓
5. Final examination
   ↓
6. Publish
```

### Quality Gate
Use examination results as a quality gate:
- **0 CRITICAL issues** → Ready for pattern application
- **0 HIGH issues** → Ready for internal use
- **0 MEDIUM issues** → Ready for publication

## 📄 License

MIT License - see [LICENSE](../../LICENSE) file for details

---

**Version:** 1.0.0  
**Part of:** [Mindspark Skills](../README.md)
