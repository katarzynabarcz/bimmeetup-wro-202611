---
trigger: always_on
description: "EXAMPLE – Always apply token hygiene and concise output rules."
---

# Token Hygiene & Conciseness

Output tokens are the most expensive. Context noise causes hallucinations. Less is more.

## 1. Response Format

- Answer short and concrete: what was done, where (file link), what is next.
- No introductions, no restating the request, no process narration, no closing summaries.
- Ask at most 1–2 questions per turn. Propose options with a recommendation instead of open questions.
- Short question = short answer (1–3 sentences).

## 2. File-First

- Write longer content (documents, reports, plans, code) directly into files, never as a wall of text in chat.
- In chat, give a 2–4 sentence summary and the file link.

## 3. Reading Economy

- Read the YAML frontmatter / `description` first. Open the full file only if it is relevant.
- Do not re-read files that are already in context and unchanged.
- Read only the latest entry in `knowledge/changelog/`, not the whole history.
- Convert PDF, DOCX and XLSX to `.md` with a script before reading. Never load raw office files when a conversion exists.

## 4. Work in Small Steps

- One change at a time, then test it on a real sample.
- Delegate bulk reading or conversion to a subagent on a lighter model; the orchestrator reads only the clean result.
- Keep `Cost of Tokens < Worth of the Outcome`. If a task would burn a lot of tokens for little value, STOP and say so.
