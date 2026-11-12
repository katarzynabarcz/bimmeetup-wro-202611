---
name: project-setup
description: "Use when turning this starter into a working agent project based on the user's intake form, or when extending an existing project with new folders, rules, skills, subagents, scripts or templates."
---

# Skill: Project Setup (Starter -> Working Project)

Procedure used by the `environment-builder` subagent. Every phase ends with a STOP gate and a ChangeLog entry, so each phase can run in a separate conversation.

## Phase 0 – Check State

1. Read `knowledge/Environment.md`. If it does not exist, STOP: setup is not done yet (see `setup/SETUP-GOALS.md`).
2. Read the latest dated ChangeLog in `knowledge/changelog/` (ignore the template). Resume from its "Next" list.
3. If `knowledge/ProjectBlueprint_v*.md` exists, read the newest version and continue from the first unfinished phase.

## Phase 1 – Intake Gate

The user never edits the YAML header. The agent manages `status`: `AWAITING` -> `IN PROGRESS` -> `READY` (see `AGENTS.md`, Section 5).

1. Read `knowledge/Project-Intake.md`.
2. If `status` is not `"READY"`, or any section still contains `[DO UZUPEŁNIENIA]` or is empty: STOP. Return the missing sections in plain words and remind the user to write "Skończyłem" when done. Do not ask the questions in chat.
3. If the user said they are finished and every section is filled in, but `status` is still `"IN PROGRESS"`: set `status: "READY"` and `created` to today's date yourself.
4. Continue only at `"READY"`.

## Phase 2 – Grill-me

1. Review every answer. Mark it as vague if it lacks a concrete example, number, file name, format or owner.
2. Ask about vague answers **one question at a time**, most important first (sections 1, 2, 3, 10 before the rest). Ask for something concrete.
3. After each answer, update the matching section in `Project-Intake.md`.
4. Flag risks immediately: sensitive data (section 8), unclear "done" criteria (section 10), steps with no human check (section 6).
5. If the task is low on repetition or value, say so and propose a simpler solution (one prompt, a plain script).
6. Finish when no vague answers remain. Show a 3–5 sentence summary. STOP.

## Phase 3 – Blueprint

1. Classify every element:
   - Always needed, 1–3 lines -> `AGENTS.md`
   - Standard needed in every conversation -> rule, always active
   - Standard needed only for some tasks or file types -> rule, conditional
   - Step-by-step procedure -> skill
   - Needs separate context or a lighter model -> subagent
   - Deterministic work (conversion, calculation) -> Python script in the skill's `scripts/`
   - Input in PDF, DOCX, XLSX or large CSV -> conversion-to-Markdown script first, in skill `file-to-markdown` (reuse an existing script if there is one)
   - Fact, glossary, project data -> `knowledge/`
   - Output with a fixed layout -> `templates/`
2. Write `knowledge/ProjectBlueprint_v01.01.md` (YAML `type: "notes"`) with:
   - Goal, input, output (3 lines).
   - Minimum viable result (from intake section 10) and how to check it.
   - Process flow: numbered steps, each marked `SCRIPT`, `AGENT` or `HUMAN (STOP)`.
   - Element table: `Element | Type | Path | Purpose`.
   - Harness notes: what each element needs to be visible in this harness (from `setup/GOOD-PRACTICES.md`, `Environment.md` and official documentation).
   - Project subfolders inside `project/` (only those the process needs).
   - Build order. Minimum viable result first.
3. **STOP:** explain the plan to the user in 3–5 plain sentences (what the system will do for them, what they will approve along the way). Do not show the Blueprint content unless asked. Wait for approval. Apply changes as `v01.02`, etc.

## Phase 4 – Build (one step per turn, or per conversation)

Follow the approved Blueprint. After each step: short report, STOP for approval, ChangeLog entry.

1. **AGENTS.md + folders.** Fill in Goal, Folder Map, Rules (Section 4), Routing, Language. Create project subfolders inside `project/` and add each one to the Folder Map. No README files.
2. **Knowledge.** Create `knowledge/ProjectContext.md` from `ProjectContext_TEMPLATE.md`. Only facts the user confirmed; unknowns as `[TO CONFIRM]`.
3. **Templates.** Create output template(s) in `templates/` from `Output-Report_TEMPLATE.md`. Keep `STATUS: DRAFT`.
4. **Rules.** Create only rules the Blueprint lists, from `setup/patterns/rule.md` (examples in `setup/patterns/rules-examples/`). Give each file its own descriptive name; state the type in `description` and set `trigger` for Antigravity. **Always** add a row to the router in `AGENTS.md`, Section 4: in Codex and Claude Code this row is the only activation. Never link the rules folder into a harness rules folder (see `setup/GOOD-PRACTICES.md`, Section 2).
5. **Scripts + skill.**
   - Ask for one real sample input file from the user (placed in `project/`). Never create, generate or invent input data, not even "for testing". No sample -> STOP and ask.
   - Ask before creating the virtual environment. Create `.venv/` in the way that fits the operating system in `Environment.md`. Right after creating it, update `Environment.md`: `.venv/` created, interpreter path, date.
   - Check `.agents/skills/*/scripts/` first; reuse or extend an existing script instead of writing a new one.
   - Copy `setup/patterns/skill/` to `.agents/skills/<skill-name>/`. Write the script in its `scripts/`, dependencies in its `requirements.txt`.
   - Write the `SKILL.md` procedure that calls the script.
   - Test the script on the user's sample file. Show the result in plain words and check it against the criterion from intake section 10. STOP.
   - This step is done only when the script ran on the user's file and the user approved the result. Do not mark it done or move the test to a later step.
6. **Subagents.** Only if the Blueprint needs separate context or a lighter model. Start from `setup/patterns/subagent.md`. Create any harness-specific wrapper the documentation requires.
7. **Visibility check.** Ask the user to start a new session if the harness needs it. Confirm that new rules, skills and subagents are visible.
8. **Dry run.** Run the minimum viable result on one real sample. List what failed. Fix one thing at a time.

## Constraints

- Build only what the Blueprint lists. No speculative files.
- Do not generate personal style or tone rules unless the user explicitly asks.
- One rule in one place. If two files would say the same thing, keep it in one and reference it.
- Never read `README*` files. Do not create new ones.
- Never delete `project-setup/`, `environment-builder.md` or `setup/patterns/`. They are needed for later extensions.
