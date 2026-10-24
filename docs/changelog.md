# What’s new

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

First public Windows x64 portable release. Download UDIM_Forge_v0.1.0_Windows_x64_Portable.zip and SHA256SUMS.txt from [GitHub Releases](https://github.com/Forge-Plugins/UDIM-forge/releases/tag/v0.1.0).

- Help & Support → Check for updates… checks the latest stable release in the background, shows release notes and opens the correct portable download in your browser.
- Manual checks only; network errors/rate limits/missing releases are explicit. No automatic installation, restart or texture uploads.
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
