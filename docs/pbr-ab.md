# PBR material A/B comparison

Open **Compare → Compare PBR materials A/B**, use **Compare A/B** inside the PBR preview, or search for **PBR A/B** in Ctrl+K. Choose two version folders and click **Scan / reload versions**. Each folder has an independent set selector: select the matching sets explicitly if a folder contains several assets. The set names may differ between A and B.

## Linked viewing

Choose **Side by side** or **Wipe**. Drag either material to orbit both, use the mouse wheel for a common zoom, and use **Reset camera** to return to the default pose. In Wipe, move the slider or drag the orange divider. Wipe composes the last completed A/B frame pair without rendering, decoding textures or uploading GPU maps again. A value of zero shows B; one shows A.

Sphere and plane show the selected UDIM. Previous/next buttons navigate the union of both versions' tiles. Missing sides use a checker placeholder and an explicit status message. The coverage summary lists tiles found only in A or B.

## One model and lighting setup

**Shared OBJ / FBX** imports one static UV0 model for both materials. Model mode uses the whole selected UDIM set; missing tiles are magenta. **Highlight selected UDIM on model** helps locate the current tile. Subdivision, geometric displacement scale and neutral height are shared, so a difference reflects the materials rather than different geometry settings.

**Load shared HDRI** prepares one HDR/EXR panorama for both versions. Choose its encoding before loading. Rotation, intensity, background, exposure, studio lights and surface defaults apply equally to A and B. HDRI lighting without a loaded panorama falls back to Unlit. Opening comparison from an existing PBR preview inherits its prepared model, HDRI and view once; subsequent changes belong to the comparison window.

Albedo input encoding and ORM/RMA layout can differ between A and B. Custom packed-channel routing is shared. Map roles are assigned by the existing map recognition rules; ambiguous assignments must be corrected before loading.

## Performance and safe snapshots

Auto uses independent persistent GPU caches for A and B. If GPU preparation fails, both sides fall back to CPU. GPU mode reports failure explicitly. The A+B GPU cap is a combined texture/geometry allocation budget split equally between versions; it is not a process RSS limit. The application's global RAM policy also applies to preparation and native workers.

Adaptive quality uses a smaller frame and simplified geometry during movement, then refines after stopping. Each published pair has the same camera and lighting state; incomplete pairs are never displayed. Preview proxies remain bounded and are not full-resolution texture analysis. The default final frame is 512 pixels per side; each selected set supports up to 128 tiles. Reload versions after editing source files externally.

**Save preview** exports the displayed composition as a PNG display image. It is not a numerical texture-difference report or a material export. Use the existing set-analysis tools for native pixel thresholds and CSV results.

## Projects and portability

Projects and autosave retain both folders, set selections, tile, camera, Wipe position, input modes, shared model/HDRI paths and view controls. Reopening prepares fresh bounded proxies. **Collect project dependencies** includes both catalogs, including unopened tiles, plus the model, HDRI and applicable OCIO dependencies. Source files are never modified by this preview.
