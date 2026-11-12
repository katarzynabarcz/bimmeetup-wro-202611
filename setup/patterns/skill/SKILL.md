---
name: skill-name
description: "[FILL IN: What the skill does]. Use when the task involves [FILL IN: task type]."
---

# Skill: [FILL IN: Skill Name]

<!--
HOW TO USE THIS PATTERN
1. Copy the folder `skill/` to `.agents/skills/` and rename it (lowercase, hyphens), e.g. `schedule-to-md/`.
2. Set `name` = folder name. Write `description` as a trigger: "Use when ...". English only.
3. Put deterministic work (conversion, calculation, validation) into `scripts/` inside the skill folder.
   The model runs the script, it does not rewrite it. Dependencies go to `requirements.txt` in the skill folder.
4. Keep only `name` and `description` in YAML. Per-harness additions (e.g. folder link for Claude Code): `setup/GOOD-PRACTICES.md`, Section 3.
5. Delete this comment block when done.
-->

## Input

- [FILL IN: e.g. one `.csv` file from `project/`]

## Output

- [FILL IN: e.g. `project/<name>.md` – Markdown table with YAML frontmatter]

## Procedure

1. Check that the input exists and has the expected format. If not, STOP and ask the user.
2. If the skill has scripts: make sure the project virtual environment `.venv/` exists. If not, STOP and ask before creating it.
3. Run the script with the Python interpreter from `.venv/`: `scripts/[FILL IN: script_name].py <input> <output.md>`.
4. Read only the generated output (not the raw input).
5. [FILL IN: agent step, e.g. "Draft the summary using `templates/Output-Report_TEMPLATE.md`."]
6. **STOP (HITL):** show a 2–4 sentence summary and the file link. Wait for the user's approval.

## Rules

- Never write or run scripts from temporary locations. Fix the script in `scripts/` instead of writing a new one.
- If the script fails, report the error and STOP. Do not process the file manually as a workaround.
