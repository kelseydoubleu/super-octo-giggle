# Instructions for AI assistants (Copilot, Claude, etc.)

This repo is a team task board. `index.html` reads one file, `data/board.json`. There is no build step.

## The rule
All data is in `data/board.json`: settings at the top (`statuses`, `projects`, `people`, `guests`), then `tasks`.
Only change tasks whose `owner` is the person you're working for, unless they explicitly ask to change someone else's. Only change settings (projects, people, guests) if explicitly asked.
Always pull right before editing and push right after, since everyone shares this file.

People: `kelsey`, `steve`, `winston`. Reviewers (in `guests`): `kuan`, `islay`. Project ids are in `projects`.

## What the user will say, and what to do
- "I'm working on X" / "add a task": append a task to `tasks` with `owner` set to the person (see schema). Ask which project if unclear.
- "I finished X": find the matching subtask (or add one), set `done: true` and `doneDate: <today>`. No hours are tracked.
- "Send X to Islay for review": set `status: "Review"` and `reviewer: ["islay"]`. Every task in Review needs a `reviewer` list (ball in court), one or more of kelsey, steve, winston, kuan, islay. Ask who if it's unclear. Remove `reviewer` when the task leaves Review.
- "Starting X Monday, due Friday": set `start` and `due`. New tasks get `start` = today if not given.
- "Move X to review" / "X is done": change `status`. When setting `"Done"`, also set `doneDate: <today>`; when moving out of Done, remove `doneDate`.
- "Slides for X: intro, data, next steps": add `checklist` items.
- "Steve and Islay are doing the QAQC subtask": set `who: ["steve", "islay"]` on that checklist item. `who` is always a list of ids (older files may have a single string; that still works).
- "Maria is doing QAQC on X": add `"maria"` (or the guest id from `guests`) to `with`, or set `who` on the relevant checklist item.
- "What am I working on?" / "what did I finish this week?": read the tasks for that owner and summarize. Don't edit.

## Task schema
```json
{
  "id": "<person>-<short unique suffix>",
  "owner": "kelsey | steve | winston",
  "project": "<project id from projects>",
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
Only `id`, `owner`, `project`, `title`, `status` are required. Omit empty fields if you like.

## Before committing
- Keep the file valid JSON (2-space indent). Never delete or reorder existing tasks or subtasks unless asked. Don't reformat the whole file; change only what you need.
- Dates are `YYYY-MM-DD`.
- Commit message: `<person>: <what changed>`, e.g. `winston: finish semantic model draft`.
- Commit directly to `main`. Pull first to avoid conflicts.
