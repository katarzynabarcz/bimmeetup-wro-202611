---
title: "GOOD-PRACTICES"
type: "knowledge"
created: "2026-11-12"
description: "Project conventions and verified harness behavior for AGENTS.md, CLAUDE.md, rules, skills, subagents, MCP and plugins in Antigravity, Claude Code and Codex. Read alongside the official documentation of the harness in use."
tags:
  - setup
  - good-practices
  - agents-md
  - rules
  - skills
  - subagents
  - mcp
---

# Good Practices

## 0. How to use this file

- Read it **alongside** the official documentation of your harness, not instead of it.
- This file holds two things: **conventions** of this project (one source of content in `.agents/`, a router in `AGENTS.md`) and **behavior verified** in the documentation and tests as of 2026-11-12.
- The official documentation is the source for everything this file does not cover, and for newer harness versions.
- If the documentation of your harness version contradicts this file, do not pick silently. Tell the user in plain words, propose what to do, and record the decision in `knowledge/Environment.md`.
- Each section: **shared core** (the same in every harness, goes into `.agents/` via `setup/patterns/`) and **per-harness additions** (apply only the column of the harness in `Environment.md`).

## 1. Main instruction file: AGENTS.md and CLAUDE.md

**Shared core**
- `AGENTS.md` in the project root is the single source of project instructions and the router to rules, skills and subagents.
- **No YAML frontmatter** in `AGENTS.md`, `CLAUDE.md` or `GEMINI.md`. Start with a plain Markdown heading. Antigravity does not support YAML there; in the other harnesses it is wasted context.
- Short: under 200 lines (project convention). Concrete and checkable ("Run `<command>` before finishing" instead of "Test your changes").
- Only what the agent cannot discover itself: commands, conventions, prohibitions, error-prone places.
- Standing rules written directly; anything that applies to some tasks only goes to a separate file with a router row.
- Plain links and explicit "read this file" instructions. No `@file` imports inside `AGENTS.md`.
- No secrets, no private absolute paths.
- Text is context, not a lock. Actions that must be blocked need permissions or hooks, not instructions.

**Per-harness additions**

| | Antigravity | Claude Code | Codex |
|---|---|---|---|
| Entry file | `AGENTS.md` natively | `CLAUDE.md` with **exactly one line**: `@AGENTS.md` | `AGENTS.md` natively |
| Do not | add `GEMINI.md` copies | add YAML, prose like "read AGENTS.md", or a symlink `CLAUDE.md → AGENTS.md` | rely on `@` imports (unsupported) |
| Size limit | 24 KB per file; 20,000-token rules budget | under 200 lines recommended | 32 KiB total by default |
| Check | ask about project rules in a fresh session | `/context` → Memory files list `CLAUDE.md` and `AGENTS.md` | prompt "Summarize the current instructions." |

Claude Code: instructions only for Claude Code may go under the import in a `## Claude Code` section. Default: add nothing there.

## 2. Rules

**Shared core**
- One rule = one file in `.agents/rules/`, one topic, short imperative bullets. Pattern: `setup/patterns/rule.md`.
- Each rule has its own descriptive file name (e.g. `meeting-minutes-style.md`, `ifc-parameter-names.md`). The name does not encode the type.
- The type is stated by the `description`. Three types, with typical description wording:

| Type | Typical `description` |
|---|---|
| Always-On | `Always apply <topic> rules.` |
| Task-Based | `Use when the task involves <task type>.` |
| File-Based | `Use when creating or editing .<filetype> files.` |

- **Every rule gets a row in the router** in `AGENTS.md`, Section 4 ("When → file to read"). The condition repeats the `description`.
- Short standing rules (1–3 lines) go directly into `AGENTS.md`, not into a rule file. Never the same content in both places.
- No `alwaysApply`, no `paths` in rule headers.

**Per-harness additions**

| | Antigravity | Claude Code | Codex |
|---|---|---|---|
| How a rule activates | YAML `trigger`: `always_on`, `model_decision`, `glob` (+ `globs: "**/*.<ext>"`), `manual`. Unknown value = rule silently dropped | Router row in `AGENTS.md` only | Router row in `AGENTS.md` only |
| Do not | put README or unfinished patterns in `.agents/rules/` (every `.md` there is scanned) | **link or copy `.agents/rules/` into `.claude/rules/`**: every rule loads in every session, `trigger` is ignored | expect `trigger` to do anything |

Check: in a fresh session run one task that matches the rule and one that does not.

## 3. Skills

**Shared core**
- One folder per skill: `.agents/skills/<skill-name>/SKILL.md`, optional `scripts/`, `references/`, `assets/`. Pattern: `setup/patterns/skill/`.
- YAML: only `name` and `description`. `name` = folder name, lowercase with hyphens, unique.
- `description`: `<What the skill does>. Use when the task involves <task type>.` Short.
- Skills need no router row in `AGENTS.md`; harnesses discover them by `description`. Matching does not guarantee selection.
- Script paths relative to the project root.
- Never keep pattern or example skills inside `.agents/skills/`: they are discovered as real skills.

**Per-harness additions**

| | Antigravity | Claude Code | Codex |
|---|---|---|---|
| Discovery | `.agents/skills/` natively | only `.claude/skills/` → link the **whole folder**: `.claude/skills → ../.agents/skills` (relative) | `.agents/skills/` natively |
| Manual call | ask for the skill by name | `/<skill-name>` | `$<skill-name>` or `/skills` |
| Extra YAML | none | `allowed-tools`, `paths`, `context` work only here; keep them out of shared files | none |
| Notes | duplicate names: higher priority wins | description cut above 1,536 characters; new skills visible in a new session | many skills: descriptions shortened, some may be skipped |

## 4. Subagents

**Shared core**
- One file per subagent: `.agents/agents/<agent-name>.md`. Pattern: `setup/patterns/subagent.md`.
- YAML: `name`, `description`, `model: inherit`. Nothing else in the shared file.
- `description`: `<What the agent does>. Use when the task involves <task type>.`
- Body: role (one sentence), steps, output returned to the main agent.
- A subagent does not see the main conversation. Everything it needs is in its file or in the delegation message.
- Add a row to the Subagents table in `AGENTS.md` ("When → Delegate to"). Codex delegates only when instructions say so.
- Never keep README files or patterns in `.agents/agents/`: Antigravity tries to load every `.md` there as a subagent.

**Per-harness additions**

| | Antigravity | Claude Code | Codex |
|---|---|---|---|
| Discovery | `.agents/agents/` natively | only `.claude/agents/` → link the **whole folder**: `.claude/agents → ../.agents/agents` (relative) | only `.codex/agents/<agent-name>.toml` → one wrapper per subagent (below) |
| Delegation | model may call it from the list | automatic by `description`, or `@agent-<name>` | only on explicit request or instruction |
| Model names | `inherit`, `flash`, `pro` (not portable) | `inherit`, `sonnet`, `opus`, `haiku` (not portable; unknown names pass unchecked) | omit `model` in the wrapper: uses the main model |
| Tools | without `tools` a subagent gets a read-only set (verified 2026-11-12: `environment-builder` used only `read_file` and `send_message`). Do not add `tools` to the shared file (names differ per harness). Instead the subagent returns `Files to write` and the main agent writes them (see `AGENTS.md`, Subagents) | without `tools` it gets all tools | inherits from the session |
| Reserved names | `self`, `browser`, `research`, `review-agent` | – | – |

Codex wrapper `.codex/agents/<agent-name>.toml` (repeat `name` and `description` from the `.md`; update both on change):

```toml
name = "<agent-name>"
description = "<What the agent does>. Use when the task involves <task type>."
developer_instructions = """
Read .agents/agents/<agent-name>.md and follow its instructions.
"""
```

Not yet verified: whether a Codex subagent really reads the `.md` before working. Check it.

Check: verify in the session log that the subagent was actually invoked, not role-played by the main agent.

## 5. MCP servers

Add MCP only when the project needs a connection the agent cannot get otherwise. Every server's tools take context.

**Shared core**
- The same server list and the same server names in every harness.
- **Secrets only in environment variables.** No tokens, API keys or passwords in config files. No private absolute paths.
- Remote servers over Streamable HTTP (URL usually ends with `/mcp`, not `/sse`).

**Per-harness additions**

| | Antigravity | Claude Code | Codex |
|---|---|---|---|
| Project file | `.agents/plugins/<plugin-name>/mcp_config.json` + `plugin.json` | `.mcp.json` in the project root | `.codex/config.toml`, tables `[mcp_servers.<name>]` |
| Secrets | local servers inherit environment; remote: `${VAR}` in `url`/`headers` after declaring it in `variables` | `${VAR}` in `command`, `args`, `env`, `url`, `headers`; remote needs `type` | `env_vars = [...]`, `bearer_token_env_var`; no `${VAR}` |
| Condition | plugin detected and enabled (on by default) | user approves on first use | project marked as trusted |
| Check | Additional Options → MCP Servers | `claude mcp list`, `/mcp` | `codex mcp list`, `/mcp` |

Check: after setup, call one tool from each server.

## 6. Plugins

Not needed in a single project: rules, skills, subagents and MCP work without them. Use a plugin only to share a set across projects or a team. If the user asks for one, read the official plugin documentation of their harness first.

Verified facts to keep in mind:
- Only skills (`skills/<skill-name>/SKILL.md`) are fully shared between harnesses inside a plugin.
- Antigravity detects `.agents/plugins/<plugin-name>/` (with `plugin.json`) and enables it by default.
- Claude Code does not scan `.agents/plugins/`; it needs `.claude-plugin/marketplace.json` and installation via `/plugin`. Plugin skills get a prefix: `<plugin-name>:<skill-name>`.
- Codex needs `.agents/plugins/marketplace.json` and installation via `/plugins`; it runs a cached copy, so source changes need a refresh.
- No paths or links pointing outside the plugin folder.

## 7. Patterns and examples

- Patterns and examples live in `setup/patterns/`, never in `.agents/`. Inside `.agents/` they would be loaded as real rules, skills or subagents.
- Patterns contain only the shared core. Per-harness additions from this file are added when a pattern is copied into `.agents/`.

## Sources

Official documentation checked on 2026-11-12. Confirm behavior in the harness version in use.

- Antigravity: https://antigravity.google/docs/rules/ · https://antigravity.google/docs/skills · https://antigravity.google/docs/subagents · built-in docs `agy-customizations`, `migrate-agent-settings`
- Claude Code: https://code.claude.com/docs/en/memory · https://code.claude.com/docs/en/skills · https://code.claude.com/docs/en/sub-agents · https://code.claude.com/docs/en/mcp · https://code.claude.com/docs/en/plugins-reference
- Codex: https://learn.chatgpt.com/docs/agent-configuration/agents-md · https://learn.chatgpt.com/docs/build-skills · https://learn.chatgpt.com/codex/agent-configuration/subagents · https://learn.chatgpt.com/docs/extend/mcp · https://developers.openai.com/plugins/build/plugins
