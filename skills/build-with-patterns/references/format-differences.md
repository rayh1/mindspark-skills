# Format Differences: Skill vs Prompt

This skill uses the same core passes (see `core-passes.md`). Only the **rendering** differs.

---

## Skill (SKILL.md)
- YAML frontmatter at top (`name`, `description`).
- Body uses XML tags (`<inputs_first>`, `<step_contract>`, ...).
- Avoid markdown headings inside XML blocks.
- Output artifacts may include extra files (`workflows/`, `references/`) if requested.

## Prompt (standalone)
- No YAML.
- Use simple headings (`## Inputs First:`, `## Step Contract:`).
- Keep it copy/pastable.
- Prefer compact schemas over verbose prose.
