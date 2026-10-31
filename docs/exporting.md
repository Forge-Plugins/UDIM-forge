# Export workflow and final checks

## One export entry point

The main **Export…** button opens the export center. Choose **Included files**, **Selected files**, or **Whole loaded set (including excluded files)**. Selected files uses the visible main-table selection and exports those files regardless of their include checkbox. Included files uses all included inputs, regardless of view filters. Whole loaded set includes excluded files too. Scope selection uses copies of the texture records and does not change their include flags or map assignments.

For **Names / format / resolution**, the center uses the main UDIM naming, layout and per-map format/resolution settings. Enter an output parent, review the output names/dimensions/depth, then choose **Check & start export**. A missing output directory is created when export starts. Changed scope, settings or source revisions invalidate the review. Existing conversion recovery still uses **Resume conversion**.

**Material recipe / packing** continues into the recipe editor with exactly the selected scope. **Grading / map utilities** continues into the advanced batch editor with those files and the selected output parent. Review its filters and numeric recipe before exporting. Advanced tools remain accessible from Tools and Ctrl+K.

## Typed per-map rules

Production Workflows now has a **Per-map rules** table instead of a JSON editor. Double-click a rule or use **Add map rule… / Edit selected… / Remove rule**. Fields include map type, format/depth, longest edge, exposure, gamma, saturation, multiply, offset, normal normalization, normal Y flip, OCIO bake and Skip. Editing a rule activates Whole-set recipe mode.

**Inherit** uses the common output setting. An inherited edge uses the common limit; **0** explicitly retains original dimensions for that map type. Blank numeric point fields mean neutral processing. Color/Albedo/BaseColor aliases cannot define conflicting separate rules. Invalid numbers and unsupported formats remain in the editor with an explanation until corrected.

Exposure/gamma/saturation must remain neutral and OCIO bake must remain off for data maps in material recipes. Numeric multiply/offset are available for intentional displacement or mask adjustments. Global Alpha/compression/filter controls remain in Format / recipe. Save/load recipe files, target presets and .udimforge projects keep their existing JSON serialization; no manual JSON editing is needed.

## Browse recipe tiles

After **2. Review output plan**, choose an image row and **Before / after…**. Select a map/UDIM in the preview list, or use **Previous / Next**. The comparison window also has Previous/Next tile navigation. Zoom, pan and Wipe/difference mode are retained when switching outputs. Different resolutions share source-pixel coordinates; use Fit both if a retained view falls outside a smaller output.

Each sample is produced by the actual full-resolution pipeline and its dependencies in capped workers. Switching during preparation requests cancellation before starting another sample. Two completed samples are retained for quick return; older owned temporary output folders are removed where decoder file locks permit, with remaining files cleaned when the preview closes. Disk preflight applies before each preparation. An initial large sample can take time.

Source revision or recipe changes mark the preview **OUT OF DATE** and prevent preparing/opening it as a current result. Review a fresh plan in the recipe window, then select **Use current reviewed plan**. The preview contains the reviewed scope and settings snapshot; it does not silently follow unreviewed edits. Normal/color outputs start in RGB, and OCIO display starts off to avoid double-baking. Native difference metrics compare stored units, so changing integer bit depth changes numeric codes even when normalized appearance is equivalent.

## Final output check

Recipe/format/packing/displacement/TX queue plans append **Final output check**, dependent on every planned image output. It reopens outputs, checks dimensions, channel count/Alpha and numeric type against the plan, and streams all encoded pixels to detect decode errors and NaN/Inf. TX validation reads all available mip levels, requires decreasing dimensions and a final 1×1 level. Expected output entries define completeness; intentional UDIM gaps are not treated as missing contiguous tiles.

A failed final check is shown as **Error / Needs attention**, with its report retained for inspection. An incomplete export blocks the dependent check instead of reporting success. This control verifies output structure and readability; it does not certify artistic quality, the intended normal convention or lossless equivalence after integer quantization/resize.

Main naming conversion and the grading/resize/packing batch tools also create **forge_export_check.json**. Encoded outputs receive a full bounded pixel scan. Unmodified copies use SHA-256 byte identity plus a bounded header check where supported; their report explicitly says that pixels were not scanned. A format whose copied header cannot be checked is **Unverified**, rather than silently passed. Native encoder/decoder limits and cancellation remain in effect.

Open **4. Final check…** in Production Workflows, open the queue's final-check row, or use **Final check report…** in the export center for the latest main conversion. Batch tools have their own Final check button. Reports list each file's status, size, type/depth, channels and error. They describe the completed check, not ongoing monitoring.

Right-click an exported batch in the queue and choose **Recheck exported batch** to verify its current files again. The queue must be paused/idle. This creates a new report without overwriting the initial one. Completed production results remain protected; a recheck does not modify textures.


## Output range and normal advisories

Version 0.35 measures per-channel min/max and percentages exactly equal to 0 or 1 during the existing full-pixel check of encoded exports. The report exposes these statistics when a file is selected. Normal outputs additionally report the proportion of RGB vectors whose reconstructed length differs from one by more than 0.05. These are advisories; RGB is assumed to encode tangent-space normals in 0–1. Unchanged copies retain their SHA-256/header check and do not claim a pixel scan. No extra full-image allocation is introduced. Source/output range equality is not asserted after intentional grading or resampling.

Grading and image encoding also accumulate their input-channel ranges during the processing pass. The final report shows them beside decoded output ranges and the actual clipped-sample count. Integer inputs are normalized to 0–1. An encoding input may already be a resized intermediate; it is labelled accordingly and is not claimed to be a scan of the full original-resolution file.
