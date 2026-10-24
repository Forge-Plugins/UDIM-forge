# Format conversion and production workflows

UDIM Forge 0.2.0 adds one persistent queue for preparing whole texture sets. Open **Convert image format…** beside the main texture tools, or **Texture tools → Production workflows / persistent queue…**. Right-click selected main-table rows to convert only those images. All processing exports new files; original textures remain unchanged.

## Which Start button?

**Start conversion** in the main window performs filename and folder-layout conversion. It copies the original file bytes; selecting a Mari naming convention does not encode an EXR as PNG.

To change the actual image format, open **Convert image format…**, choose **Output format / bit depth**, then **Preview export plan**. Review the target filenames and press **Start format conversion**. Whole-set recipes and the other operations use **Start queued workflow**. A Start button is enabled after a valid plan has been prepared. Changing settings requires a new preview.

## True image formats, depth and Alpha

Available outputs include PNG uint8/uint16, TIFF uint8/uint16/float32, JPEG uint8, and EXR half16/float32. Half has a finite magnitude limit of 65504; exceeding it aborts that output rather than creating infinity. Integer outputs normalize unsigned source samples and clamp processed values to 0–1. Use EXR float32 for signed or high-range displacement. This is actual decoding/encoding, not an extension rename.

Choose **Automatic** compression for recipes with mixed formats. Explicit EXR compression supports ZIP, PIZ, RLE and None; TIFF supports Deflate, LZW and None. Unsupported format/codec combinations are rejected. PNG uses its lossless codec; JPEG is lossy.

**Alpha policy** offers Keep, Drop and Opaque. Opaque sets an existing Alpha to one; it does not invent a missing Alpha channel. JPEG cannot preserve Alpha: explicitly choose Drop for RGBA inputs. An Alpha-only image cannot lose its sole channel. Unsupported multipart/multipage or ambiguous channel layouts produce an explicit error instead of silently dropping data. Saved PSD/PSB composites use the existing disk-backed preparation path.

## Several sets and one durable queue

Use **Add folder / set…** repeatedly, **Use main file list**, or **Use selected files**. Choose an existing **Output parent**. Each plan creates a unique Forge output directory. Per-file input/output paths appear in the preview; sources are checked against the revision captured by the plan before execution.

**Workers** controls 1–4 simultaneous processes. **Queue memory cap** limits their combined committed memory, including native subprocesses. It excludes the interactive application/viewers. Allow at least 512 MiB per worker; large native operations may need more. Exceeding the cap fails a job safely; raise the budget and retry. This is a hard safety limit, not a guarantee that every decoder can process every image within that budget.

**Pause queue** stops unfinished work and retains completed files. **Resume queue** restarts remaining files; it does not resume halfway through a scanline. **Retry failed** retries errors and blocked dependencies. Closing the workflow window leaves the queue running; closing the application pauses it. On the next launch, reopen Production workflows and Resume. No work automatically restarts on launch.

The SQLite queue is stored at `%APPDATA%\UDIM Studio\production-queue.sqlite3`; owned intermediates and completion markers are in `queue-work` beside it. One application instance owns that queue at a time. Completed output and source SHA-256 hashes are verified before reuse. An output committed just before an interruption can be adopted from its owned marker; unrelated existing files are never overwritten. Changed inputs need a fresh plan. Do not edit/delete the queue database while jobs are running.

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

Choose **TX mipmaps** for bundled OpenImageIO `maketx`: 64×64 tiled textures, mipmaps, selected filter/wrap and Keep/half/float/uint16/uint8 storage. This exports `.tx` copies without a separate tool installation. NaN/Inf abort export. Large native processing shares the queue memory cap. PSD/PSB input first prepares its saved composite as float EXR; Keep therefore retains that intermediate float storage. Choose explicit storage when required.

For details see [OpenImageIO maketx](https://openimageio.readthedocs.io/en/main/maketx.html), [OCIO](ocio.md), [support and diagnostics](support.md), and [quick start](quick-start.md).
