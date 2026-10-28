# What’s new

## 0.21.0 — Linked PBR material A/B comparison

- Compare two UDIM material versions side by side or with a cached Wipe divider.
- Shared camera, zoom, exposure, lighting, HDRI, model and displacement controls; independent input encoding and packed-map layouts.
- Independent persistent GPU caches, combined A+B GPU budget, adaptive movement and consistent CPU fallback for both sides.
- Union tile navigation, explicit missing-side placeholders, whole-set UV0 model viewing and PNG display snapshots.
- Project/autosave recovery and dependency collection for both complete version folders, including unopened tiles.
- Verified with 659 unit tests, linked-view acceptance, and a 2.05-million-triangle model with 16 displacement EXRs and HDRI. Source assets remain unchanged.

## 0.20.0 — Contextual Surface Tools and ordered recipes

- Operation-specific controls with optional mask sections; hidden fields cannot affect other operations.
- Ordered, scoped, enabled/disabled recipe steps with move, duplicate and remove controls.
- Full-chain and selected-prefix previews from originals, with complete UDIM dependencies.
- Float32 intermediates and one final format conversion; stage-aware resumable queues and final-output verification.
- Recipe presets, project/autosave persistence, stale-plan restoration guards and dependency collection across steps.
- Retry transient Windows progress-file sharing conflicts without weakening atomic writes; dispose nested Tcl variables on the UI thread.

## 0.19.0 — Surface tools and UV inspection

- Batch multi-channel unpack/remap with explicit RGB/Alpha routing and output names.
- Normal X/Y/Z, Z reconstruction, normalization and RNM detail blending; neighbor-aware Height → Normal.
- UDIM-aware resize, blur and sharpen with bounded streaming row caches.
- Mask/range-based grading and .cube LUT strength, explicit preview versus baked copy export.
- UV0 overlays/checker, tile navigation and CPU/GPU model tile highlighting.
- Contact sheets with source metadata; typed presets, projects, real worker previews and resumable queue exports with final checks.

## 0.18.0 — Portable project collection

- Review and collect the current project, textures, model, HDRI, comparison versions and OCIO LUTs into one new folder.
- Relative project paths survive relocation; comparison hashes and queue input guards are verified before reuse.
- Background copy, progress, cancellation, SHA-256 manifest and safe failure cleanup.
- Project menu and Ctrl+K entry; original workspace stays unchanged.
- Explicit OBJ/MTL and FBX external-media boundaries; external OCIO trust remains required.

## 0.17.0

- Added an export center with explicit Included / Selected / Whole loaded set scopes and a review-before-start flow.
- Replaced per-map JSON editing with a validated rule table and typed editor; retained preset/project compatibility.
- Added recipe map/UDIM navigation with shared view state, two-result caching, stale detection and reviewed-plan refresh.
- Added final output verification for main and batch exports and dependent queue/CLI checks, including encoded pixel scans and TX mip checks.
- Added per-file verification reports and non-overwriting batch rechecks; failures remain visible in queue status.


## 0.16.0

- Added target delivery presets for PBR 4K/2K and float EXR archives, plus reusable custom presets.
- Added exact representative recipe before/after previews using the real capped export workers and the shared A/B/Wipe/difference viewer.
- Added explicit color/data export summaries, guarded material data-map processing and OCIO assignment change detection.
- Added shared queue progress/ETA, verification phases, colored states, clearer cancellation and automatic stale-plan invalidation.


## 0.15.2

- Added starred favorite commands and a Favorites only filter to Ctrl+K.
- Persist the eight most recently run commands, favorites and custom shortcuts across application restarts.
- Added a shortcut editor with conflict detection, protected editing keys and application-only bindings that respect modal dialogs and unavailable actions.
- Save command preferences atomically with bounded validation, multi-instance edit merging and explicit reset with a backup.


## 0.15.1

- Added a local Ctrl+K command palette for tools, workspace actions, projects, exports and support.
- Added keyword/alias search, keyboard navigation, session-only recent commands and live availability explanations.
- Preserve Entry/Text contents when invoking Ctrl+K; active modal dialogs retain control. Commands recheck availability and use existing export/preflight guards.


## 0.15.0

- Added export preflight for main conversion, batch tools, queued workflows and CLI: disk space, permission probes, names, missing inputs/tiles and floating-point conversion risk.
- Added a review dialog with blocking errors, explicit warning acknowledgement, input location and JSON reporting. Estimates include resize, TX and PSD cache requirements without decoding all pixels.
- Corrected MSI shortcut naming, project-association removal and shortcut cleanup. Added a real isolated install/upgrade/launch/uninstall test with user-file preservation checks.
- Wait for capped worker processes to exit before deleting their temporary files, fixing an intermittent cancellation cleanup race.


## 0.14.1

- Prepared Gumroad purchase verification and a License / Gumroad dialog; the product is not created and activation remains disabled pending its real Product ID.
- Added explicit, bounded HTTPS verification, purchase-state checks and encrypted local key storage. No seller API token, automatic network check or device-use counter increment.
- Added customer library access and a Gumroad setup guide. Paid files are not published or uploaded by this release. License enforcement remains disabled.


## 0.14.0

- Added full `.udimforge` workspace projects, atomic save, bounded crash recovery, project relink, paused durable queues and revision-checked comparison results.
- Added resumable main conversion with source/output hash verification and idempotent final publishing.
- Extended Undo/Redo to conversion preferences, profiles and grading/packing/resize settings.
- Added isolated OpenGL GPU PBR preview with managed VRAM budget, vertex displacement, whole-set UDIMs, cutout Opacity and CPU fallback.
- Added headless plan/run/resume workflows and opt-in local synthetic benchmarks.
- Added a per-user Windows MSI installer and offline signed-license/private-update mechanisms. Store integration and license enforcement remain unconfigured in this internal build; Authenticode remains deferred.
- Added project/automation guides and beta acceptance checklist. Real hardware checks include RTX 3090, Smaug and the user's EXR/HDRI dataset; independent AMD/Intel beta testing remains outstanding.


## 0.13.0 — 2026-10-25

- Main-preview Undo / Redo covers map-type assignments and their output names, channel rename/reset, manual texture-set grouping and export inclusion. Entire-series edits form one atomic history step; no-op actions do not clutter history.
- Visible Undo / Redo buttons sit beside Selection. Ctrl+Z undoes, Ctrl+Y or Ctrl+Shift+Z redoes in the main file workspace; text-entry controls and other tool windows retain their own keyboard behavior.
- Selection → Preview edit history shows applied/undone actions and affected-file counts. History is bounded to 100 steps and 16 MiB of metadata, without image-pixel snapshots. A batch exceeding the budget applies normally and reports that history was cleared.
- Undo/Redo respects active filters, selects affected visible rows and reports hidden files. It is disabled while scanning/converting or when the source scan is stale. New scans, loaded conversion profiles and saved map-detection rules clear history; conflicting external metadata edits are detected before any restoration.
- Preview edits can be undone after export without changing completed outputs. History is local to the current application run and does not undo profile settings, image operations or filesystem writes.
- The duplicate lower Compare sets button is replaced by Undo / Redo; set comparison remains in the main Compare menu. Rename Channel now consistently calls the export action Start conversion, and stale rename/reset dialogs cannot mutate textures from a replaced scan.
- Acceptance checks cover actual keyboard events, filtered rows, batch scope, rename/reset, grouping, busy states, scan/profile boundaries, completed exports and unchanged source files.

## 0.12.0 — 2026-10-25

- One application-wide Windows memory budget includes the GUI, portable launcher, production workers, EXR decoders, model importers and native subprocesses. Existing individual worker caps remain in effect.
- A live Memory button in the main footer shows total committed usage and the configured limit. Open it for peak usage, admitted/waiting tasks and a persistent limit in MiB. The default is half physical RAM, between 2 and 16 GiB; manual limits range from 2 to 256 GiB.
- Production concurrency adapts to committed usage and conservative task reservations. New work waits for 512 MiB of headroom plus its estimate; admitted files continue. Waiting jobs remain cancellable/pausable and resume automatically as memory becomes available.
- Grading, resizing, channel packing, model loading, PBR rendering, PSD ZIP preparation and EXR decoder admission cooperate with the shared budget. Nested operations share their parent task reservation.
- Under memory pressure, disposable image/geometry caches and idle EXR processes are retired in a background thread. Persistent PSD disk caches and completed analysis results are retained.
- A limit cannot be lowered below current committed usage plus 512 MiB. Committed memory differs from physically resident RAM; large previews can require a higher limit. If an allocation exceeds the hard OS budget it can fail; active work is not forcibly interrupted to meet a smaller soft target.
- Regression checks include aggregate allocations in real sibling/nested processes, concurrency recovery, cancellation and queue pause/resume, settings persistence and frozen executable UI checks.

## 0.11.0 — 2026-10-25

- EXR metadata, previews and native block reads move into a pool of at most two disposable processes, each capped at 768 MiB on Windows with a 60-second request deadline. Crashes, cancellation and timeouts discard the process; subsequent files can open in a fresh worker.
- Explicit EXR limits cover parts, channels, dimensions, tiles, compression chunks and IPC buffers. Native precision and source-revision checks are retained; files are not locked between decoder calls.
- PSD/PSB parsing adds metadata deadlines, section-boundary checks, block/composite ceilings, strict bounded PackBits decoding and cancellation inside ZIP loops. ZIP cache preparation has a 300-second deadline and removes partial output on failure.
- OBJ/FBX parsing starts only after the 2 GiB Windows memory budget is assigned. Imports have a 120-second end-to-end deadline, bounded diagnostics, validated transfer arrays and index/coordinate checks. OBJ material-library directives are omitted from the private import copy.
- TX is available in the main output format selection, per-map rules and profiles, plus Convert selected to TX in the table context menu. Start conversion combines UDIM names and TX mipmaps, using an isolated 2 GiB/300-second worker.
- TIFF-backed TX promotes half input to float32 without precision loss. Keep mode rejects mixed/higher precision channels that cannot be preserved safely; Alpha is retained. Advanced TX settings remain in Production Workflows.
- Regression coverage includes real process timeout/crash/cancellation/memory tests, corrupt inputs, forged array headers and streamed 16K EXR/PSB fixtures.

## 0.10.1 — 2026-10-25

- External session OCIO configs must use an absolute .ocio path and require explicit permission before loading their config/LUTs. Automatic recovery defers them; the requested settings are preserved for later approval.
- Bounded JSON reads for workflow recipes, conversion manifests, session files and production payloads; malformed manifest file lists are ignored.
- Spreadsheet-safe CSV exports neutralize formula-like text while retaining numeric statistics.
- Production workers validate options and TX filter/wrap/storage values before starting native tools; Pillow has an explicit 16K pixel ceiling with decompression warnings treated as errors.
- Cache locks reject symbolic links/reparse points on the opened handle.
- UPX disabled in both build specifications; hash-locked dependencies include build dependencies. Build environments and artifacts live outside the source tree.
- Optional Authenticode and detached checksum signature workflow is prepared. This local build remains unsigned because no publisher certificate is configured.

## 0.10.0 — 2026-10-25

- Adaptive PBR orbit uses a UV-preserving simplified mesh and smaller display buffers, then restores full geometry after release or a brief pause. Object-space displacement and HDRI backgrounds are cached.
- Subdivision now changes primitive tessellation; imported models use indexed midpoint refinement, with selectable levels constrained by a 12 million triangle render cap.
- HDRI mode falls back to Unlit when no environment is available; Unlit is also selectable directly.
- Batch Texture Tools presets retain the map filter, selection and preview. Main Selected type choices immediately update classification and output names for selected rows.
- Editing Output keeps Start conversion available without rescanning. Missing destination folders are created when conversion starts.

## 0.9.0 — 2026-10-25

- Static OBJ/FBX import with transformed geometry and UV0 in the PBR viewport.
- Whole-set multi-UDIM routing for Color, Normal, masks, packed maps and native displacement; custom HDRI lighting remains cached.
- Linear mesh subdivision, missing-tile diagnostics/magenta display, cancellable import, memory/time/geometry limits and bounded visibility caches.
- Large-model rasterization rejects invisible/subpixel triangles before expanding shading data.
- Release text and first-party code metadata are reviewed for product-only branding; required third-party legal notices remain intact.

## 0.8.0 — 2026-10-25

- Real tessellated displacement in PBR sphere/plane previews with automatic set/UDIM map assignment, signed scale, neutral level and an enable switch.
- Displaced surface normals, perspective-correct triangle visibility and changed silhouettes under Studio or custom HDRI lighting.
- Native float height interpretation without OCIO/gamma or per-tile normalization; bounded geometry cache and background cancellation.
- Explicit invalid-height, collapsed-sphere and preview-range errors.
- Commercial builds remain local; GitHub hosts documentation and support only.

## 0.7.0 — 2026-10-25

- Custom HDR/EXR equirectangular environment loading in the PBR viewport, with input-encoding selection and background preparation/progress.
- HDRI rotation and linear intensity controls; optional panorama background; independent Studio/HDRI lighting modes.
- Environment-driven diffuse irradiance and roughness-filtered GGX reflections with cached specular integration. HDR values above 1 are retained.
- Bounded, solid-angle-weighted HDRI downsampling and reusable lighting caches; rotation/intensity/material changes do not decode the panorama again.
- Clear/cancel, stale-source checks, explicit projection/channel/NaN/Inf errors and snapshot protection for environment files. The latest lighting choice is retained when a background load completes.
- Commercial distribution remains local only. No public application releases or download links are published.

## 0.6.0 — 2026-10-25

- PBR material preview: sphere/plane, texture-set and UDIM selection, automatic Albedo/Normal/Roughness/Metallic/AO/ORM/RMA assignment and explicit conflict/manual choices.
- Separate-mask precedence with packed fallback, ORM/RMA/custom channel layouts and explicit packed precedence.
- Cached 512px proxies, background GGX studio shading, orbit/zoom, lighting/exposure, UV repeat, normal convention/strength and display PNG snapshots.
- Explicit Albedo sRGB/linear/ACEScg/OCIO input handling; data maps bypass color transforms and original files remain unchanged.
- Commercial distribution: public GitHub application releases/assets and download links removed. Public update fetching and publishing are disabled; GitHub remains documentation/changelog/issues only.

## 0.5.0 — 2026-10-25

- Added Tools → UDIM boundary validator and a shortcut from the set validator.
- Native U/V border checks between observed neighboring tiles, grouped by set and map type, with correct image-Y/UV-V orientation and no horizontal row wrap.
- Per-channel or all non-alpha analysis, native-unit thresholds, maximum/mean/RMS differences, exact affected-pixel counts/percentages and a problems-only filter.
- A/B border strips, peak-preserving difference bins, threshold/invalid highlights, native hover values, peak marker and original-map viewer shortcuts.
- Explicit duplicate, incompatible, unsupported, nonfinite, stale and unfinished results; no guessed pairs, resampling or automatic repairs.
- Background bounded edge reads, cancellation preserving completed connections and full CSV/JSON reports with source-change checks.
- Added a 16K tiled float32 TIFF benchmark and regression checks for orientation, counts, Alpha, cancellation, changed files and report safety. Numerical boundaries remain inspection warnings: unrelated UV islands and tangent-space normals may differ intentionally.

## 0.4.1 — 2026-10-24

- Named custom packing presets in Pack ORM / RMA: select to apply, Load, Save as and Delete.
- Presets retain R/G/B/Alpha map routing, source-sample selectors, constants, Alpha enable state, output format with bit depth, packed map name and missing-map policy.
- Presets persist between launches and apply to new UDIM sets without changing source/output folders or tile selections.
- ORM/RMA built-ins remain available and protected; replacing a custom preset asks for confirmation. Invalid files are reported and preserved.
- Atomic preset writes retain the previous file if saving fails. Loading applies the settings together before refreshing the selected preview.

## 0.4.0 — 2026-10-24

- Reorganized the main window into compact Folders, Conversion and Preview steps, with a larger file table and fewer duplicate actions.
- Output, By map type and Advanced tabs keep common settings visible and expand to fit the selected controls. Metadata filters open from Filters, with an active-filter count.
- Grouped Compare, Tools and Selection actions; preserved naming, format, resolution, profile and queue behavior.
- Added a direct Pack ORM / RMA button in the main header and prominent color-coded R/G/B/Alpha routing cards.
- Each packed destination chooses AO, Roughness, Metallic or constant 0/1, plus the source sample R/G/B/Alpha/Luminance. A live routing summary shows exactly what will be written.
- Optional fourth Alpha output, RGB/single-channel previews, and RGBA PNG/TIFF/EXR export with independent unassociated RGB values and mapping recorded in the manifest. JPEG with Alpha is explicitly rejected.
- Changing packing settings refreshes the selected preview. Original files remain unchanged.

## 0.3.4 — 2026-10-24

- Main-window folder, conversion and preview sections now use distinct muted blue, warm charcoal and teal backgrounds with matching controls.
- Thin colored edge accents, outlines and clearer headings separate the dense panels; output/status and cache have their own subtle tones.
- Workflow/viewer tabs have stronger selected/hover contrast; dropdown arrows and keyboard focus remain visible in the main sections.
- Primary actions, warnings, file processing, profiles and view-filter behavior are unchanged.

## 0.3.3 — 2026-10-24

- Clickable main-table headings with ascending/descending indicators, numeric resolution/depth/UDIM sorting and natural filename/path ordering. Missing metadata stays last.
- Independent source/output resolution, format and depth filters, combined with type and Search. Resolution supports exact dimensions and longest-edge presets.
- Visible/total counts and Clear filters; late background headers and changed output rules update matches automatically.
- View filters preserve conversion inclusion and stable file identities. Selected-file operations still target the correct source after sorting/filtering.
- Export visible CSV follows the filtered rows and their current sort order.

## 0.3.2 — 2026-10-24

- Main-table source/output dimensions, actual source container and per-sample depth, and planned output container/depth.
- Predictions follow global/per-type formats, type assignments, aspect ratio, reduce-only limits and retained-encoding rules; no pixel decode is needed for inspection.
- One background header reader with a small metadata cache, pending/unreadable states and rescan refresh. Metadata updates retain selection and table position.
- Preview CSV and row details include metadata; Search matches the new fields. Multi-image inputs and unsupported retained formats are explicitly identified.

## 0.3.1 — 2026-10-24

- Main-window resolution limits by map type, an 8K Displacement / 4K Normal / 2K Color preset, custom pixel sizes and filter selection.
- One Start conversion combines UDIM naming, format/depth and reduce-only resizing, including selected-file conversion.
- Aspect ratio and float displacement are preserved; resized Normal maps are renormalized. Disk-backed float32 intermediates avoid early quantization.
- Profiles/settings retain resolution rules, enabled state and filter. All map types supports custom format/resolution rules; older profiles keep resizing disabled.
- Output preview/reports include resolution settings and actual output dimensions. Taller default window gives the file list more space.

## 0.3.0 — 2026-10-24

- Main-window formats by map type, inline Displacement/Normal/Color selectors, mixed-set preset and custom-type editor; rules and enabled state persist in profiles.
- Fixed WinError 17 when publishing queue outputs to another drive. Final staging now lives on the output drive; verified prepared outputs from earlier failures can be recovered without re-encoding.
- Cancel current work stops preparation and active queue/native maketx processes; pending work remains resumable.
- TX Auto uses a measured four-thread budget, with manual CPU/compression controls and optional per-job resizing.
- Right-click multi-selection edits image encoding, Alpha, compression, resolution/filter and TX settings in preview or paused unfinished jobs. Dependent paths are rebased; finished results are protected.
- Corrected misleading TX details and excluded owned output staging from recursive scans.


## 0.2.1 — 2026-10-24

- Main-window Output image format / bit depth, compression and Alpha controls. One Start conversion applies UDIM naming, folder layout and real EXR/PNG/TIFF/JPEG encoding together.
- Preview extensions and collision detection reflect the selected encoding, including selected-file conversion.
- Encoding settings persist in profiles/settings; legacy profiles preserve original-format copying.
- Neutral bounded-memory encoding protects source files. Reports distinguish source/output hashes and record clipping; failed/cancelled batches are discarded.


## 0.2.0 — 2026-10-24

- Visible Convert image format command and Start format conversion button; the main filename-copy action is now Start conversion. Fixed main-table horizontal scrolling.
- PNG16, EXR half/float, compression and explicit Alpha policies, with protected source files.
- Persistent multi-operation queue with dependency ordering, per-file recovery, SHA-256 verification, 1–4 workers and a hard combined worker memory cap.
- Saved whole-set recipes with per-map grading/resize/output rules, normal-Y conversion, normal resampling, ORM/RMA dependencies and reviewed output manifests.
- Shared displacement min/max, normalize/neutral/scale/offset operations and per-output transform/inverse sidecars.
- Advisory uniform/clipping/normal-length/Alpha quality checks with source inspection; corrupt/NaN/Inf inputs are reported.
- Self-contained visual HTML A/B reports and bundled tiled/mipmapped .tx export.
- Explicit opt-in baked OCIO Display/View for Color exports, separate from ordinary display-only preview; data maps bypass it.
- Persistent rotating error logs and reviewed, redacted diagnostic ZIP export.
- Relink moved comparison projects and OCIO folder scopes/config paths; retain only content-verified analysis. Legacy results without hashes require accessible originals or reanalysis.

See [production workflows](production.md) and [support/relink instructions](support.md).

## 0.1.0 — 2026-10-24

Initial portable build. Public binary distribution has since been withdrawn; the application is distributed commercially.

- Introduced the legacy update dialog, now retired for commercial distribution.
- The legacy public update mechanism has since been disabled.
- A versioned portable package and SHA-256 checksum are attached to the release. Git stores documentation/changelog and support templates; the application source and EXE are not committed.

## Original preview functionality

- New application name and Windows executable: UDIM Forge.
- Help & Support menu with embedded offline documentation, Quick start, user guide, naming/OCIO references, troubleshooting and contact links.
- GitHub issue forms for problems and feature requests; diagnostics copied locally on request.
- Existing UDIM Studio settings, profiles, caches, reports and comparison sessions remain compatible.

## Included workflows

- UDIM/UV filename conversion, channel renaming, folder organization, folder drops and profiles.
- Native pixel inspection, channels/Alpha, EXR exposure/gamma, histograms, min/max, ROI statistics and validation.
- A/B/C/D set comparison, cached Wipe, native differences, threshold highlights, change-region navigation and CSV reports.
- Persistent comparison sessions, incremental source revision checks, autosave and recovery history.
- Large-texture streaming and persistent PSD/PSB saved-composite cache with queue/progress/cancellation.
- Copy-only batch texture operations, ORM/RMA channel packing and UDIM resizing with normal-map normalization.
- Display-only OCIO with global, set/folder/type/extension input assignments and session-local policies.

See the [user guide](user-guide.md) and [update instructions](updates.md) for supported behavior, limitations and installation steps.
