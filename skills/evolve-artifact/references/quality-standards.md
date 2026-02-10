# Quality Standards

**Purpose:** Comprehensive quality criteria for analyzing skills and prompts. Used when assessing artifact quality during the evolution process.

## Table of Contents

- [SHARED (Both Skills and Prompts)](#shared-both-skills-and-prompts)
- [SKILLS ONLY](#skills-only)
- [PROMPTS ONLY](#prompts-only)

---

## SHARED (Both Skills and Prompts)

*Context Window Philosophy:*
- Challenge test: "Does Claude really need this explanation?"
- Preference hierarchy: concise examples > bulleted lists > short sentences > verbose explanations
- Every token must justify its cost
- Example (30 tokens) preferred over explanation (90+ tokens) for same concept
- Remove content Claude can infer from training or context

*Duplication Rule:*
- Information appears in ONE place only
- No identical examples, instructions, or code snippets repeated
- Acceptable: brief overview + full details elsewhere (not identical content)
- Detection: same concept appearing multiple times with identical/similar wording
- Remediation: keep in primary location, remove from others, add reference if needed

*Writing Voice:*
- Imperative form for instructions ("Extract text" not "Extracting text" or "Extraction of text")
- Exception: canonical pattern labels (Inputs First, Quality Gates - keep noun form)
- Section headings: imperative verbs ("Parse Tables" not "Table Parsing")
- Direct, actionable, scannable

*Severity Assignment:*
- CRITICAL: Blocks core function (missing triggers, dead references, 3+ duplications)
- HIGH: Significantly degrades usability (no structure, >500/700 lines, <20 words description)
- MEDIUM: Quality evolution needed (500-700 lines, <50/>200 words, verbose content)
- LOW: Polish issue (voice inconsistency, minor redundancy)

---

## SKILLS ONLY

*Progressive Disclosure (3-Level Loading):*
- Level 1 (metadata): YAML frontmatter always loaded for ALL skills - must be comprehensive for triggering
- Level 2 (SKILL.md): Loaded when triggered - contains workflow, <500 lines target
- Level 3 (references/): Loaded as needed - unlimited size, one level deep only
- Red flags: essential workflow in references/, "When to Use" section in body (belongs in description)

*Description Field Excellence:*
- Formula: [What it does] + [When to use with specific examples]
- Components: capability summary, trigger conditions, file types, enumerated use cases, keywords
- Size: ~50-150 words (20-50 HIGH, <20 CRITICAL, >200 MEDIUM)
- Must include BOTH what AND when (description determines triggering before body loads)
- Example: "Comprehensive PDF manipulation including... Use when Claude needs to: (1) extract..., (2) fill forms..."

*Split Strategy (500-Line Guideline):*
- Target: SKILL.md <500 lines
- 400-500: plan split, 500-700: actively split (MEDIUM), 700+: urgent restructuring (HIGH)
- Move to references/: domain-specific details, framework variants, advanced features, detailed examples, API docs
- Keep in SKILL.md: core workflow, navigation logic for references/, essential patterns
- After split target: 100-300 lines for SKILL.md

*Resource Organization:*
- scripts/: executable code (reused, deterministic, complex, or repeatedly rewritten)
- references/: documentation Claude reads (domain knowledge, detailed specs, variant instructions)
- assets/: files used in output (templates, configs, brand assets)
- Descriptive file names (authentication-guide.md not advanced.md)
- References one level deep, files >100 lines need TOC
- Dead references → CRITICAL

---

## PROMPTS ONLY

*Heading Structure:*
- H1 exactly once at top (title)
- H2 for major sections (Purpose, Inputs, Steps, Output, Examples)
- H3 for subsections within H2
- H4+ avoid or use sparingly (<5 times, indicates over-nesting)
- Descriptive headings ("Parse Input" not "Details" or "Step 1")
- Consistent hierarchy, no level skipping

*Section Organization:*
- Recommended order: Purpose → Inputs → Steps → Output Format → Examples → Constraints
- Rationale: establish context, define needs, explain process, specify results, illustrate, handle exceptions
- Anti-patterns: outputs before inputs, constraints before instructions, examples before rules
- Positive instructions first, exceptions/constraints after main flow

*Content Efficiency:*
- Token budget test for blocks >30 words: Does this change behavior? Would Claude fail without it? Most efficient way?
- Convert: prose → lists (50 words → 5 bullets), explanations → examples (100 words → 10-line example)
- Remove: obvious content Claude knows, filler phrases ("please note", "it's important"), repeated instructions
- Keep explanations only when: "why" affects "how", Claude would choose wrong approach, domain-specific nuance critical

*Progressive Complexity:*
- First 20 lines: primary purpose, main task, critical constraints
- Complexity gradient: core task (80% case) → main variations → edge cases → advanced options
- Exception handling after main flow
- Examples follow instructions they illustrate
- Front-load essentials, defer nice-to-have details

*Length Guidelines:*
- <100 lines: concise (check completeness)
- 100-200: ideal
- 200-300: long (review for trimming)
- 300-500: too long (MEDIUM - needs condensing)
- 500+: excessive (HIGH - major restructuring)
- Justification: complexity/workflows/examples required, not padding/obviousness/verbosity
