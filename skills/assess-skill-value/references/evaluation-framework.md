# Evaluation Framework

**The 7 Dimensions (0-2 points each)**

## 1. Knowledge Gap Test
**Question:** Does the skill contain specialized knowledge the Claude doesn't have?
- **2 points:** Domain-specific databases, proprietary frameworks, niche methodologies, current data beyond training cutoff
- **1 point:** Synthesizes existing knowledge in a novel way
- **0 points:** General knowledge already available to Claude

## 2. Structural Process Test
**Question:** Does the skill provide a systematic, repeatable process the Claude would inconsistently apply?
- **2 points:** Multi-step workflows with specific sequences, mandatory checkpoints, enforced order
- **1 point:** Provides structure that improves consistency
- **0 points:** Simple prompts that don't add systematic rigor

## 3. Tool Integration Test
**Question:** Does the skill orchestrate tools or external systems the Claude can't access directly?
- **2 points:** MCP integrations, API calls, file system operations, database queries
- **1 point:** Could integrate with tools but basic version works without them
- **0 points:** Pure reasoning/analysis with no tool orchestration

## 4. Consistency & Guardrails Test
**Question:** Does the skill enforce discipline the Claude might skip under time pressure or ambiguity?
- **2 points:** Mandatory steps, error checking, quality gates, "don't skip this" enforcement
- **1 point:** Provides helpful structure but not strictly enforced
- **0 points:** Suggestions the Claude would naturally make anyway

## 5. Complexity Management Test
**Question:** Does the skill manage complexity that overwhelms typical Claude context/attention?
- **2 points:** Multi-stage workflows, state tracking, parallel analyses, synthesis of 5+ factors
- **1 point:** Manages moderate complexity helpfully
- **0 points:** Simple one-shot analysis

## 6. User Experience Test
**Question:** Does the skill significantly reduce friction or cognitive load for the user?
- **2 points:** Transforms 10+ message conversation into 1-2 message invocation
- **1 point:** Saves 3-5 messages or provides clearer interface
- **0 points:** Minimal UX improvement (saves only 1-2 messages)

## 7. Specialization Depth Test
**Question:** Does the skill go significantly deeper than Claude's general knowledge?
- **2 points:** 10x more detail, expert-level execution, domain mastery
- **1 point:** Noticeably deeper than casual Claude response
- **0 points:** Same depth as general Claude knowledge

## Scoring Interpretation

- **10-14 points:** Build this skill - significant value add
- **6-9 points:** Moderate value - consider if strategic
- **0-5 points:** Likely redundant - skip

## Evaluation Guidelines

**Evaluation Tone:**
Be brutally honest. Users benefit more from candid assessment now than discovering redundancy after building. If a skill is marginal, say so clearly.

**The Acid Test:**
Simple question: "Could I get 80% of this value by just asking Claude directly?"
- If YES → Likely redundant
- If NO → Genuinely new capability