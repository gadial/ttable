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
- Add external courses manually or import the CSV format described in `outline.md`. Conflicting timetable cells are striped orange and the schedule health summary reports them.
- Export all application data as JSON and restore it with **Import JSON**. JSON backup includes every draft, course, placement, and external constraint.

## CSV format

The importer expects a header followed by these columns:

```text
course number,course name,Sunday times,Monday times,Tuesday times,Wednesday times,Thursday times
```

Each row may provide one meeting time in one day column, as specified. The importer also tolerates multiple times in a cell when separated by `;` or `|`. Times must match configured slots such as `8:30-9:30`.

## Data model

The persisted JSON contains `courses`, `constraints`, `drafts`, and `activeDraftId`. Draft placements map a course ID to a zero-based day and time index. The exported object includes a `version` field for future migrations.

## Browser support

The prototype uses standard HTML5 drag and drop, File/Blob APIs, and localStorage. It is intended for current Chrome, Edge, Firefox, and Safari. Mouse or trackpad interaction is recommended for timetable drag-and-drop.
