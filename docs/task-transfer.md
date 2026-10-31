# Copy whole tasks and resume

Select a paused task in **Tasks**, then choose **Copy whole task…**. Choose a parent folder; UDIM Forge creates a new unique task folder. It copies inputs, completed results, owned staging and production-worker recovery files. Originals are never moved or deleted. Hash verification runs before the copy is published. Existing destinations and changed completion hashes are rejected.

The supported task types are main conversions, saved grading/resize/ORM/RMA batches, and production queues including TX, surface operations and recipes. PBR reports retain their separate report-recovery relocation flow. Cache data remains governed by its own cache settings and can be rebuilt. Tasks using external OCIO configurations require the existing **Collect project dependencies** workflow and its explicit configuration review; whole-task copying does not silently parse external OCIO.

## Open on another machine or directory

Keep the entire copied folder together. In Tasks choose **Open transferred task…**, or find that command with Ctrl+K, and select **Task.udimtask**. Every inventoried file is checked against SHA-256 before activation. Relative bundle paths reconnect inputs and outputs even after the whole folder is moved. Changed or missing files block opening rather than silently becoming trusted completed results.

Opening restores paused work; it never launches conversion automatically. Grading, resize and packing retain their original settings, channel samples, map pairings, relative output names and completed-file states. Retry unfinished files explicitly. Main conversion preserves its original plan identity and progress ledger while resolving copied inputs through aliases. Production work keeps completed outputs and verified native staging; commit guards are rebased to the restored settings before recovery.

For main/production tasks the app writes **Restored.udimforge** and keeps the normal local project/queue recovery. The source workspace is checkpointed before activation. Use that project and Tasks history for subsequent restarts. Task.udimtask is a point-in-time transfer snapshot: resuming in place can consume staging files listed in it. To transfer newer progress, make a fresh whole-task copy. Keep an untouched extra copy if you also want an immutable backup.

## Progress and cancellation

Inventory and copy/verification run in the background, with Tasks progress and Cancel. A cooperative cancellation removes only the newly created owned transfer staging directory, after verifying its location; original inputs/results are preserved. A process crash may leave a `.forge-task-copy-*` directory without a published Task.udimtask. That incomplete directory is not admitted as a usable transfer; repeat the copy from the intact original task. Task export/resume itself retains its existing durable per-file checkpoints.

Input inventories are limited to 100,000 files and 120 seconds. Task metadata is limited to 128 MiB. Copying uses bounded chunks, checks source revisions, verifies destination hashes and requires available disk space plus a reserve. Symbolic links/junctions are rejected. Outputs outside owned task/result paths are not gathered indiscriminately.
