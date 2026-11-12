# AGENTS.md – [FILL IN: Project Name]

> Status: TEMPLATE. Sections marked `[FILL IN]` are completed by `environment-builder` together with the user.

## 0. Setup (first session only)

If `knowledge/Environment.md` does not exist, the environment is not set up yet. Do nothing else: read `setup/SETUP-GOALS.md` and follow it.

If `knowledge/Environment.md` exists, read it first. It tells you which operating system and harness this project runs on and how the harness is connected to `.agents/`.

## 1. Project Goal

[FILL IN: 1–3 sentences. What does this agent system do, for whom, and what is the final result?]

- **Input:** [FILL IN]
- **Output:** [FILL IN]
- **Human approval (HITL):** [FILL IN: where a human must review before anything leaves the project]

Source of answers: `knowledge/Project-Intake.md`.

## 2. Folder Map

| Path | Content | When to read |
|---|---|---|
| `.agents/rules/` | Rules: standards and constraints (Markdown + YAML) | See Section 4 |
| `.agents/skills/` | Skills: step-by-step procedures (`SKILL.md` + optional `scripts/`) | When the task matches the skill `description` |
| `.agents/agents/` | Subagent definitions | When work is delegated |
| `setup/` | Setup goals, good practices per harness, inactive patterns for new rules, skills and subagents | Setup, or when creating or connecting a rule, skill, subagent or MCP server |
| `knowledge/` | Project facts, intake form, environment record | When needed for the task |
| `knowledge/changelog/` | Session history (done, to do, decisions) | At session start: the latest dated entry only |
| `templates/` | Output document templates | When creating output documents |
| `project/` | Working files of the project: inputs, drafts, outputs. Subfolders are designed by `environment-builder` | When the task works on project files |

## 3. Global Constraints

1. Never open or read `README*` files. They are written for humans (`audience: "human-only"`).
2. Base every decision about harness configuration on the harness's official, current documentation together with `setup/GOOD-PRACTICES.md`. Do not guess. If neither covers it, say so and ask the user.
3. Run Python only through the project virtual environment in `.venv/`. Create it only when the first script is needed, and ask before creating it.
4. Never write scripts to temporary locations. Save reusable scripts inside the project (e.g. `.agents/skills/<skill-name>/scripts/`).
5. Scripts are written once and reused. Before writing one, check `.agents/skills/*/scripts/` for a script that already does the job and use it. Keep every script in the `scripts/` folder of the skill that uses it. Fix a broken script instead of writing a new one. Never delete a script after use.
6. Never read PDF, DOCX, XLSX or large CSV files raw. Convert them to Markdown with a script first (shared converters live in skill `file-to-markdown`; `environment-builder` creates it when first needed) and read only the `.md` result.
7. HITL STOP gates: if information is missing, ambiguous or contradictory, STOP and ask. Never guess. Every output document is a DRAFT until the user approves it.
8. Ask before changing anything outside this project folder or in global settings.
9. Every new `.md` file outside `.agents/` starts with YAML frontmatter in English: `title`, `type`, `created`, `description`, `tags`.
10. One rule lives in one place. Connect harness-specific files to `AGENTS.md` and `.agents/`; do not copy their content.

## 4. Rules

Rules live in `.agents/rules/`. This table is the router for every harness. Before starting a matching task, read the listed file and apply it. Do not read the whole rules folder in advance.

| When | File to read |
| :--- | :--- |
| [none yet – `environment-builder` adds rows here] | |

Typical "When" wording: "Always, at the start of every session" / "When the task involves <task type>" / "Before creating or editing .<filetype> files". Each rule file has its own descriptive name.

## 5. Routing

- Environment not set up -> Section 0.
- Environment set up, `knowledge/Project-Intake.md` has `status: "AWAITING"` (e.g. the user asks "what is my next step?") -> set the status to `"IN PROGRESS"`. Invite the user in plain words to fill in `knowledge/Project-Intake.md`: focus only on the answers, the agent handles the header. When finished, they write "Skończyłem". Do not ask the questions in chat.
- Intake `"IN PROGRESS"` and the user says they are finished -> check every section. If any is empty or still `[DO UZUPEŁNIENIA]`, name the missing sections in plain words and wait. If all are filled: set `status: "READY"` and `created` to today's date, write a ChangeLog entry, then delegate to `environment-builder`.
- Intake `"READY"`, project not designed yet, or the user wants to extend or restructure it -> delegate to subagent `environment-builder` (see Subagents below).
- [FILL IN: task type] -> skill `[FILL IN: skill-name]`
- Session start -> read only the latest `YYYY-MM-DD_ChangeLog.md` in `knowledge/changelog/`. Ignore the template and older entries.
- After every completed step (setup, intake, each approved build step), on "save changelog", or when tokens run low -> update today's ChangeLog (one file per day) following the rules in `ChangeLog_TEMPLATE.md`: telegraphic bullets, small in-place edits, "Next" replaced, not appended. Do it silently; do not mention it to the user.

### Subagents

Delegate the task to the subagent by name. Do not perform its role yourself.

If a subagent returns `Files to write` (its tools are read-only in some harnesses), write those files and create those folders exactly as returned: no edits, no extra content, nothing shown to the user. Then relay only its `For the user` and `Question for the user`.

| When | Delegate to |
| :--- | :--- |
| When the task involves designing, building or extending this project (folders, rules, skills, subagents, scripts, templates) | `environment-builder` |

## 6. Talking to the User

The user is new to agent environments. Talk to them as you would to a curious beginner: simple, warm, short.

1. Use the user's language. Use everyday words. If a technical term is unavoidable, explain it in one short sentence (e.g. "A rule is a note that tells me how to work in this project.").
2. Do the technical work in the background. Do not show YAML, file contents, commands, paths or code unless the user asks for them.
3. Ask for approval in one plain sentence that says what the user gains, e.g. "I'll save a short note about your computer, so I don't have to ask again. OK?"
4. One question at a time. Offer a recommended answer when you can.
5. After a step, summarize in 2–3 plain sentences: what is ready and what happens next. No file lists.
6. If something fails, say what it means for the user and what you propose. No error dumps.

## 7. Language

- Agent configuration files and YAML: English.
- Conversation with the user: the user's language ([FILL IN: e.g. Polish]).
- Output documents: [FILL IN: e.g. Polish].
