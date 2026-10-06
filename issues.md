# Prototype issues and specification notes

## Ambiguities resolved for the prototype

- The specification says timetable times and days are configuration-provided but does not define a configuration UI or file format. The prototype uses the supplied defaults as constants. A production version should add configuration and data migration rules.
- Course input data and course duration are not specified. The prototype lets users add a name, optional number, and a duration of one to three hourly slots.
- The external-course CSV says each line describes one time, without defining how time ranges are delimited or quoted. The importer accepts exact default time labels and additionally supports `;` or `|` separators inside a day cell.
- Conflict behavior is described as a warning but not a blocker. The prototype allows placement and continuously highlights conflicts.
- Draft naming/deletion behavior is unspecified. The prototype prompts for a name, supports rename by double-click, and prevents deletion of the final draft.

## Prototype limitations

- Browser drag-and-drop is less ergonomic on touch-only devices. A production version should add tap-to-place controls and full keyboard placement.
- Undo/redo history is intentionally session-only and capped at 80 snapshots; the current state itself is durable across reloads.
- Browser storage can be cleared by the user or browser. JSON export is therefore the durable, portable backup mechanism.
- CSV character encoding is delegated to the browser's text decoder. Legacy non-UTF-8 files may need to be resaved as UTF-8.
- The prototype does not validate duplicate course numbers or duplicate external meetings.
- Courses occupy consecutive hourly slots and cannot extend beyond Thursday's final configured slot; dropping a multi-hour course late in the day displays only the available portion.
- Data is local to one browser profile and is not synchronized between users or devices.
- Directly opening the HTML file works, but some hardened browser policies may restrict file downloads or storage. Static hosting avoids those restrictions.

## Creation and verification notes

- The workspace did not provide a command-line Node.js executable. This does not affect the app (which is intentionally browser-only); JavaScript syntax was validated through the available JavaScript runtime instead.
- Automated browser navigation to local `file://` pages was blocked by the browser-control security policy, so the final automated check covered source syntax and file integrity rather than a scripted visual walkthrough.
