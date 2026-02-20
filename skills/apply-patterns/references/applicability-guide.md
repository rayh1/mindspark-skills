# Pattern Applicability Guide (Tier-Aware)

This reference helps determine which patterns are most applicable for different types of targets, organized by tier priority.

## Table of Contents

- [Type Detection](#type-detection) - Line 14
- [Tier 1 - Core (Always Consider)](#tier-1---core-always-consider) - Line 21
- [Tier 2 - Recommended (When Situation Warrants)](#tier-2---recommended-when-situation-warrants) - Line 40
- [Tier 3 - Situational (Require Justification)](#tier-3---situational-require-justification) - Line 59
- [Gap-Driven Selection Flow](#gap-driven-selection-flow) - Line 76
- [Applicability by Target Type](#applicability-by-target-type) - Line 154
- [Partial Match Handling](#partial-match-handling) - Line 206
- [Conflict Resolution](#conflict-resolution) - Line 219
- [Quick Decision Matrix](#quick-decision-matrix) - Line 232

---

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

**For simple targets:** Focus on critical gaps from this tier (typically 2-4 patterns).

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

**For moderate targets:** Consider patterns from both Tier 1 and Tier 2 based on detected gaps (typically 4-6 patterns).

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

**For complex targets:** Audit all tiers; Tier 3 patterns require explicit ROI justification (typically 6-8 patterns max).

---

## Gap-Driven Selection Flow

### Step 1: Detect Existing Patterns

```
1. Read target file
2. Detect patterns already present:
   - Check for Tier 1 pattern tags/sections
   - Check for Tier 2 pattern tags/sections
   - Check for Tier 3 pattern tags/sections
3. For each detected pattern, classify as:
   - Present: Pattern fully implemented
   - Partial: Pattern exists but incomplete
   - Missing: Pattern not present
```

### Step 2: Assess Target Complexity

```
1. Measure indicators:
   - Line count: <100 (simple), 100-300 (moderate), 300+ (complex)
   - Step count: single-step (simple), 2-5 steps (moderate), 6+ steps (complex)
   - Branching: linear (simple), some branches (moderate), extensive branching (complex)
2. Classify target: simple | moderate | complex
```

### Step 3: Identify Gap Severity

For each missing pattern, assess impact:

```
**Critical gaps** (blocks correctness/clarity):
- Missing Inputs First when target takes inputs
- Missing Step Contract when multi-step workflow
- Missing Quality Gates when errors propagate
- Missing Stop Conditions when scope unbounded

**High-value gaps** (significant reliability gain):
- Tier 1 patterns applicable but missing
- Tier 2 patterns with clear ROI for this target
- Patterns that prevent common failure modes

**Nice-to-have gaps** (incremental benefit):
- Tier 2 patterns with moderate ROI
- Tier 3 patterns with specific justification
- Enhancements to solid targets
```

### Step 4: Propose Based on Gaps + Complexity

```
Simple targets with few gaps:
  → Propose 2-4 critical patterns (Tier 1 focus)

Moderate targets with several gaps:
  → Propose 4-6 patterns (Tier 1 + applicable Tier 2)
  → Prioritize: critical → high-value

Complex targets with many gaps:
  → Propose 6-8 patterns, strictly prioritized
  → Include Tier 3 only with explicit ROI justification

Solid targets with minimal gaps:
  → Acknowledge quality ("94% pattern coverage")
  → Suggest 0-2 optional improvements
  → "No critical gaps found" is valid outcome
```

### Step 5: Present Gap Analysis to User

Show:
- Gap summary table (present/missing by tier)
- Prioritized recommendations: critical → high-value → nice-to-have
- ROI rationale for each proposed pattern
- Estimated file length increase

---

## Applicability by Target Type

### Skills (YAML frontmatter)

**Skills use progressive disclosure:**
- Metadata (name + description) always in context (~100 words)
- SKILL.md body loaded when triggered (<5k words, <500 lines guideline)
- Bundled resources (scripts/, references/, assets/) loaded as needed

**When proposing patterns for skills that orchestrate MCP:**
- If the skill's core value depends on MCP server access, check for MCP error handling: connection failures, auth failures, case-sensitive tool names
- Propose S-3 (MCP Error Handling) from pattern-catalog.md if missing — this is the primary failure mode for MCP skills

**When proposing patterns for skills:**
- Check current line count against 500-line guideline
- If approaching limit, recommend moving detailed content to references/
- Prefer concise additions; skills compete for context with other system components
- Avoid duplication between SKILL.md and references/

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
