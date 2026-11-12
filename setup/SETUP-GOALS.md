---
title: "SETUP-GOALS"
type: "knowledge"
created: "2026-11-12"
description: "Target state of the agent environment for the first session. Describes goals, not steps: the harness decides how to reach them, based on its official documentation."
tags:
  - setup
  - environment
---

# Setup Goals

This file describes **what** the environment must look like after setup. **How** to get there is your decision: you run inside the harness, on the user's machine, and you know your own mechanisms best.

## 1. Detect, then confirm

Do not ask open questions. Check first, then ask the user to confirm.

1. **Operating system:** run a terminal command to detect it (e.g. `uname -s` on macOS/Linux, `ver` or `$env:OS` on Windows). If the first command fails, try the other.
2. **Harness:** state which tool you are (e.g. Antigravity, Claude Code, Codex) and its version if you can read it.
3. Show both results in one message: "Detected: <OS>, <harness>. Is this correct?" Wait for the user's confirmation.

If detection fails, say what you tried and only then ask the user directly.

## 2. Target state

Bring the project to a state where:

1. **`AGENTS.md` is the main instruction file** and your harness reads it at session start. If your harness needs another entry file or folder, connect it to `AGENTS.md` and `.agents/` (import, link or reference). Do not copy content.
2. **Rules, skills and subagents live once, in `.agents/`**, as Markdown with YAML frontmatter. Your harness must be able to see them. Patterns for new ones are in `setup/patterns/` and must stay inactive.
3. **Python runs in a project virtual environment in `.venv/`.** Do not create it now. `environment-builder` creates it when the first script is needed. Only check that Python is available and note its version.
4. **`knowledge/Environment.md` records the setup:** operating system, harness and its version (ask the user), Python version, what you created or connected and why, which official documentation pages you used, and the date.
5. **Nothing outside this project folder and no global settings were changed** without the user's explicit approval.

## 3. Ground rules

- Base every configuration decision on the harness's official, current documentation together with `setup/GOOD-PRACTICES.md` (Section 5). Do not rely on memory or guesses. If neither covers something, say so and ask the user.
- Before creating or changing anything, tell the user in one or two plain sentences what you will do and why it helps them, and wait for approval. Do not show file contents, YAML or commands unless asked (see `AGENTS.md`, Section 6).
- Keep it minimal. Add only what your harness needs to reach the target state.
- If a step fails, do not force it. Describe the problem, propose a simpler alternative, and record it in `Environment.md`.

## 4. Finish

1. Confirm the target state in 2–3 plain sentences.
2. Write the first ChangeLog entry: `knowledge/changelog/YYYY-MM-DD_ChangeLog.md` from `ChangeLog_TEMPLATE.md` (telegraphic: done = environment setup; next = project intake).
3. Ask the user to start a **new session**, so the harness loads the new configuration. Give them this exact sentence to paste as the first message of the new session, in a code block:

   ```text
   Skończyłem ustawianie środowiska. Jaki jest mój następny krok?
   ```

Do not open or change `knowledge/Project-Intake.md` in this session.

## 5. Good practices next to the documentation

Read `setup/GOOD-PRACTICES.md` together with the official documentation of your harness. Apply the shared core and only the column of your harness. Most important for setup:

- `AGENTS.md` and `CLAUDE.md` have **no YAML frontmatter**. In Claude Code, `CLAUDE.md` contains exactly one line: `@AGENTS.md`.
- **Never link or copy `.agents/rules/` into a harness rules folder** (e.g. `.claude/rules/`). Rules are activated by the router in `AGENTS.md`, Section 4.
- Skills and subagents: link whole folders where the harness needs it; Codex subagents get TOML wrappers.

If your harness documentation contradicts `GOOD-PRACTICES.md`, tell the user in plain words before deviating and record the decision in `Environment.md`.

**Links:** folder links behave differently on macOS and Windows and may not survive ZIP or cloud-drive sync. If linking fails, tell the user in plain words, propose a fallback and record it in `Environment.md`.
