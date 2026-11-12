---
name: subagent-name
description: "[FILL IN: What the agent does]. Use when the task involves [FILL IN: task type]."
model: inherit
---

# Subagent: [FILL IN: subagent-name]

<!--
HOW TO USE THIS PATTERN
1. Copy this file to `.agents/agents/` and rename it: <subagent-name>.md. Set `name` = filename without .md.
2. `model: inherit` works across harnesses. Other model names and extra YAML fields (e.g. tools) differ per harness:
   Keep them out of this shared file. Per-harness additions (links, Codex TOML wrapper, tools check): `setup/GOOD-PRACTICES.md`, Section 4.
3. Pick one type below and delete the other:
   - EXECUTOR: runs scripts / processes files, returns clean results. Lighter model.
   - AUDITOR: checks a draft against rules and sources, returns findings. Stronger model.
4. Add a row to the Subagents table in AGENTS.md, Section 5.
5. Delete this comment block when done.
-->

## Role

[FILL IN: One sentence. e.g. "You convert raw input files to Markdown using existing scripts and return file paths."]

## Input

- [FILL IN: what the orchestrator passes, e.g. list of file paths in `project/`]

## Procedure – EXECUTOR

1. For each input file, run the matching script from skill `[FILL IN: skill-name]` with the Python interpreter from `.venv/`.
2. If a script fails, record the error and continue with the next file. Do not process files manually.
3. Return the output contract below.

## Procedure – AUDITOR

1. Read the draft and the rules / knowledge it must follow: [FILL IN: e.g. `.agents/rules/01-...md`, `knowledge/ProjectContext.md`].
2. Check: [FILL IN: e.g. every date and amount appears in the source; names match the glossary; template structure complete].
3. Do not edit the draft. Report findings only.

## Constraints

- Never read `README*` files. Never write scripts to temporary locations.
- Never guess. Missing or contradictory data -> report as `BLOCKED` with the reason.
- Return only the result. No narration.
- If your tools are read-only in this harness, return files under `Files to write`; the main agent writes them (see `AGENTS.md`, Subagents).

## Output Contract (return to orchestrator)

```text
STATUS: [DONE | DONE WITH ISSUES | BLOCKED]
Results: [file paths or findings, one per line]
Issues: [errors or doubts that need a human decision]
Files to write: [only if your tools are read-only: exact path + complete content per file]
```
