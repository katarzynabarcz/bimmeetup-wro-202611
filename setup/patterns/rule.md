---
trigger: model_decision
description: "Use when the task involves [FILL IN: task type]."
---

# Task-Based Rule: [FILL IN: Topic]

<!--
HOW TO USE THIS PATTERN
1. Copy this file to `.agents/rules/` and give it its own descriptive name (e.g. `meeting-minutes-style.md`).
   Pick the type and state it in `description` and `trigger`:

   | Type       | YAML                                         | Typical description                                |
   | Always-On  | trigger: always_on                           | "Always apply <topic> rules."                      |
   | Task-Based | trigger: model_decision                      | "Use when the task involves <task type>."          |
   | File-Based | trigger: glob + globs: "**/*.<filetype>"     | "Use when creating or editing .<filetype> files."  |

2. ALWAYS add one row to the router in AGENTS.md, Section 4 (same condition as `description`).
   - `trigger` / `globs` are read only by Antigravity. Codex and Claude Code find the rule only through the router.
   - Never link or copy the rules folder into a harness rules folder (e.g. `.claude/rules/`): the harness would load every rule all the time.
3. Short standards that always apply and fit in 1–3 lines go directly into AGENTS.md, not into a rule file.
4. Per-harness details: `setup/GOOD-PRACTICES.md`, Section 2.
5. One rule = one topic. Short imperative bullets. Do not repeat AGENTS.md.
6. Delete this comment block when done.
-->

## Scope

- Applies to: [FILL IN: task type, folder or file type]
- Does not apply to: [FILL IN: optional]

## Must

- [FILL IN: e.g. Use the structure from `templates/Output-Report_TEMPLATE.md`.]
- [FILL IN: e.g. Use names and abbreviations from the glossary in `knowledge/ProjectContext.md`.]

## Must Not

- [FILL IN: e.g. Never invent dates, amounts or names that are not in the source file.]

## STOP Gate

- STOP and ask the user when: [FILL IN: e.g. the source has no date / two sources contradict each other].
