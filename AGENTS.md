# Agent start here

This is a **learning project** with adaptive tutoring and progress tracking.

1. Read `progress/learner-model.json` → check `next_session` for what to do next
2. Read `framework/README.md` for the session loop and learning principles
3. Score using `framework/rubrics/scoring.md`
4. After each session: update the learner model and append a log under `progress/sessions/`

Project rules in `.cursor/rules/` enforce this workflow automatically.

## Cursor Cloud specific instructions

This repo is **content/data, not an application** — there is no package manager, build
step, dev server, or test framework. "Running the app" means executing the session loop
(`framework/README.md`) over `progress/learner-model.json`. No dependency install is
needed; `git`, `python3`, `jq`, and `yq` are already available in the VM.

- Lint/test (data integrity): the meaningful check is that every `*.json` and
  `*.yaml` file parses and the learner model stays consistent with the track topic
  graphs. Validate JSON with `python3 -c "import json,glob;[json.load(open(f)) for f in glob.glob('**/*.json',recursive=True)]"` and YAML with `yq . tracks/*/topics.yaml`.
- Build: none.
- Run: perform the session loop — read `next_session`, deliver/score per
  `framework/rubrics/scoring.md`, then update `mastery` / `review_queue` /
  `next_session` and append a log under `progress/sessions/`.
- Gotcha: a real session loop **mutates tracked files** in `progress/`. When you only
  need to verify the environment (not conduct a real learning session), run the loop
  against a throwaway copy of the repo so you don't pollute real progress history.
