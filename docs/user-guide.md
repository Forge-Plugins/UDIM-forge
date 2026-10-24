# UDIM Forge

A small, portable Windows application for converting **UV/UDIM texture filenames and folder layouts**. It works locally and does not upload textures. Run **UDIM Forge.exe** — no Python installation is needed.

Use **Help & Support** for embedded offline documentation, contact and GitHub issue links.

The rename preserves settings, profiles, caches and saved sessions. The settings folder intentionally remains `%APPDATA%\UDIM Studio`.

[Quick start](quick-start.md) · [OCIO guide](ocio.md) · [Naming reference](naming.md) · [Troubleshooting](troubleshooting.md) · [Support](support.md)

## Quick start

1. Choose your source folder. Enable **Include subfolders** if needed.
2. Select the source format, target format and folder layout.
3. Click **Scan files** and review the proposed filenames and map types.
4. Correct a map type for selected files or an entire series. Click ✓ to exclude a file. Use Ctrl or Shift for multiple selection.
5. Click **Create copies**, then **Open output**.

Each conversion creates a new `UDIM_date_id` subfolder in the output location. Originals remain unchanged. Filename collisions block copying until you change the layout or exclude duplicates. Cancellation removes only the current operation's temporary copies.

## Drag and drop

Drop one or more folders anywhere in the main window to select and scan them. Spaces and braces in paths are supported. **Include subfolders** controls recursion. When multiple folders are dropped, source folder names prefix their relative paths; duplicate folder names are made unique, and overlapping roots never scan the same file twice. Drop a single folder into **Output folder** to select the destination instead. Drops are disabled during copying or scanning.

## Settings profiles

Use **Profile → Save as…** to save a named profile. A profile stores source/target naming format, custom template, folder layout, gain placement, scan recursion, untiled copying, verification and map detection rules. **Load** restores the selected profile; when its scan settings change, the source is scanned again. **Manage…** lets you load, delete, import and export profiles as JSON. Importing a duplicate name creates a separately numbered profile. Saving under the same name updates that profile.

Profiles do not store or change conversion source/output paths. They also save OCIO configuration, display settings and input assignments, including absolute folder scopes. They are saved in `%APPDATA%\UDIM Studio\profiles.json`. Writes are atomic, and invalid or damaged profile files are not silently overwritten. Legacy profiles without OCIO leave the current color settings unchanged. Applying an OCIO profile validates its config and spaces first; keep external config/LUT files accessible on the receiving machine.

## Built-in image browser

Double-click any file in the preview list, press Enter, or right-click and choose **View image(s)…**. Files without a UDIM number can be viewed without enabling untiled copying. With several files selected, the browser shows that selection; with one selected, it browses the current visible file list.

Use **Previous/Next** or Left/Right keys to navigate, **Fit / 50% / 100% / 200%** to zoom, and drag the image to pan. Ctrl + mouse wheel changes zoom; the wheel scrolls, and Shift + wheel scrolls horizontally. **Transparency** previews alpha against a checkerboard. **Auto / Raw 0–1 / Auto contrast** affect the display only. The viewer loads up to a 2048-pixel preview; zoom percentages refer to that preview, not necessarily the original image dimensions. Original dimensions, type and preview size appear below the image.

### Channels, exposure and gamma

**Channel** selects RGB, R, G, B or Alpha when those channels are actually present. Scalar images expose their stored channel, such as Y. Single-channel views are grayscale, use the stored values without an automatic color transfer/contrast in Auto mode, and ignore the transparency mask. This lets you inspect RGB values even where Alpha is zero. Explicit **Auto contrast** can still normalize a selected channel. Files without Alpha do not get a synthetic alpha channel; if a selected channel is absent in the next file, the viewer returns to RGB and indicates that fallback. For layered EXR, the chosen RGB group's channel names are shown in the metadata; its alpha or a global alpha is included when present.

For EXR files, sliders and numeric fields provide **Exposure (EV)** from -12 to +12 and **Gamma** from 0.10 to 4.00. Exposure multiplies values by 2^EV before display transfer and clipping, so +1 EV doubles values and -1 EV halves them. Gamma applies a positive-value power of 1/gamma; gamma 1 is neutral and higher gamma brightens midtones. Auto RGB color display may first include the existing linear-to-sRGB transfer, so gamma is an additional adjustment to the current display mode. Auto contrast normalizes sampled values before exposure; use Raw data for unnormalized inspection. Negative values are clipped for display unless contrast mapping brings them into range.

**Reset EXR** restores EV 0 / Gamma 1 without changing the channel or display mode. **Raw data** also selects Raw 0–1. Exposure and gamma are disabled on non-EXR files, while channel selection remains available. The viewer caches sampled values at their original precision: moving these controls or changing channels does not repeatedly decode the same file or adjust an already quantized 8-bit preview. The cache is refreshed when the file changes. All adjustments are read-only viewing operations and never affect texture copies or displacement gain.

Bundled readers support JPG/JPEG, PNG, TIFF/TIF (including tested 16-bit RGB and floating-point LZW TIFF), EXR, PSD/PSB, GIF, BMP, TGA, DDS, WebP, AVIF, HEIC/HEIF, JPEG XL, JPEG 2000, QOI, PPM/PGM and other Pillow formats. **Formats…** lists the registered image extensions. The app uses Pillow, OpenEXR, OpenImageIO, psd-tools, tifffile/imagecodecs and HEIF/JPEG XL plugins; XnView MP is not required.

### Source pixel values

Move the pointer over the image to read **X, Y, R, G, B and Alpha** below it. The readout maps the cursor through zoom and panning to the **full-resolution source**, independently of the sampled preview. Values are read before exposure, gamma, auto contrast, color transfer, alpha compositing or 8-bit display conversion. EXR retains the selected channels in their native half/float/uint precision. Negative values, values above 1, NaN and infinity are preserved. Missing channels show **N/A**; scalar maps additionally show their actual stored channel, such as **Y**, without inventing RGB values.

Coordinates use a top-left origin and zero-based local X/Y. For EXR with a nonzero data-window origin, the readout also gives file-space EXR X/Y. **Pin sample** holds a pixel while adjusting the display; switching files clears it. Moving out of the image keeps a clearly labeled **Last sample** so the Pin/Copy controls remain usable. **Copy values** includes the source path, coordinates, original channel names, data types and values with up to 17 significant digits. The image context menu also offers **Copy source pixel values**. The live readout uses 9 significant digits for half/float32 and 17 for float64, sufficient to round-trip the native precision.

PNG/JPEG and other Pillow images report decoded channel code values (for example, 0–255 for uint8), rather than normalized 0–1 values. TIFF uses original native values where supported; Radiance HDR reports decoded floats. For palette/CMYK images, values refer to decoded RGB/RGBA. Multipage/animated files use the first page/frame. **PSD/PSB readouts use native saved-composite samples; layer pixels are not sampled.** Small images retain native buffers; large supported images keep lazy native block readers and bounded caches. Grid thumbnails retain only sampled pixels. EXR inspection currently uses the same first part/channel group as its preview, with full-resolution, nonsubsampled channels.

PSD/PSB reads the saved RGB/grayscale composite at 8/16/32 bits, using bounded native blocks at every size. Save in Photoshop with **Maximize Compatibility** enabled. Files without a real composite show an explicit message; an embedded thumbnail is not used as a substitute for native comparison. Layers/effects and ICC profiles are not rendered. RGB/grayscale raw composite values can include Photoshop's stored white matte; they are not reconstructed layer colors. GIF and multipage images show the first frame/page. EXR shows the first part and a selected RGB/scalar channel group. Deep EXR, large unsupported encoding variants and unusual formats may not have a preview; copying still works without decoding their pixels. No claim of support for every format recognized by XnView is made.

## Compare two maps A/B

Select two rows and click **Compare A/B…** or choose it from the right-click menu. With one row selected, that image fills A and **Browse…** chooses B; with no selection you can browse both files. The built-in image viewer also offers comparison in its file/image context menus. A and B may come from different folders and versions. **Swap A/B** exchanges them and reverses signed differences.

- **Side by side**: both maps use the same source-pixel scale and center.
- **Wipe**: A on the left, B on the right, with a slider or draggable vertical split line.
- **Absolute difference**: `abs(B − A) × Difference gain`; identical finite pixels are black.
- **Signed difference**: `0.5 + 0.5 × (B − A) × Difference gain`; zero difference is midgray, positive is brighter, negative darker.

Drag to pan both views together. **Fit both** resets framing; **Ctrl + wheel** changes shared zoom. The wheel pans vertically and Shift + wheel horizontally. Percent zoom is relative to original decoded pixels: **100% is one source pixel per screen pixel**, using native viewport samples rather than an enlarged thumbnail. Cursor readouts show native A and B values at the same local source coordinate. Local coordinates have a top-left origin; different EXR data-window origins are explicitly noted.

**Shared contrast** uses the same full-image min/max display range across both maps, avoiding independent normalization that could hide changes. EV and gamma also affect both views equally. **Raw display** disables contrast and resets EV/gamma. Integer channels are normalized by their dtype maximum only for these A/B display views; no automatic color transfer or alpha masking is applied. This is a data-inspection view. The **Channel** selector offers common stored channels plus **First channel** for explicitly comparing scalar Y against, for example, R of an RGB displacement map. The exact stored-channel mapping is shown in the summary.

Difference views use original decoded values in **native units**, before display normalization, EV/gamma or contrast. They use **Difference gain** only (1e-12 to 1e12, including scientific notation). For example, uint16 differences remain code-value differences; float EXR differences remain float-value differences. Compare matching encodings/units for meaningful results. Quantitative signed min/max, mean absolute difference, RMS, changed sample count and invalid pair count cover every source sample in the selected channels, independently of display gain. NaN/Inf pairs are excluded from these numerical metrics and highlighted magenta. Maps with different dimensions can be viewed side by side or wiped on the same local pixel grid; numerical difference is unavailable and no resizing/registration is performed. Raw 64-bit integer difference is also unavailable to avoid false zeros from double-precision conversion. Float16/32/64 and integer channels up to 32 bits are supported.

Comparison reads native channels; large supported maps use bounded native blocks instead of full-resolution image buffers. PSD/PSB saved RGB/grayscale composites support all A/B modes, native differences, shared zoom, pixel readouts and region navigation. Comparison is limited to the saved composite; it does not compare layer stacks or color-profile interpretation. EXR uses the same selected first-part channel group as the ordinary viewer. Source files remain unchanged.

### Highlight changed pixels and jump to the largest difference

Enable **Highlight |Δ| > threshold** in the A/B viewer. Cyan overlays pixels where any selected channel has a finite absolute raw `B − A` difference strictly greater than the threshold. The overlay works in side-by-side, Wipe and both difference views, identically on A and B. The threshold uses native source units, before EV, gamma, shared contrast, integer normalization and Difference gain. Equality with the threshold is not highlighted. Nonfinite pairs remain magenta, and out-of-image margins are not highlighted. The texture remains visible through the overlay. Below 100% zoom, overlays describe sampled viewport pixels; use 100% or higher to examine individual source pixels.

**Go to max difference** locates the largest finite absolute difference over every full-resolution pixel in the current **Channel** selection, even outside the visible region. It centers both views on that source pixel and raises shared zoom to at least **100%**; an existing 200%/400% zoom is preserved. Matching cyan crosshairs mark the location on A and B. The summary shows its coordinates and stored channel; the pixel readout includes the raw A/B values. Ties choose the first maximum in selected-channel order and then row order. Identical maps, entirely invalid samples, incompatible pairs and unfinished analysis disable the button. It can locate the largest nonzero difference even if that difference is below the highlight threshold.

The target and overlay use the viewer's selected channels from its decoded first EXR part/channel group, or first ordinary page/frame. **RGB excludes Alpha**; select **Alpha** to inspect it separately. Whole-set analysis still checks all EXR parts/channels and TIFF pages, so changes outside the viewer's channel scope can appear in the set report. These are explicitly separate scopes. Changing files or channel clears the previous target. A source file modified externally is refreshed before a jump.

In set comparison, the list's **Max |Δ| threshold** is copied into the viewer when opening or navigating pairs, and when the set threshold changes. You can also adjust it locally in the A/B viewer. Threshold changes prepare new overlay layers using cached native data; full-image metrics and the max target are reused. Moving Wipe reuses both prepared layers and does not rerender or decode sources, including when highlighting is enabled.

### Change regions and threshold pixel counts

The A/B viewer automatically analyzes the current channel selection and threshold over the **entire source tile**, independently of the visible viewport and highlight checkbox. **Above threshold** reports the exact number of pixels where at least one selected channel has `abs(B − A) > threshold`, together with the total tile pixels and percentage. RGB counts a pixel once even when R, G and B all change. The percentage denominator is the complete tile (`width × height`), not only finite pixels. Pixels with a nonfinite selected A/B sample or difference are excluded from changed pixels and reported separately as **Invalid**. Alpha is included when selected explicitly; RGB excludes Alpha. Values equal to the threshold are not above it.

Changed pixels are grouped into **8-connected regions**: neighbors sharing an edge or a corner belong to one region. No blurring, dilation, gap merging or minimum-area filtering is applied; isolated single pixels remain valid regions. **Next region / Previous region** navigate these groups in source scan order. Initially Next selects the first region and Previous selects the last; after selecting one, navigation stops at the ends. The summary shows the selected region number and its actual changed-pixel count, while an orange bounding box marks it on both A/B views (or the shared Wipe/difference canvas). Bounds include unchanged holes inside an irregular region; its pixel count does not include those holes.

Navigation centers both views on the selected bounding box and preserves numeric shared zoom, including 25%/50%. Fit changes to 100% when navigating a region. Zoom and pan remain synchronized afterward. Large regions may extend beyond the viewport at the chosen zoom; zoom out to see the full bounds. **Go to max difference** clears the selected region box and still works independently of the threshold.

Region computation uses full-resolution native data in a separate cancellable worker and does not block viewport rendering. Results are cached by file signatures, channel and threshold, with a 32 MiB metadata cache retaining the active result. Pan, zoom, EV/gamma, difference gain and Wipe reuse region counts; revisiting a cached threshold does not repeat analysis. Changing the threshold, source or channel clears obsolete region navigation immediately. Closing the window cancels work and releases cached metadata. Large maps use 64-row masks/labels with boundary merging, without allocating a full-image mask. Navigation metadata has a 32 MiB cap; unusually noisy inputs explicitly report limited navigation while preserving exact pixel and region counts. Small maps use the existing 512 MiB label-work guard. Region detection uses the bundled SciPy library; no separate installation is required.

### Wipe and A/B performance

Wipe prepares A and B once for the current viewport. Moving the split copies already displayed pixels; it never rereads source files, resamples native channels or recomputes statistics. Zoom, pan and display changes prepare a new viewport in a background worker. Recent viewports use a 96 MiB image cache; native decoded maps use a 512 MiB cache, retaining the active pair even when it exceeds that budget. File size and modification time invalidate changed inputs. Full-resolution difference metrics compute in a separate cancellable worker, so the image can appear while the summary says **computing in background**. Changing the split or pan never restarts that analysis. Raw differences and pixel readouts retain their original precision.

## Compare whole texture sets A/B/C/D

Click **Compare sets…** in the main window, its right-click menu, or **Sets A/B/C/D…** in the comparison window. Choose two to four version folders A/B/C/D, their individual source naming formats, and **Include subfolders** if needed. Empty folder slots are ignored. Click **Scan versions**. Scanning only reads files; it does not create copies.

Choose **Reference → viewer A** and **Compare → viewer B** in either the set list or the viewer. **Previous/Next Version** cycles through the other loaded versions without rescanning. The reference remains on viewer side A; the candidate is on side B, so signed difference is candidate minus reference. Changing the reference to the current candidate automatically selects another candidate. This is one shared two-image view of up to four loaded versions, with Side by side, Wipe, Absolute difference and Signed difference. Switching versions always preserves the selected tile, zoom, pan, Wipe position and display mode. **Keep zoom/pan between pairs** separately controls navigation between different tiles.

Automatic matching uses a stable union of tiles across all loaded versions. A tile present only in C or D remains in the list even when both active sides are missing. If any loaded version has multiple assets, automatic matching requires asset names across every version; sparse versions are never cross-paired as single renamed assets. Explicit Set A / Set B mapping applies to the currently selected reference and candidate and resets when switching versions. Pixel analysis and CSV export apply to that active comparison; switching versions cancels old analysis, clears its results and resets the difference-result filter. Re-run **Analyze paired sets** for the new comparison. Folder or naming changes in any slot invalidate all scanned catalogs until the next scan.

Maps pair by recognized map channel and UDIM, independently of filename UDIM syntax. Aliases such as Albedo/Color and disp/Displacement use the configured **Map type rules**. The table shows **Matched**, **Missing A**, **Missing B**, and **Ambiguous A/B** with candidate paths. Duplicate candidates are never selected automatically. Untiled, excluded or invalid filenames are counted as skipped; scanning does not replace **Validate sets** for checking image contents.

When every loaded version contains at most one asset, different asset basenames are allowed. When any loaded folder contains several assets, automatic matching also requires matching texture-set names and folder context, preventing unrelated assets with the same tile number from being paired. Select individual **Set A** and **Set B** to map renamed assets explicitly. Unknown **Other** channels match only identical parsed basenames; add custom detection aliases in Map type rules for other names.

Filter by map channel or matched/missing/ambiguous status. Double-click a row or click **Open comparison…**. **Previous pair / Next pair**, or Left/Right outside an input field, navigate the filtered list in one reusable viewer. **Pair list…** returns to the table. **Keep zoom/pan between pairs** preserves the shared source-pixel framing; disable it to fit each pair. Missing or ambiguous rows clear the previous image and show an explicit placeholder. Changing either folder or source naming invalidates the list until **Scan versions** runs again.

### Analyze all versions against one reference

After scanning two to four folders, choose **Reference** and click **Analyze all versions…** in the set list. The new window starts one combined run for every other loaded version. It includes all scanned texture sets, independently of the active Set A / Set B mapping and the main table filters. Automatic matching follows the same stable union of sets, channels and UDIM tiles used by version navigation. The worker compares one pair at a time to keep memory bounded and supports **Cancel analysis**; completed results remain available. A separate active-pair analysis cannot run at the same time.

Each table row identifies the compared version, texture set, channel, UDIM, difference status, maximum/mean absolute difference and RMS. Per-version summaries count changed, identical, below-threshold, attention and pending rows. Use the version and channel selectors plus **Changed above threshold**, **Identical**, **Below threshold**, **Needs attention**, or **Not analyzed**. **Max |Δ| threshold** uses original native pixel units with the strict rule `max_abs > threshold`; changing it reclassifies cached results without decoding textures again. Missing tiles, ambiguous matches, corrupt/unsupported files, NaN/Inf and incompatible layouts are kept as explicit attention statuses. Missing A means the reference is missing; missing B means the compared version is missing. Existing set-analysis format/precision limits also apply, including the explicit partial-support status for PSD/PSB document layers.

Double-click a result or choose **Open selected comparison…** to inspect that reference/candidate pair with the existing shared zoom, Wipe and difference modes. Changing the preview candidate keeps the combined analysis results. Changing the reference, any folder/naming setting, or rescanning invalidates the combined snapshot and cancels its work. After scanning, choose **Use current reference**, then **Analyze all versions** to rebuild it.

**Export combined CSV…** writes one atomic UTF-8 CSV with a BOM, containing every version/tile row, including rows hidden by filters. `visible_in_table` records the current filters. The report adds `reference_version`, `compared_version`, `reference_folder` and `compared_folder` to the normal detailed difference columns. It records the native threshold and Running/Complete/Cancelled state; unfinished pairs remain Not analyzed. Source timestamps are checked when rendering/exporting, so results from modified files become Stale. Cancelled runs can be exported; invalidated folder/reference snapshots cannot. The report cannot replace a source texture.

### Save and resume comparison sessions

Comparison sessions are automatically saved locally and restored when the app starts. Completed result records are committed immediately, before completion is displayed; valid folder/reference/threshold settings are checkpointed about once per second and on close. The local store is `%APPDATA%\UDIM Studio\last-comparison.sqlite3`. It uses SQLite transactions with durable WAL commits, writes only changed metadata records, and never stores texture pixels. After a crash, committed results are reused and unfinished or changed comparisons resume automatically. Do not delete the adjacent `-wal` / `-shm` files while the app is running. **Autosaved** confirms a successful checkpoint; an autosave failure is shown in the comparison window and main status, with the preceding checkpoint retained. Incomplete/invalid settings do not replace a valid checkpoint. A missing directory remains an explicit scan error; reconnect the disk and scan or load the session again.

**Sessions… → Recovery history…** lists local recovery points and their folder paths, reference, threshold and saved-result count. Create a manual recovery point or restore a selected one; source revisions are checked again and only missing/changed comparisons are computed. Automatic history preserves the previous session when its folders/reference/rules change, before the first changed checkpoint after restart, and about every five minutes of checkpoint changes. History retains at most 20 points and 64 MiB of compressed snapshot payloads (SQLite may retain freed pages for reuse). **New empty session** archives the current session before clearing its configuration. The history window also lets you disable **Restore last session when the application starts** while keeping autosave enabled. Local history shares the autosave database; use exported `.udimsession` files for independent backups.

Use **Sessions… → Save session… / Load session…** above the version folders. **Save session…** is also available in Analyze All Versions. A `.udimsession` file is ordinary versioned JSON containing absolute A/B/C/D folder paths, each naming format, recursive scanning, map-type rules, reference/candidate selection, native difference threshold and completed analysis records. Session rules belong to this comparison; loading them does not change the main converter's map-type rules. Sessions contain metadata and statistics, not texture pixels. Save during a run to keep completed comparisons; unfinished comparisons are resumed on load. Manual session files are portable snapshots; save again to update an exported snapshot.

Loading rescans the folders in a background worker, restores valid results into **Analyze All Versions**, and automatically analyzes only new, changed or unfinished comparisons. No image decoding is needed for unchanged results. Source identity includes absolute paths, size, modification time, file identity and change time (including the native Windows change time). Revisions are captured before and after analysis, so saving later cannot accidentally bless an old result with a new file revision. A candidate edit invalidates its affected comparison; a reference edit invalidates every candidate comparison using that reference tile. Added/deleted/renamed files and duplicate candidates are reparsed and re-paired. Transient errors and stale results are retried; unsupported or incompatible but unchanged files keep their diagnostic statuses. An incompatible analysis-cache version causes recomputation while preserving the session settings.

**Refresh changed** in Analyze All Versions performs the same rescan and selective update using the current in-memory results, without requiring a save/load round trip. The status reports **Reused** and **Analyzed** counts. **Analyze all versions** remains an explicit full recalculation. Existing progress, cancellation, threshold filters, comparison preview and combined CSV export work with restored results. A missing/inaccessible version directory is reported as a scan error rather than silently displaying old results as current. Session writes are atomic and cannot replace source textures. Loading malformed or unsupported session files reports an error before changing the comparison settings.

### Analyze differences across paired sets

After scanning, click **Analyze paired sets**. Analysis reads every pair in the chosen Set A / Set B mapping, independently of the channel and table filters. It compares original decoded pixel values, including Alpha, all nonsubsampled channels of every ordinary EXR part, and all ordinary TIFF pages. It runs one pair at a time in a cancellable background worker. Exposure, gamma, shared contrast, viewport zoom and displacement filename gain have no effect on the results. Source files remain unchanged.

**Pixel difference** shows **Identical** for exactly equal finite decoded samples, **Changed**, or **Below threshold**. Identical refers to pixel values, so different lossless compression can still produce identical maps. **Max |Δ| threshold** uses the largest absolute `B − A` sample difference in native units across the entire tile and every compared channel/part/page. A tile is Changed only when that value is **strictly greater** than the threshold; a nonzero change equal to or below the threshold is Below threshold. Zero-difference tiles always remain Identical. Float EXR uses original float values; integer images use original code units, e.g. 0–65535 for uint16. The same threshold applies to all map channels; use the Channel filter when inspecting maps with different scales. Scientific notation is accepted. Changing the threshold or result filter never rereads images or recomputes metrics.

The table shows maximum absolute difference, mean absolute difference and RMS. The summary counts identical/changed/below-threshold/not-analyzed tiles. The result filter offers **Changed above threshold**, **Identical**, **Below threshold**, **Needs attention**, and **Not analyzed**. Pair status filters and channel filters combine with this result filter. Missing/ambiguous pairs, NaN/Inf, incompatible layouts, stale files and decoding errors are never treated as identical or hidden as below-threshold changes; use Needs attention to find them. The details include nonfinite counts and diagnostic messages.

Comparisons require matching dimensions and channel names, EXR part names/data-window origins, page/frame counts, and compatible native sample types. Float16/32/64 may be compared directly; integer bit depths must match, and 64-bit integer differences are unavailable to preserve exactness. No resizing, registration, integer normalization or color-space conversion between encodings is performed. PNG16 values retain native precision. PSD/PSB layer stacks, deep/subsampled EXR, TIFF sub-IFDs, unsupported streaming encodings and partially decoded containers are marked **Not fully compared**. Ordinary image frames use the decoder's pixels; container metadata, color profiles and hidden codec-specific layers are outside pixel equality. Large supported images use streaming; unsupported whole-image decoders retain the 384 MiB safety limit. Use the regular validator for independent file-quality checks.

**Cancel analysis** preserves completed rows; remaining pairs show Not analyzed. Changing folders clears results, and changing the set mapping cancels obsolete work. A file that changes after scanning or while being analyzed is Stale and requires a fresh scan. CSV export also checks source timestamps so changed files do not keep an old Identical status.

**Export CSV…** saves a UTF-8 CSV with a BOM for spreadsheet compatibility. It includes **every pair in the selected set mapping**, including rows hidden by current filters; `visible_in_table` records the filter selection. Columns include set, map channel, UDIM, pairing/difference statuses, the applied threshold, signed min/max, maximum/mean absolute difference, RMS, exact nonzero sample count, finite/invalid counts, compared parts/pages, stored channels, dimensions, both source paths, diagnostic reason and run state. Exports made before completion retain Running/Cancelled/Partial state and explicit Not analyzed rows. Report writing is atomic and never replaces a source texture.

## Histogram, min/max and selected-area statistics

In the image viewer, click **Histogram / stats** to open the collapsible source-data panel. **Shift + drag** draws a rectangular selection; the panel opens automatically when a selection is completed. Normal dragging still pans. The selection is stored in full-resolution, zero-based, top-left source coordinates and stays attached to those pixels when zooming/panning or changing display settings. **Whole image** clears the selection. Changing files clears it too.

The **Histogram** tab counts every finite source sample using 256 bins. RGB displays separate R/G/B curves with a shared range; scalar images show their actual channel. The viewer's **Channel** selector also selects the statistics channel, including Alpha when present. RGB statistics ignore the alpha mask and include transparent pixels. The horizontal range uses the actual minimum/maximum, including negative and HDR values; it is not restricted to 0–1. **Log counts** changes only the vertical histogram display scale.

The **Statistics** tab reports minimum, maximum, mean, finite sample count, NaN, +Inf and -Inf per channel. Values come from every pixel in the whole image or selected area, not from the sampled preview. Nonfinite values have separate counters and are excluded from min/max/mean/histograms. An entirely nonfinite selection reports N/A for numerical statistics and no finite histogram. A constant selection still has a valid minimum/maximum/mean and populated histogram.

Exposure, gamma, contrast, color transfer and transparency never alter these statistics. Computation runs in a cancellable background worker, in small row blocks; changing file/channel/selection discards stale work. EXR and supported TIFF/16-bit PNG readers preserve native precision. Integer values remain code units (e.g. 0–65535 for uint16), without normalization. Other formats report the decoder's channel values; some advanced codecs may decode at lower precision than the encoded source. PSD/PSB statistics use saved-composite samples, including native uint16/float32 values; layers are excluded. Large supported files use two native row passes for exact statistics and histograms; temporary arrays stay bounded. Other whole-image decoders retain the 384 MiB safety limit.

## Set validator

After scanning, click **Validate sets…**. It checks all **included** files from the current scan, independently of search/type filters. Invalid or untiled filenames can still have their pixels checked. It uses the same texture-set grouping and manual **Assign set…** overrides as the UDIM inspector. Changes made after opening the window require **Validate included files** to run again.

Checks include full pixel decoding where supported, original dimensions, bit depth/sample type, and NaN/+Inf/-Inf counts. EXR validation examines every channel of every part, including channels not selected by the viewer. TIFF examines all ordinary pages. Comparisons across files use first-part/first-page dimensions and the original sample types; color, alpha and other stored EXR channels may have mixed types. PNG encoded bit depth is read from its header, including 16-bit RGB/RGBA. The viewer also uses a native PNG decoder to preserve those values for statistics.

- **OK**: supported checks completed without differences or problems.
- **Info**: differing resolution or bit depth between map types in the same set; these may be intentional (e.g. 8-bit Color and 32-bit Displacement).
- **Warning**: differing resolution or bit depth/sample type between tiles of the same map type and set.
- **Error**: nonfinite samples, a changed/unavailable file, or a decode failure. A decode failure can indicate corruption or an unsupported encoding variant; it is not presented as proof of corruption.
- **Partial / Not checked**: validation could not cover the entire file. These are never clean passes. Reasons include memory limits, deep/subsampled EXR, TIFF sub-IFDs, PSD/PSB individual layers, unsupported extensions, some encoded bit depths/mipmaps, or cancellation.

The report lists dimensions, sample types, NaN/+Inf/-Inf counts and per-file reasons. Click a row for detailed units/issues; double-click it to open the image viewer. **Problems / differences** filters out clean files. **Export report…** writes JSON (including all decoded part/page metadata and counts) or CSV. Report writes are atomic. Validation never repairs, renames or changes image data; copying remains a separate action. **Cancel** allows the current decoder call to finish, then marks unfinished files Not checked. Closing a validation/viewer window safely cancels its background work.

## Rename channel filenames with Apply type

**Apply type** now updates both the assigned map type and the output channel token by default. For example, selecting a tile of `body_displacement.1001.exr`, choosing **Color** and leaving **Entire series** enabled produces `Color/body_Color.1001.exr`, `Color/body_Color.1002.exr`, etc. **Albedo** or lowercase **color** may be chosen/typed instead: filenames preserve the chosen label, while recognized aliases use the canonical **Color** map folder. The active folder layout is respected; flat or source-folder layouts remain available.

**Entire series** applies to files with the same source parent and parsed basename, including tiles hidden by search/type filters. It does not rename other channels of the asset. With it disabled, only selected rows change. The **Rename files with Apply type** checkbox allows type-only assignment while preserving any existing pending filename edits. UDIM/UV coordinates, extensions and gain handling are formatted by the existing naming settings. Repeated changes replace the pending token instead of adding duplicates. The main preview shows final output paths and blocks copies on filename collisions.

**Rename channel…** handles arbitrary source names with three methods:

- **Automatic channel token** replaces a recognized alias while retaining the asset text/casing. If none is recognized, it preserves the unknown asset basename and appends the new channel. Ambiguous multiple channel tokens require explicit text.
- **Replace literal text** replaces a supplied case-insensitive substring, e.g. `body_weirdToken` → `body_Albedo` by replacing `weirdToken`. It requires that text to exist in every selected source/pending basename.
- **Set full basename** assigns an exact basename, e.g. `Smaug_Albedo` or simply `Color`, to the selected series. Tile tokens and extensions are added by current formatting settings. Do not include them in this field.

The dialog previews paths/collisions before **Apply to preview**. **Reset to source names** clears pending edits and restores filename-based classification. **Create copies** writes the renamed maps into a new result folder; Apply never renames originals in place. Reports preserve the effective output basename and gain so a copied set scans correctly on its next conversion. Pending edits are session-only until copies are created.

## Convert only selected files

Select one or more rows with Ctrl/Shift, then right-click **Convert selected files (N)…**. Right-clicking a row already in the selection preserves the whole selection; right-clicking another row selects that row. The viewer also offers **Convert selected…**, and its image context menu offers **Convert this file…**.

Selection-only conversion uses the current filename template, layout, gain and verification settings. It creates a new output subfolder and report containing only that operation. The selection overrides whole-batch include checkmarks and is independent of unrelated filename collisions elsewhere in the batch. Collisions within the chosen selection still block copying. Untiled files still require **Also copy maps without tile numbers**. Changed scan settings require a new scan first.

**Conversion means copying with different names and folder layout. It does not transcode JPG/EXR/PSD image formats.**

## UDIM grid and completeness

After scanning, click **UDIM grid…** to open the inspector. Choose a texture set and map type. The grid places U=0 on the left and V=0 at the bottom. **Fit** shows all ten columns when space allows; Small, Medium and Large sizes provide a closer view. Scroll vertically for more UV rows, or use Shift + mouse wheel to scroll horizontally. Click a tile for its source filename, preview information and status.

- **Green**: a tile is present.
- **Red**: the tile is missing from this map type but exists in the comparison baseline.
- **Orange**: multiple files represent the same tile and map type.
- **Blue**: a tile exists in this map type but is absent from the selected reference type.
- **Gray**: unused space. A gap absent from every recognized channel is not automatically a missing tile.

By default, **Union of map types** compares each channel against the union of observed tiles in its own texture set. For example, if Color has 1001 and 1003 while Normal has only 1001, Normal is missing 1003; 1002 remains unused. You can instead choose an existing reference map type. Unknown **Other** files are displayed but do not define the comparison baseline. Excluded, invalid and untiled files are not included in coverage.

Use **Expect type** to check a completely absent channel, such as Roughness in a set containing only Color. Added expectations remain in the current inspector session. **Export coverage…** saves a CSV for all sets. If a selected reference type is absent from another set, that set is exported using the union baseline, clearly labeled in the report.

Automatic grouping uses the base name after removing a recognized map type and the asset's folder context. Folders named Color, Normal, Displacement and other exact type aliases do not split a set. Separate asset folders remain separate. To correct grouping, select files in the main list and click **Assign set…**. Use the same set name for related channels. **Entire series** extends the assignment to all tiles in each selected series. An empty assignment restores automatic grouping. Manual set names are preserved in conversion reports and restored when loading converted output.

EXR thumbnails use OpenEXR or OpenImageIO streaming; other readers are shared with the built-in browser. Previews load in the background only for visible tiles and are cached in memory. Unsupported or damaged files show **No preview** without preventing copying. Deep EXR and large encodings without a bounded-memory reader are not previewed; supported large maps are sampled through native blocks. Multipart EXR previews show the first part. Alpha is ignored in grid thumbnails for visibility; the full browser can show transparency.

**Display** affects previews only: Auto gives floating-point scalar maps per-tile contrast and uses a linear-to-sRGB display adjustment for EXR Color/Emissive. **Raw 0–1** clips values to that range without these adjustments. **Auto contrast** stretches the sampled 1st–99th percentile range for visibility. These previews are an inspection aid, not a calibrated color-management or displacement-strength preview. The source and copied pixels remain untouched.

## Large textures: 8K / 16K

EXR, ordinary RGB/grayscale TIFF, non-interlaced PNG, baseline JPEG and Radiance HDR switch to **native block streaming** when their estimated decoded data exceeds 96 MiB. Files are read with OpenImageIO scanline/tile input; uncompressed contiguous TIFF can use bounded file-offset reads. No full image array or memory-mapped image is created. Native half/float/uint channels retain their stored precision, including EXR uint32 and RGB values under zero Alpha.

A/B reads only visible native blocks at **100% and above**. Below 100%, cached, source-anchored power-of-two overviews provide reduced detail; tiny changes can disappear in this display. **Pixel readouts, min/max, histogram, differences, threshold counts, change regions, Go to max, validator and set reports always analyze original pixels.** Their background jobs are cancellable. A provisional sampled contrast range is replaced by the exact range when analysis completes. The ordinary single-image browser retains its 2048-pixel display preview, while pixel readouts and ROI statistics use native blocks.

Wipe composites already rendered A/B layers: dragging the split does not decode files, render native viewports or restart analysis. Cursor probing in streamed A/B and the ordinary browser for maps of 64 megapixels and above is asynchronous, with stale requests discarded. Pin/copy use the completed native sample. Interactive native tiles, analysis tiles and overviews have separate shared LRU budgets of 96, 16 and 64 MiB, plus at most 32 MiB of recent scanline bands per active I/O worker. Codec work, image previews, viewport frames and UI need additional memory; these budgets are not a total process-memory guarantee. Decoder/math thread pools are bounded. The first overview and exact whole-file analysis still require disk reads/decompression; they are not instantaneous.

Streaming rejects deep/volume or subsampled EXR, 64-bit integer TIFF, large palette/CMYK TIFF, and compressed native strips/tiles/chunks above the 64 MiB work limit. Large interlaced PNG and progressive JPEG require whole-image/coefficient storage and show an explicit resave message; use non-interlaced PNG or baseline JPEG. PSD/PSB uses its dedicated composite reader described below. Other unsupported container variants keep existing decode guards. Multipart/page-aware validation and set analysis visit every supported part/page; the viewer still displays its selected first part/channel group. TIFF sub-IFDs are explicitly reported as incomplete.


### PSD / PSB saved composites at 8K / 16K

The reader skips color/resource/layer sections by their lengths and never opens the full document through a layer parser. It supports **RGB and grayscale at 8/16/32 bits**, native extra channels, PSD and PSB section/row-count widths, and all four composite compression modes. RAW uses bounded file-offset reads; RLE indexes compressed row offsets (maximum 32 MiB index) and decodes only requested rows. Alpha is identified from merged-transparency metadata/negative layer counts and alpha identifiers; saved masks are not automatically treated as transparency.

**ZIP and ZIP with prediction require a first preparation pass.** They inflate one row at a time into an owned persistent planar file in the selected cache folder. Prediction is undone at native depth per row. Native viewport reads, A/B differences and statistics then use that file without mapping or loading an entire plane. The cache is shared across thumbnail, viewer and comparison requests for the same original file revision. Complete caches are reused across process exits and application restarts. Wipe never decompresses PSD data or rebuilds this cache.

Scratch space equals the uncompressed stored composite: e.g. a 16K RGB float32 document needs 3 GiB and a 16K RGBA float32 pair needs **8 GiB**. The app checks free space on the selected drive with an additional 256 MiB margin. The configurable total limit covers persisted composites in the selected folder, active leases and full preparation reservations. Recognized files locked by another instance also count toward the budget. Unused entries are evicted when needed; active viewers keep their leases. Cancelling preparation removes partial output; normal process exit preserves complete cache files and releases their locks. Preparing and complete entries have separate states; only a fully flushed composite with an atomically committed manifest is reusable. Interrupted preparations are discarded on the next cache scan. Originals remain unchanged. Cache preparation is not instantaneous; disk speed, compression and depth determine its time.

**Cache…** in the main window and viewers opens **PSD/PSB Cache Settings**. Choose a drive or browse to an absolute folder, set **Disk limit (GiB)** (0.125–4096; default 16), then **Apply settings**. The default folder is `%TEMP%/UDIMStudioPSDCache`. The dialog shows used, in-use and reserved space plus available drive space. Settings persist across restarts in the machine's settings file, separately from naming profiles. Changing folders affects new preparations; open files keep using their original cache location. A limit below active files plus reservations is rejected until those viewers close or preparation is cancelled.

**Clear cache** removes unused cache and recognized, unlocked orphan cache folders in the selected location. Open viewers retain their active files until closed. Files locked by another running instance and unrelated files are preserved. Complete entries survive restarts and are evicted in least-recently-used order when room is needed. Changing the cache location preserves complete entries in the old folder; select that location again to reuse or clear them. Active files remain locked; another instance may require its own preparation or available space.

Before reusing a saved entry, the app checks original size, nanosecond modification time, file identity and change time (native NTFS change time on Windows), together with cache format, dimensions, byte length and cache-file revision. Changed/replaced originals and truncated/modified cached pixels cause automatic reconstruction. This includes normal edits that preserve the original modification timestamp. Validation reads bounded metadata, not an entire multi-gigabyte PSD/layer stack. It does not claim cryptographic bit-rot detection on storage that conceals both file identity and change metadata.

Open PSD/PSB image viewers and A/B comparisons check original revisions in a background thread about once per second. A changed original automatically reloads the composite and invalidates previews, native blocks, statistics and differences; A/B keeps shared zoom and pan. Existing analysis generations are cancelled before old cache leases are released. A write in progress may briefly report an unavailable preview and recover after the next detected revision. **Reused** in the preparation bar means no ZIP inflation was necessary. RAW/RLE continues reading directly from the original.

The preparation bar shows the filename and progress from decoded rows/bytes. **Cancel prep** cancels the selected preparation, including other windows waiting for the same shared file, and removes partial output. **Retry** in the viewer or A/B window starts preparation again; it also allows retrying after changing a previously insufficient disk limit. Main-window and settings bars can cancel all active preparations. RAW/RLE sources do not need this disk preparation pass.

### Prepare a whole folder or selection in the background

Open **Queue…** at the bottom of the main window or a viewer. **Add folder…** scans PSD/PSB files in the chosen directory, optionally using **Include subfolders**. **Add files…** chooses explicit files; **Add selected** uses the main list's current selection. Right-click files in the main list for **Prepare cache for selected files…** or **Prepare cache for source folder(s)…** (including all currently selected source roots). Naming/conversion errors in the output preview do not prevent read-only cache preparation.

The queue prepares **one composite at a time** with bounded row buffers. It displays per-file status, decoded composite size, current-file percentage and overall file progress. More folders/files can be added while it runs; duplicates in the active batch are ignored. Existing valid entries are **Reused**. RAW/RLE is **Direct read** and needs no preparation; non-PSD selections are **Skipped**. Broken/unsupported files show **Error** and other files continue. Scans do not follow directory symlinks/junctions.

Before writing a batch, the queue calculates its full uncompressed composite size. A batch larger than the cache limit **pauses before preparing files**. Completed composites stay pinned until that active batch finishes, so preparing later tiles cannot silently evict its earlier ones. Open viewers, another process, or insufficient physical free space can also pause preparation. Increase the limit / free space or close other viewers, then **Resume / Retry**. Cache settings and clearing are disabled during active preparation; wait or cancel first. Changing source/cache contents during a batch pauses it for rechecking.

**Hide** closes only the queue panel; work continues in the background and the main preparation bar shows overall progress. **Cancel queue** stops the current owned preparation, removes its partial data and cancels waiting files, retaining completed cache. When waiting for a preparation already owned by another viewer, cancelling the queue stops its own wait; it does not cancel that viewer's job. Cancelling a queue-owned preparation also stops windows waiting for that same shared task. **Resume / Retry** rechecks every listed file, including completed rows (they may have changed or been evicted). **Remove selected** and **Clear finished** edit the idle queue; they do not delete the disk cache.

Completed cache survives application restarts. The queue list itself is per session: add the folder again to reuse existing completed entries and prepare anything missing. Closing the main app cancels active queue work and waits for partial-file cleanup automatically. Later explicit cache clears or normal LRU eviction for other work can invalidate historical Ready rows; these rows record the last result. Once warmed, A/B still reads/resamples visible native data, but it does not repeat ZIP preparation for valid retained entries.

`test_cache_queue.py` covers selection/deduplication, recursive scanning, RAW/RLE and non-PSD skips, whole-batch limits, cancellation/partial cleanup, preserving completed work, appending files during work, shared-owner cancellation, source edits and retry. `--check-cache-queue <16K-A/B-parent-folder> <report.json>` checks the actual desktop selection/context action, folder warming, progress, cancellation, hidden-panel work, warm A/B with ZIP inflation forbidden, Wipe and closing during preparation on real 16K composites.

Missing composites, unsupported color modes/depths, malformed/truncated rows, invalid ZIP checksums and output exceeding the declared dimensions produce explicit errors. No black replacement image receives a successful native read. The validator checks **all saved-composite channels**, including NaN/Inf, but reports **Partial** because layers are not checked. **Analyze paired sets** still excludes full PSD/PSB document comparisons and explains how to open saved composites in A/B or export flattened maps.


## Naming formats

| Format | First tile | Tile at zero-based U=1, V=1 |
| --- | --- | --- |
| UDIM with dot | `body_color.1001.exr` | `body_color.1012.exr` |
| UDIM with underscore | `body_color_1001.exr` | `body_color_1012.exr` |
| Mudbox / UV 1-based | `body_color_u1_v1.exr` | `body_color_u2_v2.exr` |
| UV 0-based | `body_color_u0_v0.exr` | `body_color_u1_v1.exr` |

Tile numbers before channel names, such as `body_1001_normal.exr`, are also recognized. The material and channel names remain in the output basename.

**Auto assumes one-based UV coordinates unless the same series contains U=0 or V=0. For an incomplete zero-based series containing only positive coordinates, explicitly select UV 0-based.** The filename alone cannot distinguish those cases. The preview shows the assumption. Results created by UDIM Forge include a report that preserves coordinates when converting incomplete series or custom templates again.

Zero-based coordinates use `UDIM = 1001 + U + 10 × V`. Supported range: U=0–9, nonnegative V, tile numbers 1001–9999. Filenames with several possible tile numbers, such as `asset_2026_color.1001.exr`, must be made unambiguous outside the application or excluded.

## Map detection and folder layouts

Map types include Displacement/Height, Color/Albedo/Diffuse, Normal, Roughness, Metallic, AO, Specular, Glossiness, Opacity, Emissive, Bump, ORM and Cavity. Unknown or ambiguous types go to **Other**.

Detection uses filenames and source folder names; it does not analyze image pixels. Use **Map type rules** to add your abbreviations, or assign map types directly in the preview. Rules are saved in `%APPDATA%\UDIM Studio\settings.json`.

Available layouts:

- **Folders by map type**: `Color/body_color.1001.exr`, `Normal/body_normal.1001.exr`.
- **All files in one folder**.
- **Keep source subfolders**.
- **Texture set / map type**: `body/Color/body_color.1001.exr`.

Recursive scanning skips the selected output folder, directory links and generated result folders containing the application's report. Maps without a detected tile number are skipped by default. Enable **Also copy maps without tile numbers** to copy and organize them under their original filenames.

Supported copy extensions include EXR, TIF/TIFF, PNG, JPG/JPEG, TGA, BMP, DDS, HDR/RGBE, TX, PSD/PSB, WebP, IFF, PIC, GIF, ICO, AVIF, HEIC/HEIF, JXL, JP2/J2K, QOI and common PNM/SGI/PCX formats. Copying does not decode or re-encode images; preview support is separate.

## Custom templates

Available fields: `{name}`, `{udim}`, `{u1}`, `{v1}`, `{u0}`, `{v0}`, `{ext}`, `{map}`, `{set}`, `{gain}`.

Examples: `{name}.{udim}{ext}`, `{name}_u{u1}_v{v1}{ext}`, `{name}_{map}_{udim}{ext}`.

The template must retain `{name}`, a tile number or both UV coordinates, and one `{ext}` field at the end. Choose folders through **Folder layout**. Other applications may not recognize arbitrary custom templates. Keep the report with the texture set to preserve conversion metadata.

## Mudbox gain

The suffix `_g0.048557` stores displacement gain separately from UV coordinates. By default, UDIM Forge removes it from the copied filename and preserves the exact value in **udim_manifest.json** and **rename_mapping.csv**. This gives all tiles a consistent pattern, such as `displacement.<UDIM>.exr`.

You can also place gain before or after the tile number. To restore Mudbox naming, choose **UV · Mudbox (1-based)** and **After tile number (Mudbox)**. Loading an earlier output folder or its map-type subfolder restores gain from the report. Keep the report with the textures. Different gain values in filenames may cause other applications to treat the tiles as separate sets.

**Only filenames and the layout of copies change. The application does not change image pixels, formats, color spaces, displacement strength, gain or DirectX/OpenGL normal orientation.** SHA-256 verification is enabled by default to check copy contents.

## Mari and Marmoset

The default `{name}.{udim}{ext}` produces names such as `body_displacement.1001.exr`. In Marmoset Toolbag 5, select **Texture → UDIM** in the material and load one tile.

Naming references:

- [Foundry: Using UDIM Values](https://learn.foundry.com/mari/Content/user_guide/working_with_patches/using_udim_values.html)
- [Autodesk Maya: UV Tiling Mode](https://help.autodesk.com/cloudhelp/2025/ENU/Maya-LightingShading/files/GUID-309A77DA-F5ED-4474-8413-317D7AB241E6.htm)
- [Autodesk Mudbox: Extract a normal or displacement map](https://help.autodesk.com/view/MBXPRO/ENU/?caas=caas%2Fdocumentation%2FMBXPRO%2F2017%2FENU%2Ffiles-to-wrap%2FGUID-A602E288-A771-4225-B67B-6034CCFDF916-htm.html)
- [Marmoset: UDIM](https://docs.marmoset.co/docs/udim-mode-texture-module/)


## Batch Texture Tools

Open **Texture tools… → Batch grading / map utilities…** below the main file list, or right-click selected rows and choose **Edit selected textures…**. Use the main scan/selection or scan a separate folder recursively. Filter by map type (default: Color) and choose **All listed files** or **Selected files only**. Selection applies only to the current filtered table. Excluded converter checkmarks are independent of this explicit editing scope.

**Color / Levels** provides exposure in stops, additive brightness, contrast and pivot, signed gamma, saturation, vibrance, warm/cool balance, green tint, input black/white levels, grayscale, invert and optional clamping. **Map utilities** provides value multiply/offset for height maps, normal Green inversion for OpenGL/DirectX, tangent-space RGB normal normalization, R/G/B/Alpha/luminance extraction and explicit sRGB/linear transfer functions. Alpha is preserved unless explicitly extracted as a scalar output. Degenerate normals are replaced with the neutral forward vector. Normal operations expect RGB normals encoded in 0–1.

Thirteen built-in presets include warm/cool albedo, soft/punchy/muted color, ±1 stop, grayscale masks, roughness/gloss inversion, normal conversion, normal normalization and signed-height remapping. Applying a built-in preset resets the other operations and selects its intended map type. **Export / recipes** saves/loads custom JSON recipes containing all operations, output format, layout and map-type scope. Recipe changes do not modify source files. Click **Update preview** after editing controls. The paired preview uses a bounded overview, displays stored values clipped to 0–1, and composites Alpha over a checker; it is not an ICC/OCIO color-managed monitor preview.

Each **Export edited copies** run creates a new `Edited_<timestamp>_<id>` directory under the chosen output parent. Filenames retain their original stem and UDIM/UV formatting; the output extension changes to the chosen format. Keep subfolders or group copies by map type. Name collisions are rejected before processing. Supported outputs: float32 EXR, float32 TIFF, unsigned 16-bit TIFF, unsigned 8-bit PNG. Integer inputs are normalized by their type range; float inputs retain native signed/HDR values. Integer output clamps to 0–1, rounds to the selected depth and reports the number of clipped samples. Use float32 output for signed displacement and HDR. No automatic color-space, ICC or OCIO conversion is applied; the explicit transfer option runs before grading.

Processing reads/writes narrow bands rather than loading full images. PNG/JPEG/TIFF/EXR/HDR and PSD/PSB saved composites use the existing bounded native reader, subject to its format/compression limits. Single RGB(A), scalar and luminance/alpha images are supported. Multipart EXR, multi-page/sub-IFD TIFF, layered channel sets, MINISWHITE TIFF, animated PNG, signed integer samples and nonfinite source samples are rejected explicitly. PSD layers and ICC evaluation remain outside composite editing. Every file uses a temporary output and is committed only after successful completion and unchanged source revisions. Cancel removes the current partial image and retains completed copies. Per-file failures are recorded and other files continue.

`udim_manifest.json` records the recipe, output format, map/UDIM identity, completed/error files and clipping counts. It is updated after each completed file and on cancellation. **Compare result A/B…** opens the first successful output alongside its source in the existing shared-zoom/Wipe/difference viewer. **Open output** opens the generated version folder.



## ORM / RMA channel packing

Open **Texture tools… → Pack ORM / RMA UDIM sets…** or use the main file-list context menu. **Use main scan** supports the main application's scanned roots and assigned map types; **Scan folder** recursively scans a dedicated root with the selected UDIM/UV naming interpretation and current map-type recognition rules.

Maps are paired by texture-set identity and tile number, across AO/Roughness/Metallic type folders as well as mixed directories. Separate asset folders remain separate sets. Excluded files, parse errors and files without a recognized tile do not become packed rows. Only AO/Roughness/Metallic types contribute to the tile union. The table lists each discovered tile, missing required maps and duplicate candidates.

**ORM** assigns R=AO, G=Roughness, B=Metallic. **RMA** assigns R=Roughness, G=Metallic, B=AO. Each destination R/G/B independently accepts any of these map types or constant 0/1. Its adjacent source-sample selector chooses R, G, B, Alpha or Luminance from the input. For scalar maps use R or Luminance; for RGB luminance uses Rec.709 coefficients. No gamma or display/color-space conversion is applied. Unsigned integer samples are normalized by their storage range, while float samples retain native values.

Select a tile to see its source paths, planned filename and packed RGB preview. **All listed tiles** exports the complete table; **Selected tiles only** limits export to the current Ctrl/Shift selection. By default a missing required input prevents that tile from being written and receives an error record. Explicitly enable **Fill missing maps** to use AO=1, Roughness=1, Metallic=0. Duplicate candidates are always errors. Resolution or pixel-origin differences are errors; packing never silently resamples or aligns unrelated pixels. Invalid/nonfinite selected source samples and unavailable source sample channels are errors. Successful tiles continue when another tile fails.

Each export creates a fresh `Packed_<timestamp>_<id>` directory under **Output parent**. Output uses the Roughness filename as an anchor, falling back to AO/Metallic. Only the channel token is replaced with the editable **Packed map name**; original asset text, UDIM/UV spelling and gain tokens remain. When no filename channel exists, the packed-map label is inserted before the tile token. Type-folder components are removed while asset folders are retained. Colliding output names are rejected before creating the export directory. The preset sets the default packed-map label; edit it for a custom channel arrangement.

Supported outputs are 8-bit PNG, 16-bit TIFF, float32 TIFF and float32 EXR. Integer output clamps to 0–1 with reported clipping counts. EXR preserves source pixel origin. Preview and export share source-sample selection and missing-map policy. Export reads/writes bounded bands, supports cancellation, removes the current partial image and retains completed tiles. Original files are never overwritten. The output `udim_manifest.json` records the actual RGB mapping, selected source sample channels, input paths, missing-map policy, statuses, dimensions and clipping counts. Exported copies can be reimported with their UDIM and map-type metadata.



## Resize UDIM sets

Open **Texture tools… → Resize UDIM sets…** or use the main file-list context menu. Use the main scan, scan a separate folder recursively with the correct source naming mode, or filter the file list by map type. **All listed files** processes the current map-type scope; **Selected files only** processes the Ctrl/Shift selection. This operation resizes pixels independently for each file; it does not require all map types to have matching original dimensions.

**Longest edge** includes 16K/8K/4K/2K/1K/512 presets and accepts custom integer values up to 32768. Aspect ratio is retained, with each dimension rounded to the nearest pixel and a minimum of one pixel. **Scale (%)** includes 50%, 25%, 12.5% and 200%, plus custom percentages. **Only reduce** is enabled initially: maps smaller than the requested target are retained at their original size, while still receiving the selected export format and enabled normal processing. Disable it to permit enlargement.

Choose **Nearest**, **Box / area**, **Bilinear**, **Bicubic** or **Lanczos 3**. Box integrates exact source pixel coverage; bilinear, bicubic and Lanczos widen their support when reducing to prevent ordinary downsampling aliasing. Nearest deliberately performs point sampling. Source coordinates use pixel-center alignment. Border samples are clamped within each image; neighboring UDIM files are not used as filter padding. Bicubic/Lanczos can ring or overshoot on sharp data; float outputs retain these values, while integer outputs clamp and report them.

**Normalize Normal maps after resizing** applies only to files classified as Normal, and expects tangent-space RGB normals encoded in 0–1. The filtered RGB is decoded to signed vectors, normalized and encoded again. A degenerate vector uses neutral forward (0.5, 0.5, 1). Alpha is filtered independently and is never included in the vector normalization. Integer output introduces the usual small vector-length quantization error; use float32 output for maximum fidelity. Displacement, AO, roughness, metallic and packed ORM/RMA maps use numerical filtering with no automatic gamma conversion or normal-vector processing.

**Color / Emissive are sRGB** is off initially. Enable it only for sRGB-encoded Color/Emissive files to decode to linear light before filtering and encode back afterward. Linear/HDR colors should leave this option off. RGB(A) Color/Emissive uses premultiplied-alpha filtering followed by unassociation in either color mode, avoiding transparent-color bleed. Its final coverage Alpha is clamped to 0–1; Alpha in data/normal maps retains its numerical range. ICC and OCIO evaluation are not applied.

Select a file and click **Update preview** after changing settings. The target preview is sampled from the real target-pixel grid using the selected resampling kernel; it is not a resized thumbnail used as an export source. The source/target dimensions are displayed. Large-image previews may require substantial reading for wide downsampling kernels; cancellation also applies to preview work.

**Export resized copies** creates a fresh `Resized_<timestamp>_<id>` directory under **Output parent**, preserves source subfolders and every filename stem including UDIM/UV/gain spelling, and changes only the output extension. Available outputs are float32 EXR (initial default), float32 TIFF, unsigned 16-bit TIFF and unsigned 8-bit PNG. Integer values are normalized by the source storage range; float data keeps signed/HDR values. Integer exports clamp to 0–1 and report clipping. EXR pixel origins are scaled and rounded to the new resolution. The per-file manifest records source/target dimensions, filter, scope settings, normal processing, clipping and errors. Cancel removes the current partial output and retains completed copies. Name collisions are rejected before export; originals are never replaced. Failures on one file do not discard other successful files.

The resampler uses one native source stripe (up to 64 MiB), a bounded horizontally filtered row cache (up to 16 MiB), compact axis weights and a narrow output band. Decoder caches and temporary arrays add to those figures; they are not total process memory limits. Source dimensions are limited to 65536, output dimensions to 32768. A tiled image whose full-width tile stripe exceeds 64 MiB needs smaller native tile heights or a scanline export. Existing single-image native reader limits also apply, including explicit rejection of unsupported multi-part/layer containers and nonfinite samples. PSD/PSB uses its saved composite and existing disk cache.



## OCIO display management

Open **OCIO…** in the main window, Image Viewer or A/B Comparison. OpenColorIO 2.6.0 is bundled, including offline built-in ACES CG/studio configurations. Choose a built-in configuration or browse to an external `config.ocio`, then select **Input space**, **Display** and **View**. Enable color previews and click **Apply**. Settings persist in the normal per-user settings file and update open viewers without re-decoding their source images. External configs and their LUT/search-path resources remain at their original locations; retain those files when moving projects. The implementation uses OCIO's [DisplayViewTransform / CPUProcessor display pipeline](https://opencolorio.readthedocs.io/en/latest/guides/developing/developing.html).

**Auto input** treats integer preview storage as sRGB texture data and floating preview storage as the config's `scene_linear` role. This is an explicit heuristic, not metadata-based color-space detection. Select an actual input space such as your config's linear Rec.709, ACEScg or sRGB texture space whenever the source encoding differs. Configs without a recognizable sRGB texture space or scene-linear role require an explicit input selection. The OCIO display canvas is 8-bit SDR; choose an appropriate SDR Display/View for ordinary monitors. HDR swap-chain output and automatic monitor ICC profiling are not implemented.

**OCIO… → Input assignments** adds input-space rules for a set folder (including subfolders), exact catalog set name, map type and/or file extension. Blank scope fields match all; combined fields must all match. Use **New rule → Save rule**, then **Apply**. For example, assign Albedo to the config's sRGB texture space and an EXR extension rule to ACEScg. Type rules take priority over extension-only rules; narrow a rule by folder/set when both encodings occur in one project. Priority is deepest matching folder, then set name, map type, extension; the last row wins exact ties. Color, Albedo, BaseColor and Diffuse are equivalent type selectors. Set names use the detected or manually assigned catalog series, without channel/UDIM tokens. Removing a rule restores the next match or the default Input space. Assignments never enable OCIO on protected data maps.

Set comparisons offer **Sessions… → Session OCIO settings…**, also available through their A/B viewer's **OCIO…** button. A new comparison initially inherits global settings; applying session settings creates a local copy. Session files, automatic checkpoints and recovery history save the effective OCIO configuration and assignments. Restoring a session restores its local display policy without changing global settings, profiles or other viewers. Legacy sessions without OCIO inherit the current global settings. Changing OCIO rerenders display frames while retaining native analysis results, threshold metrics, shared zoom and decoded source buffers. Absolute folder scopes are retained as entered; update them when moving version directories.

Managed previews apply only to **Color / Albedo / BaseColor / Diffuse / Emissive** RGB. Optionally treat **unclassified EXR** as color; this option is off initially. Known Displacement, Normal, masks and packed ORM/RMA types always bypass OCIO. Classify files correctly or use the main map-type rules / Apply type before opening them. A/B also follows map types supplied by the set comparison catalog. For managed color, exposure operates before the OCIO display transform; legacy automatic sRGB transfer, gamma and contrast are bypassed to avoid applying a second display transform.

**Raw 0–1**, R/G/B/Alpha channel inspection and A/B absolute/signed differences bypass OCIO. A/B's **OCIO** checkbox toggles the managed color display; **Raw display** switches it off locally. OCIO operates on a separate RGB display copy. Alpha composition, native pixel probes, histograms/ROI statistics, change regions, thresholds, set analysis, saved analysis results and CSV metrics continue to use original native data. Texture conversion, grading/export math, packing and resizing are unaffected. The grading/packing/resize tool thumbnails retain their labelled raw-data preview behavior; use the read-only Image Viewer / A/B viewer for managed color inspection.

Built processors are cached with configuration identity and a reload generation. **Reload config / LUTs**, then **Apply**, refreshes config/LUT resources and managed display frames. Native decoded source buffers and analysis caches are reused. Wipe uses pre-rendered managed A/B layers and does not run OCIO on each slider movement. A missing/broken config produces an explicit preview error, rather than silently displaying an unintended transform; Raw and known data-map inspection remain available. Invalid Apply selections retain the preceding settings.

