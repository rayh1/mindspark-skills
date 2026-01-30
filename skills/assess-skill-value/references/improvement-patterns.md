# Improvement Patterns

## Generating Initial Improvement Suggestions

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

---

## Additional Improvements (Step 8)

**Step 8: Additional Improvements (if verdict ≠ skip)**

After presenting the initial evaluation report, generate additional features and improvements:

**Ranking criteria:**
1. Value impact (how much it increases the skill score)
2. Implementation feasibility
3. User demand/benefit
4. Strategic fit with skill ecosystem

**Format:**
Present as a numbered list with:
- Improvement name and description
- Estimated score impact (e.g., "+2 points" or "+1-2 points")
- Dimension(s) affected
- Priority level (High/Medium/Low)

**Example output:**
```
## Additional Improvement Opportunities

Based on the evaluation, here are further enhancements ranked by value impact:

1. **[High Priority] Add real-time data integration** (+2 points on D-1, D-3)
   - Integrate with live APIs to fetch current data beyond training cutoff
   - Addresses Knowledge Gap and Tool Integration dimensions

2. **[High Priority] Implement multi-stage workflow** (+2 points on D-5)
   - Break complex process into 5+ distinct phases with state tracking
   - Dramatically improves Complexity Management

3. **[Medium Priority] Add validation checkpoints** (+1 point on D-4)
   - Mandatory quality gates between each major step
   - Strengthens Consistency & Guardrails

**Potential improved score:** 12/14 (Build) if all high-priority items implemented
```
