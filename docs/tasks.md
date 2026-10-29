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

The task ledger retains up to 300 recent rows, preserving active work. **Clear finished** clears the panel's finished rows without deleting files, production job history, cache or report checkpoints. Unchanged finished queue summaries stay hidden until their state changes. Switching projects detaches actions for the former queue so they cannot control a different project.

Recent live task history is local to the current application run. Durable production queues, project/session autosaves and PBR recovery checkpoints keep their existing persistence and resume behavior; this panel does not create a second execution queue. Use [Projects and crash recovery](projects.md) and [PBR report recovery](pbr-ab.md#automatic-startup-report-recovery) for persisted work.
