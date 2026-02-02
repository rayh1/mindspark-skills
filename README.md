# Mindspark Skills

A collection of pattern-guided skills for building, improving, and analyzing AI prompts and skills. These skills help you create more reliable, maintainable, and effective AI interactions.

## 📦 Skills Included

### 1. [apply-patterns](skills/apply-patterns/)
Adds reliability patterns to existing prompts or skills using a tiered pattern system.

It reads your target file, identifies high-leverage reliability gaps (inputs, scope, output contracts, validation), and proposes a small set of tier-appropriate pattern insertions. Nothing is changed until you explicitly approve the proposed edits.

**Use when:** You want to improve determinism, debuggability, and scope control without changing core intent.

**Quick start:**
```
apply-patterns path/to/file.md

# Optional: add a short note to constrain the analysis, e.g.
# "Tier 1 only" or "include Tier 2 if clearly justified".
```

### 2. [build-with-patterns](skills/build-with-patterns/)
Builds skills and prompts from scratch using a structured, example-driven approach.

It starts from one concrete “happy path” example, tightens that into a testable output contract, and then produces an executable step sequence (typically 4–6 steps). Use it when you want repeatable results, clear acceptance criteria, and fewer ambiguous instructions.

**Use when:** Creating new skills or prompts with a focus on testable outputs and clear execution steps.

**Quick start:**
```
build-with-patterns
# Follow interactive prompts to build a skill or prompt
```

### 3. [assess-skill-value](skills/assess-skill-value/)
Evaluates whether a proposed skill is worth building using a 7-dimension scoring framework (0-14 points).

It outputs a scored table with rationales, a clear verdict (Build / Consider / Skip), and concrete improvement suggestions to increase uniqueness or reduce redundancy.

**Use when:** You want to avoid building redundant skills and focus effort on ideas that add real capability.

**Quick start:**
```
assess-skill-value "A skill that helps users create morning routines"
assess-skill-value path/to/SKILL.md
assess-skill-value pdf
```

### 4. [examine-artifact](skills/examine-artifact/)
Analyzes skill files and standalone prompts for problems, contradictions, redundancies, structural issues, and outdated content. Automatically detects artifact type (skill vs prompt).

It produces a written examination report (not just chat notes), with specific issues and actionable fixes, so you can confidently refactor or prepare a skill for pattern application.

**Use when:** Auditing skills, reviewing quality, or preparing for improvements.

**Quick start:**
```
examine-artifact path/to/SKILL.md
# Produces EXAMINATION-REPORT.md with detailed findings
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

1. **Build** a new skill or prompt (`build-with-patterns`)
2. **Examine** it for issues (`examine-artifact`)
3. **Apply** reliability patterns (`apply-patterns`)
4. Iterate as needed

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

**Version:** 1.0.0  
**Last Updated:** February 2026
