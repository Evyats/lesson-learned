# lesson-learned

`lesson-learned` is a Codex skill that adds persistent memory for bug prevention and execution quality across tasks.

## What it does

- Reads a small set of relevant past lessons before implementation.
- Applies those lessons as concrete checks while coding, debugging, and reviewing.
- Writes new lessons after meaningful fixes, regressions, or explicit "remember this" requests.
- Updates or supersedes outdated lessons when better evidence appears.
- Keeps lessons organized in a fluid category structure that can be split/merged over time.

## How memory is organized

- Uses a global runtime store at `$CODEX_HOME/lesson-learned/` (fallback: `~/.codex/lesson-learned/`).
- Maintains `index.yaml` for taxonomy and retrieval metadata.
- Maintains `lessons/` for active categorized lesson files.
- Maintains `archive/` for superseded entries and historical structure snapshots.
- Maintains `history/` for append-only change events and project source mapping.
- Uses a small retrieval scope by default (typically 1-3 files) to keep context efficient.

## History and auditability

- Tracks lesson mutations (`added`, `updated`, `superseded`, `restructured`) in `history/events.ndjson`.
- Stores `project_id` (and optional project name) on each event so users can inspect recent changes by source project.
- Keeps active lessons reusable and cross-project; project context stays in history/evidence, not taxonomy names.

## Lesson quality model

Each lesson captures:
- failure pattern and root cause
- reusable prevention rule
- concrete checklist steps
- confidence level and validation evidence

This helps Codex avoid repeating mistakes and improve reliability over repeated tasks.

## Response reporting

When the skill is used, Codex appends a short structured suffix reporting whether lessons were read, applied, added, updated, or restructured.
