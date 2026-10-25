# UDIM Forge

A Windows desktop toolkit for UDIM texture sets: organize names and folders, inspect native pixels, compare A/B/C/D versions, validate assets and export processed copies.

This Git repository contains documentation, changelog and issue templates. Application source and EXE files are not committed to Git. UDIM Forge is commercial software; application binaries are not distributed through this repository.

## Commercial application

UDIM Forge is commercial software. GitHub hosts documentation, changelog and issue tracking only. Application binaries and updates are distributed by Forge-Plugins through its authorized purchase channel; no public application downloads are provided here.

## Documentation

- [Production workflows](docs/production.md) — image formats, queues, recipes, displacement, HTML and .tx.
- [Quick start](docs/quick-start.md) — first conversion, inspection and comparison.
- [User guide](docs/user-guide.md) — complete workflows, controls and limitations.
- [PBR material preview](docs/pbr.md) — sphere/plane, automatic UDIM assignment and packed-mask inspection.
- [UDIM boundary validator](docs/boundaries.md) — native border statistics, previews and CSV/JSON reports.
- [OCIO and input assignments](docs/ocio.md) — color spaces per set/type and session-local policies.
- [Naming reference](docs/naming.md) — UDIM, UV coordinates, templates and channel naming.
- [Troubleshooting](docs/troubleshooting.md) — previews, large files, cache, colors and recovery.
- [External references](docs/references.md) — official format and pipeline documentation.
- [Contact / support](docs/support.md) — reporting problems and copying diagnostics.
- [Licensing and updates](docs/updates.md) — commercial distribution and local update instructions.
- [Changelog](CHANGELOG.md) — released changes and included workflows.

The application includes these guides offline in **Help & Support**. Its UI and documentation are in English.

![UDIM Forge offline Help and Support](docs/images/help-center.png)

## New in 0.10.0

- Adaptive orbit for heavy models, full-quality refinement after stopping, working subdivision and Unlit fallback for missing HDRI.
- Preserved Batch Texture Tools selection, immediate Selected type/name updates, and editable output destinations without rescanning.

## Added in 0.9.0

- Import static OBJ/FBX models and preview the entire selected UDIM set on UV0, with geometric displacement and custom HDRI.
- Subdivision, missing-tile display, cancellable import and bounded large-model processing. [Guide](docs/pbr.md#imported-obj--fbx-with-the-whole-udim-set).

## Added in 0.8.0

- Geometric displacement on the PBR sphere and plane: set/UDIM height assignment, signed Scale and Neutral controls.
- Displaced silhouettes and surface normals with cached Studio/HDRI shading. [Guide](docs/pbr.md#geometric-displacement).

## Added in 0.7.0

- Local HDR/EXR environment lighting in PBR preview: rotation, intensity and optional panorama background.
- Cached diffuse/GGX lighting, bounded HDR radiance preparation, progress/cancel and explicit image/input errors. [HDRI guide](docs/pbr.md#custom-hdri-lighting).
- Commercial application binaries remain outside GitHub; this repository provides documentation/support only.

## Added in 0.6.0

- PBR sphere/plane material preview with automatic map assignment per set/UDIM, manual choices and ORM/RMA/custom layouts.
- Cached texture proxies, lighting/exposure, orbit/zoom, normal-map controls and display snapshots. [Guide](docs/pbr.md).
- GitHub is documentation, changelog and issue tracking only. Public application distribution and the legacy updater have been removed.

## Added in 0.5.0

- Native U/V boundary checks for observed neighboring UDIMs, grouped by set and map type.
- Threshold statistics, pixel counts/percentages, A/B border strips, peak markers, native hover values and CSV/JSON reports.
- Bounded background reads, cancel with completed results retained, and explicit invalid/incompatible/stale states.
- Image-space warnings; no automatic repairs or assumptions about UV topology. [Guide](docs/boundaries.md).

## Added in 0.4.1

- Persistent custom packing presets with R/G/B/Alpha routing, source channels, Alpha enable state, format/depth, packed map name and missing-map policy.
- Select to apply, Load, Save as and Delete directly in the packing window; source/output folders remain independent.
- Protected ORM/RMA built-ins and atomic validated preset storage. [Guide](docs/user-guide.md#custom-packing-presets).

## Added in 0.4.0

- A larger preview table and compact main workspace with Output, By map type and Advanced settings tabs.
- Collapsible metadata filters and grouped Compare/Tools/Selection menus.
- A visible **Pack ORM / RMA…** button, explicit R/G/B/Alpha source routing, live summaries and channel previews.
- Optional RGBA PNG/TIFF/EXR packing with preserved independent channel values. [Quick start](docs/quick-start.md#pack-orm--rma-channels).

## Added in 0.3.4

- Distinct muted section colors, slim accent edges and coordinated inputs separate main-window folders, conversion, preview, status and cache.
- Clearer selected/hover tabs and readable dropdown arrows, with primary actions and warning colors preserved.
- Presentation-only changes; processing rules and saved profiles remain compatible.

## Added in 0.3.3

- Click headings to sort main-table rows; numeric sizes/depths/UDIM and natural filename ordering.
- Combine source/output resolution, format and depth filters with type/Search; exact dimensions or longest-edge presets.
- Visible counts, Clear filters and CSV of the displayed rows/order. View filters preserve conversion inclusion. [Guide](docs/production.md#sorting-and-filtering-the-main-table).

## Added in 0.3.2

- Source/output dimensions, image format and sample depth directly in the main table before Start conversion.
- Background header inspection with cached metadata, immediate rule-driven predictions and clear unreadable-header states.
- Metadata search, detailed row summaries and preview CSV fields. [Guide](docs/production.md#source-and-output-columns-before-start).

## Added in 0.3.1

- Main-window per-map resolution limits, an 8K Displacement / 4K Normal / 2K Color preset, custom pixel sizes and filter selection.
- One Start conversion applies names, formats and resolutions. Aspect ratio is preserved without upscaling; resized normals are renormalized.
- Resolution rules save in profiles/settings, including custom types through All map types. [Guide](docs/production.md#resolutions-by-map-type-in-the-main-window).

## Added in 0.3.0

- Main-window per-map formats: Displacement EXR float32, Normal PNG16 and Color PNG8 in one Start conversion; profile persistence and custom types.
- Fixed cross-drive publishing (WinError 17), recoverable prepared outputs and a working Cancel current work action for queue/native TX tasks.
- TX CPU/compression tuning and right-click multi-selection format/resolution edits in preview and paused unfinished jobs.

## Added in 0.2.1

Select **Output image format / bit depth** directly in the main window, then **Start conversion** to apply UDIM naming, folder layout and actual EXR/PNG/TIFF/JPEG encoding in one operation. Compression and Alpha are beside the format selector. Preview shows final extensions and collisions; settings save in profiles. **Keep original format** retains byte-identical copying when resizing is disabled.

## Added in 0.2.0

**Start conversion** handles naming/layout and the main-window image encoding selection. For true EXR/PNG/TIFF/JPEG encoding use **Convert image format… → Preview export plan → Start format conversion**, with bit depth, compression and Alpha controls. The main file table now scrolls horizontally.

- Persistent processing queue with resume, dependencies, parallel workers and a hard queue memory cap.
- Per-map recipes for whole sets; shared displacement transforms with inverse sidecars.
- Quality findings, offline HTML A/B reports and tiled/mipmapped .tx export.
- Explicit baked OCIO exports for Color, separate from ordinary preview.
- Persistent error logs, reviewed/redacted diagnostic ZIPs and moved-project relinking with content-verified analysis.

See the [production guide](docs/production.md), [relink/support instructions](docs/support.md) and [changelog](CHANGELOG.md).

## Main workflows

- Mudbox UV/UDIM naming conversion and whole-set channel renaming, with new output copies.
- Native RGBA sampling, histogram/min/max/ROI statistics and set validation.
- Up to four version folders with shared A/B zoom/pan, Wipe, differences, change highlights and CSV reports.
- Incremental comparison sessions, autosave, recovery history and source-change detection.
- Supported 8K/16K streaming and persistent PSD/PSB saved-composite cache preparation.
- Batch grading/map utilities, ORM/RMA packing and resizing with normal-map normalization.
- OCIO color viewing with input-space rules, while displacement/normal/data values remain native.

## Support

[Report a problem](https://github.com/Forge-Plugins/UDIM-forge/issues/new?template=bug_report.yml) or [request a feature](https://github.com/Forge-Plugins/UDIM-forge/issues/new?template=feature_request.yml). Search [existing issues](https://github.com/Forge-Plugins/UDIM-forge/issues) first. Use **Help & Support → Export diagnostics…** to review/export logs, or Copy diagnostics for version basics. Include the reviewed diagnostics and reproduction steps. Review attachments before posting publicly.

UDIM Forge was previously named UDIM Studio. Existing settings, profiles, reports, caches and comparison sessions remain compatible. Settings intentionally stay under `%APPDATA%\UDIM Studio`.
