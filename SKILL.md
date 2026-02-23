---
name: lesson-learned
description: Persistent lesson-learning workflow that stores, retrieves, updates, and restructures bug-prevention knowledge across tasks. Use for most coding and debugging tasks, especially when implementing features, fixing regressions, reviewing code, handling repeated failures, or when the user asks to "remember this for next time." Use to record what failed, why it failed, and how it was fixed; retrieve relevant lessons before implementation; and revise or supersede outdated lessons when new evidence appears.
---

# Lesson Learned

## Purpose

Use a lightweight memory system that helps avoid repeating mistakes.
Read small relevant lesson files before work, apply matching lessons during work, and update memory after work.

## Required Response Suffix

When this skill is used, append this exact one-line suffix at the end of the final user response:

```text
[lesson-learned] used=<yes|no>; read=<n>; applied=<n>; added=<n>; updated=<n>; restructured=<yes|no>
```

Keep values terse. Use integers for counts.

### Suffix Accounting Rules
- Count only runtime memory actions under `$CODEX_HOME/lesson-learned/` (or fallback path).
- Do not count edits to skill source files (`SKILL.md`, `references/*`, `agents/*`) in `added`, `updated`, or `restructured`.
- `read` = number of runtime memory files read from the memory store.
- `applied` = number of lessons from runtime memory concretely applied during execution.
- `added` = number of new lesson entries created in runtime memory.
- `updated` = number of existing lesson entries modified in runtime memory.
- `restructured` = `yes` only when runtime memory taxonomy/files were reorganized.

## Memory Storage Location

Store runtime memory in one global location shared by all projects:

`$CODEX_HOME/lesson-learned/`

If `CODEX_HOME` is not set, use `~/.codex/lesson-learned/`.

Do not use project-local memory folders for normal operation.

Use this structure:
- `$CODEX_HOME/lesson-learned/index.yaml`: category tree, tags, and file pointers
- `$CODEX_HOME/lesson-learned/lessons/`: hierarchical lesson files
- `$CODEX_HOME/lesson-learned/archive/`: superseded entries or old category snapshots

If the folder does not exist, create it with the starter layout from `references/storage-layout.md`.

## Operating Workflow

### 1. Prime Before Work
- Read `$CODEX_HOME/lesson-learned/index.yaml` (or fallback path).
- Select only relevant lesson files by matching task signals (language, framework, error class, task type).
- Always include one general behavior file if present (for communication and validation habits).
- Keep read scope small: usually 1-3 files unless the task is broad.

### 2. Apply During Work
- Convert matched lessons into concrete checks while implementing.
- Prefer prevention checks over post-hoc fixes (input validation, edge-case guards, compatibility checks, test coverage).
- If multiple lessons conflict, prioritize the most recently validated and highest-confidence entry.

### 3. Capture After Work
- Add a lesson entry when any of these are true:
- A bug/regression was fixed.
- The user corrected behavior or pointed out a repeat mistake.
- The user explicitly asked to remember something.
- A non-obvious tactic prevented a likely failure.

### 4. Update or Supersede
- Update an existing lesson when the core rule is still correct but incomplete.
- Supersede a lesson when the old rule is materially wrong.
- Preserve history by moving superseded content to `archive/` and linking replacement IDs.

### 5. Restructure Categories
- Treat taxonomy as fluid, not fixed.
- Trigger category refactor when a file becomes too large, noisy, or mixed-topic.
- Default restructure triggers:
- More than 25 entries in one file.
- Average entry scan time becomes high (hard to find relevant rules quickly).
- Repeated cross-category duplication.
- Presence of project/app-specific names that reduce cross-project retrieval quality.

Refactor by splitting, merging, or nesting categories and then updating `index.yaml`.

### 6. Normalize Project-Specific Lessons
- If a lesson came from one specific app, store the reusable rule in a general category.
- Keep project detail only in `evidence` context, not in category or file names.
- When project-specific files already exist, migrate them to general files on next touch.

## Entry Quality Rules

- Keep each lesson atomic: one failure pattern, one corrective strategy.
- Record both trigger signals and prevention steps.
- Include a brief "apply when" clause so retrieval is cheap.
- Generalize from local context: do not encode app/project names in lesson IDs, titles, categories, or file names.
- Name by retrieval signals that transfer across repos: technology, failure mode, lifecycle phase, or behavior rule.
- Use evidence-based confidence:
- `high`: repeated success or direct confirmation.
- `medium`: one successful fix.
- `low`: plausible but lightly validated.

Use the template in `references/entry-template.md`.

## Retrieval Heuristics

- Match by:
- technology (`python`, `react`, `sql`, `powershell`, etc.)
- failure mode (`off-by-one`, `race-condition`, `api-contract`, `escaping`, etc.)
- lifecycle phase (`implementation`, `review`, `test`, `release`)
- user preference lessons (`ask-clarifying-questions`, `verify-on-web-when-volatile`)

If no direct match is found, read one general file and proceed without broad loading.

## Minimal Maintenance Routine

- On every task: retrieve small, apply, then optionally write.
- Every few tasks: prune duplicates and merge near-identical lessons.
- When structure drifts: perform a small taxonomy refactor and update `index.yaml`.

## Quick Start Commands

Use these patterns:

```powershell
# Resolve global lesson-learned path
$codexHome = if ($env:CODEX_HOME) { $env:CODEX_HOME } else { Join-Path $HOME ".codex" }
$memoryRoot = Join-Path $codexHome "lesson-learned"

# Initialize memory folders
New-Item -ItemType Directory -Force $memoryRoot, (Join-Path $memoryRoot "lessons"), (Join-Path $memoryRoot "archive") | Out-Null

# Create index if missing
if (-not (Test-Path (Join-Path $memoryRoot "index.yaml"))) {
  Copy-Item lesson-learned\references\index-starter.yaml (Join-Path $memoryRoot "index.yaml")
}
```

## References

- Read `references/storage-layout.md` for folder layout and taxonomy rules.
- Read `references/entry-template.md` for entry schema and examples.
- Read `references/index-starter.yaml` when bootstrapping a new memory store.
