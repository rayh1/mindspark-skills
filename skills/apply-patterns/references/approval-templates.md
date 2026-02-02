# Approval Presentation Templates

Use these templates when presenting pattern proposals to users for approval.

---

## Standard Proposal Format

Use this format when proposing patterns for targets with identified gaps:

```
**Pattern Proposal - Gap Analysis**

Target: {target_path}
Type: {skill|prompt}
Complexity: {simple|moderate|complex}

**Gap Summary:**
| Tier | Present | Missing | N/A |
|------|---------|---------|-----|
| 1    | {n}     | {n}     | {n} |
| 2    | {n}     | {n}     | {n} |
| 3    | {n}     | {n}     | {n} |

**Critical Gaps** (blocks correctness/clarity)
| ID | Pattern | Tier | Rationale |
|----|---------|------|-----------|
| [P-1] | {name} | 1 | {why critical} |

**High-Value Improvements** (significant reliability gain)
| ID | Pattern | Tier | Rationale |
|----|---------|------|-----------|
| [P-3] | {name} | 1/2 | {why valuable} |

**Optional Enhancements** (nice to have)
| ID | Pattern | Tier | Rationale |
|----|---------|------|-----------|
| [P-5] | {name} | 2/3 | {lower priority benefit} |

⚠️ **Considerations:**
- Proposing {n} total patterns
- Estimated length increase: {percent}% (warn if >50%)
- Poor fits flagged: {list any patterns inappropriate for skill type}
- **Skills only:** Current line count: {n}/500 guideline {if approaching limit, suggest references/ offload}
- **Skills only:** Description field completeness: {complete with triggers | missing "when to use" info}

**Recommendation:** {Apply critical + high-value | Target already solid, optional only | etc.}

[approve all] [select subset: P-1, P-3...] [analysis only]
```

---

## Solid Target Format

Use this format when analyzing targets that already have good pattern coverage:

```
**Pattern Analysis - Target Quality Assessment**

Target: {target_path}
Type: {skill|prompt}

✓ **Existing Coverage:**
- {List patterns already present}

**Gap Analysis:**
No critical gaps detected. Target demonstrates:
- {strength 1}
- {strength 2}

**Optional Improvements** (low priority)
| ID | Pattern | Tier | Rationale |
|----|---------|------|-----------|
| [P-1] | {name} | 2 | {minor benefit} |

**Recommendation:** No changes needed. Target is well-structured.

[apply optional] [analysis only]
```

---

## Usage Notes

**When to use Standard vs Solid format:**
- **Standard:** Target has 2+ gaps, especially in Tier 1
- **Solid:** Target has 0-1 gaps, mostly Tier 2/3 optional improvements

**Presentation guidelines:**
- Always show gap summary table for transparency
- Categorize recommendations by impact (Critical/High-Value/Optional)
- Include ROI rationale for each proposed pattern
- Warn if file length increase would exceed 50%
- For skills: highlight description completeness and line count status

**On analysis only:** produce analysis output; do not modify files.
