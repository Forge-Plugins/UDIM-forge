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

**Retry selected files…** supports production/TX jobs, cache files and grading/resize/channel-packing batches created from version 0.34. It does not start other queued jobs or folder scans. Production retries require the original project/queue to be open, pass export preflight, validate worker/RAM settings, require unresolved prerequisites to be included, and protect existing outputs. Cache retries retain completed composites and retry only selected PSD/PSB paths. The panel shows up to 1,000 current failures; saved history retains up to 200, trimmed further if needed to fit a 1 MiB record. Fixing/retrying those rows reveals the next current failures on Refresh.

The main conversion uses its existing **Resume conversion** flow when the matching project and unfinished conversion are restored. It verifies and keeps completed copies while continuing the original plan; it is not a per-file selected retry. Older grading/resize/packing histories without a saved batch snapshot retain their output and errors; reopen the original tool/project to rerun them. New 0.34 batches support exact-setting selected retry as described below. Arbitrary historical settings are never silently applied to another workspace.

Clearing finished rows also clears those history records, without deleting logs, images or worker queues. Saving failures are indicated in the Tasks summary; the live panel continues working if history storage is unavailable. Start/finish transitions are committed synchronously; progress is flushed during normal panel polling, so a crash can lose roughly one polling interval of progress text, while completed queue outputs and report checkpoints retain their own durable records.

## Exact-setting retry for grading, resize and ORM packing

From **0.34**, starting a grading, resize or channel-packing export saves a validated operation snapshot before processing begins. The snapshot lives in **task-batches/** beside `task-history.sqlite3`; keep this folder with your settings backup. It contains parameters, original texture records or tile/channel pairings, relative output names and per-file outcomes, not copies of source images. Back it up with the application closed so SQLite data is complete.

After restarting, open **Tasks → Previous runs → Details / failed files…**. The details show the original saved settings. Select failed/interrupted rows with Ctrl/Shift-click and use **Retry selected files…**. Review the selected export preflight. A new task processes only those inputs into a new output subfolder under the saved output parent; completed results and unselected failures remain untouched. Refresh details after the task finishes to see remaining failures.

Saved grading includes the full recipe, format/depth and folder layout. Resize includes mode, amount, filter, normal normalization, Color sRGB filtering and reduce-only policy. Packing includes original UDIM/map pairing, R/G/B/Alpha mapping, source sample channels, missing-map fill policy, output label, format/depth and naming rules. Changing currently open tools or global output controls does not alter this retry recipe.

Sources must remain available at their recorded paths. A repaired source is processed with the original parameters; the snapshot does not preserve an old copy of its pixel data or discover new map files. Missing snapshots, corrupt metadata, unsupported processing revisions and already-completed selections are rejected clearly. A batch lock prevents concurrent processing of the same snapshot. App shutdown cancels active retries and waits for cleanup; project switching is blocked while a retry runs.

Snapshots are bounded to 100,000 inputs, 64 MiB input metadata, 64 KiB settings and a 128 MiB database. Failure lists expose up to 1,000 unfinished inputs at a time; successful selected retries reveal the next remaining entries on Refresh. Clearing history does not delete source/results or batch snapshot files. No image data is embedded and no historical operation starts automatically.


## Moved inputs and disk usage

Version 0.35 adds **Relink inputs…** and **Storage / cleanup…** directly to Tasks. See [Storage and task relinking](storage.md) for operation-specific recovery, protected data, retention limits and original-project requirements.
