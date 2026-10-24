# UDIM Forge

A Windows desktop toolkit for UDIM texture sets: organize names and folders, inspect native pixels, compare A/B/C/D versions, validate assets and export processed copies.

This repository hosts documentation and the public issue tracker. Application source and binaries are distributed separately; the documentation does not imply that a downloadable release is already published here.

## Documentation

- [Quick start](docs/quick-start.md) — first conversion, inspection and comparison.
- [User guide](docs/user-guide.md) — complete workflows, controls and limitations.
- [OCIO and input assignments](docs/ocio.md) — color spaces per set/type and session-local policies.
- [Naming reference](docs/naming.md) — UDIM, UV coordinates, templates and channel naming.
- [Troubleshooting](docs/troubleshooting.md) — previews, large files, cache, colors and recovery.
- [External references](docs/references.md) — official format and pipeline documentation.
- [Contact / support](docs/support.md) — reporting problems and copying diagnostics.
- [What’s new](docs/changelog.md) — current preview functionality.

The application includes these guides offline in **Help & Support**. Its UI and documentation are in English.

![UDIM Forge offline Help and Support](docs/images/help-center.png)

## Main workflows

- Mudbox UV/UDIM naming conversion and whole-set channel renaming, with new output copies.
- Native RGBA sampling, histogram/min/max/ROI statistics and set validation.
- Up to four version folders with shared A/B zoom/pan, Wipe, differences, change highlights and CSV reports.
- Incremental comparison sessions, autosave, recovery history and source-change detection.
- Supported 8K/16K streaming and persistent PSD/PSB saved-composite cache preparation.
- Batch grading/map utilities, ORM/RMA packing and resizing with normal-map normalization.
- OCIO color viewing with input-space rules, while displacement/normal/data values remain native.

## Support

[Report a problem](https://github.com/Forge-Plugins/UDIM-forge/issues/new?template=bug_report.yml) or [request a feature](https://github.com/Forge-Plugins/UDIM-forge/issues/new?template=feature_request.yml). Search [existing issues](https://github.com/Forge-Plugins/UDIM-forge/issues) first. Include **Help & Support → Copy diagnostics** and reproduction steps. Review attachments before posting publicly.

UDIM Forge was previously named UDIM Studio. Existing settings, profiles, reports, caches and comparison sessions remain compatible. Settings intentionally stay under `%APPDATA%\UDIM Studio`.
