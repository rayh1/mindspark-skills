# Evaluation Framework

## Table of Contents

- [The 7 Dimensions](#the-7-dimensions-0-2-points-each) - Lines 15-95
- [Scoring Interpretation](#scoring-interpretation) - Lines 97-103
- [The Acid Test](#the-acid-test) - Lines 105-112

---

## The 7 Dimensions (0-2 points each)

### D-1: Knowledge Gap Test
**Question:** Does the skill contain specialized knowledge Claude doesn't have?

| Score | Criteria |
|-------|----------|
| 2 | Domain-specific databases, proprietary frameworks, niche methodologies, current data beyond training cutoff |
| 1 | Synthesizes existing knowledge in a novel way |
| 0 | General knowledge already available to Claude |

**Enhancement connection:** V-5 (Domain Depth) improvements typically increase this score.

---

### D-2: Structural Process Test
**Question:** Does the skill provide a systematic, repeatable process Claude would inconsistently apply?

| Score | Criteria |
|-------|----------|
| 2 | Multi-step workflows with specific sequences, mandatory checkpoints, enforced order |
| 1 | Provides structure that improves consistency |
| 0 | Simple prompts that don't add systematic rigor |

**Enhancement connection:** V-3 (Intelligence Layer) improvements often increase this score.

---

### D-3: Tool Integration Test
**Question:** Does the skill orchestrate tools or external systems Claude can't access directly?

| Score | Criteria |
|-------|----------|
| 2 | MCP integrations, API calls, file system operations, database queries |
| 1 | Could integrate with tools but basic version works without them |
| 0 | Pure reasoning/analysis with no tool orchestration |

**Enhancement connection:** V-2 (Tool Integration) improvements directly increase this score.

---

### D-4: Consistency & Guardrails Test
**Question:** Does the skill enforce discipline Claude might skip under time pressure or ambiguity?

| Score | Criteria |
|-------|----------|
| 2 | Mandatory steps, error checking, quality gates, "don't skip this" enforcement |
| 1 | Provides helpful structure but not strictly enforced |
| 0 | Suggestions Claude would naturally make anyway |

**Enhancement connection:** V-3 (Intelligence Layer) with validation/guardrails increases this score.

---

### D-5: Complexity Management Test
**Question:** Does the skill manage complexity that overwhelms typical Claude context/attention?

| Score | Criteria |
|-------|----------|
| 2 | Multi-stage workflows, state tracking, parallel analyses, synthesis of 5+ factors |
| 1 | Manages moderate complexity helpfully |
| 0 | Simple one-shot analysis |

**Enhancement connection:** V-1 (Input/Output) batch processing and V-4 (UX) progress tracking increase this score.

---

### D-6: User Experience Test
**Question:** Does the skill significantly reduce friction or cognitive load for the user?

| Score | Criteria |
|-------|----------|
| 2 | Transforms 10+ message conversation into 1-2 message invocation |
| 1 | Saves 3-5 messages or provides clearer interface |
| 0 | Minimal UX improvement (saves only 1-2 messages) |

**Enhancement connection:** V-4 (User Experience) improvements directly increase this score.

---

### D-7: Specialization Depth Test
**Question:** Does the skill go significantly deeper than Claude's general knowledge?

| Score | Criteria |
|-------|----------|
| 2 | 10x more detail, expert-level execution, domain mastery |
| 1 | Noticeably deeper than casual Claude response |
| 0 | Same depth as general Claude knowledge |

**Enhancement connection:** V-5 (Domain Depth) improvements directly increase this score.

---

## Scoring Interpretation

| Score Range | Verdict | Meaning |
|-------------|---------|---------|
| 10-14 | Build | Significant value add - build this skill |
| 6-9 | Consider | Moderate value - consider if strategic priority |
| 0-5 | Skip | Likely redundant - don't build |

---

## The Acid Test

Simple question: **"Could I get 80% of this value by just asking Claude directly?"**

- **YES** → Likely redundant (lean toward Skip)
- **NO** → Genuinely new capability (lean toward Build)

