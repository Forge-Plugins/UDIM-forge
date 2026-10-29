# PBR material A/B comparison

Open **Compare → Compare PBR materials A/B**, use **Compare A/B** inside the PBR preview, or search for **PBR A/B** in Ctrl+K. Choose two version folders and click **Scan / reload versions**. Each folder has an independent set selector: select the matching sets explicitly if a folder contains several assets. The set names may differ between A and B.

## Linked viewing

Choose **Side by side** or **Wipe**. Drag either material to orbit both, use the mouse wheel for a common zoom, and use **Reset camera** to return to the default pose. In Wipe, move the slider or drag the orange divider. Wipe composes the last completed A/B frame pair without rendering, decoding textures or uploading GPU maps again. A value of zero shows B; one shows A.

Sphere and plane show the selected UDIM. Previous/next buttons navigate the union of both versions' tiles. Missing sides use a checker placeholder and an explicit status message. The coverage summary lists tiles found only in A or B.

## One model and lighting setup

**Shared OBJ / FBX** imports one static UV0 model for both materials. Model mode uses the whole selected UDIM set; missing tiles are magenta. **Highlight selected UDIM on model** helps locate the current tile. Subdivision, geometric displacement scale and neutral height are shared, so a difference reflects the materials rather than different geometry settings.

**Load shared HDRI** prepares one HDR/EXR panorama for both versions. Choose its encoding before loading. Rotation, intensity, background, exposure, studio lights and surface defaults apply equally to A and B. HDRI lighting without a loaded panorama falls back to Unlit. Opening comparison from an existing PBR preview inherits its prepared model, HDRI and view once; subsequent changes belong to the comparison window.

Albedo input encoding and ORM/RMA layout can differ between A and B. Custom packed-channel routing is shared. Map roles are assigned by the existing map recognition rules; ambiguous assignments must be corrected before loading.

## Performance and safe snapshots

Auto uses independent persistent GPU caches for A and B. If GPU preparation fails, both sides fall back to CPU. GPU mode reports failure explicitly. The A+B GPU cap is a combined texture/geometry allocation budget split equally between versions; it is not a process RSS limit. The application's global RAM policy also applies to preparation and native workers.

Adaptive quality uses a smaller frame and simplified geometry during movement, then refines after stopping. Each published pair has the same camera and lighting state; incomplete pairs are never displayed. Preview proxies remain bounded and are not full-resolution texture analysis. The default final frame is 512 pixels per side; each selected set supports up to 128 tiles. Reload versions after editing source files externally.

**Save preview** exports the displayed composition as a PNG display image. It is not a numerical texture-difference report or a material export. Use the existing set-analysis tools for native pixel thresholds and CSV results.

## Projects and portability

Projects and autosave retain both folders, set selections, tile, camera, Wipe position, input modes, shared model/HDRI paths and view controls. Reopening prepares fresh bounded proxies. **Collect project dependencies** includes both catalogs, including unopened tiles, plus the model, HDRI and applicable OCIO dependencies. Source files are never modified by this preview.

## Difference and Heatmap

Choose **Difference** to view absolute RGB changes or **Heatmap** to show their magnitude from blue through cyan and yellow to red. Both use the last completed, synchronized A/B frame pair. Camera, zoom, model and HDRI remain shared.

**Threshold (0–1)** filters by the largest absolute R/G/B channel difference for each rendered pixel, divided by 255. A pixel is counted only when its difference is strictly greater than the threshold. Pixels at or below the threshold are black. A threshold of zero includes every visible change; one excludes all changes. The initial threshold is 0.02.

**Display gain ×** amplifies visible differences from 1× to 32×. It does not affect the threshold, changed-pixel count, percentage or reported maximum. Heatmap colors use a fixed absolute scale multiplied by gain, so colors do not rescale automatically for each tile. High differences saturate red. The status displays the count, percentage and maximum difference across all viewport pixels, including background.

Threshold, gain and display-mode changes use cached RGB differences without re-rendering, decoding textures or uploading GPU maps. A new completed frame pair replaces this cache. Adaptive and full-quality frames can have different counts because their viewport resolutions differ. A missing side has an explicit unavailable message rather than statistics comparing a placeholder to a material. In whole-model view, missing UDIM markers remain part of the rendered image and the coverage summary identifies missing tiles.

These differences measure the **8-bit displayed PBR image after lighting, exposure and display encoding**, including its silhouettes and background. They are not linear-radiance, native-EXR, height or UV-space differences. Clipped highlights or changes invisible from the current view may not appear. Use native set analysis to measure source texture values. Disable the shared UDIM highlight when measuring material changes alone.

**Save preview** exports the current Difference or Heatmap image as PNG. Projects and autosave retain the selected mode, threshold and gain along with the linked camera and lighting; old projects default to threshold 0.02 and gain 4×.

## Change regions and linked detail navigation

The comparison detects **8-connected regions** of viewport pixels whose maximum absolute RGB difference exceeds the current threshold. Diagonal neighbors belong to the same region. Regions are ordered by peak difference, then changed-pixel area, then top/left position. Isolated single-pixel changes are retained. The count and bounds describe the whole rendered frame, including background, even while a detail is enlarged.

**Previous / Next** select and frame a region in both versions. Navigation wraps at either end. **Max difference** selects the region with the largest difference and centers a shared detail view on its strongest pixel, with at least 4× magnification. Ties select the first top/left peak. The status shows region index/count, changed-pixel area, peak value and its original rendered-pixel coordinates. **Region outline** draws its clipped bounds and a cross at the peak in each side-by-side panel or in the Wipe/difference view.

Navigation uses a shared **screen-space crop** of the completed synchronized frames, with padding and a 16× magnification limit. It preserves the 3D camera's angle, zoom and HDRI and does not request a render, decode or GPU upload. This enlarges existing preview pixels; it does not add texture resolution or move the camera through 3D space. **Show full frame** removes the detail crop; **Reset camera** also resets the 3D view. Both sides always use identical crop coordinates, including when switching to Wipe.

Wait for camera movement/rendering to finish before selecting a region. A new frame or threshold recomputes regions and clears the selected index. Gain and view-mode changes preserve the region cache. A missing side or zero pixels above threshold disables navigation. The normalized detail center and magnification are saved in projects/autosave; the component list is rebuilt from fresh frames on reopening. **Save preview** saves the current cropped composition, without the UI's outline/cross annotations.

## Export an offline HTML comparison report

Click **Export HTML report** in the PBR A/B window, below the region-navigation controls. Finish camera movement and preparation first. Both sides of the active tile must be available; a placeholder cannot be presented as a material comparison. Model mode reports the full rendered UV0 material view, with the active UDIM recorded in its settings. This exports the current completed view, not a batch of all camera angles or UDIM views.

The single HTML file embeds the original A/B images, heatmap and absolute difference, marked A/B images, and the current shared detail crop when zoomed. It includes threshold, display gain, changed viewport-pixel count/percentage, maximum difference and region count. Selected regions are white with a peak cross; other annotated regions are orange. Tables and image outlines show the first 200 regions by severity plus the selected region if necessary. Complete numeric region data remains available in the expandable JSON section.

Camera pose/zoom, shape, actual renderer, exposure, displacement, subdivision, HDRI filename/encoding/rotation/intensity/background, version folders/set names, input routing, OCIO settings and source revision stamps belong to the synchronized rendered snapshot. Threshold, gain and current detail crop belong to the exported analysis. Metadata is copied before background export; pending renders block export rather than mixing live controls with an old image. The report includes source paths, so review its contents before sharing.

Open the HTML in a local browser. Images are embedded PNGs; there are no scripts, remote assets, downloads or automatic uploads. No companion folder is required. Export uses the existing cancellable background worker and an atomic final write. Cancellation or a failed write preserves an existing report. Single-view reports are limited to 256 MiB and 4096 pixels per side. Source textures, model and HDRI are protected destinations.

Statistics cover the original full viewport and background, even if the detail images are cropped. They measure displayed 8-bit RGB after lighting/exposure/encoding; this is not native EXR or physical-material analysis. Reload external texture edits before preparing a new snapshot. Missing model tiles remain visible markers and their per-version coverage is recorded in the snapshot settings.

## Saved camera views and multi-view HTML

Open **Saved views / UDIM reports** in the PBR A/B window. Enter a name and **Save current as new** to capture yaw, pitch, camera zoom and the normalized detail crop. **Apply selected** recalls it for A and B together. **Update from current** replaces the selected name/pose with the current camera. Remove, move up/down and include/exclude controls define the report order. **Add standard four** adds Front/Right/Back/Left without duplicating existing names; adjust these poses for the actual geometry, especially a plane. Save up to 16 uniquely named views.

Camera views are stored in projects and autosave. **Save views / Load views** transfers a bounded JSON pose preset to another project. Presets contain camera/detail settings and inclusion flags only; they do not activate OCIO configs or load model/HDRI files. Old projects open with an empty view list.

**Export included views to HTML** renders every included pose sequentially at the chosen report resolution. The current shape, active UDIM (sphere/plane) or whole model UV0 set, materials, displacement, lighting/HDRI rotation/intensity, exposure, threshold and difference gain are frozen once for the batch. Only the saved camera/detail pose changes. The active on-screen camera is preserved. Pending preparation or a missing primitive-side tile blocks export. Editing live controls during a batch affects later preview work, not the report's frozen snapshot.

The single offline HTML contains a view index, per-view summary table and a complete A/B/heatmap/region report for each camera, including its pose and optional detail crop. Statistics are per viewport; counts from different angles are not unique material-pixel totals. GPU caches are reused between poses. Auto falls back consistently: if a later view loses GPU support, all views restart on CPU; explicit GPU errors abort the report. The manager displays progress and offers **Cancel export**. Rendering does not decode source textures again.

The final report is replaced atomically only after every included view succeeds. Cancellation, a failed renderer or the 512 MiB combined size limit preserves an existing report. Individual view documents retain their 256 MiB/200-outlines limits and full numeric region data. No scripts, external images or automatic uploads are added. Temporary per-view documents are local; review source-path metadata before sharing. Full-frame PBR differences remain display-RGB comparisons, not native EXR analysis. Detail zoom enlarges existing preview pixels.


## Report resolution and memory

Choose **Report resolution** in the PBR A/B window or saved-view manager: **Preview (512), 1K (1024), 2K (2048), 4K (4096)**. Dimensions are square pixels per version, not the combined side-by-side width. Preview exports the completed cached pair for a single report. Higher resolutions render a fresh synchronized pair at the requested dimensions; saved views all use the same resolution. The active preview camera and its cached image remain unchanged. Project/autosave stores the resolution and RAM budget.

**2K and 4K require a working GPU renderer. CPU supports up to 1K.** Auto does not silently upscale or reduce report resolution. An unavailable GPU aborts a 2K/4K report; lower-resolution batches may restart consistently on CPU. These are higher-resolution material views; source texture proxies and mesh/subdivision budgets remain bounded. They are not native full-resolution texture analysis.

**Report RAM MiB** reserves conservative workspace through the global scheduler and monitors growth of the application process tree during export. Minimum budgets: Preview 512 MiB, 1K 768 MiB, 2K 1280 MiB, 4K 2048 MiB. Choose up to 16384 MiB. Too-small budgets are rejected before rendering. Analysis runs in row blocks and views run sequentially. Excess memory growth cancels at a cooperative checkpoint with a clear message, preserving an existing report. This is an admission/growth budget, not a separate OS-enforced per-report cap. The global application RAM cap remains the hard process-tree ceiling; other application jobs can contribute to observed growth. Leave headroom for loaded models, textures, renderer workers and report encoding.

The **A+B GPU cap** also applies. High-resolution framebuffers/readback are reserved from each side's GPU budget; increase this cap for heavy models at 4K. Insufficient GPU memory produces an error instead of bypassing the cap. Temporary per-view HTML is staged beside the destination, one view at a time. Reports exceeding 100,000 connected change regions stop explicitly; increase the difference threshold to remove insignificant isolated changes.

The progress bar remains active during rendering, analysis, PNG encoding and assembly. Status text identifies the current view (for example 2/4) and phase. Use **Cancel** in PBR A/B or **Cancel export** in the saved-view manager. Cancellation is checked between renderer operations, analysis blocks, images and assembly steps; an active native PNG encode finishes before its next checkpoint. Existing report files are replaced only after all requested views succeed. Temporary files are removed on normal cancellation/failure.


## Export every UDIM tile and selected camera views

Open **Saved views / UDIM reports** in PBR A/B. Save camera poses or load a camera preset, then use **Include / exclude** to choose the views and the up/down controls to order them. Choose the shared **Report resolution** and **RAM budget MiB**. Under **All UDIM tiles on**, choose **Sphere** or **Plane** and click **Export all UDIM tiles + included views**. Select a parent folder; the application creates a uniquely named `PBR_UDIM_Report_...` directory there.

The export includes the union of A and B tile numbers, in numerical order. Each paired tile gets a linked `UDIM_<number>.html` containing all included saved views, A/B images, heatmaps, regions and statistics. Open `index.html` for coverage and per-tile/per-view results. Tiles missing in A or B are explicitly marked **Missing A / Missing B** and have no image-difference statistics or placeholder comparisons. The batch can start while the active preview tile is missing on one side. Missing tiles count as coverage findings, not render failures.

This operation inspects **each tile separately on a sphere or plane**, including when the active preview shows a custom model. It does not repeat the same full-model render for every UDIM or claim to isolate a model's individual UV region. Use the existing saved-view export for the whole model. The all-tile surface choice is saved in projects/autosave. The active tile, camera, preview shape and prepared on-screen image remain unchanged.

Lighting, prepared HDRI, rotation/intensity, exposure, displacement, input routing, threshold and gain are frozen once for the batch. Only the tile material and saved pose vary. Each tile/view uses the chosen real output resolution; 2K/4K require GPU, CPU supports up to 1K. Texture proxies remain bounded. Statistics measure displayed viewport RGB, including background, rather than native EXR/UV-space values; counts from tiles/views must not be summed as unique source pixels.

Tiles and views run **sequentially**, under the report workspace/growth budget and hard global application RAM cap described above. Status identifies the UDIM, tile counter, view counter and rendering/analysis/encoding phase. The progress bar is active throughout. **Cancel export** in the manager or **Cancel** in PBR A/B stops at the next cancellation checkpoint. Native image encoding finishes its current image before checking cancellation.

Reports are staged in a private temporary sibling directory. The final report directory appears only after the complete batch and coverage index succeed. Cancellation, a renderer error or the 8 GiB total-output limit removes the staging directory during normal cleanup and leaves existing report directories untouched. Every run creates a new directory; existing report folders are never merged or replaced. Individual tile reports retain their 512 MiB combined-view limit and 100,000-region bound per view. Lower resolution or include fewer views if a size bound is reached.

Auto rendering remains consistent across the entire batch: a later GPU-to-CPU fallback restarts earlier tiles on CPU when the selected resolution supports it. GPU errors at 2K/4K abort without publishing a partial report. No network transfers or automatic browser opening occur. Copy the **whole report directory**, including the index and linked tile HTML files, when sharing. Tile documents include source-path metadata; review before sharing.


## Resume cancelled or interrupted report exports

Fresh multi-view, high-resolution single-view and all-UDIM report exports automatically keep a **`.pbrresume` checkpoint beside the output**. For example, `PBR_AB_views.html.pbrresume` or `PBR_UDIM_Report_...pbrresume`. A small `.lock` sibling prevents two exports from writing the same checkpoint. Completed A/B frame pairs are stored losslessly in SQLite transactions with full synchronization, so cancellation or a process crash does not require rendering those matching pairs again. An interrupted, uncommitted pair is recomputed.

From 0.29, use Resume in the recovery list to restore saved report inputs automatically. The older manual checkpoint picker still uses the current prepared PBR inputs. In **Saved views / UDIM reports**, click **Resume report…** and choose its `.pbrresume` file. The picker starts beside the last checkpoint, whose path is stored in project/autosave. The checkpoint recalls included/ordered saved poses, output resolution, threshold, display gain, report shape and tile. It uses at least the saved RAM budget and preserves a higher current report RAM budget. Manual Resume report uses current prepared inputs. Resume from the recovery task list restores its saved project/input snapshot after an explicit click, with external OCIO approval before parsing.

Matching is based on compatible render-cache revision, prepared map values, geometry, prepared HDRI, camera pose, resolution and rendering controls. Changed maps invalidate their corresponding render inputs rather than the entire set. Changing a pose or resolution creates new render work. Changing view names, detail crops, threshold or gain rebuilds analysis/HTML from existing frames without a new render. The report records the original renderer/time and whether each pair was reused. Status distinguishes reused and newly rendered pairs. Auto fallback still keeps the whole report's renderer consistent; a necessary switch to CPU can require new CPU frames.

**Reusing completed renders does not skip report assembly.** Existing frame pairs are verified/decoded and statistics, PNGs and HTML are rebuilt with the current metadata. Source proxies also need preparing after restart. If files changed after preparation, resume stops and asks you to reload versions/model/HDRI before proceeding. Damaged frame records are recomputed individually. An unreadable checkpoint database produces an error and is kept; start a fresh export to a new destination instead of deleting uncertain data automatically.

Final HTML files remain atomic, and an all-UDIM directory is published only when the complete batch succeeds. Interrupted temporary HTML staging is removed during normal cleanup; after an abrupt crash a private `.udim-pbr-*` staging directory may remain, while the checkpoint remains usable. Resume writes a new staging directory. A completed all-UDIM output folder is protected and cannot be overwritten by Resume; start a new export for an updated set. Single cached **Preview (512)** exports already use the displayed frame pair and do not create a separate render checkpoint.

Checkpoints retain source-independent rendered images and saved export settings, with an **8 GiB limit per checkpoint**, under the existing global/report RAM limits. They remain after successful export. Once a report is final and you no longer need resume, you may delete its `.pbrresume` and `.lock` files while no export is using them. Keep the checkpoint beside the original output location until recovery is complete. It contains material images and camera/settings data; treat it as project data when sharing or archiving.


## Automatic startup report recovery

From 0.28, every GUI report export registers its checkpoint **before starting**, in the local `pbr-report-recovery.sqlite3` index beside application settings. This index survives cancellation, application crashes and normal restarts. Startup checks known checkpoints in the background and automatically opens **Interrupted PBR report exports** when unfinished or unavailable tasks are found. The window is non-modal: it does not block project restoration or start rendering automatically.

The list shows **State, Report, Resolution, Views, Cached pairs and Output**, with the checkpoint/project paths in its details. **Cached pairs** is the count of stored render records, including older poses/resolutions in that checkpoint, rather than a percentage of the current job. Select **Interrupted** and click **Resume**. No `.pbrresume` file picker is needed. From 0.29 the saved project, A/B folders, model, HDRI and scene settings are restored and prepared automatically. **Open project** opens the associated saved project when known, or the normal project picker. Once inputs are ready, the selected report resumes with its saved views/export settings and matching completed renders are reused. **Stop waiting** or closing the task list cancels a pending resume request without deleting checkpoint data.

Startup only lists tasks. Explicit Resume restores inputs using the rules below and keeps OCIO trust prompts. Active-work waiting and automatic preparation are each bounded to five minutes. A report already running in another process is marked **Running** and cannot be resumed concurrently. Missing/offline or unreadable checkpoints remain **Unavailable** with a reason, so reconnect the disk and use **Refresh**. Existing blocked output directories remain protected.

Completed reports are hidden by default; enable **Show completed reports** to inspect them. A completed checkpoint whose output was removed is recoverable again. A whole-set index published immediately before a crash is recognized as completed. **Hide from list** hides the local registry entry only; it never removes reports, checkpoints or source files. Explicitly resuming the checkpoint or starting a new export with that checkpoint makes it visible again.

Open the list later from **Saved views / UDIM reports → Interrupted reports**, or search **Interrupted PBR reports** in Ctrl+K. **Refresh** checks current states. **Add report folder** discovers `.pbrresume` files in one chosen directory without browsing individual checkpoint files.

For existing 0.27 work, discovery also uses checkpoint locations in the current/recovered project and looks for sibling checkpoints in their parent folders and known output directories. **No full-drive or recursive search runs at startup.** Discovery is bounded to 64 known folders, 4096 entries per directory and 256 tracked checkpoints; older checkpoints outside those locations can be added by folder. The scan reads bounded checkpoint metadata and counts records without decoding/rendering image frames. It can safely recover an interrupted SQLite transaction under the checkpoint's exclusive lock.

Compatible 0.27 render records remain reusable in 0.28. Cache compatibility now follows an explicit renderer revision rather than invalidating every render at each UI release. Incompatible render changes must use a new render-cache revision. Recovery indexes and checkpoints stay local; no data is uploaded automatically.


## Restore project and inputs with one Resume action

From **0.29**, new GUI report exports save a **validated project snapshot inside `.pbrresume`**, together with the report settings in one synchronized SQLite transaction, before worker rendering begins. The snapshot includes the project identity/workspace, A/B folder paths and selected sets, input/packed-map routing, map-recognition rules, model and HDRI paths, encoding/lighting settings, camera, displacement and export controls. Unsaved projects are supported; the original `.udimforge` file is not required to restore this saved report context. Existing project files are never overwritten by creating a report snapshot.

Click **Resume** on an interrupted task. The application checks required folders/resources, checkpoints current project work, restores the report project through the normal project controller, prepares A/B maps plus the saved OBJ/FBX and HDRI, and continues the report when preparation succeeds. No checkpoint, texture-folder, model or HDRI picker is needed in this normal path. The separate **Resume report…** file picker continues to be available for intentionally using already-prepared inputs.

If another operation is active, the request waits for an idle project instead of interrupting that work. **Stop waiting** cancels the request; during automatic input preparation it also stops that preparation safely. Existing completed report frames and source files are retained. Other production-queue work restored with the project remains paused: only the selected PBR report is resumed automatically. The list/status indicates restoration and preparation before the existing report render/analysis progress takes over.

The snapshot uses the **effective OCIO settings of the prepared report**, rather than a project configuration that was still awaiting approval. An external configuration requires **Trust report OCIO?** approval after Resume and before configuration/LUT parsing. Declining stops restoration before replacing the current workspace. Built-in configurations need no trust prompt. Approved transforms are validated before switching projects. A missing folder, model or HDRI blocks automatic resume with the affected paths; decoder/import failures stop the request rather than producing a report with an unintended missing model or Unlit fallback.

For **older checkpoints without an embedded project snapshot**, Resume uses the associated saved project when available, or a matching current project autosave. If no trustworthy input context exists, it explains that the original project must be opened once. It cannot reconstruct source paths that older checkpoints never stored. New exports and successful manual resumes attach the full context automatically.

The checkpoint stores project metadata and rendered pairs, **not copies of textures, models, HDRI or LUT files**. Those dependencies must still be available at their saved paths; use **Relink inputs…** in the task list when moving assets. Keep the full `.pbrresume` file for recovery. Project metadata retains its existing 128 MiB validation bound and shares the checkpoint's disk budget. Startup discovery still reads only small task metadata and does not load full projects until explicit Resume.

## Relink moved report inputs

From **0.30**, select an unfinished task in **Interrupted PBR report exports**, click **Relink inputs…** and choose the new project directory. The background search proposes A/B folders, the model, HDRI and an external OCIO configuration. Review the saved and replacement paths; ambiguous matches require a choice or **Browse…**. **Apply links** saves the replacements; **Apply & Resume** also restores and continues the report.

Matching uses relative directory layout, trailing path components and recorded texture filenames. Existing inputs are kept by default; enable **Relink all inputs** when switching to another copy. If the checkpoint moved too, a unique file with the same name under the chosen root can be found automatically. For multiple matching checkpoints, use **Add report folder** first to select the intended task.

Relinking changes checkpoint metadata only and retains cached frame records byte-for-byte. Resume checks actual prepared pixels, geometry and lighting: unchanged inputs reuse completed renders, while changed inputs render again. Existing published reports are protected. Original project files and source assets are not overwritten. OCIO folder assignments are remapped, but configuration contents and absolute LUT references inside them are not edited; normal external-config approval still applies on Resume.

Search is cancellable and bounded to 200,000 entries, 20,000 directories and 90 seconds; symlinks and junctions are skipped. Choose a narrower root if a limit is reached. No texture, model or OCIO decoding occurs during search.

## Check report inputs before Resume

From **0.31**, select a report and click **Check inputs…**. A background, read-only check lists saved A/B folders, individual included texture files, model, HDRI and external OCIO config, plus output free space and stored render-pair count. Missing, unreadable and empty required files block Resume in this review. Changed texture size/timestamp is marked **Review**; metadata changes alone do not invalidate a render.

**Stored pairs are candidates, not a reuse guarantee.** Exact reuse is determined on Resume from prepared pixel, geometry, lighting and rendering inputs. The check reads file metadata and one byte per file, without decoding images/models or parsing OCIO/LUTs. It cannot diagnose codec corruption; normal preparation/decoder validation still runs on Resume. Folder availability is not a complete dependency inventory when an older snapshot has no texture list; a warning explains this limitation. New, unrecorded files are rescanned during normal preparation.

Use **Export diagnostics JSON…** for the complete results (including full local paths). The table shows up to 5,000 rows with problems first, while JSON includes all checked rows. Checks are cancellable, limited to 100,000 saved texture records and a cooperative 90-second deadline between filesystem operations. An offline filesystem operation itself can take longer to return. Published reports/output conflicts remain protected. Checking does not restore a project or render; **Resume** remains an explicit action and revalidates resources.
