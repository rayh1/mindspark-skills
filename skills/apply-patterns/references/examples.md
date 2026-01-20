# Pattern Integration Examples

## Good Example: Adding Inputs First to a skill

```xml
<inputs_first>
**Required inputs:**
- `config_path`: Path to configuration file

**Optional inputs:**
- `verbose`: Enable detailed logging (default: false)

**Input validation:**
- config_path must exist and be valid JSON
</inputs_first>
```

**Why good:** Clear structure, required vs optional, validation rules explicit.

## Bad Example: Adding Inputs First to a skill

```xml
<inputs_first>
The skill needs a config file and maybe some options.
</inputs_first>
```

**Why bad:** Vague, no structure, no validation, doesn't follow pattern format.

---

## Good Example: Adding Quality Gates to a prompt

```text
Quality Gates:
G1 (after Step 2): Input parsed correctly? Required fields present?
G2 (after Step 4): Output matches schema? No validation errors?

If gate fails: log error, attempt fix once, re-check. If still failing: ERROR: GATE_FAIL.
```

**Why good:** Specific gates tied to steps, clear pass/fail criteria, failure handling.

## Bad Example: Adding Quality Gates to a prompt

```text
Quality Gates:
- Make sure everything is correct
- Check that it works
```

**Why bad:** Vague criteria, not tied to steps, no failure handling.

---

## Good Example: Integrating multiple patterns coherently

```xml
<inputs_first>
**Required:** target_file
**Validation:** must exist
</inputs_first>

<step_contract>
1. Read target_file → extract data
2. Process data → generate output
3. Validate output → check constraints
</step_contract>

<quality_gates>
G1 (after Step 1): File read successfully? Data extracted?
G2 (after Step 3): Output valid? Constraints met?
</quality_gates>
```

**Why good:** Patterns reference each other, steps align with gates, coherent structure.

## Bad Example: Patterns that conflict

```xml
<step_contract>
1. Read file
2. Process
3. Output
</step_contract>

<quality_gates>
G1 (after analysis phase): Check results
G2 (after generation): Verify output
</quality_gates>
```

**Why bad:** Gate names don't match step names, unclear which gates apply to which steps.

---

## Integration Principle

When integrating patterns, ensure they reference each other consistently:
- Step names in Step Contract should match references in Quality Gates
- Stop Conditions should reference defined steps
- Quality Gates should reference specific step numbers or names from Step Contract
