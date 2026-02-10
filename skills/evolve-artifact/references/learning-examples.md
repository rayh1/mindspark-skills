# Learning Examples

**Purpose:** Example learning formulations showing the difference between actionable, evidence-based learnings (GOOD) and vague or generic observations (BAD). Used when formulating learnings during evolution analysis.

---

## GOOD Learnings

### For Skills

```
[L-1] Skill description is 15 words, missing "when to use" triggers | evidence: user asked "when should I use this?" | severity: CRITICAL
  → [R-1] Expand description to 60-150 words with enumerated triggers | addresses: L-1 | impact: high | risk: safe | standard: Description Field Excellence

[L-2] SKILL.md is 650 lines with embedded examples | evidence: file size check | severity: MAJOR
  → [R-2] Move examples to references/examples.md, keep workflow in SKILL.md | addresses: L-2 | impact: medium | risk: moderate | standard: Progressive Disclosure

[L-3] References dead file scripts/validator.py | evidence: line 45, file not found | severity: CRITICAL
  → [R-3] Remove reference or create missing file | addresses: L-3 | impact: high | risk: safe | standard: Resource Organization
```

### For Prompts

```
[L-1] Prompt has three H1 headings (lines 1, 23, 67) | evidence: file read | severity: MAJOR
  → [R-1] Change lines 23, 67 to H2, keep only line 1 as H1 | addresses: L-1 | impact: high | risk: safe | standard: Heading Structure

[L-2] Output format appears after examples (line 234) | evidence: section order | severity: MINOR
  → [R-2] Move Output Format section before Examples section | addresses: L-2 | impact: low | risk: safe | standard: Section Order

[L-3] Prompt is 547 lines with redundant explanations | evidence: file size + repeated content | severity: HIGH
  → [R-3] Consolidate redundant sections, target <500 lines | addresses: L-3 | impact: high | risk: moderate | standard: Length Limits
```

### For Patterns (both skills and prompts)

```
[L-1] Skill failed at step 4 without detecting error | evidence: user reported "wrong output, no warning" | severity: CRITICAL
  → [R-1] Add <quality_gates> section with validation after step 4 | addresses: L-1 | impact: high | risk: safe | standard: Quality Gates (Tier 1)

[L-2] User asked "what inputs does this need?" during execution | evidence: line 45 in conversation | severity: HIGH
  → [R-2] Add/expand <inputs_first> section with required/optional inputs | addresses: L-2 | impact: high | risk: safe | standard: Inputs First (Tier 1)

[L-3] Skill kept processing after goal achieved | evidence: "generated 20 items when I only needed 5" | severity: MAJOR
  → [R-3] Add <stop_conditions> section defining completion criteria | addresses: L-3 | impact: medium | risk: safe | standard: Stop Conditions (Tier 1)

[L-4] Made destructive change without asking permission | evidence: "deleted files without warning" | severity: CRITICAL
  → [R-4] Add <user_approval_gate> before irreversible actions | addresses: L-4 | impact: high | risk: safe | standard: User Approval Gate (Tier 2)
```

---

## BAD Learnings (filter out)

```
[L-X] The artifact ran successfully ❌ (too vague, not actionable)
[L-X] User seemed happy with results ❌ (opinion-based, no evidence)
[L-X] Could add more comments ❌ (not artifact-specific, generic)
[L-X] Python version was 3.11 ❌ (tangential observation)
```

Only include learnings that match GOOD pattern.
