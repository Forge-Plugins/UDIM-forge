# Format conversion and production workflows

For the current scope-based export flow, typed rules, tile navigation and final verification, see [Export workflow](exporting.md).


UDIM Forge 0.2.0 adds one persistent queue for preparing whole texture sets. Open **Tools → Convert image format…** or **Tools → Production workflows / task queue…** in the main header. Right-click selected main-table rows to convert only those images. All processing exports new files; original textures remain unchanged.

## Target presets and exact recipe preview

Open **Target presets / recipe…** in the main Output section, or search `target presets` in Ctrl+K. In Production Workflows, choose a **Target preset** and click **Apply preset**. Presets switch to Whole-set recipe mode and retain source basenames and UDIM tokens in folders grouped by detected set and map type. The main Start conversion settings remain a separate workflow: use Start queued workflow for the applied delivery recipe.

Built-in choices:

- **PBR 4K / OpenGL / separate maps**: Color PNG8, Normal PNG16 with normalization, other maps PNG16, longest edge 4096; Displacement EXR float32 at original resolution. No normal Y flip.
- **PBR 4K / DirectX / ORM**: the same map policies plus an explicit normal Y flip and ORM packing. This assumes source normals use OpenGL convention; orientation cannot be inferred from pixels.
- **PBR 2K / OpenGL / ORM**: longest edge 2048 and ORM packing, with displacement retaining original resolution and EXR float32.
- **Archive / EXR float32 / original resolution**: no resizing, packing, grading or implicit normal orientation conversion.

Preset resolution limits only reduce size. PBR recipes use Automatic compression and Keep Alpha. ORM outputs use the existing RGB PNG16 pipeline with R=AO, G=Roughness and B=Metallic; missing/ambiguous inputs block planning unless missing-input defaults are explicitly enabled. Color grading, sRGB filtering and OCIO baking are OFF in these presets: existing samples are preserved through format conversion unless a rule explicitly requests another operation. A preset does not infer or assign a new color space.

**Save current as…** saves a validated local custom target preset, including format/depth, compression, Alpha, filter, per-map resolution/point rules, normal Y flip, packing, missing-input policy, sRGB resize and OCIO bake switches. Reusing a name replaces that custom preset. Presets are independent of texture paths and stored in `%APPDATA%\UDIM Studio\target-presets.json`. Save/load workflow JSON remains available for exchanging recipes. OCIO bake uses the currently trusted application configuration and input assignments, rather than embedding an arbitrary config path in a target preset.

After **2. Review output plan**, select a row and choose **Before / after…** (also in its right-click menu). The preview runs the actual full-resolution export and its dependencies in a separate capped worker queue, within the global RAM budget. It writes into owned temporary storage, runs preflight and leaves the production queue and originals untouched. This is an exact representative output, not an instant low-resolution approximation; large images and shared displacement statistics can take time and temporary disk space. Preview one output at a time.

The completed sample opens the shared A/B viewer: side by side, Wipe, difference view, synchronized zoom/pan and native pixel inspection. Color/Normal/packed outputs start in RGB. OCIO display and automatic contrast start OFF to avoid double-applying a baked transform. Both remain viewer-only controls. Native difference metrics use stored sample units: PNG8 and PNG16 numeric codes differ even for visually equivalent normalized values. For packed outputs, A is the first original input and B is the packed output, not a same-channel identity test. Closing the preview stops its workers and removes its temporary files; it also closes its associated comparison window.

**Color / data details…** describes the selected output's actual point operations, resize transfer, format, Alpha, compression and OCIO bake/input assignment. The main Tools menu offers **Selected export color / data details…**, and Batch Texture Tools has an equivalent details button. An OCIO display transform only changes exported RGB when Bake is explicitly enabled. Material recipes block exposure/gamma/saturation and OCIO/sRGB transforms on data maps, including unknown EXRs; explicit numeric multiply/offset remain available. Batch map utilities still accept intentional numeric edits, which are shown in their details. Integer output clamps to 0–1 and quantizes; float preserves signed/HDR ranges subject to output precision.

Changing common format/resize/packing controls or per-map rules disables the old plan. A changed OCIO configuration/input assignment is also detected before starting or previewing it. Prepare a new plan after any such change.

## Operation status and cancellation

The queue shows **Preparing**, **Running**, **Cancelling**, **Waiting for RAM**, **Paused**, **Needs attention** and **Completed** states as appropriate. A shared progress bar reports completed jobs plus current worker progress, alongside active/pending/error counts. Rows distinguish input verification, processing and output verification; completed/error/blocked rows use different text colors. ETA is approximate and varies with image size, compression and job complexity; it is not a completion guarantee. Main conversion and batch tools also show progress and an ETA once enough progress is available.

Cancel/Pause requests stop active workers safely and retain completed production outputs for Resume queue. Cancelling is displayed until workers have stopped. Recipe-preview cancellation removes its temporary output. Preparing a plan is distinct from exporting it; the export Start control becomes available after a valid plan.

## Which Start button?

**Export… → Review output plan → Check & start export** in the main window applies UDIM naming, folder layout and the selected **Output image format / bit depth** in one operation. Choose EXR, PNG or TIFF (or JPEG), compression and Alpha directly beneath the naming controls. The preview shows final extensions and collisions before starting. With resizing disabled, **Keep original format** copies bytes unchanged and disables compression/Alpha controls. Image-encoding settings persist in normal settings and saved profiles; legacy profiles default to Keep original format.

Encoding exports new files using a neutral pixel recipe, with no OCIO bake. Format-only conversion does not normalize normals; actual normal-map resizing does. Float EXR preserves signed/HDR heights; integer formats clamp to 0–1. Reports contain source/output sizes and hashes, encoding settings and clipping counts. SHA verification for encoded files verifies the written output hash; it is not a source/output byte-equality test. Main conversion cancellation retains completed staging copies for Resume conversion; changed sources require review. The context-menu Convert selected files action uses these same main-window settings.

The separate format/queue tool remains available: open **Convert image format…**, choose **Output format / bit depth**, then **2. Review output plan**. Review the target filenames and press **3. Check & export**. Whole-set recipes and the other operations use **3. Check & export**. A Start button is enabled after a valid plan has been prepared. Changing settings requires a new review. Per-map rules are edited in a table; recipe JSON remains a file format only.

## Formats by map type in the main window

Enable **Use formats by map type** beneath the main output format. Set Displacement, Normal and Color directly in the three inline selectors. The preset selects Displacement → EXR float32, Normal → PNG16, Color → PNG8. **All map types…** adds rules for AO, Roughness, Metallic and custom classifications. **Use global format** removes a type override; **Keep original format** retains that type’s container and depth; it copies bytes unchanged when no resize rule applies. Color/Albedo/BaseColor aliases share the same rule. Apply type updates the effective format along with the naming preview.

One **Export… → Review output plan → Check & start export** runs the complete mixed-format set. Save as a profile to keep both rules and their enabled state. Older profiles leave overrides disabled. Use Automatic compression for mixed EXR/PNG/TIFF outputs; incompatible explicit codecs are reported in preview. Native float displacement and ordinary Normal samples are not graded or OCIO-baked by this operation.

## Resolutions by map type in the main window

Enable **Use resolutions by map type** below the format selectors. Choose Displacement, Normal and Color limits directly in the main window. **8K / 4K / 2K preset** sets Displacement to 8192, Normal to 4096 and Color to 2048 pixels. These are **longest-edge limits**: aspect ratio is preserved, dimensions are rounded to whole pixels and smaller images are never enlarged. For example, a 16384 × 8192 displacement becomes 8192 × 4096 with the 8K limit.

Choose **Keep original resolution** to skip resizing for a type, or type an integer from 1 to 32768 for a custom limit. Types without a rule keep their original dimensions. **All map types…** edits format and resolution together for AO, Roughness, Metallic or a custom classification. Rules use the same map-type aliases as format overrides. If a file is misclassified, correct it with **Apply type** before converting.

**Filter** selects Nearest, Box / area, Bilinear, Bicubic or Lanczos 3. Normal maps are treated as tangent-space RGB vectors encoded in 0–1 and renormalized after actual resizing. Displacement is spatially filtered without per-tile value normalization: signed/HDR values remain in float when EXR float32 or TIFF float32 is selected. Color is filtered in stored values with alpha association, with no implicit gamma or OCIO transform. Use the dedicated resize tool for explicit sRGB linear-light filtering.

One **Export… → Review output plan → Check & start export** applies naming, folder layout, per-map image format/depth and per-map resolution. The preview includes the selected edge limit/filter. **Profile → Save as…** saves resolution rules, their enable switch and the filter alongside formats; ordinary settings also retain them. Older profiles leave resizing disabled. Disable the resolution checkbox to temporarily retain all source dimensions without deleting the rules.

With resizing enabled and **Keep original format**, supported PNG, EXR, TIFF and JPEG inputs retain their container and sample depth, but may be re-encoded; JPEG is lossy. For PSD/PSB or unsupported/mixed sample depths, choose an explicit output format. Compression/Alpha policies apply during re-encoding. Choose Automatic compression for mixed containers. A disk-backed float32 intermediate avoids premature integer quantization and keeps large-image work bounded; it requires free space on the output drive and is removed after use. Cancellation preserves original textures; completed batch copies or resumable main-conversion staging may remain.

The output manifest records each limit, filter, actual dimensions, normal normalization, effective encoding and source/output hashes. Displacement ranges are not remapped. Filters can smooth peaks or introduce ringing; Nearest and Box / area have different sampling tradeoffs from Lanczos.

## Source and output columns before Start

The main table shows **Source size**, **Output size**, **Source format**, **Source depth**, **Output format** and **Output depth** for every row. All six columns appear before the output filename. For example, a 16384 × 8192 TIFF float32 Displacement with the 8K/EXR32 rule shows **8192 × 4096**, **EXR**, **32-bit float** as its planned result. Dimensions are pixels; depth is per sample/channel, with float and unsigned/signed integer types distinguished. EXR dimensions describe the data window, not an optional larger display window.

Headers are read in one background worker without decoding texture pixels or preparing PSD/PSB composites. **Reading…** and a pending-header count are shown until inspection completes; Start conversion waits for the planned files’ header reads. Failures show **Unknown** with a Header warning rather than guessed dimensions. Ordinary byte copying can still preserve a file whose header is unsupported; use **Validate sets…** to check integrity and conversion eligibility. Header inspection alone does not verify compressed pixels, NaN/Inf, PSD saved-composite compatibility or every codec constraint.

Changing output formats, type assignments, limits or profiles immediately recalculates the planned columns using cached headers. Source dimensions/depth remain unchanged. **Keep original resolution** and reduce-only limits on smaller inputs show the original dimensions. Unlisted types inherit the global format and retain their size. Excluded/skipped rows retain source metadata and show a dash for output fields.

With both Keep settings, source/output metadata match the byte copy. With a resize rule and **Keep original format**, the pipeline re-encodes supported inputs; packed PNG/TIFF inputs can expand to their decoded native depth (for example PNG 1-bit → PNG 8-bit). The output column reflects this. Mixed-depth or unsupported retained encodings request an explicit format. For multi-page TIFF/multipart EXR, source columns explicitly describe the first image; ordinary copying keeps every image, while image encoding is marked unsupported for those inputs.

**Scan files** refreshes cached headers, even when paths/size/timestamps have not changed. Scan again after editing source files externally. Horizontal scrolling reaches full output paths/status, while selecting a row also shows its complete filename and source → output summary below the table. Background header updates preserve selection and scroll position. Search includes metadata, and **Export visible CSV…** includes all six metadata fields; exporting while inspection is pending keeps the visible Reading/Unknown state rather than inventing values.

## Sorting and filtering the main table

Click any column heading to sort ascending; click it again for descending. The active heading shows **▲ / ▼**. Resolution sorts by pixel count, then width/height; UDIM and sample depth sort numerically. Filenames and output paths use case-insensitive natural ordering (map2 before map10). Equal values keep scan order. Unknown, pending, skipped and unsupported metadata remain at the end in both directions. The inclusion column sorts by its checkmark; it never changes inclusion.

The filter bar has independent **Source** and **Output** selectors for resolution, format and depth. Combine them with **All map types** and Search; every active condition must match. For example, select Output **PNG**, **8-bit uint** to find low-depth planned results, or Source **EXR**, **32-bit float** to inspect high-precision inputs. Each list is populated from the whole scanned set, independently of the other filters.

Resolution choices include exact dimensions and longest-edge choices such as **4K edge** (4096 pixels). The edge filter includes 4096 × 4096, 4096 × 2048 and 2048 × 4096, but excludes 8192 × 4096. Exact dimensions distinguish landscape/portrait. **Unknown**, pending or dash choices appear when those states exist. Active filters update as headers arrive or output/type/profile rules change. A selected value is retained when it no longer exists, giving zero matches rather than silently clearing the filter.

The preview summary shows **visible/total shown**, alongside the total planned conversion count. **Clear filters** resets all six selectors, map type and Search; it keeps the chosen sort order. Selection and horizontal/vertical scroll are retained for rows that remain visible. Hidden selections are cleared so context-menu operations apply to visible selected files.

**These are view filters. Start conversion still processes every included file in the plan, even when no rows match the view.** Use Include / exclude or right-click **Convert selected files…** to restrict processing. Sorting/filtering never rewrites the source list, map rules, filenames or inclusion flags. View filters/sorting are independent of conversion profiles and comparison sessions.

**Export visible CSV…** exports only currently visible rows, in their displayed sort order, with source/output metadata and status. Clear filters first to export the entire preview. The final conversion manifest and mapping CSV continue to describe the actual batch, independently of the view.

## Right-click per-file settings in Production Workflows

Ctrl/Shift-select preview rows, right-click, then choose **Change image format / bit depth…**, **Change resolution** (Keep, 16K/8K/4K/2K/1K or Custom/filter), or **TX storage / compression / threads…**. Edits apply only to selected rows. Dependent sources and packing paths follow extension changes; collisions are rejected. A Copy job needs an explicit image output format before resizing. Reports, statistics and packed-channel nodes are protected from unsupported direct image edits.

After enqueueing, pause the queue before editing unfinished Queued/Error/Blocked/Cancelled jobs. SQLite saves these changes, and edited failures return to Queued; **Resume queue** starts them when ready. Running and Done jobs are protected. The exported forge-workflow.json is the initial enqueue snapshot; later per-job edits live in the durable queue. Changed settings invalidate previous prepared-output reuse. Input inspection and Open output are also available in this menu.

TX resolution edits resize before building mipmaps. Integer storage is retained when Keep is selected; normal-map resampling uses normal renormalization when the job was classified as Normal. A TX task can also be changed into an EXR/PNG/TIFF image export through the format command.

## True image formats, depth and Alpha

Available outputs include PNG uint8/uint16, TIFF uint8/uint16/float32, JPEG uint8, and EXR half16/float32. Half has a finite magnitude limit of 65504; exceeding it aborts that output rather than creating infinity. Integer outputs normalize unsigned source samples and clamp processed values to 0–1. Use EXR float32 for signed or high-range displacement. This is actual decoding/encoding, not an extension rename.

Choose **Automatic** compression for recipes with mixed formats. Explicit EXR compression supports ZIP, PIZ, RLE and None; TIFF supports Deflate, LZW and None. Unsupported format/codec combinations are rejected. PNG uses its lossless codec; JPEG is lossy.

**Alpha policy** offers Keep, Drop and Opaque. Opaque sets an existing Alpha to one; it does not invent a missing Alpha channel. JPEG cannot preserve Alpha: explicitly choose Drop for RGBA inputs. An Alpha-only image cannot lose its sole channel. Unsupported multipart/multipage or ambiguous channel layouts produce an explicit error instead of silently dropping data. Saved PSD/PSB composites use the existing disk-backed preparation path.

## Several sets and one durable queue

Use **Add folder / set…** repeatedly, **Use main file list**, or **Use selected files**. Choose an existing **Output parent**. Each plan creates a unique Forge output directory. Per-file input/output paths appear in the preview; sources are checked against the revision captured by the plan before execution.

**Workers** controls 1–4 simultaneous processes. **Queue memory cap** limits their combined committed memory, including native subprocesses. It excludes the interactive application/viewers. Allow at least 512 MiB per worker; large native operations may need more. Exceeding the cap fails a job safely; raise the budget and retry. This is a hard safety limit, not a guarantee that every decoder can process every image within that budget.

**Cancel current work** stops plan preparation and pauses the active queue, including native maketx subprocesses. It retains completed outputs and leaves unfinished jobs queued. **Pause queue** has the same queue-stopping behavior without cancelling preparation. **Resume queue** restarts remaining files; it does not resume halfway through a scanline. **Retry failed** retries errors and blocked dependencies. Closing the workflow window leaves the queue running; closing the application pauses it. On the next launch, reopen Production workflows and Resume. No work automatically restarts on launch.

The SQLite queue is stored at `%APPDATA%\UDIM Studio\production-queue.sqlite3`; control records, scratch intermediates and completion markers are in `queue-work` beside it. Final-output staging is created beside the destination as `.udim-job-<id>`, on the output drive, so publishing never requires a cross-drive rename. Interrupted owned staging is checked or replaced on retry and excluded from recursive scans. One application instance owns that queue at a time. Completed output and source SHA-256 hashes are verified before reuse. An output committed just before an interruption can be adopted from its owned marker; unrelated existing files are never overwritten. Changed inputs need a fresh plan. Do not edit/delete the queue database while jobs are running.

Each batch also saves `forge-workflow.json` with reviewed jobs and parameters. These manifests and queue records contain local paths. They are local records, not part of diagnostic exports.

## Whole-set recipes

Choose **Whole-set recipe**. Common controls select output format, longest edge (zero preserves resolution), filter, Alpha, compression and optional ORM/RMA packing. **Per-map rules** overrides format, edge and point operations by detected map type. The **4K material preset** provides a starting point:

- Color: common format and 4K, optional exposure/color edits.
- Normal: PNG16; normalize vectors and renormalize after resampling. Flip Y is explicit.
- Displacement: EXR float32 with original resolution.
- AO/Roughness/Metallic: processed tile inputs are dependencies of the packed output.

For example, replace the Color rule with `{"exposure": 0.5, "saturation": 0.9, "edge": 4096}`. Additional point keys are multiply, offset, gamma, normalize and flip_green. Set skip to true to exclude a map type. Save/load recipes as JSON; saved rules remain available even if the current operation is format-only. Existing main conversion profiles remain independent.

**Resize sRGB Color maps in linear light** is an explicit option for known sRGB Color inputs. Leave it off for already-linear inputs or data maps. It is not inferred from file extensions. Normal vectors use their dedicated resampling path.

The ordinary format-only operation ignores per-map recipe rules. It preserves samples apart from the selected encoding/depth/Alpha and any explicitly enabled common resizing or normal-Y conversion. In particular, a simple Normal format conversion does not silently normalize vectors.

## Explicit baked OCIO exports

Viewer OCIO remains display-only. **Bake current OCIO Display/View into COLOR exports** is a separate, off-by-default export operation. It uses the current config and scoped input assignments on float RGB before quantization. Alpha bypasses the transform. Normal, displacement and mask maps bypass it; direct attempts to bake into protected data maps are rejected.

This bakes the selected display/view appearance, not an arbitrary output-color-space transform. Keep the configuration and external LUT resources accessible and unchanged until queued work finishes. Baked results contain modified Color samples and should not be confused with scene-linear source textures.

## Displacement tools

Choose **Displacement**. A shared min/max is computed once for each detected texture set, then every tile uses that range. Normalizing separate tiles independently would alter their relative heights; this workflow uses the common range. **Preview shared min/max** inspects loaded height files without exporting.

**Normalize** maps the shared input minimum/maximum to Target min/max. A uniform set cannot be normalized to a nonzero interval. **Neutral / scale / offset** applies `(input − neutral) × scale + offset`. By default only detected Displacement maps participate; **Treat all loaded files as height** is an explicit override. Unsigned integers use normalized 0–1 input units; float heights retain native units.

The output directory contains `source-range.json` and each texture has a `.height.json` sidecar recording source range, affine scale/offset, neutral and inverse parameters. A zero scale has no inverse. For an affine transform, recovered input is `(output − offset) / scale`; integer quantization/clipping can make exact recovery impossible. Prefer float output for height processing.

## Quality findings

Choose **Quality validation** to produce `quality.json`. It reports native min/max, uniform maps, possible 0/1 clipping, normal-vector length problems, all-zero Alpha, inconsistent Alpha presence, corrupt images and NaN/Inf. These are advisory findings: constant maps and endpoint values can be intentional. Set the normal tolerance and explicitly select signed −1…1 normals when appropriate; the default expects encoded 0…1 vectors.

Select the completed job and **Open output** to inspect the table. Double-click a finding or **Inspect original map** to open its original in the built-in viewer. Use the existing **Validate sets…** for resolution/depth consistency and broader set completeness checks. Neither validator repairs data automatically.

## Visual HTML report

In **HTML A/B report**, choose reference/candidate folders and the native difference threshold, then use the report operation. The resulting single HTML includes embedded A/B thumbnails, missing/ambiguous tile information, comparison metrics and threshold status. It works offline and contains no external scripts. Heatmaps are explicitly labelled **preview-space, auto-scaled**; use native metrics or the interactive difference viewer for numerical thresholds. CSV exports remain available in the comparison windows.

## Tiled rendering textures

Choose **TX mipmaps** for bundled OpenImageIO `maketx`: 64×64 tiled textures, mipmaps, selected filter/wrap and Keep/half/float/uint16/uint8 storage. This exports `.tx` copies without a separate tool installation. NaN/Inf abort export. Large native processing shares the queue memory cap. **CPU threads per file** offers Auto or explicit counts; Auto resolves up to four threads shared across the planned workers. ZIP/LZW/None compression is selectable; None can write faster at the cost of larger files. Thread allocation and file staging were improved without changing the chosen mipmap filter. More threads are not always faster. PSD/PSB input first prepares its saved composite as float EXR; Keep therefore retains that intermediate float storage. Choose explicit storage when required.

For details see [OpenImageIO maketx](https://openimageio.readthedocs.io/en/main/maketx.html), [OCIO](ocio.md), [support and diagnostics](support.md), and [quick start](quick-start.md).

## Main workspace (0.4.0)

**Folders** contains the source/output locations and Scan files. **Conversion → Output** contains UDIM naming, the global image format/depth and folder layout. **By map type** groups per-map formats and resolutions; **Advanced** holds source naming, templates, compression, Alpha, verification and map detection rules. The tab height follows its controls, leaving more room for the preview table.

Open **Filters** above the preview for source/output resolution, format and depth filters; an active count remains visible when collapsed. **Selection** groups row actions and the Rename files with Apply type toggle. **Compare** and **Tools** group version comparison, production workflows, grading, resizing, OCIO, validation and cache tools. **Export… → Review output plan → Check & start export** remains at the bottom.

Use the visible **Pack ORM / RMA…** button in the main header to open channel packing. The tool is also available in Tools and the file-list context menu.

## Inspect UDIM boundaries

Use **Tools → UDIM boundary validator…** or **UDIM boundaries…** in the set validator. Choose a map type, stored channel and threshold in native units, then **Analyze boundaries**. Inspect the A/B border strips, highlighted differences, largest-difference marker and exact pixel counts; **Export CSV / JSON…** includes all connections. This compares stored neighboring image borders and cannot infer mesh/UV continuity or tangent frames. See [boundary validation](boundaries.md) for units, orientation, streaming limits and interpretation.

## TX from the main window

Choose **TX / mipmaps** in the main output-format selector, review `.tx` destinations and click **Export… → Review output plan → Check & start export**. Naming, folder layout and per-map resolution rules apply together. TX can also be assigned to individual map types and saved in profiles. Right-click one or more rows and choose **Convert selected to TX (mipmaps)** to export just that selection without replacing the global format setting.

This shortcut uses 64×64 tiles, Lanczos3 mipmaps, clamp wrap, ZIP compression and retained Alpha/native values, without an implicit OCIO/gamma transform. TIFF-backed TX stores half sources as float32; the main preview shows that promotion. Keep mode accepts uniform uint8, uint16, half and float32 inputs; mixed/higher precision channels require an explicit intermediate conversion. Normal-map resizing uses the existing normal-aware path. Each main-window TX export runs in an isolated 2 GiB worker with a 300-second deadline and cancellable child processes. Files publish only after completion/verification; originals remain unchanged. For explicit storage/filter/wrap/thread choices use **Production Workflows → TX / mipmaps**.

## Global application memory budget

Click **Memory … / … GiB** beside the cache controls in the main footer. Set the global limit in MiB and click **Apply limit**. This machine setting persists between launches and is independent of conversion profiles. The dialog shows current committed memory, peak usage, admitted tasks and waiting tasks. It counts the application and every descendant process, including nested native tools and the portable launcher.

The default is half physical RAM, clamped to 2048–16384 MiB. Manual limits are 2048–262144 MiB. Windows enforces a combined committed-memory cap; committed memory is different from resident physical RAM or the Task Manager working-set column. Per-worker limits can be stricter than the global limit. Native allocations that exceed either cap may fail and report an error; this is not a guarantee that an arbitrarily large model can render within a small limit.

The scheduler reserves 512 MiB of headroom and conservative per-task estimates before admitting more work. Requested production workers remain an upper bound: effective concurrency decreases under pressure and recovers as jobs finish. Active files continue, waiting jobs remain queued, and Pause / Cancel also works while waiting for memory. A queue waiting for budget says so in Production Workflows. Model imports and other interactive operations have cancellable, bounded waits; raise the limit or close large previews if needed. Limits below current usage plus 512 MiB are rejected.

Grading, resizing, channel packing, PSD ZIP preparation, PBR rendering, model loading and the EXR pool participate in admission. Under pressure, background cleanup releases disposable RAM caches and idle decoder processes. Saved sessions, analysis results and the persistent PSD/PSB disk cache are retained. This memory budget is separate from the PSD/PSB disk cache size limit and from the additional queue-local budget.

Windows job accounting includes nested processes; see [Microsoft job limits](https://learn.microsoft.com/en-us/windows/win32/api/winnt/ns-winnt-jobobject_extended_limit_information) and [nested jobs](https://learn.microsoft.com/en-us/windows/win32/procthread/nested-jobs).

See [Export preflight](preflight.md) for the checks performed before starting an export.
