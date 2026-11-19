# Super Octo Giggle

Team to-do board: kanban, calendar (week and month), projects, and a 3D view of what got done. Static site: `index.html` reads JSON from `data/`.

## How it works
Everything lives in one file, `data/board.json`: settings (statuses, projects, people, guests) at the top, then `tasks`. Each task has an `owner`.
- Each task has subtasks (each assignable to a person), notes, and collaborators (`with`). No time tracking: checking something off stamps the date automatically.

## Daily use
1. Open the site (GitHub Pages). Pick yourself under **I Am**.
2. Add or edit tasks, drag cards between columns, check off subtasks.
3. Edits are saved as a draft in your browser. Click **Push Changes**, copy, **Open On GitHub**, paste over `board.json`, commit.
4. Click **Mark As Pushed**. Teammates see it after Reload.

Push Changes always pulls the latest `board.json` first and merges your edits in (task by task), so pasting won't erase what someone else pushed in the meantime. If you both edited the same task, your version wins.

Or skip the UI: edit `board.json` directly, or tell an AI assistant (Copilot, Claude) what you did and let it update `board.json` and commit. Instructions for assistants are in `AGENTS.md` (Copilot reads `.github/copilot-instructions.md`, same content).

## Setup
1. Create a repo, add these files, turn on GitHub Pages (Settings > Pages > main / root).
2. In `data/board.json` set `repo` to `org/repo-name` (enables the Open On GitHub button), and edit people and projects.
3. Add a person: add `{ "id": "<id>", "name": "AB" }` to `people`.

Local preview: `python3 -m http.server` in this folder, then open http://localhost:8000.

## Projects
Projects tab: **+ New Project**, or **Edit** on a project to rename, recolor, archive, or delete (only if no tasks use it). Renaming is safe: tasks reference the project id, not the name.

## People outside the team
- Recurring outsiders (QAQC, PMs): add to `guests` in `board.json` as `{ "id": "maria", "name": "Maria" }`.
- One-offs: open any person picker and type a name in **Add a name**. It's saved as plain text on the task.
- Subtasks can be assigned to anyone on the task (e.g. a QAQC check to a reviewer).
- Outsiders show with a dashed avatar and appear under **Others** in the Person filter. They don't get a calendar row.

## Task fields
```json
{
  "id": "kelsey-001",
  "owner": "kelsey",
  "project": "gca",
  "title": "Writeback input for contract due date rules",
  "status": "In Progress",
  "start": "2026-11-16",
  "due": "2026-11-20",
  "with": ["winston"],
  "notes": "What exactly needs to be done",
  "checklist": [{ "text": "Slide 1: Title", "done": true, "who": "steve", "doneDate": "2026-11-17" }]
}
```
