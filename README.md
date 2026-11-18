# Super Octo Giggle

Team to-do board: kanban, calendar (week and month), projects, and a 3D view of what got done. Static site: `index.html` reads JSON from `data/`.

## How it works
- `data/team.json`: shared setup (projects, colors, statuses, people). Rarely changes.
- `data/people/<you>.json`: your tasks. **Only edit your own file**, so nobody gets merge conflicts.
- Each task has subtasks (each assignable to a person), notes, and collaborators (`with`). No time tracking: checking something off stamps the date automatically.

## Daily use
1. Open the site (GitHub Pages). Pick yourself under **I Am**.
2. Add or edit tasks, drag cards between columns, check off subtasks.
3. Edits are saved as a draft in your browser. Click **Push Changes**, copy your file, **Open On GitHub**, paste, commit.
4. Click **Mark As Pushed**. Teammates see it after Reload.

Or skip the UI: edit your JSON directly, or tell an AI assistant (Copilot, Claude) what you did and let it update your file and commit. Instructions for assistants are in `AGENTS.md` (Copilot reads `.github/copilot-instructions.md`, same content).

## Setup
1. Create a repo, add these files, turn on GitHub Pages (Settings > Pages > main / root).
2. In `data/team.json` set `repo` to `org/repo-name` (enables the Open On GitHub button), and edit people and projects.
3. Add a person: create `data/people/<id>.json` with `{ "person": "<id>", "capacity": 40, "tasks": [] }` and add them to `people` in `team.json`.

Local preview: `python3 -m http.server` in this folder, then open http://localhost:8000.

## Projects
Projects tab: **+ New Project**, or **Edit** on a project to rename, recolor, archive, or delete (only if no tasks use it). These edits change the shared `data/team.json`, so push them soon. Renaming is safe: tasks reference the project id, not the name.

## People outside the team
- Recurring outsiders (QAQC, PMs): add to `guests` in `team.json` as `{ "id": "maria", "name": "Maria" }`.
- One-offs: open any person picker and type a name in **Add a name**. It's saved as plain text on the task.
- Subtasks can be assigned to anyone on the task (e.g. a QAQC check to a reviewer).
- Outsiders show with a dashed avatar and appear under **Others** in the Person filter. They don't have their own file.

## Task fields
```json
{
  "id": "kelsey-001",
  "project": "gca",
  "title": "Writeback input for contract due date rules",
  "status": "In Progress",
  "due": "2026-11-20",
  "with": ["winston"],
  "notes": "What exactly needs to be done",
  "checklist": [{ "text": "Slide 1: Title", "done": true, "who": "steve", "doneDate": "2026-11-17" }]
}
```
