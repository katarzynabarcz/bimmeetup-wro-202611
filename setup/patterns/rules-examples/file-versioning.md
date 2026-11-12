---
trigger: always_on
description: "EXAMPLE – Always apply file versioning rules (_vXX.YY, archive)."
---

# File Versioning

## 1. Format

- Pattern: `<BaseName>_v<Major>.<Minor>.<ext>`, e.g. `Meeting-Minutes_v01.01.md`.
- Two digits for each part. Underscore before `v`, dot between major and minor.
- Keep the YAML `title` identical to the filename without extension.

## 2. Working Drafts

- First draft always starts at `v01.01` (never `v1`, never `v01.00`).
- Each revision cycle: increment minor (`v01.02`, `v01.03`, ...).
- Never overwrite an existing version. Create the next version as a new file.
- Set YAML `source` of the new version to the path of the previous version.

## 3. Approved Versions

- `vXX.00` is reserved for versions the user explicitly approves as final.
- Reopening an approved `v01.00` for substantial changes starts a new cycle at `v02.01`.

## 4. Archive

- Keep the previous version next to the new one until the user accepts the new version.
- After acceptance, move older versions to an `_archive/` subfolder in the same folder. Never delete them.
- Never replace a working file before its new version has been tested on a real sample.

## 5. Exceptions (no version suffix)

- `AGENTS.md`, `.agents/rules/*`, `setup/**`, `.agents/skills/**/SKILL.md`, `.agents/agents/*`, `README.md`, harness entry files (e.g. `CLAUDE.md`).
- Template files: `<Name>_TEMPLATE.md`.
- ChangeLog entries: `YYYY-MM-DD_ChangeLog.md` (one file per day, updated in place).
- History of unversioned files is tracked in `knowledge/changelog/`.
