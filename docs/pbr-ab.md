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

## Difference and Heatmap

Choose **Difference** to view absolute RGB changes or **Heatmap** to show their magnitude from blue through cyan and yellow to red. Both use the last completed, synchronized A/B frame pair. Camera, zoom, model and HDRI remain shared.

**Threshold (0–1)** filters by the largest absolute R/G/B channel difference for each rendered pixel, divided by 255. A pixel is counted only when its difference is strictly greater than the threshold. Pixels at or below the threshold are black. A threshold of zero includes every visible change; one excludes all changes. The initial threshold is 0.02.

**Display gain ×** amplifies visible differences from 1× to 32×. It does not affect the threshold, changed-pixel count, percentage or reported maximum. Heatmap colors use a fixed absolute scale multiplied by gain, so colors do not rescale automatically for each tile. High differences saturate red. The status displays the count, percentage and maximum difference across all viewport pixels, including background.

Threshold, gain and display-mode changes use cached RGB differences without re-rendering, decoding textures or uploading GPU maps. A new completed frame pair replaces this cache. Adaptive and full-quality frames can have different counts because their viewport resolutions differ. A missing side has an explicit unavailable message rather than statistics comparing a placeholder to a material. In whole-model view, missing UDIM markers remain part of the rendered image and the coverage summary identifies missing tiles.

These differences measure the **8-bit displayed PBR image after lighting, exposure and display encoding**, including its silhouettes and background. They are not linear-radiance, native-EXR, height or UV-space differences. Clipped highlights or changes invisible from the current view may not appear. Use native set analysis to measure source texture values. Disable the shared UDIM highlight when measuring material changes alone.

**Save preview** exports the current Difference or Heatmap image as PNG. Projects and autosave retain the selected mode, threshold and gain along with the linked camera and lighting; old projects default to threshold 0.02 and gain 4×.

## Change regions and linked detail navigation

The comparison detects **8-connected regions** of viewport pixels whose maximum absolute RGB difference exceeds the current threshold. Diagonal neighbors belong to the same region. Regions are ordered by peak difference, then changed-pixel area, then top/left position. Isolated single-pixel changes are retained. The count and bounds describe the whole rendered frame, including background, even while a detail is enlarged.

**Previous / Next** select and frame a region in both versions. Navigation wraps at either end. **Max difference** selects the region with the largest difference and centers a shared detail view on its strongest pixel, with at least 4× magnification. Ties select the first top/left peak. The status shows region index/count, changed-pixel area, peak value and its original rendered-pixel coordinates. **Region outline** draws its clipped bounds and a cross at the peak in each side-by-side panel or in the Wipe/difference view.

Navigation uses a shared **screen-space crop** of the completed synchronized frames, with padding and a 16× magnification limit. It preserves the 3D camera's angle, zoom and HDRI and does not request a render, decode or GPU upload. This enlarges existing preview pixels; it does not add texture resolution or move the camera through 3D space. **Show full frame** removes the detail crop; **Reset camera** also resets the 3D view. Both sides always use identical crop coordinates, including when switching to Wipe.

Wait for camera movement/rendering to finish before selecting a region. A new frame or threshold recomputes regions and clears the selected index. Gain and view-mode changes preserve the region cache. A missing side or zero pixels above threshold disables navigation. The normalized detail center and magnification are saved in projects/autosave; the component list is rebuilt from fresh frames on reopening. **Save preview** saves the current cropped composition, without the UI's outline/cross annotations.

## Export an offline HTML comparison report

Click **Export HTML report** in the PBR A/B window, below the region-navigation controls. Finish camera movement and preparation first. Both sides of the active tile must be available; a placeholder cannot be presented as a material comparison. Model mode reports the full rendered UV0 material view, with the active UDIM recorded in its settings. This exports the current completed view, not a batch of all camera angles or UDIM views.

The single HTML file embeds the original A/B images, heatmap and absolute difference, marked A/B images, and the current shared detail crop when zoomed. It includes threshold, display gain, changed viewport-pixel count/percentage, maximum difference and region count. Selected regions are white with a peak cross; other annotated regions are orange. Tables and image outlines show the first 200 regions by severity plus the selected region if necessary. Complete numeric region data remains available in the expandable JSON section.

Camera pose/zoom, shape, actual renderer, exposure, displacement, subdivision, HDRI filename/encoding/rotation/intensity/background, version folders/set names, input routing, OCIO settings and source revision stamps belong to the synchronized rendered snapshot. Threshold, gain and current detail crop belong to the exported analysis. Metadata is copied before background export; pending renders block export rather than mixing live controls with an old image. The report includes source paths, so review its contents before sharing.

Open the HTML in a local browser. Images are embedded PNGs; there are no scripts, remote assets, downloads or automatic uploads. No companion folder is required. Export uses the existing cancellable background worker and an atomic final write. Cancellation or a failed write preserves an existing report. Reports are limited to 32 MiB and display frames to 768 pixels per side. Source textures, model and HDRI are protected destinations.

Statistics cover the original full viewport and background, even if the detail images are cropped. They measure displayed 8-bit RGB after lighting/exposure/encoding; this is not native EXR or physical-material analysis. Reload external texture edits before preparing a new snapshot. Missing model tiles remain visible markers and their per-version coverage is recorded in the snapshot settings.

## Saved camera views and multi-view HTML

Open **Saved views / multi-view HTML** in the PBR A/B window. Enter a name and **Save current as new** to capture yaw, pitch, camera zoom and the normalized detail crop. **Apply selected** recalls it for A and B together. **Update from current** replaces the selected name/pose with the current camera. Remove, move up/down and include/exclude controls define the report order. **Add standard four** adds Front/Right/Back/Left without duplicating existing names; adjust these poses for the actual geometry, especially a plane. Save up to 16 uniquely named views.

Camera views are stored in projects and autosave. **Save views / Load views** transfers a bounded JSON pose preset to another project. Presets contain camera/detail settings and inclusion flags only; they do not activate OCIO configs or load model/HDRI files. Old projects open with an empty view list.

**Export included views to HTML** renders every included pose sequentially at 512 pixels per side. The current shape, active UDIM (sphere/plane) or whole model UV0 set, materials, displacement, lighting/HDRI rotation/intensity, exposure, threshold and difference gain are frozen once for the batch. Only the saved camera/detail pose changes. The active on-screen camera is preserved. Pending preparation or a missing primitive-side tile blocks export. Editing live controls during a batch affects later preview work, not the report's frozen snapshot.

The single offline HTML contains a view index, per-view summary table and a complete A/B/heatmap/region report for each camera, including its pose and optional detail crop. Statistics are per viewport; counts from different angles are not unique material-pixel totals. GPU caches are reused between poses. Auto falls back consistently: if a later view loses GPU support, all views restart on CPU; explicit GPU errors abort the report. The manager displays progress and offers **Cancel export**. Rendering does not decode source textures again.

The final report is replaced atomically only after every included view succeeds. Cancellation, a failed renderer or the 64 MiB combined size limit preserves an existing report. Individual view documents retain their 32 MiB/200-outlines limits and full numeric region data. No scripts, external images or automatic uploads are added. Temporary per-view documents are local; review source-path metadata before sharing. Full-frame PBR differences remain display-RGB comparisons, not native EXR analysis. Detail zoom enlarges existing preview pixels.
