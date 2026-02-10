# Step Ledger Guide

Update after each major step to track progress and support observability.

---

## State Tracking

**done:** [completed steps in current iteration]

**next:** {current step number and name}

**framework_iteration:** {current framework being executed}

**used_frameworks:** [list of frameworks already executed in this session]

**decisions:** [key choices made: auto-detected framework, user override, framework-specific decisions]

**open_issues:** [blockers, failed gates, missing inputs]

---

## Loop Tracking (Multi-Framework Sessions)

**iteration_count:** {number of frameworks analyzed so far}

**decision_context:** {original decision context preserved across all iterations}

**locked_recommendations:** [{framework: recommendation} for each completed framework]

---

## Update Frequency

- **After step 3 (interactive selection):** log framework choice + whether auto or user-selected
- **After step 4 (framework execution):** log framework completion + recommendation
- **After step 7 (multi-framework offer):** log user choice (new framework or Done)
- **On gate failure:** log which gate failed + issue

---

## Purpose

- **Prevents context drift** in long multi-framework sessions
- **Enables resumption** if session interrupted
- **Supports observability** for metric tracking

---
