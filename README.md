# Linux Lab

Personal learning workspace for career-focused Linux and networking skills.

Progress is tracked in `progress/learner-model.json` so sessions stay connected across conversations.

**Repository:** https://github.com/ivanchinook/linux-lab

## How it works

1. **Placement** — baseline assessment maps what you already know
2. **Lecture** — short, targeted lesson (~30 min, flexible)
3. **Retrieval** — post-lesson check (mixed formats)
4. **Update model** — mastery levels and review queue updated
5. **Next session** — picks the next step from your profile

Each session updates files under `progress/` — use `git diff` and commit history to see how the learning engine adapts.

## Structure

```
framework/     Universal learning system (reusable for other tracks)
tracks/        Topic maps and assessments for Linux & networking
progress/      Your learner model and session history
labs/          Hands-on artifacts you create during exercises
```

## Current status

See `progress/learner-model.json` → `next_session`.
