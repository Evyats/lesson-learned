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
