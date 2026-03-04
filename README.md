# Mindspark Skills

A collection of pattern-guided skills for building, improving, and analyzing AI prompts and skills. These skills help you create more reliable, maintainable, and effective AI interactions.

## 📦 Skills Included

### 1. [analyze-skill](skills/analyze-skill/)
Comprehensively analyzes a skill idea or existing skill: evaluates value with a 7-dimension scoring framework (0-14 points), explores enhancement opportunities across 5 vectors, and optionally generates a design specification.

Works for both proposed skills (pre-build decision) and existing skills (post-build audit). Outputs a scored table with rationales, a clear verdict (Build / Consider / Skip), and a ranked set of enhancement opportunities.

**Use when:** Deciding if a skill is worth building, auditing existing skills for value and growth potential, or generating specifications for new or enhanced skills.

**Quick start:**
```
analyze-skill "A skill that helps users create morning routines"
analyze-skill path/to/SKILL.md
analyze-skill pdf
analyze-skill pdf --output enhanced-pdf.skill-design.md
```

### 2. [apply-patterns](skills/apply-patterns/)
Adds reliability patterns to existing prompts or skills using gap-driven analysis.

It reads your target file, identifies high-leverage reliability gaps (inputs, scope, output contracts, validation), and proposes a small set of tier-appropriate pattern insertions. Nothing is changed until you explicitly approve the proposed edits.

**Use when:** You want to improve determinism, debuggability, and scope control without changing core intent.

**Quick start:**
```
apply-patterns path/to/file.md

# Optional: add a short note to constrain the analysis, e.g.
# "Tier 1 only" or "include Tier 2 if clearly justified".
```

### 3. [build-with-patterns](skills/build-with-patterns/)
Builds skills and prompts from scratch using a spec-by-tightening methodology.

It starts from one concrete "happy path" example, tightens that into a testable output contract, and then produces an executable step sequence (typically 4-6 steps). Use it when you want repeatable results, clear acceptance criteria, and fewer ambiguous instructions.

**Use when:** Creating new skills or prompts with a focus on testable outputs and clear execution steps.

**Quick start:**
```
build-with-patterns
# Follow interactive prompts to build a skill or prompt

build-with-patterns --from-design path/to/file.skill-design.md
# Build from a design file (e.g. produced by analyze-skill)
```

### 4. [decide](skills/decide/)
Provides structured decision-making using 9 frameworks that prevent "agreeable LLM" spirals - where the model agrees with every option the user proposes.

It analyzes options systematically using explicit criteria, locks recommendations, and requires new information before allowing reversals.

**Use when:** Comparing 2+ alternatives, making high-stakes architectural or vendor decisions, or needing rigorous risk/failure-mode analysis.

**Quick start:**
```
decide "Should I use PostgreSQL or MongoDB for this use case?"
decide --framework matrix  # force Decision Matrix framework
```

### 5. [delpher-newspaper-analyst](skills/delpher-newspaper-analyst/)
Deep-dives into historical Dutch newspaper pages from Delpher.nl. Given a newspaper image and OCR text, it produces a structured 9-section report: curious facts, advertisement analysis, cross-article patterns, OCR error flags, themed research questions with clickable Delpher search URLs, gaps, and ranked research paths.

Persists findings to Joplin and can hand off context to `delpher-research-assistant` for multi-session projects. All output is in Dutch.

**Use when:** Analyzing a Delpher newspaper scan, finding interesting dates to research, or generating search strategies from historical newspaper content.

**Quick start:**
```
# Paste a Delpher newspaper image + OCR text -> single-page analysis
# Paste 2-3 images/OCR blocks -> batch/comparison mode
delpher-newspaper-analyst  # no input -> Discovery mode: suggests 3 historically interesting dates
```

### 6. [delpher-research-assistant](skills/delpher-research-assistant/)
Enables rigorous multi-session historical research using Delpher.nl (Dutch newspaper archive, 1618-1995) through a 5-phase workflow: Planning -> Analysis -> Synthesis -> Gap Analysis -> Journaling, with built-in review-fix cycles and an optional Final Consolidation phase.

You manually search and paste article content; the skill analyzes, synthesizes, tracks citations, and identifies research gaps. All output is in Dutch.

**Use when:** Conducting systematic historical research with Delpher, building timelines from primary sources, or doing biographical/event research in Dutch archives.

**Quick start:**
```
delpher-research-assistant "De watersnoodramp van 1953"
# Describe topic -> receive search strategy -> paste articles -> get analysis + gaps
```

### 7. [evolve-artifact](skills/evolve-artifact/)
Evolves SKILL.md files and prompt files by analyzing the current conversation to extract learnings and generate type-appropriate improvement recommendations. Automatically detects artifact type (skill vs prompt) and applies the relevant quality standards.

**Use when:** A skill or prompt was just used in conversation and improvements are evident, or after testing to consolidate learnings.

**Quick start:**
```
evolve-artifact path/to/SKILL.md
# Analyzes the current conversation and proposes targeted improvements
```

### 8. [review-artifact](skills/review-artifact/)
Reviews SKILL.md files and standalone prompts for contradictions, redundancies, structural issues, and outdated content. Automatically detects artifact type via YAML frontmatter and applies the appropriate quality standards.

Produces a detailed report with severity-tagged issues and prioritized, actionable recommendations.

**Use when:** Auditing skills before deployment, validating structure against quality standards, or preparing artifacts for refactoring.

**Quick start:**
```
review-artifact path/to/SKILL.md
# Produces a detailed review report with severity-tagged findings
```
## 🚀 Installation

### For Claude Desktop

1. Clone this repository:
   ```bash
   git clone https://github.com/rayh1/mindspark-skills.git
   ```

2. Install as a plugin:
   - Place the repository in your Claude plugins directory
   - Or add each skill folder from `skills/` individually to your skills configuration

### For Claude Projects

1. Copy the skill folder(s) you want to use into your project
2. Reference the skill in your project context

## 📖 Documentation

Each skill includes:
- **SKILL.md** - Core skill definition with usage instructions
- **references/** - Supporting documentation and guides
- **workflows/** - Step-by-step workflow guides (where applicable)

## 🎯 Use Cases

- **Prompt Engineering:** Build reliable prompts with clear contracts
- **Skill Development:** Create and refine Claude Desktop skills
- **Quality Assurance:** Audit and improve existing skills
- **Pattern Application:** Add reliability patterns systematically

## 🔄 Workflow

Common workflow for using these skills together:

1. **Analyze** a skill idea (`analyze-skill`) to decide if it's worth building and optionally generate a design spec
2. **Build** the skill or prompt (`build-with-patterns`), optionally from the design spec
3. **Review** it for structural issues (`review-artifact`)
4. **Apply** reliability patterns (`apply-patterns`)
5. **Evolve** after real-world use (`evolve-artifact`) to consolidate learnings
6. Use **decide** whenever you need structured comparison between options at any step

## 📋 Requirements

- Claude Desktop (for interactive skills)
- Or any Claude interface with skill/prompt support

## 🤝 Contributing

Contributions welcome! Please:
1. Fork the repository
2. Create a feature branch
3. Submit a pull request with clear description

## 📄 License

MIT License - see [LICENSE](LICENSE) file for details

## 🙋 Support

For issues, questions, or suggestions:
- Open an issue in this repository
- Check individual skill documentation in their folders

## 🔗 Links

- GitHub: https://github.com/rayh1/mindspark-skills
- Claude Documentation: https://claude.ai/docs

---

**Version:** 1.1.0  
**Last Updated:** March 2026
