# Error Taxonomy

## Input errors (Step 1)

| Code | Meaning | Next Action |
|------|---------|-------------|
| `INPUT_NOT_FOUND` | target_path doesn't exist | Ask user to verify path |
| `INPUT_NOT_READABLE` | File exists but can't be read | Check permissions, report |
| `INPUT_EMPTY` | File is empty | Report, suggest creating content first |
| `REFERENCE_MISSING` | llmprog.md not found | Check skill installation |

## Analysis errors (Step 2-3)

| Code | Meaning | Next Action |
|------|---------|-------------|
| `TYPE_AMBIGUOUS` | Can't determine skill vs prompt | Ask user to clarify |
| `PARSE_ERROR` | Target has malformed structure | Report location, suggest fix |
| `PATTERN_CONFLICT` | Existing patterns contradict | Ask user how to resolve |
| `PATTERN_OVERLAP` | Existing content maps to multiple patterns | Ask user which pattern to formalize, note others as conflicts |

## Integration errors (Step 6-8)

| Code | Meaning | Next Action |
|------|---------|-------------|
| `GATE_FAIL` | Quality gate failed after retry | Report gate name, stop or ask user |
| `STRUCTURE_INVALID` | Integration broke file structure | Revert, try different approach |
| `TAG_UNCLOSED` | XML tag not properly closed | Fix tag, re-check |
| `CONTENT_CONFLICT` | New pattern conflicts with existing | Ask user to resolve |

## Process errors

| Code | Meaning | Next Action |
|------|---------|-------------|
| `USER_REJECTED` | User rejected all patterns | Output analysis-only report |
| `USER_CANCELLED` | User cancelled before completion | Preserve state, report progress |
| `MAX_CYCLES` | Hit max_cycles limit with unresolved issues | Report remaining issues, finalize |
| `SCOPE_VIOLATION` | Attempted out-of-scope change | Revert, note in issues |

## Error Response Format

```
ERROR: {CODE}
Context: {what was being attempted}
Details: {specific information}
Next action: {suggested resolution}
```

## Recovery Priority

1. Attempt automatic fix (once)
2. Ask user for guidance
3. Revert to safe state
4. Report error with full context
