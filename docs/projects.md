# Projects and crash recovery

Use **Project** in the main window to create, open or save a `.udimforge` project. Ctrl+S saves and Ctrl+O opens. Projects contain texture references, assignments, names, inclusion, selection, conversion formats and resolutions, profiles and OCIO assignments, tool recipes, PBR model/HDRI references, queued jobs and comparison sessions/results. Pixel data, models and HDRIs are not embedded: keep the source files with the project.

Recovery metadata is committed about every two seconds with SQLite FULL/WAL and checksums. The newest eight valid checkpoints are retained, within a bounded recovery database. An incomplete write falls back to the last valid checkpoint. The last project is restored at startup; **Recovery history** opens earlier checkpoints. A second running instance does not overwrite the first instance's recovery journal. Recovery failure is shown in the status bar. Edits made since the last successful checkpoint can be lost; no software can guarantee recovery from disk or OS failure.

Manual project files are written atomically. A newer local recovery checkpoint for that exact project path and saved revision is preferred on opening. Opening another project checkpoints the current work. New project retains the current conversion preferences but starts with an empty source list. Preview/tool Undo history starts again when opening a project; it is not serialized.

Production queues restore paused. Resume queue verifies completed outputs before reuse. The local queue ledger is authoritative after a crash because it commits each job transition immediately. An imported project gets an isolated local queue. No imported project starts exporting automatically.

The main **Export… → Review output plan → Check & start export** also records a durable run identity before starting. Cancel retains completed copies in its staging directory. **Resume conversion** continues the original saved plan and verifies source/output hashes; changed settings do not silently alter the paused plan. Use **Project > Start a new conversion** to use current settings and retain older partial copies. Source changes require a new run. A rescan clears the current resume pointer. Relinking also starts a fresh conversion run rather than claiming another disk's staging files.

**Relink project folders** replaces an old root prefix with a new root. Comparison results are retained only after the relocated sources pass content verification. Missing or changed sources are reported. Original files are never moved. Paths remain absolute until relinked; the project file alone is not a source archive.

External OCIO configurations remain deferred when opening/recovering projects. Use **Review external OCIO** to explicitly trust the configuration and its referenced LUTs. Built-in configurations are safe to restore automatically. Displacement/normal analysis still uses original values.

Open tools restore their recipes and source selection. Pending UI work, an uncommitted image edit, in-flight GPU frames and temporary proxy caches are rebuilt rather than embedded. Completed comparison results are revision-checked and reused; changed sources are reanalyzed on request. Keep recovery/project files private: they contain local paths and metadata.
