# Enhancement Vectors

Five systematic vectors for exploring skill enhancement opportunities. Each vector includes exploration prompts and maps to dimension impacts.

## Table of Contents

- [V-1: Input/Output Expansion](#v-1-inputoutput-expansion) - Lines 18-33
- [V-2: Tool Integration](#v-2-tool-integration) - Lines 35-60
- [V-3: Intelligence Layer](#v-3-intelligence-layer) - Lines 62-87
- [V-4: User Experience](#v-4-user-experience) - Lines 89-114
- [V-5: Domain Depth](#v-5-domain-depth) - Lines 116-141
- [Enhancement Exploration Process](#enhancement-exploration-process) - Lines 143-162
- [Ranking Logic](#ranking-logic) - Lines 164-177

---

## V-1: Input/Output Expansion

**Focus:** What additional inputs could the skill accept? What new output formats could it produce?

### Exploration Prompts
- Could it accept multiple inputs at once (batch processing)?
- Could it stream results incrementally?
- What additional input formats would be valuable? (files, URLs, databases)
- What output formats are missing? (JSON, CSV, HTML, PDF)
- Could it convert between formats?
- Could it accept input from other skills' outputs?

### Dimension Impact
- Primary: D-5 (Complexity Management) - batch/streaming adds complexity handling
- Secondary: D-6 (User Experience) - more formats = less friction

### Example Enhancements
```
[I-1] Batch Processing Mode (V-1) [High Priority]
- What: Process multiple items in single invocation with glob patterns
- Impact: (↑ D-5 by +1, ↑ D-6 by +1)
- Effort: Low
- Rationale: Users processing 10+ items currently run command repeatedly
```

---

## V-2: Tool Integration

**Focus:** What external tools, APIs, or MCP servers could extend capabilities?

### Exploration Prompts
- What MCP servers could add functionality? (Joplin, databases, APIs)
- Could it integrate with version control (git)?
- Could it connect to external APIs for real-time data?
- Could it orchestrate other skills?
- What file system operations would help?
- Could it use browser automation (Playwright)?

### Dimension Impact
- Primary: D-3 (Tool Integration) - direct impact
- Secondary: D-1 (Knowledge Gap) - external data sources add knowledge

### Example Enhancements
```
[I-2] Joplin MCP Integration (V-2) [High Priority]
- What: Save outputs directly to Joplin notes
- Impact: (↑ D-3 by +2, ↑ D-6 by +1)
- Effort: Medium
- Rationale: Eliminates manual copy-paste for persistent storage
```

---

## V-3: Intelligence Layer

**Focus:** What validation, recommendations, or auto-corrections could be added?

### Exploration Prompts
- What validation should happen before processing?
- What quality checks should happen after?
- Could it detect and correct common errors automatically?
- Could it provide recommendations based on analysis?
- Could it learn from usage patterns?
- What predictive capabilities would help?

### Dimension Impact
- Primary: D-4 (Consistency & Guardrails) - validation adds discipline
- Secondary: D-2 (Structural Process) - recommendations add structure

### Example Enhancements
```
[I-3] Input Validation Layer (V-3) [Medium Priority]
- What: Validate inputs against schema before processing
- Impact: (↑ D-4 by +1)
- Effort: Low
- Rationale: Catches errors early, prevents wasted processing
```

---

## V-4: User Experience

**Focus:** How could the skill be easier or more pleasant to use?

### Exploration Prompts
- Could it offer an interactive/guided mode?
- Could it show progress for long operations?
- Could it be resumed if interrupted?
- Could users customize behavior via preferences?
- Could it provide better onboarding for new users?
- Could it offer presets or templates?

### Dimension Impact
- Primary: D-6 (User Experience) - direct impact
- Secondary: D-5 (Complexity Management) - progress tracking helps manage complexity

### Example Enhancements
```
[I-4] Progress Tracking Mode (V-4) [Medium Priority]
- What: Show progress bar and ETA for batch operations
- Impact: (↑ D-6 by +1)
- Effort: Low
- Rationale: Reduces user anxiety during long operations
```

---

## V-5: Domain Depth

**Focus:** What advanced techniques or edge cases could be handled?

### Exploration Prompts
- What professional/expert features are missing?
- What edge cases does it not handle?
- What advanced methodologies could be incorporated?
- What domain-specific best practices should be enforced?
- What specialized knowledge databases could be added?
- What niche scenarios would power users need?

### Dimension Impact
- Primary: D-7 (Specialization Depth) - direct impact
- Secondary: D-1 (Knowledge Gap) - specialized knowledge adds gaps

### Example Enhancements
```
[I-5] Advanced OCR Support (V-5) [High Priority]
- What: Handle scanned PDFs with optical character recognition
- Impact: (↑ D-7 by +1, ↑ D-1 by +1)
- Effort: High
- Rationale: 30% of real-world PDFs are scanned images
```

---

## Enhancement Exploration Process

### For New Skills (during Step 8)
1. Review current capabilities from evaluation
2. For each vector, ask: "What's obviously missing?"
3. Focus on improvements that push score toward Build territory
4. Prioritize by: impact on verdict > implementation feasibility

### For Existing Skills (during Step 8 or --enhancements-only)
1. Map current capabilities thoroughly:
   - What inputs does it accept?
   - What outputs does it produce?
   - What tools does it use?
   - What validation exists?
   - What UX features exist?
2. For each vector, systematically explore gaps
3. Check existing "Enhancement Opportunities" section if present
4. Prioritize by: user value > implementation feasibility

---

## Ranking Logic

**Priority calculation:**
- Impact score (1-10): How much user value added?
- Feasibility score (1-5): How hard to implement? (5=easy, 1=very hard)
- Priority = Impact × Feasibility / 10

| Priority | Score Range |
|----------|-------------|
| HIGH | ≥ 4.0 |
| MEDIUM | 2.0-3.9 |
| LOW | < 2.0 |

**Quick Wins filter:** Effort = Low AND Impact ≥ 6
