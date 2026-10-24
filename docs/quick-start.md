# UDIM Forge — Quick start

UDIM Forge is a Windows desktop tool for organizing, inspecting, comparing and processing UDIM texture sets. Download the Windows x64 Portable.zip asset from [GitHub Releases](https://github.com/Forge-Plugins/UDIM-forge/releases/latest), extract it and run UDIM Forge.exe. Python is not required. [Download/update instructions](updates.md) explain how to replace an existing portable copy.

## Convert Mudbox names to UDIM names

1. Drop your source folder into the main window, or browse to it. Enable Include subfolders when needed.
2. Choose the correct Source format. Auto detects common UDIM and UV names; explicitly choose UV 0-based if an incomplete set has no u0/v0 tile.
3. Choose the target naming format and folder layout. A common Mari / Toolbag pattern is {name}.{udim}{ext}.
4. Click Scan files. Review map types, tile numbers, target names and collisions.
5. Correct map types with Apply type or use Rename channel. Apply type can update the output channel token for the whole series.
6. Choose Output image format / bit depth (Keep original format or EXR/PNG/TIFF/JPEG), compression and Alpha, then an output location and click Start conversion. Each run creates its own output subfolder. Source texture files remain unchanged.

For example, one-based body_color_u1_v1.exr becomes body_color.1001.exr. The equivalent zero-based source is body_color_u0_v0.exr. Filename conversion copies pixel data unchanged; it does not bake color transforms or displacement gain.

Enable **Use formats by map type** for a mixed set: Displacement EXR float32, Normal PNG16, Color PNG8. The inline preset and All map types editor configure these rules; save a profile to reuse them.

## Inspect a texture

To change actual image encoding, use **Convert image format…**, choose **Output format / bit depth** (EXR/PNG/TIFF/JPEG), then **Preview export plan → Start format conversion**. You can also choose Output image format / bit depth directly in the main window and press Start conversion to combine naming and encoding in one pass. See [production workflows](production.md) for compression, Alpha, durable queues, recipes and displacement processing.

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
- [Format conversion and production workflows](production.md)
- [OCIO and input assignments](ocio.md)
- [Naming reference](naming.md)
- [Troubleshooting](troubleshooting.md)
- [Contact / report a problem](support.md)

## Different resolutions in one set

In the main window enable **Use resolutions by map type**, then choose **8K / 4K / 2K preset** (Displacement / Normal / Color) or enter your own longest-edge limits. Choose per-map formats above, review the preview and press **Start conversion** once. Save these settings through **Profile → Save as…**. Smaller images are never enlarged; **Keep original resolution** leaves a type at its source dimensions. See [resolution rules](production.md#resolutions-by-map-type-in-the-main-window) for custom types, filters and format limits.
