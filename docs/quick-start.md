# UDIM Forge — Quick start

UDIM Forge is a commercial Windows desktop tool for organizing, inspecting, comparing and processing UDIM texture sets. Use the application build provided through your authorized purchase channel, extract it and run UDIM Forge.exe. Python is not required. [Licensing and updates](updates.md) explains distribution and replacing a local copy.

## Convert Mudbox names to UDIM names

1. Drop your source folder into the main window, or browse to it. Enable Include subfolders when needed.
2. Choose the correct source naming under **Conversion → Advanced**. Auto detects common UDIM and UV names; explicitly choose UV 0-based if an incomplete set has no u0/v0 tile.
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

Open **Tools → OCIO display settings…** and choose Configuration, Input space, Display and View. Input assignments can scope color spaces to folders, set names, map types or extensions. Use the actual encoding of your files. Normal, Displacement, masks and packed maps bypass OCIO.

## Process a set

The main-header **Tools** menu offers batch grading/map utilities, ORM/RMA packing and resizing with normal-map normalization. Review the settings and export copies to a new output location. These tools intentionally change exported pixels; the filename converter does not.

## Continue

- [Full user guide](user-guide.md)
- [Format conversion and production workflows](production.md)
- [OCIO and input assignments](ocio.md)
- [Naming reference](naming.md)
- [Troubleshooting](troubleshooting.md)
- [Contact / report a problem](support.md)

## Different resolutions in one set

In the main window open **By map type** and enable **Resolutions by map type**, then choose **8K / 4K / 2K preset** (Displacement / Normal / Color) or enter your own longest-edge limits. Choose per-map formats above, review the preview and press **Start conversion** once. Save these settings through **Profile → Save as…**. Smaller images are never enlarged; **Keep original resolution** leaves a type at its source dimensions. See [resolution rules](production.md#resolutions-by-map-type-in-the-main-window) for custom types, filters and format limits.

Before **Start conversion**, check **Source size → Output size**, **Source format / depth** and **Output format / depth** in the main table. Values update when settings change. Headers load in the background; Unknown indicates an inspection warning, not an assumed size. The same fields are included in **Export visible CSV…**. See [preview columns](production.md#source-and-output-columns-before-start).

## Find mismatched settings

Click a table heading to toggle ascending/descending order. Open **Filters** and combine the **Source** and **Output** size/format/depth selectors above the table; use an exact dimension or **4K edge** to match any aspect ratio with that longest edge. For example, Output **PNG** + **8-bit uint** finds planned low-depth outputs. **Clear filters** restores all rows; **Export visible CSV…** follows the view/order. View filters do not exclude files from Start conversion. To process a subset, select the matched rows and right-click **Convert selected files…**. See [sorting/filtering](production.md#sorting-and-filtering-the-main-table).

## Pack ORM / RMA channels

Click **Pack ORM / RMA…** in the main header. Use **ORM** for R=AO, G=Roughness, B=Metallic, or **RMA** for R=Roughness, G=Metallic, B=AO. In **CHANNEL ROUTING**, choose the source map and sample for every output channel; scalar maps use R. Constants 0/1 are available. Enable **Include Alpha in output (RGBA)** when needed. Review the routing summary, select a paired UDIM tile, inspect RGB or individual channels, choose PNG/TIFF/EXR and **Export packed copies**. Missing/duplicate inputs are shown in the pairing table. See [packing details](user-guide.md#orm--rma-channel-packing).

Choose **Packing preset → Save as…** to reuse this setup: routing, source channels, Alpha, format/depth, map name and missing-map policy. Select the saved name for the next set; presets remain available after restarting. Source/output folders are kept independent.

## Inspect UDIM boundaries

Use **Tools → UDIM boundary validator…** or **UDIM boundaries…** in the set validator. Choose a map type, stored channel and threshold in native units, then **Analyze boundaries**. Inspect the A/B border strips, highlighted differences, largest-difference marker and exact pixel counts; **Export CSV / JSON…** includes all connections. This compares stored neighboring image borders and cannot infer mesh/UV continuity or tangent frames. See [boundary validation](boundaries.md) for units, orientation, streaming limits and interpretation.

## Preview a PBR material

Click **PBR preview…** in the main header, choose a texture set and UDIM, then Sphere or Plane. Map slots assign unique matches automatically. Separate masks take precedence, with ORM/RMA fallback. Inspect the resolved assignments, choose normal orientation and Albedo encoding, and drag to rotate. Light/exposure changes reuse cached proxies. See [PBR material preview](pbr.md) for channel layouts, color handling and renderer limits.

In **PBR preview…**, click **Load HDRI…** for a local 2:1 HDR/EXR panorama. Adjust **HDRI rotation** and **Intensity ×**, toggle **HDRI background**, or switch back to **Studio lights**. Lighting caches are prepared once in the background. See [HDRI lighting](pbr.md#custom-hdri-lighting) for input encoding, limits and cancellation.

For geometric height preview, open **PBR preview…**, check the **Displacement** assignment and enable **Geometry displacement** above the viewport. Adjust **Scale** and **Neutral** (default 0.5); choose Sphere or Plane and rotate to inspect the changed surface. See the [displacement guide](pbr.md#geometric-displacement).

For the whole UDIM set on a model, use **PBR preview… → Import OBJ / FBX…**. Model mode assigns channels/tiles from the selected set via UV0. Choose Subdivision, adjust displacement Scale/Neutral and load your HDRI. Missing UDIMs are magenta. See [model import](pbr.md#imported-obj--fbx-with-the-whole-udim-set).

### Destination and selected map names

After scanning, type or browse a different **Output** folder and click **Start conversion** directly. Missing folders are created at conversion time; typing does not write to disk. Changing the source folder or scan scope still requires Scan files.

Select one or more table rows, then choose a **Selected** map type to apply it immediately. Typed custom names use Enter or Apply type. Entire series extends the change to matching map tiles. Enable Rename files with Apply type in Selection to update output filenames together with the map type. Originals are preserved.

Batch Texture Tools presets change operation settings while keeping your chosen map filter and file selection.

## Find commands quickly

Press **Ctrl+K**, type a tool name or keyword, select with Up/Down and press Enter. Escape closes the palette. Star favorites, use Favorites only, or open Shortcuts / preferences to assign custom keys. Favorites and eight recent commands persist between runs. Unavailable actions show their requirements. See [Command palette](commands.md).

For a complete material recipe, open **Target presets / recipe…**, apply a target preset, then **Preview export plan**. Select an output and choose **Before / after…** to inspect an exact representative result before **Start queued workflow**. **Color / data details…** explains the selected output transforms.
