# Lesson Entry Template

Use one entry per distinct lesson.

```yaml
id: "L-YYYYMMDD-short-slug"
title: "Short actionable title"
status: "active" # active | superseded
confidence: "medium" # low | medium | high
apply_when:
  - "condition or signal"
signals:
  technologies: ["python", "react"]
  failure_modes: ["api-contract", "null-handling"]
  task_types: ["implementation", "debugging"]
problem:
  summary: "What failed or almost failed"
  root_cause: "Why it happened"
solution:
  rule: "What to do next time"
  checklist:
    - "Concrete prevention check"
    - "Concrete validation step"
evidence:
  - "Bug fixed in <file/PR/commit/context>"
supersedes: [] # previous lesson IDs replaced by this entry
last_validated: "YYYY-MM-DD"
```

## Authoring Rules

- Write imperative rules.
- Avoid vague advice.
- Include at least one concrete prevention check.
- Keep lesson IDs and titles generic; do not include project/app names.
- If the lesson originated in a specific app, keep that detail only under `evidence`.
- Update `confidence` as evidence improves.
- If inaccurate, create a replacement entry and set `supersedes`.
