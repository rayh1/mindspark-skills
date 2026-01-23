# Workflow: Build a Skill (Lean)

<required_reading>
**Recommended reading (pointer-only):**
1. `../references/core-passes.md` (canonical procedure)
2. `../references/patterns-when-needed.md` (authoritative triggers for optional add-ons)
3. `../references/worked-example-skill.md` (example)
4. `../references/tightening-methodology.md` (optional narrative)
</required_reading>

<process>
**Goal:**
Build a Claude Code skill by following the canonical procedure in `../references/core-passes.md`, then applying skill-specific formatting.

**Procedure (authoritative):**
- Target type is already **skill**.
- Follow `../references/core-passes.md`.
- Use optional add-ons only when triggered: `../references/patterns-when-needed.md`.

**Skill-specific output**:
Render `SKILL.md`:
- YAML frontmatter (`name`, `description`)
- XML-tag sections (per `../SKILL.md` `<output_schema>`)

**Approval:**
Run the single approval checkpoint before finalizing (per `../SKILL.md` `<user_approval_gate>`).
</process>

<stop_conditions>
Stop when the skill draft is complete and approved (see `../SKILL.md` `<stop_conditions>`).
</stop_conditions>
