# Learning Framework

Reusable system for adaptive tutoring sessions. Designed around evidence-based learning principles.

## Principles applied

| Principle | How we use it |
|-----------|----------------|
| **Retrieval practice** | Pre/post checks; you recall, not re-read |
| **Spaced repetition** | `review_queue` resurfaces weak topics |
| **Interleaving** | Linux + networking mixed when skills overlap |
| **Desirable difficulty** | Target ~60% success; stretch without overload |
| **Varied practice** | CLI, scenarios, MCQ, explain — not one format |
| **Concrete → abstract** | Native Linux hands-on before theory-heavy blocks |
| **Metacognition** | Confidence ratings + “explain why” prompts |

## Session loop

```
read learner-model.json
    → deliver lesson OR assessment
    → score + notes
    → update mastery / review_queue / next_session
    → append session log
```

## Files

- `../progress/learner-model.json` — single source of truth for your level
- `rubrics/scoring.md` — how answers are graded
- `../tracks/*/topics.yaml` — topic graph with prerequisites

## Adding a new track later

1. Copy `tracks/linux/topics.yaml` structure
2. Add assessments under `tracks/<name>/assessments/`
3. Register track in `learner-model.json` → `tracks`
