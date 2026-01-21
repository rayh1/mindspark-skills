---
name: assess-skill-value
description: Evaluates whether a proposed skill is worth building using a 7-dimension scoring framework (0-14 points). Provides build/skip recommendation and improvement suggestions. Use when planning to create a new skill, questioning if a skill idea adds value, or auditing existing skills for redundancy.
---

<objective>
Determine if a skill adds genuine new capabilities to Claude or is redundant with existing LLM knowledge. Apply systematic evaluation to prevent building superfluous skills. **Be brutally honest** in assessments—it's better to discourage a weak skill than waste effort on redundant capabilities.
</objective>

<quick_start>
**Input:** Skill description, skill name, or path to SKILL.md file
**Output:** Evaluation report with scoring table, verdict, and improvement suggestions

**Example invocations:**
- `assess-skill-value` → will ask for skill input
- `assess-skill-value "A skill that helps users create morning routines"`
- `assess-skill-value pdf`
- `assess-skill-value /workspace/.claude/skills/tarot-card-generator/SKILL.md`
</quick_start>

<inputs_first>
**Required inputs:**
- `skill_input`: Either:
  - Text description of a proposed skill (2+ sentences)
  - Name of an existing skill (e.g., "pdf", "commit")
  - Path to a SKILL.md file

**Optional inputs:**
- None

**Validation:**
- `skill_input` must be non-empty
- If text description: must be at least 20 characters
- If path: file must exist and be readable

**Missing input handling:**
- If `skill_input` missing → ask user: "Please provide either a skill description, skill name, or path to a SKILL.md file"
- If description too vague (< 20 chars or unclear) → ask user for clarification

**Input type detection:**
- If contains ".md" or "/" → treat as file path
- If single word or hyphenated → treat as skill name
- Otherwise → treat as text description
</inputs_first>

<step_contract>
1. **Parse input** → Identify input type (description/name/path) and extract/load skill content
2. **Validate completeness** (if description) → Reformulate understanding, present to user, wait for confirmation
3. **Evaluate all 7 dimensions** → Assign 0/1/2 score + rationale for each dimension
4. **Calculate total & interpret** → Sum scores (0-14), determine verdict (Build/Consider/Skip)
5. **Generate improvements** (if applicable) → Identify concrete suggestions to increase value
6. **Compile report** → Produce final output with all required sections
7. **Review** → Check consistency, arithmetic, alignment before delivery
</step_contract>

<decision_points>
**D1: Input type routing (after Step 1)**
- If input contains "/" or ".md" → treat as file path, read file with Read tool
- Else if single word or hyphenated → treat as skill name, search in `.claude/skills/[name]/SKILL.md`
- Else → treat as text description

**D2: Completeness validation (after Step 1)**
- If text description AND length < 20 chars → ask user for more detail
- Else if text description AND sufficient → reformulate understanding, show to user, wait for confirmation before proceeding
- Else (existing skill file/name) → skip reformulation, proceed directly to Step 3

**D3: Improvement section inclusion (after Step 5)**
- If improvement suggestions generated → include "Improvement Suggestions" section
- Else → omit section entirely
</decision_points>

<quality_gates>
**G1 (after Step 3 - Evaluate dimensions):**
- All 7 dimensions must be scored
- Each score must be exactly 0, 1, or 2
- Each dimension must have a rationale
- If fail → Re-evaluate missing/invalid dimensions

**G2 (after Step 4 - Calculate total):**
- Total score must equal sum of 7 individual scores
- Verdict must match interpretation rules (10-14=Build, 6-9=Consider, 0-5=Skip)
- If fail → Recalculate

**G3 (after Step 6 - Compile report):**
- All required sections present
- Scoring table includes all 7 dimensions
- If fail → Add missing sections
</quality_gates>

<scope_fence>
**In scope:**
- Reading and analyzing the target skill/prompt description
- Evaluating against 7 dimensions using the framework
- Generating evaluation report with scores and recommendations
- Proposing improvement suggestions

**Out of scope:**
- Implementing or building the assessed skill
- Modifying any files except generating the evaluation report
- Comparing to other skills in the ecosystem
- Making strategic decisions about whether user should build it

**If boundary crossed:** Note the desire, but return to pure evaluation mode.
</scope_fence>

<interpretation_check>
**For text descriptions only:**
- Reformulate understanding in own words
- Present inferred name, purpose, and likely features
- Ask: "Is this understanding correct?"
- Wait for confirmation before proceeding to evaluation
- If corrected: update understanding and re-confirm if significant

**For existing skills/files:**
- Skip explicit check (content is unambiguous)
- Proceed directly to evaluation
</interpretation_check>

<evaluation_framework>
**The 7 Dimensions (0-2 points each)**

**1. Knowledge Gap Test**
**Question:** Does the skill contain specialized knowledge the LLM doesn't have?
- **2 points:** Domain-specific databases, proprietary frameworks, niche methodologies, current data beyond training cutoff
- **1 point:** Synthesizes existing knowledge in a novel way
- **0 points:** General knowledge already available to LLM

**2. Structural Process Test**
**Question:** Does the skill provide a systematic, repeatable process the LLM would inconsistently apply?
- **2 points:** Multi-step workflows with specific sequences, mandatory checkpoints, enforced order
- **1 point:** Provides structure that improves consistency
- **0 points:** Simple prompts that don't add systematic rigor

**3. Tool Integration Test**
**Question:** Does the skill orchestrate tools or external systems the LLM can't access directly?
- **2 points:** MCP integrations, API calls, file system operations, database queries
- **1 point:** Could integrate with tools but basic version works without them
- **0 points:** Pure reasoning/analysis with no tool orchestration

**4. Consistency & Guardrails Test**
**Question:** Does the skill enforce discipline the LLM might skip under time pressure or ambiguity?
- **2 points:** Mandatory steps, error checking, quality gates, "don't skip this" enforcement
- **1 point:** Provides helpful structure but not strictly enforced
- **0 points:** Suggestions the LLM would naturally make anyway

**5. Complexity Management Test**
**Question:** Does the skill manage complexity that overwhelms typical LLM context/attention?
- **2 points:** Multi-stage workflows, state tracking, parallel analyses, synthesis of 5+ factors
- **1 point:** Manages moderate complexity helpfully
- **0 points:** Simple one-shot analysis

**6. User Experience Test**
**Question:** Does the skill significantly reduce friction or cognitive load for the user?
- **2 points:** Transforms 10+ message conversation into 1-2 message invocation
- **1 point:** Saves 3-5 messages or provides clearer interface
- **0 points:** Minimal UX improvement (saves only 1-2 messages)

**7. Specialization Depth Test**
**Question:** Does the skill go significantly deeper than LLM's general knowledge?
- **2 points:** 10x more detail, expert-level execution, domain mastery
- **1 point:** Noticeably deeper than casual LLM response
- **0 points:** Same depth as general LLM knowledge

**Scoring Interpretation:**
- **10-14 points:** Build this skill - significant value add
- **6-9 points:** Moderate value - consider if strategic
- **0-5 points:** Likely redundant - skip

**Evaluation Tone:**
Be brutally honest. Users benefit more from candid assessment now than discovering redundancy after building. If a skill is marginal, say so clearly.

**The Acid Test:**
Simple question: "Could I get 80% of this value by just asking Claude directly?"
- If YES → Likely redundant
- If NO → Genuinely new capability
</evaluation_framework>

<confidence_signal>
**When scoring each dimension:**
- If score is clear and unambiguous (e.g., skill explicitly integrates MCP tools) → proceed with confidence
- If score requires judgment or interpretation (e.g., "does this *really* add structure?") → note in rationale with reasoning

**Thresholds:**
- High confidence (>85%): Proceed with score
- Medium confidence (60-85%): Proceed but explain reasoning clearly in rationale
- Low confidence (<60%): Note uncertainty in rationale; consider suggesting user validate assumption

**Include in rationale:** What evidence supports the score, and what would change the assessment
</confidence_signal>

<review_step>
**After Step 6 (Compile report), before delivery:**

Check for:
- **Arithmetic:** Total score equals sum of 7 dimensions
- **Alignment:** Verdict matches score range (10-14=Build, 6-9=Consider, 0-5=Skip)
- **Consistency:** Rationales don't contradict each other
- **Completeness:** All required sections present
- **Quality:** Each rationale explains the "why" not just repeats the score

**Max cycles:** 1
**If issues found:** Fix once and re-check. If still failing, note limitation in output.
</review_step>

<output_schema>
**Format:** Conversation output (markdown formatted)

**Required sections (in order):**
1. **Skill Understanding** (only for text descriptions; separate interaction, wait for confirmation)
2. **Scoring Table** (all 7 dimensions with scores and rationales)
3. **Total Score** (sum of all dimensions, 0-14)
4. **Interpretation & Verdict** (Build ✅ / Consider ⚠️ / Skip ❌)
5. **Recommendation** (clear guidance)
6. **Improvement Suggestions** (optional - only if suggestions exist)
7. **Strategic Considerations** (meta-value, ecosystem fit)

**Forbidden sections:**
- Comparison to existing skills
- Future roadmap
- Implementation details

**Validation:**
- All 7 dimensions evaluated
- Each dimension scored 0, 1, or 2
- Total equals sum of dimensions
- Verdict matches score interpretation
</output_schema>

<improvement_generation>
When generating improvement suggestions, identify what's missing:

**Common improvements:**
- **Add tool integration:** "Integrate with [X tool/API] to [specific capability]"
- **Incorporate knowledge base:** "Add curated database of [domain-specific data]"
- **Enforce guardrails:** "Add mandatory checkpoints for [specific steps]"
- **Increase depth:** "Expand [dimension] with [specific framework/methodology]"
- **Manage complexity:** "Add multi-stage workflow for [complex aspect]"
- **Improve UX:** "Create interactive interface for [user interaction]"
- **Add structure:** "Define systematic process for [current ad-hoc step]"

**Format:**
- Concrete, actionable suggestions
- Explain how each suggestion increases the score
- Prioritize suggestions by potential impact
</improvement_generation>

<stop_conditions>
**Done when:**
- All required sections present in output
- All 7 dimensions evaluated with scores and rationales
- Total score calculated and interpreted correctly
- Clear verdict provided (Build/Consider/Skip)
- Improvement suggestions included (if applicable)
- Strategic considerations addressed

**Don't:**
- Compare to existing skills in the ecosystem
- Provide implementation details
- Make assumptions about user's strategic priorities
- Skip the confirmation step for text descriptions
</stop_conditions>

<examples>
**Example 1: Text Description (Two-Step Interaction)**

**User input:** "A skill that helps users organize their morning routine"

**Step 1 - Skill Understanding (wait for confirmation):**
```
## Skill Understanding

Based on your description, I understand this proposed skill as:

**Name:** morning-routine-organizer (or similar)
**Purpose:** Helps users create, structure, and optimize their morning routines
**Likely features:**
- Suggests routine activities based on user goals
- Sequences activities in optimal order
- Provides time estimates
- May include habit-building elements

**Is this understanding correct?** Please confirm or clarify before I proceed with evaluation.
```

**User confirms:** "Yes, that's correct"

**Step 2 - Full Evaluation Report:**
```
## Evaluation Report: morning-routine-organizer

### Scoring Table

| Dimension | Score | Rationale |
|-----------|-------|-----------|
| Knowledge Gap | 0/2 | General knowledge about routines; no specialized data |
| Structural Process | 1/2 | Provides some structure but LLM can do this casually |
| Tool Integration | 0/2 | No external tools mentioned |
| Consistency & Guardrails | 0/2 | No enforcement mechanisms |
| Complexity Management | 0/2 | Simple one-shot analysis |
| User Experience | 1/2 | Saves a few messages but minimal UX improvement |
| Specialization Depth | 0/2 | Same depth as general LLM advice |

### Total Score: 2/14

### Interpretation & Verdict
**Score Range:** 0-5 points = Likely redundant (skip)
**Verdict:** ❌ **SKIP - This skill is redundant**

### Recommendation
Do not build this skill. Claude can already provide morning routine advice without a dedicated skill. Users can simply ask "Help me organize my morning routine" and get similar results.

### Improvement Suggestions

To make this skill worth building, consider:

1. **Add tool integration (+2 points):** Integrate with calendar APIs to automatically schedule routines, or habit-tracking apps to monitor completion
2. **Incorporate knowledge base (+2 points):** Include research database on chronobiology, peak performance times, sleep science
3. **Enforce guardrails (+2 points):** Add mandatory validation steps (time budget check, energy level assessment, conflict detection)
4. **Manage complexity (+1 point):** Create multi-stage workflow: assessment → design → optimization → tracking
5. **Increase depth (+2 points):** Partner with behavioral psychologists to add evidence-based habit formation protocols

**Potential improved score:** 9/14 (Consider) if items 1-4 implemented

### Strategic Considerations
- **Low ecosystem value:** Overlaps with general productivity advice
- **No multiplier effect:** Doesn't enable other skills
- **Acid test result:** 90%+ of value available by asking Claude directly
```

---

**Example 2: Existing Skill File**

**User input:** `/workspace/.claude/skills/pdf/SKILL.md`

**Output (single step - no confirmation needed):**
```
## Evaluation Report: pdf

### Scoring Table

| Dimension | Score | Rationale |
|-----------|-------|-----------|
| Knowledge Gap | 1/2 | Synthesizes PDF manipulation knowledge systematically |
| Structural Process | 2/2 | Enforces systematic PDF processing workflows |
| Tool Integration | 2/2 | Orchestrates multiple PDF libraries and tools |
| Consistency & Guardrails | 2/2 | Mandatory validation of PDF operations |
| Complexity Management | 2/2 | Manages multi-page, multi-format complexity |
| User Experience | 2/2 | Transforms complex PDF tasks into simple invocations |
| Specialization Depth | 2/2 | Deep PDF manipulation expertise beyond general knowledge |

### Total Score: 13/14

### Interpretation & Verdict
**Score Range:** 10-14 points = Build this skill (significant value)
**Verdict:** ✅ **BUILD - Significant value add**

### Recommendation
This skill provides genuine new capabilities. PDF manipulation requires specialized tools and systematic processes that Claude cannot reliably perform without a dedicated skill.

### Strategic Considerations
- **High ecosystem value:** Enables document workflows across many domains
- **Multiplier effect:** Used by other skills and workflows
- **Acid test result:** <20% of value available by asking Claude directly
- **Tool orchestration:** Critical capability that requires skill structure
```
</examples>
