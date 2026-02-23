# Lesson History Schema

Use an append-only event log so users can inspect what changed recently and where it came from.

## Files

- `history/events.ndjson`: one JSON object per line, chronological append-only log
- `history/projects.yaml`: optional lookup table for project metadata

## Event Schema

Each event should follow this shape:

```json
{
  "event_id": "E-20260223-a1b2c3",
  "timestamp_utc": "2026-02-23T18:12:45Z",
  "action": "added",
  "lesson_id": "L-20260223-short-slug",
  "lesson_path": "lessons/failure-modes/api-contract.md",
  "project_id": "P-9f3c2d1a",
  "project_name": "example-repo",
  "summary": "Added guard for null API payload before parse"
}
```

## Field Rules

- `event_id`: stable unique event id
- `timestamp_utc`: ISO-8601 UTC time
- `action`: one of `added`, `updated`, `superseded`, `restructured`
- `lesson_id`: id of affected lesson (or taxonomy id for restructure events)
- `lesson_path`: active path touched by the event
- `project_id`: stable project identifier used for filtering
- `project_name`: optional human-readable project name
- `summary`: short human-readable change description

## Project ID Guidance

- Prefer deterministic ids to avoid duplicates.
- Use a hash of normalized repo root path, remote URL, or both.
- Persist the mapping in `history/projects.yaml`.
- Keep ids stable across tasks for the same project.

## Query Patterns

Use simple filtering for audits:

- Recent changes: tail `history/events.ndjson`
- Recent additions: filter events where `action == "added"`
- Changes by project: filter events where `project_id == "<id>"`
- Recent by project and action: combine both filters

PowerShell example:

```powershell
Get-Content "$memoryRoot/history/events.ndjson" |
  ForEach-Object { $_ | ConvertFrom-Json } |
  Where-Object { $_.project_id -eq "P-9f3c2d1a" -and $_.action -eq "added" } |
  Sort-Object timestamp_utc -Descending |
  Select-Object -First 20
```
