# Lesson Learned Storage Layout

Use one global runtime layout shared by all projects:

- Primary: `$CODEX_HOME/lesson-learned/`
- Fallback: `~/.codex/lesson-learned/`

```text
lesson-learned/
  index.yaml
  lessons/
    general/
      behavior.md
      validation.md
    tech/
      web/
        react.md
      backend/
        python.md
    failure-modes/
      api-contract.md
      state-sync.md
  archive/
    2026-02/
      superseded-entries.md
  history/
    events.ndjson
    projects.yaml
```

## Category Design Rules

- Keep category files small and focused.
- Prefer depth over huge flat files when entries grow.
- Split categories when one file exceeds about 25 entries.
- Merge categories when two files overlap heavily.
- Treat taxonomy as editable: restructure whenever retrieval becomes noisy.
- Category and file names must be cross-project and semantic.
- Avoid app/repo names in active lesson paths (for example, avoid `israel-geography-map-game.md`).
- Prefer names that match retrieval signals (for example, `react/map-state-sync.md`, `failure-modes/coordinate-normalization.md`).

## Index Requirements

`index.yaml` should track:
- category tree
- per-file tags
- entry count
- last update time
- optional aliases for quick matching

Keep `index.yaml` authoritative after each split or merge.

## History Tracking Requirements

- Keep `history/events.ndjson` append-only.
- Write one event for each lesson mutation (`added`, `updated`, `superseded`, `restructured`).
- Include timestamp and stable lesson identifiers in every event.
- Include `project_id` for source tracing across repositories.
- Keep project-specific details out of active category naming and paths.
- Use `history/projects.yaml` to map `project_id` to readable project metadata when needed.

Suggested `projects.yaml` shape:

```yaml
projects:
  - project_id: "P-9f3c2d1a"
    project_name: "example-repo"
    first_seen: "2026-02-23"
    last_seen: "2026-02-23"
```
