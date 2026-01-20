# Optional Patterns (Add When Triggered)

Core structure comes from the **7 required patterns**. Add these only when the trigger condition is true.

| Trigger | Pattern | One-liner |
|---------|---------|-----------|
| Output has list items needing follow-up? | Addressable Output | Add `[F-1]`, `[R-2]` style IDs |
| Multiple distinct workflows? | Mode Selection | Present choices upfront |
| Will ask user questions when blocked? | Clarifying Questions | Define when/how to ask |
| Scope could creep? | Scope Fence | Explicit in/out boundaries |
| Irreversible actions? | User Approval Gate | Pause before destructive ops |
| High-stakes decisions? | Confidence Signal | Surface uncertainty levels |
| External deps might fail? | Fallback Chain | Primary → fallback → error |
| Handles secrets or needs error taxonomy? | Error Handling | Codes + redaction rules |
| Complex output needs coherence check? | Review Step | Post-completion review cycle |
| Need multiple perspectives? | Lens | Analyze from N viewpoints |
| Workflow 10+ steps? | Step Ledger | Track done/next/decisions |
| Need run comparison/debugging? | Observability | Run headers + step traces |
| Audit trail required? | Assumption Registry | Log assumptions with confidence |
| Very long task, context drift risk? | Context Window | Explicit memory management |
| Ambiguous requirements? | Interpretation Check | Restate + confirm before starting |

**Usage:** Scan triggers during Pass 4–5. If none apply, skip.

## Appendix
Detailed templates and expanded guidance were moved to `pattern-scaffold-appendix.md`.
