# Worked Example (Prompt) — Lite Mode

This is a small, complete example of a **lite-mode** standalone prompt produced using the shared passes.

---

## Scenario

**User request:** “Given my daily journal entry, extract what I should do tomorrow.”

**Mode:** lite

---

## Example Prompt (copy/paste)

```text
# Tomorrow Plan Extractor

## Purpose
Turn a journal entry into a compact plan for tomorrow.

## Inputs First:
required: journal_entry
optional: timezone=UTC, max_tasks=5
validation: journal_entry must be non-empty
missing → ask the user to paste their journal entry

## Step Contract:
1) Read journal_entry → extract concrete obligations and intents.
2) Convert to tasks → write up to max_tasks tasks.
3) Identify blockers → list up to 3 blockers.
4) Output → format exactly as the Output Schema.

## Decision Points:
If journal_entry contains explicit dates/times → preserve them.
Else → propose times only if user implied urgency.

## Quality Gates:
G1: tasks count ≤ max_tasks
G2: each task starts with a verb and is actionable
G3: output matches Output Schema

## Output Schema:
format: markdown
sections:
- Tomorrow (date)
- Tasks (checkbox list)
- Blockers (bullets)
constraints:
- no extra sections

## Stop Conditions:
done when: output produced and all gates pass (user can copy tasks and execute tomorrow)
don't: add long analysis or background

## Example:
Input: "Today was chaotic..."
Output: (shows the required sections)

```
