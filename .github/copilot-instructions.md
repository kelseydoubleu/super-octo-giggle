# Instructions for AI assistants (Copilot, Claude, etc.)

This repo is a team task board. `index.html` reads JSON files in `data/`. There is no build step.

## The rule
Only edit the file for the person you're working for: `data/people/<id>.json`.
Never edit another person's file. Only edit `data/team.json` if explicitly asked (e.g. to add a project or guest).

People: `kelsey`, `steve`, `winston`. Reviewers outside the team (listed under `guests` in team.json): `kuan`, `islay`. Project ids and statuses are in `data/team.json`.

## What the user will say, and what to do
- "I'm working on X" / "add a task": add a task (see schema). Ask which project if unclear.
- "I finished X": find the matching subtask (or add one), set `done: true` and `doneDate: <today>`. No hours are tracked.
- "Send X to Islay for review": set `status: "Review"` and `reviewer: ["islay"]`. Every task in Review needs a `reviewer` list (ball in court), one or more of kelsey, steve, winston, kuan, islay. Ask who if it's unclear. Remove `reviewer` when the task leaves Review.
- "Starting X Monday, due Friday": set `start` and `due`. New tasks get `start` = today if not given.
- "Move X to review" / "X is done": change `status`. When setting `"Done"`, also set `doneDate: <today>`; when moving out of Done, remove `doneDate`.
- "Slides for X: intro, data, next steps": add `checklist` items.
- "Steve and Islay are doing the QAQC subtask": set `who: ["steve", "islay"]` on that checklist item. `who` is always a list of ids (older files may have a single string; that still works).
- "Maria is doing QAQC on X": add `"maria"` (or the guest id from team.json) to `with`, or set `who` on the relevant checklist item.
- "What am I working on?" / "what did I finish this week?": read the file and summarize. Don't edit.

## Task schema
```json
{
  "id": "<person>-<short unique suffix>",
  "project": "<project id from team.json>",
  "title": "Short sentence-case title",
  "status": "Backlog | This Week | In Progress | Review | Done",
  "start": "YYYY-MM-DD",
  "due": "YYYY-MM-DD",
  "doneDate": "YYYY-MM-DD (only when status is Done)",
  "reviewer": ["islay"],  // only when status is Review: who has the ball
  "with": ["steve"],
  "notes": "What exactly needs to be done. Plain text, \n for new lines.",
  "checklist": [
    { "text": "Slide 1: Title", "done": true, "who": ["steve"], "doneDate": "YYYY-MM-DD" },
    { "text": "QAQC", "done": false, "who": ["kuan", "islay"] }
  ]
}
```
Only `id`, `project`, `title`, `status` are required. Omit empty fields if you like.

## Before committing
- Keep the file valid JSON (2-space indent). Never delete existing tasks or subtasks unless asked.
- Dates are `YYYY-MM-DD`.
- Commit message: `<person>: <what changed>`, e.g. `winston: finish semantic model draft`.
- Commit directly to `main`. Pull first to avoid conflicts.
