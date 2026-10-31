# Storage and task relinking

## Reconnect moved inputs

Open **Tasks → Relink inputs…**, or use the same button in task details. Enter the original root and choose its replacement. The relative subfolders and filenames must match. Preview runs in the background, checks that replacements are readable, and shows the exact substitutions. Acknowledge that replacement content may differ, then choose **Apply links**. Work never starts automatically.

- Grading, resize and ORM/RMA batches created from 0.34 retain their original recipes, RGBA sample assignments, pairing rules and output names. Only unfinished input records change; the updates are transactional and persist after restart. Close active work first.
- Production/TX/surface jobs require the original project/queue to be open and paused. Only unfinished jobs change; outputs and dependency-generated paths are protected. Input revision stamps are refreshed. Already completed range/statistics dependencies still validate their original source hashes; prepare a fresh plan if the derived range no longer matches the relinked input.
- Cache failures reconnect without starting decoding. Select the required files and Retry explicitly.
- Interrupted main conversions require the matching restored project/run. Input aliases are stored in the project without changing the original plan identity or progress ledger. Completed inputs must match their recorded SHA-256; completed output hashes are verified again on Resume. The output/staging directory must remain available at its existing path. For a moved output directory, preserve it and use project relocation / a new output plan; task relinking changes inputs only.
- PBR reports open the existing report input-relink flow, including moved checkpoints, A/B maps, model, HDRI and OCIO dependencies. Existing render reuse is decided by content verification.
- New analysis/preparation task entries record their project identity and open project relinking only for that matching workspace. Session relinking verifies retained analysis results. Older task entries without identity require opening the original project and using **Project → Relink project folders**.

This action does not guess by basename, scan unrelated disks, trust external OCIO automatically, rename originals or move output files. The full-project relinker is separate and reconnects workspace folders, tools, comparison assignments and OCIO paths.

## Storage and retention

Open **Tasks → Storage / cleanup…**, or search **Storage** in Ctrl+K. The panel shows owned recipe/log bytes, protected bytes, the Tasks database, PSD/PSB cache usage, active readers, reservations, quota and location. Cache settings and its guarded clear operation are directly accessible.

Choose retention age for completed history and a recipe/log disk budget. **Review cleanup…** shows the number of completed entries and eligible owned files before deletion. Applying it removes only old Completed history rows, compacts the Tasks database, then rechecks and removes eligible orphan completed recipes and inactive logs. The settings persist. Cleanup runs explicitly; it does not run on startup.

Unfinished recipe snapshots remain protected even after history was cleared. Running-process logs, snapshots referenced by retained history, unreadable/corrupt metadata and active batch leases are protected. Project recovery, PBR resume data, production queues, textures, models, HDRI, exported results and unrelated files are never deleted by this panel. Cache has its own existing quota and reader-aware cleanup; it is not counted against the recipe/log budget.

The budget is a cleanup target, not a hard cap: protected work may exceed it. Inventory is bounded to 20 seconds and 10,000 entries per category; the table displays up to 2,000 rows. Incomplete scans are labelled. No recursive directory deletion is used.
