# Optional Add-ons (Use Only When Triggered)

If **none** of these triggers match, do **not** add the pattern.

| Add-on | Use it when… (measurable trigger) | What it adds |
|---|---|---|
| Scope Fence (in/out) | User request mentions **3+** distinct features/outputs, **or** uses open-ended language (“and more”, “anything else”), **or** no non-goals are stated | Explicit in-scope / out-of-scope bullets |
| Mode Selection | User describes **2+** distinct workflows (“create AND update”, “summarize OR quiz”), **or** asks for multiple modes/variants | Upfront choice list + routing |
| Clarifying Questions | Any required input is missing, **or** constraints conflict (e.g., “ultra short” + “include everything”) | 1–3 focused questions, asked in one batch |
| Addressable Output (IDs) | Output contains a list of **5+** items that the user is likely to iterate on individually | Stable IDs like `[Q-1]`, `[TERM-3]` |
| User Approval Gate | Action is **destructive/irreversible** (overwriting files, deleting content), **or** touches files outside the requested artifact | “Approve / edit / cancel” checkpoint |
| Fallback Chain | Plan depends on an external tool/API/network, **or** parsing a format that may fail | primary → fallback → terminal error surface |
| Review Step | Output is **150+ lines**, **or** spans **2+ files**, **or** user explicitly asks for “comprehensive” | Post-build coherence + constraint check |
| Observability (trace) | Workflow is **10+** steps, **or** user wants debuggability/run comparison | step ledger + brief run header |
| Confidence Signal | You must guess due to missing info, **or** user asks for correctness-critical domains (legal/medical/financial) | Explicit uncertainty + what would reduce it |

## Rule
If you can’t point to a trigger, don’t add the add-on.
