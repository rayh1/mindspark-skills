# build-with-patterns

Build reliable skills and prompts from scratch using a structured, example-driven approach. Creates outputs with clear contracts, testable steps, and minimal complexity.

## 🎯 What It Does

Guides you through building skills or prompts by:
1. Starting with one concrete example (your happy path)
2. Defining a tight output contract (format, sections, validation)
3. Specifying explicit inputs (required/optional, defaults)
4. Breaking down into 4–6 checkable steps
5. Adding decision points and quality gates as needed

## 🚀 Quick Start

```bash
build-with-patterns
```

The skill will guide you through an interactive process:
1. Choose what to build (skill or prompt)
2. Choose mode (lite or full)
3. Provide one concrete example
4. Answer structured questions
5. Review and approve the generated artifact

## 📊 Modes

### Lite Mode (Default)
- 4–6 execution steps
- ≤3 decision points
- ≤3 quality gates
- Fast, focused, sufficient for most cases
- Caps complexity automatically

### Full Mode
- No step/gate limits
- Additional optional patterns when needed
- For complex or critical use cases
- Requires justification for extra complexity

## 🏗️ Building Process

### For Skills (SKILL.md)

```
1. Describe what the skill does
   → Provide 1 concrete example
   
2. Define output contract
   → What format? What sections? Validation?
   
3. Specify inputs
   → What's required? Optional? Defaults?
   
4. Break into steps
   → 4–6 executable steps from input → output
   
5. Add decision points
   → Where does execution branch?
   
6. Set quality gates
   → Where do you validate before proceeding?
   
7. Review and approve
   → Get generated SKILL.md with YAML frontmatter
```

### For Prompts (Standalone)

```
1. Describe the prompt goal
   → Provide 1 concrete example
   
2. Define output structure
   → Sections, format, required elements
   
3. Specify inputs
   → Parameters, context needed
   
4. Write instruction flow
   → Clear steps to achieve goal
   
5. Review and approve
   → Get structured prompt ready to use
```

## 💡 Examples

### Input: "Build a code reviewer skill"

**Your example:**
```
Review this function and suggest improvements:
function calc(a,b){return a+b}

Expected output: Issues found + specific suggestions
```

**Generated output:**
```yaml
---
name: code-reviewer
description: Reviews code for issues and suggests specific improvements
---

<inputs>
- code_snippet: The code to review (required)
- language: Programming language (optional, auto-detect if not provided)
- focus_areas: Specific aspects to review (optional, default: all)
</inputs>

<output_contract>
Review report containing:
1. Issues Found section (list with severity)
2. Specific Suggestions section (code examples)
3. Summary (1-2 sentences)
Format: Markdown
</output_contract>

<steps>
1. Parse and validate code_snippet
2. Detect language if not provided
3. Analyze code for issues (syntax, logic, style)
4. Generate specific improvement suggestions
5. Format output per contract
6. Validate all required sections present
</steps>
...
```

## 🎓 Philosophy

### Start Concrete
One real example beats ten abstract requirements. Show don't tell.

### Lock Down Outputs
Explicit contracts make validation possible. "Return a JSON object with..." is better than "Return relevant data."

### Minimize Branching
Every decision point doubles complexity. Start with 0-2, add only when necessary.

### Test Early
If you can't verify a step succeeded, make it more specific.

## ✅ Best Practices

- **Lite mode first** - Start simple, upgrade only if needed
- **One example** - Don't overthink, just show what success looks like
- **Tight contracts** - Specify format, required sections, validation
- **4–6 steps** - If you have more, you're too detailed or too vague
- **Name meaningfully** - Use kebab-case for skills, clear titles for prompts

## 🔍 When To Use

✅ **Use build-with-patterns when:**
- Creating a new skill from scratch
- Converting informal instructions into structured prompts
- Need testable, maintainable outputs
- Want guidance on prompt structure
- Building for production use

❌ **Don't use when:**
- Making small edits to existing skills (use direct editing)
- Experimenting with quick one-offs
- You already have a well-structured prompt

## 📚 Documentation

- **[pattern-scaffold.md](references/pattern-scaffold.md)** - Full scaffolding methodology
- **[artifact-contracts.md](references/artifact-contracts.md)** - Output contract examples
- **[core-passes.md](references/core-passes.md)** - Building process details
- **[worked-example-skill.md](references/worked-example-skill.md)** - Complete example walkthrough
- **[worked-example-prompt.md](references/worked-example-prompt.md)** - Prompt building example
- **[tightening-methodology.md](references/tightening-methodology.md)** - How to refine outputs

### Workflows
- **[build-skill.md](workflows/build-skill.md)** - Step-by-step skill creation
- **[build-prompt.md](workflows/build-prompt.md)** - Step-by-step prompt creation

## 🔗 Related Skills

- **[apply-patterns](../apply-patterns/)** - Add patterns to existing prompts/skills
- **[examine-skill](../examine-skill/)** - Analyze and audit completed skills

## 🎯 Common Questions

**Q: Lite vs Full - which should I choose?**  
A: Start with Lite. It's sufficient for 90% of cases. Full mode only when you have complex branching or need sophisticated error handling.

**Q: Can I modify the generated output?**  
A: Yes! The generated artifact is a starting point. Edit freely after creation.

**Q: What if I don't have a concrete example?**  
A: Make one up. Even a fake example helps you think through what success looks like.

**Q: How is this different from just writing a prompt?**  
A: This creates *structured, testable* prompts with clear contracts. Freeform prompts work until they don't.

## 📄 License

MIT License - see [LICENSE](../../LICENSE) file for details

---

**Version:** 1.0.0  
**Part of:** [Mindspark Skills](../README.md)
