# What’s new

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
