# assess-skill-value

Evaluate whether a proposed skill is worth building using a 7-dimension scoring framework (0–14 points). Produces a clear Build/Consider/Skip recommendation plus concrete improvement suggestions.

## 🎯 What It Does

Assesses a skill idea (or an existing `SKILL.md`) across 7 dimensions:
1. **Knowledge Gap Test** - Is there specialized knowledge beyond general LLM capability?
2. **Structural Process Test** - Does it enforce a repeatable, reliable workflow?
3. **Tool Integration Test** - Does it orchestrate tools/external systems?
4. **Consistency & Guardrails Test** - Does it prevent common LLM shortcuts or omissions?
5. **Complexity Management Test** - Does it handle multi-factor, multi-step complexity well?
6. **User Experience Test** - Does it meaningfully reduce user friction?
7. **Specialization Depth Test** - Does it go materially deeper than a generic response?

Each dimension is scored **0 / 1 / 2** with rationale, then summed to a total score.

## 🚀 Quick Start

```bash
# Assess a skill idea (text)
assess-skill-value "A skill that helps users create morning routines with constraints"

# Assess an existing skill definition
assess-skill-value path/to/SKILL.md

# Assess by skill name (e.g., an existing installed skill)
assess-skill-value pdf
```

## 📊 Scoring & Verdicts

- **10–14:** Build - significant value add
- **6–9:** Consider - moderate value; build if strategically useful
- **0–5:** Skip - likely redundant with standard prompting

## 📋 Output

Produces a markdown-formatted evaluation report including:
- Scoring table (all 7 dimensions)
- Total score (0–14)
- Interpretation & verdict (Build / Consider / Skip)
- Recommendation (clear guidance)
- Improvement suggestions (only when applicable)
- Strategic considerations (fit and meta-value, without implementation details)

## ✅ Best Practices

- Use early (before building) to avoid investing in redundant skills
- Provide at least 2–3 sentences of description for best results
- Treat “Consider” as a prompt to tighten scope and add differentiation
- If you can get ~80% of the value by asking Claude directly, treat that as a red flag

## 🔍 When To Use

✅ **Use assess-skill-value when:**
- You have a new skill idea and want to sanity-check value
- You’re unsure whether a skill is redundant with normal prompting
- You’re triaging a backlog of skill ideas

❌ **Don’t use when:**
- You want implementation guidance (this is evaluation-only)
- You need functional testing (it doesn’t execute the skill)

## 🔗 Related Skills

- **[build-with-patterns](../build-with-patterns/)** - Build the skill after it earns a “Build” verdict
- **[examine-skill](../examine-skill/)** - Audit an existing skill for quality issues
- **[apply-patterns](../apply-patterns/)** - Add reliability patterns after the structure is solid

## 📄 License

MIT License - see [LICENSE](../../LICENSE) file for details

---

**Version:** 1.0.0  
**Part of:** [Mindspark Skills](../README.md)
