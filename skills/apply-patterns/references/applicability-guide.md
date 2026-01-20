# Pattern Applicability Guide (Tier-Aware)

This reference helps determine which patterns are most applicable for different types of targets, organized by tier priority.

## Type Detection

- If file starts with `---` and contains `name:` and `description:` → type = "skill"
- Else → type = "prompt"

---

## Tier 1 - Core (Always Consider)

These patterns apply to most non-trivial targets. Check all 8.

| Pattern | Applies when | Skip when |
|---------|--------------|-----------|
| **Inputs First** | Target takes inputs | No inputs; trivially simple |
| **Step Contract** | Multi-step work | Single-step task |
| **Decision Points** | Conditional logic; branching | Linear workflow |
| **Quality Gates** | Correctness matters; errors propagate | Low-stakes; easily reversible |
| **Stop Conditions** | Scope could creep; open-ended | Tightly scoped already |
| **Scope Fence** | Could expand beyond intent; sensitive areas | Boundaries already clear |
| **Interpretation Check** | Complex/ambiguous requests; high-stakes | Trivially clear task |
| **Output Schema** | Output needs parsing; consistency matters | Freeform output acceptable |

**Lite mode:** Pick 2-4 from this tier based on target characteristics.

---

## Tier 2 - Recommended (When Situation Warrants)

Add these based on specific situational triggers.

| Pattern | Applies when | Skip when |
|---------|--------------|-----------|
| **User Approval Gate** | Irreversible actions; significant phases; low confidence on high-impact | All actions reversible; user trusts autonomous operation |
| **Clarifying Questions** | Blocked by missing info; consequential decisions | User unavailable; info in context; safe defaults exist |
| **Confidence Signal** | Consequential decisions; human judgment valuable | Low-stakes; high confidence throughout |
| **Fallback Chain** | External dependencies (APIs, tools, files) | No external deps; failure = stop |
| **Lens** | Review/analysis task; tunnel vision risk; multiple concerns | Single-concern task; one perspective dominates |
| **Addressable Output** | Output has discrete items; follow-up expected | Single-item output; no follow-up |
| **Review Step** | Complex outputs needing coherence; multi-step assembly | Simple output; inline gates sufficient |
| **Step Ledger** | Long/complex enough to drift; audit trail needed | Short task; few steps |

**Standard mode:** Consider all Tier 1 + Tier 2. Propose 3-6 recommended, 0-3 optional.

---

## Tier 3 - Situational (Require Justification)

Use only when specifically relevant. Justify ROI.

| Pattern | Applies when | Skip when |
|---------|--------------|-----------|
| **Mode Selection** | Multiple workflows supported; user preference matters | Single workflow; obvious from request |
| **Example Anchor** | Style/format hard to describe; match existing patterns | Format easily described; no style concerns |
| **Assumption Registry** | Auditability matters; "why did it do that?" needs answering | Low-stakes; assumptions obvious |
| **Context Window** | Very long tasks; shifting focus; stale context risk | Short task; stable focus |
| **Observability** | Debugging/comparison needed; reproducibility matters | One-off task; no replay needed |
| **Error Handling** | Failures need taxonomy; sensitive data in logs | Simple errors; no sensitive data |

**Full mode:** Audit all 22 patterns. Justify Tier 3 additions.

---

## Mode-Based Selection Flow

### Lite Mode (default)

```
1. Check all Tier 1 patterns
2. Mark applicable: yes/no
3. Rank by impact for this target
4. Propose top 2-4
```

### Standard Mode

```
1. Check all Tier 1 patterns → mark applicable
2. Check all Tier 2 patterns → mark applicable
3. Tier 1 applicable = recommended
4. Tier 2 applicable = recommended or optional (rank by ROI)
5. Caps: 3-6 recommended, 0-3 optional; hard cap 8 total unless user requests more
```

### Full Mode

```
1. Audit all 22 patterns
2. For each: present / partial / missing / N/A
3. Categorize applicable as: must-have / should-have / optional
4. Tier 3 patterns require explicit ROI note
```

---

## Applicability by Target Type

### Skills (YAML frontmatter)

**Almost always applicable:**
- Inputs First (skills always have inputs)
- Step Contract (skills are multi-step)
- Output Schema (skills produce structured output)
- User Approval Gate (skills often modify files)

**Often applicable:**
- Scope Fence (skills should have clear boundaries)
- Quality Gates (skill outputs need validation)
- Stop Conditions (prevent scope creep)

**Sometimes applicable:**
- Mode Selection (if skill has multiple workflows)
- Lens (if skill does analysis/review)

### Raw Prompts

**Almost always applicable:**
- Inputs First
- Step Contract
- Stop Conditions

**Often applicable:**
- Decision Points (prompts often have branching)
- Quality Gates (if correctness matters)
- Output Schema (if output is consumed)

**Sometimes applicable:**
- Scope Fence (if prompt is open-ended)
- Review Step (if output coherence matters)

---

## Partial Match Handling

When a pattern partially exists in the target:

| Situation | Action |
|-----------|--------|
| Equivalent exists (different name) | Normalize to canonical form if low-risk |
| Partial implementation | Offer to upgrade: "Strengthen to full pattern?" |
| Content scattered | Consolidate into canonical section |
| Stronger version exists | Keep it; don't downgrade |

---

## Conflict Resolution

When patterns seem to conflict:

| Conflict | Resolution |
|----------|------------|
| Interpretation Check + Mode Selection | Mode Selection first (what workflow), then Interpretation Check (did I understand within that workflow) |
| Quality Gates + Review Step | Gates during work (inline); Review Step after completion (holistic) |
| Stop Conditions + Scope Fence | Scope Fence = spatial boundaries; Stop Conditions = completion criteria. Both apply. |
| Clarifying Questions + Confidence Signal | Low confidence triggers asking; if can't ask, document assumption |

---

## Quick Decision Matrix

For fast pattern selection, answer these questions:

| Question | If Yes → Pattern |
|----------|-----------------|
| Takes inputs? | Inputs First |
| Multi-step? | Step Contract |
| Could creep? | Stop Conditions + Scope Fence |
| Output consumed? | Output Schema |
| Irreversible actions? | User Approval Gate |
| Analysis task? | Lens |
| External deps? | Fallback Chain |
| Follow-up expected? | Addressable Output |
| Multiple workflows? | Mode Selection |
| Need debugging? | Observability |
