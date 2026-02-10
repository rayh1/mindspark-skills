# Pattern Catalog

**Purpose:** Reliability patterns from apply-patterns and build-with-patterns skills. Used for detecting missing or broken patterns during evolution analysis.

Patterns add structure (contracts, validation, scope control) without changing core intent. Conversations may reveal missing or broken patterns.

---

## Tier 1 - Core (always consider for non-trivial workflows)

- **Inputs First** - Declare required inputs before work | Missing: user confused about what's needed
- **Step Contract** - Explicit step sequence | Missing: ambiguous instructions, unclear workflow
- **Decision Points** - Make branching explicit | Missing: conditional logic hidden or unclear
- **Quality Gates** - Validation checkpoints between steps | Missing: errors undetected, no verification
- **Stop Conditions** - Define "done" precisely | Missing: scope creep, didn't stop when should
- **Scope Fence** - Bound in/out of scope explicitly | Missing: expanded beyond intent
- **Interpretation Check** - Verify understanding before starting | Missing: misunderstood requirements
- **Output Schema** - Define required output structure | Missing: inconsistent output, parsing issues

## Tier 2 - Recommended (when situation warrants)

- **User Approval Gate** - Human oversight at checkpoints | Missing: irreversible actions without approval
- **Clarifying Questions** - Protocol for asking user | Missing: blocked but didn't ask
- **Confidence Signal** - Surface uncertainty explicitly | Missing: high-stakes decision without flagging uncertainty
- **Fallback Chain** - Recovery when primary fails | Missing: failed with no backup approach
- **Lens** - Multi-perspective analysis | Missing: tunnel vision in analysis/review
- **Addressable Output** - Unique IDs for output items | Missing: can't reference specific outputs
- **Review Step** - Holistic post-completion check | Missing: no coherence verification
- **Step Ledger** - Running execution state | Missing: lost track in long/complex task

## Tier 3 - Situational (specific use cases only)

- **Mode Selection** - Present workflow choices upfront | Multiple workflows without clear selection
- **Example Anchor** - Ground behavior with examples | Style/format hard to describe
- **Assumption Registry** - Log assumptions as made | Audit trail needed
- **Context Window** - Working memory policy | Very long tasks with shifting focus
- **Observability** - Run metadata + traces | Debugging/comparison needed
- **Error Handling** - Error classification + redaction | Failures need taxonomy

---

## Format

- Skills: XML tags `<inputs_first>`, `<step_contract>`, etc.
- Prompts: Markdown headings `## Inputs First:`, `## Step Contract:`, etc.

---

## Detection Signals in Conversation

- "Skill failed silently" → missing Quality Gates
- "User asked what inputs needed" → missing/incomplete Inputs First
- "Kept going beyond purpose" → missing Stop Conditions
- "Instructions were ambiguous" → missing/incomplete Step Contract
- "Made irreversible change without asking" → missing User Approval Gate
- "Got confused about what to do" → missing Decision Points or Interpretation Check
- "Output format inconsistent" → missing Output Schema

---

## Recommendation Format

When pattern is missing: cite pattern name + tier + purpose

Example: "Add <quality_gates> section after step 4 | standard: Quality Gates (Tier 1 - validation checkpoints)"
