# Surface tools, normals and UDIM filters

Open **Tools → Surface / UDIM utilities…**, or search **Surface** in Ctrl+K. Choose All included files or Main selection, and optionally a map type. Review the output plan before exporting. Presets save/load typed settings without editing JSON. Open .udimforge projects preserve this window's settings and reviewed plans.

## Preview versus exported pixels

**Preview selected output** prepares a temporary actual worker output and opens Before / After with UDIM navigation, shared zoom, Wipe and differences where the channel layouts permit comparison. It never changes originals. Changing settings invalidates the reviewed plan. **Open export queue** sends the reviewed plan to Production Workflows. Inspect preflight and use **Start queued workflow** to write new copies. Final checks reopen all produced images and scan their values. Cancel, Resume and the global memory budget use the existing durable queue.

PBR exposure, gamma in the image viewer, lighting, UV wireframes, UV highlight and display OCIO are **display only**. Surface grading, a LUT or channel/normal operation is **baked into copies only when explicitly exported**. No implicit OCIO conversion is performed by these utilities.

## Channels

**Unpack channels** exports 1–4 selected R/G/B/Alpha channels. Give each a channel name (ORM defaults R=AO, G=Roughness, B=Metallic; change for RMA or your layout). New names replace a recognized old channel token while keeping tile/gain formatting, and output channels get separate folders. Missing channels are errors; no Alpha is silently invented. Scalar R extraction accepts a single grayscale source.

**Remap channels** selects each destination R/G/B/Alpha independently from R/G/B/Alpha, luminance, Zero or One. Disable Write Alpha for RGB output. Output precision follows the explicit format. Integer formats quantize and may clip values outside 0–1; use float32 for arbitrary displacement values.

## Normals and height

**Normal tools** flips X/Y/Z, changes XY strength, reconstructs positive Z and optionally normalizes vectors. Y flip converts the usual OpenGL / DirectX tangent normal conventions. Alpha is preserved. Input normals must be RGB encoded in 0–1, not signed float/object-space data.

**Blend normals RNM** pairs one detail map per UDIM from the chosen detail folder and combines it with the base using [Reoriented Normal Mapping](https://blog.selfshadow.com/publications/blending-in-detail/). Strength 0–1 adjusts detail XY before reorientation. Missing/ambiguous detail tiles and exactly backward base normals are errors. Use one asset's detail folder at a time; arbitrary multi-asset folder matching is not guessed.

**Height to Normal** uses central differences from the chosen scalar channel, including neighboring UDIMs when enabled. Scale is height per UV unit: derivatives multiply by source width/height. Y flip produces the alternate tangent convention. Flat height produces (0.5, 0.5, 1). Generated files use the Normal channel name/folder. Native float heights remain native; unsigned samples are normalized to 0–1. Mudbox filename gain is not automatically applied: prepare a consistent height range first where necessary.

## Spatial UDIM operations

**Seam-aware resize**, **Seam-aware blur** and **Seam-aware sharpen** read a neighborhood of adjacent UDIMs from the same classified channel/set. Enable Read neighboring UDIMs. Resize offers Nearest, Box/area, Bilinear, Bicubic and Lanczos 3; longest edge reduces only. Blur uses a box kernel of radius 1–16 pixels; sharpen is original + strength × (original − blurred). Alpha is preserved by blur/sharpen. Normal outputs are renormalized.

For image coordinates, a tile above the current image is the next UV row (+10 UDIM); image Y increases downward. Neighbors must share dimensions and channel layout. Duplicate tiles stop planning. Missing neighbors clamp to the current tile and are listed in worker result details. Only the immediately adjacent neighborhood is included. Numerically adjacent UV tiles need not be continuous surfaces: review UV seams yourself. These operations cannot guarantee seamless results on disconnected islands.

Spatial filtering uses stored values by default. **sRGB resize** explicitly linearizes RGB, associates Alpha during filtering, unassociates and encodes back to sRGB. It applies to resize only; it is not an ACEScg transform. The native source stripe must fit 16 MiB. Normalized row and filtered-row caches are bounded; no whole 8K/16K raster is allocated.

## Masks and LUTs

**Masked grading** uses exposure, gamma, saturation, contrast, multiply and offset. Optionally select a mask folder (one unambiguous map per UDIM) and its R/G/B/Alpha/luminance channel. Mask lower/upper values remap to a clamped 0–1 weight. Invert reverses it. With no external mask, Use source value range as mask selects samples between the inclusive lower/upper limits. Alpha is not graded. The same external/range masks can limit Normal tools, RNM, Height to Normal, blur and sharpen. Masked normals are renormalized; Height to Normal fades against a flat normal. Channel routing and resize do not use these masks.

**LUT grading** loads a trusted .cube LUT and blends its effect with strength 0–1. It can use the same masks. Input pixels must already be in the LUT's authored color space; no color space is inferred. .cube files are limited to 16 MiB, 3D dimensions 2–65 or 1D length 2–65536. Classified Normal/Displacement/data maps are blocked from color grading and LUT operations. Color transforms occur in capped queue workers; project restore does not automatically execute LUTs.

## UV and contact sheets

Import an OBJ/FBX in PBR Preview, then click **UV / UDIM…**. Select a used tile to view UV0 over its map or a checker. Highlight tile on model applies an amber tint in CPU/GPU PBR display. Heavy models sample at most approximately 200,000 triangles for the wireframe; the status reports sampling. Geometry, UVs, displacement and exported textures remain unchanged. Checker is a 2D UV inspection background, not a material export.

**Contact sheet** exports a PNG display sheet with thumbnails, filenames, map type/UDIM, source dimensions, native types and channels. Choose 1–8 columns, up to 256 maps with an 8192-pixel height cap. Contact sheets are visual references and use display previews, not float data archives.

Originals and source revisions are verified by the queue. Sources changed after review require a fresh plan. Surface operation-specific format/resolution edits are made in Surface Tools; the generic queue context editor deliberately refuses them. Collected projects include referenced LUTs, masks and detail files via their saved plans and paths.
