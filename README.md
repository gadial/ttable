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
- Export all application data as JSON and restore it with **Import JSON**. JSON backup includes every draft, course, placement, and external constraint.

## CSV format

The importer accepts a headerless export (or an optional header) with these leading columns:

```text
course number,course name,section/group,meeting type,Sunday time,Monday time,Tuesday time,Wednesday time,Thursday time,...
```

Each row provides one time range in one weekday column. Repeated rows for the same course, section, and type are combined into a multi-meeting group. Trailing source-system columns are ignored.

## Data model

The persisted JSON contains `courses`, structured external-course `constraints`, `drafts`, and `activeDraftId`. Each constraint contains groups, and each group contains typed meetings with day, start, and end values. Draft placements map a course ID to a zero-based day and time index. The exported object includes a `version` field; version 1 constraints are migrated to standalone laboratory groups so old local data and backups remain usable.

## Browser support

The prototype uses standard HTML5 drag and drop, File/Blob APIs, and localStorage. It is intended for current Chrome, Edge, Firefox, and Safari. Mouse or trackpad interaction is recommended for timetable drag-and-drop.
