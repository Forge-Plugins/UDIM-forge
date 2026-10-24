# UDIM Forge — Quick start

UDIM Forge is a Windows desktop tool for organizing, inspecting, comparing and processing UDIM texture sets. Download the Windows x64 Portable.zip asset from [GitHub Releases](https://github.com/Forge-Plugins/UDIM-forge/releases/latest), extract it and run UDIM Forge.exe. Python is not required. [Download/update instructions](updates.md) explain how to replace an existing portable copy.

## Convert Mudbox names to UDIM names

1. Drop your source folder into the main window, or browse to it. Enable Include subfolders when needed.
2. Choose the correct Source format. Auto detects common UDIM and UV names; explicitly choose UV 0-based if an incomplete set has no u0/v0 tile.
3. Choose the target naming format and folder layout. A common Mari / Toolbag pattern is {name}.{udim}{ext}.
4. Click Scan files. Review map types, tile numbers, target names and collisions.
5. Correct map types with Apply type or use Rename channel. Apply type can update the output channel token for the whole series.
6. Choose an output location and click Create copies. Each run creates its own output subfolder. Source texture files remain unchanged.

For example, one-based body_color_u1_v1.exr becomes body_color.1001.exr. The equivalent zero-based source is body_color_u0_v0.exr. Filename conversion copies pixel data unchanged; it does not bake color transforms or displacement gain.

## Inspect a texture

Double-click a row to open Image Viewer. Select RGB or an actual stored channel, adjust EXR exposure/gamma, and inspect native RGBA values under the cursor. Histogram, min/max and selected-area statistics use original pixel values. Raw 0–1 bypasses display color processing.

## Compare versions

Open Compare sets, choose two to four folders A/B/C/D, then Scan versions. Pick the reference and candidate. Double-click a tile to use Side by side, Wipe or native differences. Zoom and pan are shared. Missing and ambiguous tiles appear explicitly.

Use Analyze all versions for a combined report. Set Max |Δ| to filter differences in native units, then export CSV. Completed results and OCIO settings are autosaved; Save session exports a portable session description. Changed sources are reanalyzed when restored.

## Set up color viewing

Open OCIO… and choose Configuration, Input space, Display and View. Input assignments can scope color spaces to folders, set names, map types or extensions. Use the actual encoding of your files. Normal, Displacement, masks and packed maps bypass OCIO.

## Process a set

Texture tools offers batch grading/map utilities, ORM/RMA packing and resizing with normal-map normalization. Review the settings and export copies to a new output location. These tools intentionally change exported pixels; the filename converter does not.

## Continue

- [Full user guide](user-guide.md)
- [OCIO and input assignments](ocio.md)
- [Naming reference](naming.md)
- [Troubleshooting](troubleshooting.md)
- [Contact / report a problem](support.md)
