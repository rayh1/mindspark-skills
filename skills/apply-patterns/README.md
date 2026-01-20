# apply-patterns

Add reliability patterns to your prompts and skills using a tiered, mode-based system. Improves determinism, debuggability, and scope control without changing your core intent.

## 🎯 What It Does

Analyzes your prompt or skill and recommends specific reliability patterns based on:
- **Content analysis** - What your prompt does
- **Mode selection** - How comprehensive you want the review
- **Pattern tiers** - Priority-based pattern organization

All recommendations require your approval before making changes.

## 🚀 Quick Start

```bash
# Lite mode (fastest, Tier 1 only)
apply-patterns path/to/file.md

# Standard mode (balanced, Tier 1 + 2)
apply-patterns path/to/file.md mode:standard

# Full mode (comprehensive, all patterns)
apply-patterns path/to/file.md mode:full
```

## 📊 Modes

| Mode | Patterns | Best For | Typical Output |
|------|----------|----------|----------------|
| **lite** (default) | Tier 1 (8 patterns) | Quick improvements | 2-4 patterns |
| **standard** | Tier 1 + 2 (16 patterns) | Balanced review | 3-6 patterns |
| **full** | All tiers (22 patterns) | Comprehensive audit | Complete analysis with prioritization |

## 🔧 Pattern Tiers

### Tier 1 - Core Patterns (Always Consider)
High-yield, low-overhead patterns that benefit most prompts:
- Input Specification
- Output Contract
- Executable Steps
- Decision Points
- Scope Fence
- Assumptions Explicit
- Error Handling (lite)
- Quality Gate

### Tier 2 - Recommended Patterns
Valuable patterns for specific situations:
- Progress Updates
- Dependency Declaration
- Tool Selection Logic
- Pre-flight Check
- State Declaration
- Validation Before Expensive Ops
- Rollback Instructions
- Guardrail Coordination

### Tier 3 - Situational Patterns
For advanced or specific use cases:
- Batch Processing Strategy
- Feedback Loop Design
- Human-in-Loop Protocol
- Resource Budget
- Idempotency Design
- Audit Trail

## 📖 How It Works

1. **Analyze** - Reads your file and determines applicable patterns
2. **Recommend** - Suggests patterns based on mode and tier
3. **Preview** - Shows you what would be added
4. **Apply** - Makes changes only with your approval

## 💡 Examples

### Before
```markdown
Create a user dashboard with charts.
```

### After (lite mode)
```markdown
<inputs>
- Required: user_id
- Optional: date_range (default: last 30 days)
</inputs>

<output_contract>
Dashboard with:
- Summary metrics (4 key stats)
- Chart visualization (line chart)
- Format: HTML
</output_contract>

<steps>
1. Validate user_id exists
2. Fetch user data for date_range
3. Calculate summary metrics
4. Generate chart data
5. Render dashboard HTML
</steps>

<scope_fence>
In scope: Read-only dashboard display
Out of scope: Data editing, real-time updates
</scope_fence>
```

## 📚 Documentation

- **[pattern-catalog.md](references/pattern-catalog.md)** - All patterns with descriptions
- **[minimal-templates.md](references/minimal-templates.md)** - Quick insertion templates
- **[applicability-guide.md](references/applicability-guide.md)** - When to use each pattern
- **[integration-guide.md](references/integration-guide.md)** - Formatting and ordering
- **[examples.md](references/examples.md)** - Good and bad examples
- **[llmprog.md](references/llmprog.md)** - Full pattern specifications

## ✅ Best Practices

- Start with **lite mode** for quick wins
- Use **standard mode** for production prompts
- Reserve **full mode** for critical or complex prompts
- Review recommendations critically - not every pattern fits every prompt
- Pattern names are internal scaffolding - user-facing language should be natural

## 🔍 When To Use

✅ **Use apply-patterns when:**
- Your prompt needs more structure
- Outputs are inconsistent
- You want better error handling
- You're preparing for production
- You need clearer scope definition

❌ **Don't use when:**
- Your prompt is already well-structured
- You need to change core functionality (use editing instead)
- You're just experimenting (patterns add overhead)

## 🎓 Learning Resources

New to reliability patterns? Start here:
1. Read [minimal-templates.md](references/minimal-templates.md) for quick templates
2. Review [examples.md](references/examples.md) for real transformations
3. Try lite mode on a simple prompt
4. Explore [applicability-guide.md](references/applicability-guide.md) for deeper understanding

## 🔗 Related Skills

- **[build-with-patterns](../build-with-patterns/)** - Build new skills with patterns from scratch
- **[examine-skill](../examine-skill/)** - Analyze skills for issues before applying patterns

## 📄 License

MIT License - see [LICENSE](../../LICENSE) file for details

---

**Version:** 1.0.0  
**Part of:** [Mindspark Skills](../README.md)
