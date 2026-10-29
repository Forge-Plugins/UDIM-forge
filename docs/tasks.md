# Tasks — progress and cancellation

Open **Tasks…** in the main window's compact status strip, **Tools → Tasks — all operations…**, or search **Tasks** with Ctrl+K. The panel is non-modal: hiding it keeps work running. The main strip shows the active count and attention state alongside the existing global RAM indicator.

## One place to monitor work

The list includes main scans/conversions; grading, resize and channel packing; production queue operations including TX, recipes, quality checks and HTML exports; PSD/PSB cache batches and viewer-triggered preparation; pair/all-version pixel analysis; PBR model/HDRI/map preparation and PBR report exports. Known interrupted PBR reports also appear with access to their existing recovery flow.

Select a row for its current stage, percentage when supplied, elapsed time and available actions. Filter by active work, completion, attention or operation type. Tasks without a reliable percentage show an animated indicator and their reported stage. Progress across unlike operations is not combined into a misleading global percentage. Production queue counts refer to jobs/recipe steps, not necessarily source-file counts.

## Cancel, inspect and resume

- **Cancel selected** calls the selected operation's existing cancellation path. Other independent tasks are unaffected.
- For the **Production queue** row, Cancel pauses the entire queue, stops its active workers/native TX processes and retains completed verified outputs. Resume/Retry reuses the production workflow's preflight and worker/RAM settings. Use Open details to edit queue settings or inspect individual errors.
- For the **cache queue**, Cancel stops the entire batch and retains completed composites; Resume/Retry retries remaining work. Viewer preparations have their own rows when they are not part of the active cache batch.
- Analysis cancellation retains completed results in the comparison session. Closing its tool requests cancellation; the task is marked Interrupted if its completion can no longer be observed by that closed tool.
- PBR report cancellation retains committed render checkpoints. Use **Interrupted reports…**, or **Resume report** on a known interrupted task, to restore/relink inputs and continue. Original OCIO approval, input checks and output protection remain in force.
- **Open details** returns to the owner tool. If that tool was closed, reopen it using the panel shortcuts; durable interrupted reports remain available in the recovery list.

The panel stays accessible during main-window conversion. Hide does not cancel anything. Closing the application still follows the existing shutdown/cancellation rules.

## History and recovery

From **0.33**, the task ledger saves recent work in the local `task-history.sqlite3` database beside application settings. It retains up to 300 recent entries within a 64 MiB payload budget and a 128 MiB database bound. Active work remains visible in the live panel. **Clear finished** clears the panel's finished rows without deleting files, production job history, cache or report checkpoints. Unchanged finished queue summaries stay hidden until their state changes. Switching projects detaches actions for the former queue so they cannot control a different project.

History survives application restarts. Use **Previous runs** and the **Started** timestamp to distinguish older work. Entries that were active in the saved snapshot are shown as **Interrupted**, detached from executable callbacks; another application instance may still own their queue. No work starts from history automatically. Durable production queues, project/session autosaves and PBR recovery checkpoints keep their existing persistence and resume behavior; this panel does not create a second execution queue. Use [Projects and crash recovery](projects.md) and [PBR report recovery](pbr-ab.md#automatic-startup-report-recovery) for persisted work.

## Errors, logs and selected retry

Select **Details / failed files…** for the full task error, suggested checks and per-file errors. Ctrl/Shift-click failed rows to select several. **View log** previews up to the last 256 KiB inside the application; production failures can open their structured worker diagnostic record, including the traceback. Missing or rotated logs are reported clearly. **Open output folder** opens a directory in Explorer; files recorded in history are never launched as programs. Full local paths are retained in this local history.

**Retry selected files…** supports production/TX jobs and cache files. It does not start other queued jobs or folder scans. Production retries require the original project/queue to be open, pass export preflight, validate worker/RAM settings, require unresolved prerequisites to be included, and protect existing outputs. Cache retries retain completed composites and retry only selected PSD/PSB paths. The panel shows up to 1,000 current failures; saved history retains up to 200, trimmed further if needed to fit a 1 MiB record. Fixing/retrying those rows reveals the next current failures on Refresh.

The main conversion uses its existing **Resume conversion** flow when the matching project and unfinished conversion are restored. It verifies and keeps completed copies while continuing the original plan; it is not a per-file selected retry. Other grading/resize/packing histories retain their output and per-file errors; reopen the original tool/project to rerun with the correct recipe. Arbitrary historical settings are never silently applied to another workspace.

Clearing finished rows also clears those history records, without deleting logs, images or worker queues. Saving failures are indicated in the Tasks summary; the live panel continues working if history storage is unavailable. Start/finish transitions are committed synchronously; progress is flushed during normal panel polling, so a crash can lose roughly one polling interval of progress text, while completed queue outputs and report checkpoints retain their own durable records.
