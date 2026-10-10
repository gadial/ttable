# CourseCanvas timetable prototype

CourseCanvas is a client-side prototype for planning university course timetables. It runs as a static website with no server, build system, or Node.js runtime.

## Run it

Open `docs/index.html` directly in a modern browser, or serve the repository with any static file server. GitHub Pages publishes the `docs` directory.

## How it is built

- `docs/index.html` contains the semantic application shell and forms.
- `docs/style.css` provides the responsive three-panel desktop layout and stacked mobile layout.
- `docs/app.js` contains the state model, rendering, drag-and-drop, imports/exports, conflict checks, and persistence.
- `.openai/hosting.json` identifies `docs` as the static publishing directory.

The app has no third-party dependencies. All state is stored in the browser under the `coursecanvas.v1` local-storage key after every state-changing action.

## How it works

- Add courses in the left panel, then drag them into any timetable slot. Drag a scheduled course back to the course bank to unschedule it.
- Courses snap to hourly slots. Their selected duration determines how many slots they occupy.
- Multiple courses may share a slot. An orange count badge opens a compact list of all courses occupying that time.
- Create multiple timetable drafts and switch between them with tabs. Double-click a tab to rename it. Each draft keeps independent placements while sharing the course bank and constraints.
- Undo and redo cover course, draft, placement, and constraint changes. Keyboard shortcuts `Ctrl/Cmd+Z` and `Ctrl/Cmd+Y` are supported.
- Add external course meetings manually or import the CSV format described in `outline.md`. The planner preserves lecture, tutorial, and laboratory groups and reports a constraint failure only when a course has no collision-free valid group combination.
- Import courses to schedule and program requirements separately. Split weekly patterns create multiple draggable meetings, and every meeting in every advertised group must be placed.
- Select a semester per draft. The scheduling-problems panel evaluates every relevant program with a global group-selection search and explains infeasible or incomplete schedules immediately.
- Export all application data as JSON and restore it with **Import JSON**. JSON backup includes every draft, course, placement, and external constraint.

## CSV format

The importer accepts a headerless export (or an optional header) with these leading columns:

```text
course number,course name,section/group,meeting type,Sunday time,Monday time,Tuesday time,Wednesday time,Thursday time,...
```

Each row provides one time range in one weekday column. Repeated rows for the same course, section, and type are combined into a multi-meeting group. Trailing source-system columns are ignored.

## Data model

The version 3 persisted JSON contains structured internal `courses`, external-course `constraints`, semester-aware `programs`, `drafts`, and `activeDraftId`. Internal groups contain draggable meetings with durations; external groups contain fixed day/start/end meetings. Draft placements map meeting IDs to zero-based day and time positions. Older local data and backups are migrated automatically.

## Browser support

The prototype uses standard HTML5 drag and drop, File/Blob APIs, and localStorage. It is intended for current Chrome, Edge, Firefox, and Safari. Mouse or trackpad interaction is recommended for timetable drag-and-drop.
