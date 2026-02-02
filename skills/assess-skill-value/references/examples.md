# Examples

## Table of Contents

- [Example 1: Text Description (Two-Step Interaction)](#example-1-text-description-two-step-interaction) - Lines 12-67
- [Example 2: Existing Skill File](#example-2-existing-skill-file) - Lines 71-105  
- [Example 3: Complete Flow with Steps 8-9 (Consider Verdict)](#example-3-complete-flow-with-steps-8-9-consider-verdict) - Lines 109-228

---

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
| [D-1] Knowledge Gap | 0/2 | General knowledge about routines; no specialized data |
| [D-2] Structural Process | 1/2 | Provides some structure but Claude can do this casually |
| [D-3] Tool Integration | 0/2 | No external tools mentioned |
| [D-4] Consistency & Guardrails | 0/2 | No enforcement mechanisms |
| [D-5] Complexity Management | 0/2 | Simple one-shot analysis |
| [D-6] User Experience | 1/2 | Saves a few messages but minimal UX improvement |
| [D-7] Specialization Depth | 0/2 | Same depth as general Claude advice |

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
| [D-1] Knowledge Gap | 1/2 | Synthesizes PDF manipulation knowledge systematically |
| [D-2] Structural Process | 2/2 | Enforces systematic PDF processing workflows |
| [D-3] Tool Integration | 2/2 | Orchestrates multiple PDF libraries and tools |
| [D-4] Consistency & Guardrails | 2/2 | Mandatory validation of PDF operations |
| [D-5] Complexity Management | 2/2 | Manages multi-page, multi-format complexity |
| [D-6] User Experience | 2/2 | Transforms complex PDF tasks into simple invocations |
| [D-7] Specialization Depth | 2/2 | Deep PDF manipulation expertise beyond general knowledge |

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

---

**Example 3: Complete Flow with Steps 8-9 (Consider Verdict)**

**User input:** "A skill that analyzes git commit history and suggests code review priorities"

**After Steps 1-7 (Evaluation Report):**
```
## Evaluation Report: commit-review-prioritizer

### Scoring Table

| Dimension | Score | Rationale |
|-----------|-------|-----------|
| [D-1] Knowledge Gap | 1/2 | Synthesizes git analysis patterns but no specialized data |
| [D-2] Structural Process | 2/2 | Enforces systematic commit analysis workflow |
| [D-3] Tool Integration | 2/2 | Orchestrates git commands and file system analysis |
| [D-4] Consistency & Guardrails | 1/2 | Some validation but could be stronger |
| [D-5] Complexity Management | 1/2 | Manages moderate complexity across commits |
| [D-6] User Experience | 1/2 | Saves several manual steps but not transformative |
| [D-7] Specialization Depth | 0/2 | Similar depth to general git analysis |

### Total Score: 8/14

### Interpretation & Verdict
**Score Range:** 6-9 points = Moderate value (consider if strategic)
**Verdict:** ⚠️ **CONSIDER - Moderate value**

### Recommendation
This skill provides moderate value. Consider building if code review efficiency is a strategic priority for your team.

### Improvement Suggestions

To increase value:
1. **Add code quality metrics database (+2 points):** Include patterns of bug-prone code, security vulnerabilities
2. **Enforce mandatory checks (+1 point):** Validate commit message quality, check for breaking changes
3. **Increase complexity management (+1 point):** Add cross-repository analysis and trend detection

### Strategic Considerations
- **Moderate ecosystem value:** Useful for development workflows
- **Team multiplier:** More valuable for larger teams
- **Acid test result:** 60% of value available by asking Claude directly
```

**Step 8 - Additional Improvements (automatically presented):**
```
## Additional Improvement Opportunities

Based on the evaluation, here are further enhancements ranked by value impact:

1. **[High Priority] Integrate with issue tracking systems** (+2 points on D-3, +1 on D-5)
   - Connect to Jira/GitHub Issues to correlate commits with reported bugs
   - Track which areas of code have highest issue density
   - Estimated impact: +3 points total (11/14 - Build territory)

2. **[High Priority] Add ML-based code quality analysis** (+2 points on D-1, +1 on D-7)
   - Integrate pre-trained models that identify code smells and anti-patterns
   - Provide deeper analysis than general git knowledge
   - Estimated impact: +3 points total (11/14 - Build territory)

3. **[Medium Priority] Create multi-stage review workflow** (+1 point on D-5)
   - Stage 1: File-level analysis → Stage 2: Function-level → Stage 3: Risk scoring
   - Track state across stages
   - Estimated impact: +1 point (9/14 - still Consider)

4. **[Medium Priority] Add team velocity insights** (+1 point on D-7)
   - Analyze commit patterns for productivity bottlenecks
   - Identify areas needing documentation or refactoring
   - Estimated impact: +1 point (9/14 - still Consider)

5. **[Low Priority] Interactive priority adjustment** (+1 point on D-6)
   - Let users adjust review priorities based on their criteria
   - Save and reuse priority profiles
   - Estimated impact: +1 point (9/14 - still Consider)

**Potential improved score with items 1-2:** 14/14 (Strong Build)
**Potential improved score with all items:** 14/14 (maxed out)
```

**Step 9 - Specification Offer:**
```
Would you like me to create a complete specification document for this skill?

This specification will include:
- Overview and objective
- Key capabilities
- Inputs (required and optional)
- Outputs and format
- High-level process flow
- Quality gates
- Features (including selected improvements)
- Edge cases and error handling
- Example invocations
- Constraints and limitations
- Implementation notes

Please let me know:
1. Should I generate this specification?
2. Which improvements from the list above should be included? (e.g., "1,2" or "all" or "none")
   - Note: "none" means generate spec with only the core capabilities already described, without any enhancements
```

**User responds:** "Yes, include improvements 1 and 2"

**Output:**
```
I've created a complete specification document for commit-review-prioritizer with improvements #1 and #2 included.

📄 Specification saved to: /tmp/claude/-workspace/7179c228-bacf-4b0b-acf0-5caef08583fa/scratchpad/commit-review-prioritizer-specification.md

The specification includes:
- Complete overview and objectives
- All core capabilities plus issue tracking integration and ML-based analysis
- Detailed inputs and outputs
- High-level 5-stage process workflow
- Quality gates for each stage
- Edge cases and error handling
- Example invocations
- Implementation guidance

This specification is ready to use as input to build-with-patterns or skill-creator.
```
