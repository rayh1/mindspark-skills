# Pattern Catalog (Tiered)

Quick reference for pattern checking. See [llmprog.md](llmprog.md) for full details.

Patterns are organized into three tiers by priority:
- **Tier 1 - Core:** High-yield, low-overhead. Apply to any non-trivial workflow.
- **Tier 2 - Recommended:** Valuable when situation warrants. Check applicability.
- **Tier 3 - Situational:** Specific use cases. Don't apply by default.

## Table of Contents

- [Tier 1 - Core (8 patterns)](#tier-1---core-8-patterns) - Line 22
- [Tier 2 - Recommended (8 patterns)](#tier-2---recommended-8-patterns) - Line 41
- [Tier 3 - Situational (6 patterns)](#tier-3---situational-6-patterns) - Line 58
- [Skill-Specific Patterns](#skill-specific-patterns) - Line 74
- [Quick Applicability Checklist](#quick-applicability-checklist) - Line 103

---

## Tier 1 - Core (8 patterns)

Always consider these for any non-trivial task.

| # | Pattern | Purpose | Applies when |
|---|---------|---------|--------------|
| 1 | **Step Contract** | Make step sequence explicit | Multi-step work |
| 2 | **Inputs First** | Declare required inputs before work | Takes inputs |
| 3 | **Decision Points** | Make branching explicit | Conditional logic exists |
| 4 | **Quality Gates** | Validation checkpoints between steps | Correctness matters |
| 5 | **Stop Conditions** | Define "done" precisely | Scope could creep |
| 6 | **Scope Fence** | Bound in/out of scope explicitly | Could expand beyond intent |
| 7 | **Interpretation Check** | Verify understanding before starting | Complex/ambiguous requests |
| 8 | **Output Schema** | Define required output structure | Output needs parsing/consistency |

**Gap analysis priority order:** When detecting gaps, consider patterns in this order: Inputs First → Step Contract → Scope Fence → Output Schema → Stop Conditions → Quality Gates → Interpretation Check → Decision Points

---

## Tier 2 - Recommended (8 patterns)

Add these when the situation warrants.

| # | Pattern | Purpose | Applies when |
|---|---------|---------|--------------|
| 9 | **User Approval Gate** | Human oversight at checkpoints | Irreversible actions; significant phases |
| 10 | **Clarifying Questions** | Protocol for asking user | Blocked by missing info |
| 11 | **Confidence Signal** | Surface uncertainty explicitly | Consequential decisions |
| 12 | **Fallback Chain** | Recovery when primary fails | External dependencies |
| 13 | **Lens** | Multi-perspective analysis | Review/analysis tasks; tunnel vision risk |
| 14 | **Addressable Output** | Unique IDs for output items | Output needs follow-up discussion |
| 15 | **Review Step** | Holistic post-completion check | Complex outputs needing coherence |
| 16 | **Step Ledger** | Running execution state | Long/complex enough to drift |

---

## Tier 3 - Situational (6 patterns)

Use when specifically relevant. Require explicit justification.

| # | Pattern | Purpose | Applies when |
|---|---------|---------|--------------|
| 17 | **Mode Selection** | Present workflow choices upfront | Multiple workflows supported |
| 18 | **Example Anchor** | Ground behavior with examples | Style/format hard to describe |
| 19 | **Assumption Registry** | Log assumptions as made | Auditability matters |
| 20 | **Context Window** | Working memory policy | Very long tasks with shifting focus |
| 21 | **Observability** | Run metadata + step traces + artifacts | Debugging/comparison needed |
| 22 | **Error Handling** | Error classification + redaction | Failures need taxonomy; sensitive data |

---

---

## Skill-Specific Patterns

One additional pattern applies only to SKILL.md files that orchestrate MCP servers. Use `[S-3]` ID in proposals.

| # | Pattern | Purpose | Applies when |
|---|---------|---------|--------------|
| S-3 | **MCP Error Handling** | Handle MCP failure modes | Skill orchestrates MCP without connection/auth/tool-name error handling |

**S-3 MCP Error Handling — what to add:**
```markdown
## Troubleshooting

### MCP Connection Failed
If you see "Connection refused": verify server is running in Settings > Extensions,
confirm API key is valid, try reconnecting.

### Authentication Errors
OAuth tokens may need refresh — disconnect and reconnect the integration.
API keys: verify correct permissions/scopes.

### Tool Call Failures
MCP tool names are case-sensitive. To isolate failures, test the MCP tool directly
(without this skill): "Use [Service] MCP to [action]". If that also fails,
the issue is the MCP connection, not this skill.
```
Also add to frontmatter: `metadata.mcp-server: <server-name>` and `compatibility: "Requires [Service] MCP"`

---

## Quick Applicability Checklist

For gap-driven selection guidance (proposal counts by target complexity), see [applicability-guide.md](applicability-guide.md) § Gap-Driven Selection Flow.

**Always applicable (Tier 1):**
- [ ] Takes inputs? → Inputs First
- [ ] Multi-step? → Step Contract
- [ ] Could creep? → Stop Conditions + Scope Fence
- [ ] Output matters? → Output Schema

**Often applicable (Tier 2):**
- [ ] Irreversible actions? → User Approval Gate
- [ ] Analysis/review task? → Lens
- [ ] High-stakes decisions? → Confidence Signal
- [ ] External deps? → Fallback Chain

**Sometimes applicable (Tier 3):**
- [ ] Multiple workflows? → Mode Selection
- [ ] Need debugging? → Observability
- [ ] Audit trail needed? → Assumption Registry
