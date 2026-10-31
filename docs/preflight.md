# Export preflight

Version 0.15 checks the export plan before the main Start conversion, batch grading/resize/packing and production queue start/resume. Use **Check export…** in the main window to inspect a plan without exporting. Missing output folders are created only by the export itself, after the check succeeds.

## Errors and warnings

Blocking errors include unreadable/missing inputs, colliding names, existing unowned output files, targets replacing sources, unusable output ancestors and insufficient space for known copies plus a 256 MiB working reserve. Preflight tests permissions with tiny, uniquely named files it creates, flushes, renames and deletes in the nearest existing output ancestor. It does not overwrite textures or create the planned output tree.

Warnings require the **I understand the warnings** checkbox before **Continue export**. They include float-to-integer range/quantization risk, float32-to-half or float64-to-float32 precision/range loss, uncertain metadata, missing channel tiles and insufficient estimated encoding/scratch space. Blocking errors cannot be acknowledged away. A plan without warnings or errors continues automatically. Cancel stops the check/export decision before writing texture outputs.

**Locate problem** closes the report and selects matching input files in the main list, clearing view filters. Some disk/cache findings describe a directory instead of a texture; its path is shown in the main status. **Save report** exports local JSON to a separate report file. Sources and planned outputs cannot be used as that report destination.

## Map completeness

For channels already present within a texture set, preflight compares UDIM coverage against the union of that set's observed tiles. A Normal tile missing where Color exists is reported. It does not assume every material needs Color, Normal, AO or Metallic, and empty cells outside observed coverage are not presumed missing. Packing additionally uses the chosen R/G/B/Alpha mapping: required missing/ambiguous inputs block the pack plan; deliberately enabled defaults produce a warning.

## Disk estimates

The report groups requirements by destination volume and shows current free space, estimated output, peak scratch for the selected worker count, PSD ZIP-composite cache and reserve. Resize estimates use the selected dimensions, including allowed upscaling. TX includes mipmap overhead. The PSD cache directory is checked separately, even on another drive; a composite exceeding its cache quota produces a warning.

Encoding sizes are conservative metadata-based estimates. Compression, codecs, channel transforms, generated dependencies, cache reuse and concurrent activity can change actual needs. The check does not reserve disk space. Existing valid PSD caches can make estimates larger than the actual additional space required. External processes can change permissions or free space after the check; runtime write errors remain handled by the export pipeline.

## Float values

Preflight reads headers, not all image pixels. A warning identifies a risky conversion; it does not claim that a measured pixel is out of range. Integer exports can clip negative/HDR values and quantize fractional values. Half precision has a smaller finite range and precision than float32. Use the native pixel inspector, histogram and validation tools when actual sample statistics are needed. Keep original format without resizing remains byte copying and does not need a float-conversion warning.

## CLI

Headless run/resume performs the same preflight. Errors stop the command. Warnings also stop it unless **--accept-preflight-warnings** was explicitly supplied. A rejected command writes the findings to its JSON report and returns exit code 2. This flag never bypasses blocking errors.


## Review before Start conversion

The main conversion now always presents an Output plan tab before work starts, including map type, operation, source dimensions/depth, output dimensions, format/depth and filename. Selecting a row shows full input/output paths and Alpha/compression handling. The second tab contains warnings and blocking errors. Errors block Start; warnings require acknowledgment. Cancel writes no output images. Save report includes the full plan; the table displays up to 2,000 files. Generated batch folder names in preflight are planning placeholders; the actual export creates its own unique folder.
