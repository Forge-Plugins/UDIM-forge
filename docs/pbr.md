# PBR material preview

After scanning textures, click **PBR preview…** in the main header. Choose a texture set and a UDIM number. **Previous / Next** cycles through observed tiles of that set. **Sphere / Plane** chooses the preview primitive. One selected UDIM is wrapped onto the primitive; this is not a combined UDIM atlas or a mesh UV preview. **Reload main scan** refreshes the catalog from the current included main-list textures.

## Automatic map assignment

The preview groups maps using the existing texture-set identity and tile number. It recognizes Color/Albedo/BaseColor/Diffuse, Normal, Roughness, Metallic, AO, packed ORM/RMA and Displacement/Height. Separate asset folders remain separate sets unless explicitly assigned in the main application. Untiled, excluded and invalidly named files are omitted. RMA filenames/type folders are recognized locally even with older map-detection rules.

Every slot has an **Auto** choice, **Default / none** and explicit files from the selected set/tile. Exactly one matching candidate is assigned automatically. Ambiguous candidates produce a warning and use a default until you select a file; no source is guessed. **Auto assign maps** resets slots and packed-layout detection. Switching set or tile resets the slots for that tile. The resolved assignment list below the viewport shows actual files and packed channels.

Missing maps use neutral defaults: linear gray Albedo, geometric normals, Roughness 0.5, Metallic 0 and AO 1. The default Roughness/Metallic sliders apply when no effective map supplies that mask. Available maps take precedence over defaults.

## ORM and RMA

By default separate AO/Roughness/Metallic maps take precedence; missing masks fall back to the packed map. Enable **Prefer packed masks over separate maps** to reverse precedence.

- **ORM:** R=AO, G=Roughness, B=Metallic.
- **RMA:** R=Roughness, G=Metallic, B=AO.
- **Auto:** follows the recognized packed-map type; custom layouts must be selected explicitly.
- **Custom channels:** choose R, G, B, Alpha or constant 0/1 separately for AO/Roughness/Metallic.

Standalone scalar masks read R, or Y for grayscale inputs. Missing requested channels produce an error. Alpha is available as packed data; material opacity is not evaluated.

## Color and normal interpretation

Albedo is converted to linear Rec.709 before lighting. **Auto** interprets unsigned integer Albedo as sRGB and floating Albedo as linear Rec.709. Select **sRGB**, **Linear Rec.709** or **ACEScg** explicitly when that assumption is unsuitable. **OCIO assignments** uses the application's selected configuration and folder/set/type/extension input-space assignments, converting to Linear Rec.709. The config must provide a recognized linear Rec.709 color space; missing transforms are reported. Global OCIO display/view transforms are not applied on top of the material rendering.

Displacement, Normal, Roughness, Metallic, AO and packed data bypass color conversion. Normal maps are RGB tangent vectors encoded in 0–1, decoded to −1…1 and normalized. Choose **OpenGL (+Y)** or **DirectX (−Y)** to interpret the green component. **Normal strength** scales tangent X/Y before normalization. Signed floating-point normal encodings and object-space normals require prior conversion; their encoding is not inferred from storage depth.

Albedo and PBR scalar masks are clamped to 0–1 for shading. All adjustments affect preview buffers only; original pixels, displacement values, comparison results and exports remain unchanged.

## View controls

Drag to rotate, use the mouse wheel to zoom, and **Reset view** to restore the primitive view. **UV repeat** repeats the selected tile. **Light rotation**, **Exposure (EV)** and normal strength update the cached material without decoding source files again. The sidebar scrolls on smaller windows.

The renderer uses GGX specular, Smith visibility, Schlick Fresnel and either procedural studio lights or a loaded HDRI environment. Studio mode uses an approximate ambient term. Output uses a neutral Reinhard tone map followed by sRGB encoding. It is a material-inspection preview, not a reference render, shadow renderer or a renderer-specific shader match. It does not evaluate opacity, emission, clearcoat or anisotropy.

## Large textures and snapshots

Each selected source is prepared as a proxy with a longest edge of at most 512 pixels through the existing bounded streaming decoder. Only one material is active. PSD/PSB use their saved composite and existing preparation cache; unsupported multipart/page/photometric variants require a dedicated conversion. Sampled NaN/Inf, decoding failures, unavailable channels and source changes are reported. This preview does not replace full-resolution validation.

Rendering runs outside the UI thread on bounded display buffers, at 256 pixels during dragging and 512 pixels when settled. Pending work is replaced by the latest request; closing cancels work before releasing buffers. Map changes reload proxies; lighting/orbit changes reuse them. Use **Reload main scan** after changing originals externally; there is no continuous source-file watcher in this window.

**Save preview PNG…** saves the current display buffer, including the shaded material/background. It is a display image, not a texture export or linear render pass. A snapshot cannot overwrite any scanned original map.

## Custom HDRI lighting

Click **Load HDRI…** above the viewport and select a Radiance **.hdr** or OpenEXR **.exr** panorama. The file must be an equirectangular **2:1** image with RGB channels. Cubemap strips/cross layouts, multipart EXR and invalid/nonfinite RGB data are rejected. Only local files are used; no environment images are downloaded or uploaded.

The **Studio lights / HDRI** selector changes the lighting source. HDRI mode uses the panorama for diffuse irradiance and roughness-filtered GGX reflections, replacing the studio lamps and their ambient term. **HDRI rotation** rotates the environment around the vertical world axis. **Intensity ×** scales its radiance linearly: 1 is unchanged, 2 doubles the light, 0 disables it. The slider covers 0–8 and typed values may extend to 32. This is independent of the existing global display **Exposure (EV)** control. **Studio light rotation** affects Studio mode only.

Enable **HDRI background** to show the panorama behind the primitive; disable it to keep the neutral background while retaining HDRI illumination/reflections. Rotation and intensity affect the visible environment and its lighting consistently. Display snapshots include the selected background.

### HDRI input encoding

The default **Linear Rec.709** expects scene-linear RGB radiance, including values above 1. Select **ACEScg**, **sRGB** or **OCIO assignments** explicitly for differently encoded images. HDRI OCIO assignments use the environment file's path/extension and the global input policy, without inheriting the selected material-set name. Color conversion occurs before lighting preparation. Negative radiance after conversion is clamped to zero with a reported sample count; RGB NaN/Inf and values above the supported 1e30 limit are errors. Alpha does not illuminate the material.

### Preparation and cache

Preparation runs in the background and reports progress. It reads the original in bounded row bands, reducing it to at most **1024×512** with solid-angle-weighted averaging so small bright features contribute to the proxy instead of being dropped by point sampling. HDR radiance is preserved above 1. Diffuse irradiance, several roughness reflection levels and specular integration factors are then cached. The image-based lighting is a deterministic finite-sample approximation; the limited proxy/filter resolution can soften or undersample small intense lights and does not replace a production renderer.

Rotation, intensity, background, camera and material-tile changes reuse the prepared environment. Input encoding changes prepare a new cache. The cache lives in this PBR window and is released when the window closes. Reselect **Load HDRI…** after editing or moving the original externally; this window does not continuously watch the file. Changed originals detected during preparation are rejected.

**Clear / cancel** cancels pending environment preparation, releases the environment and returns to Studio lighting. Closing the window also cancels preparation. Selecting Studio while a new HDRI is being prepared keeps that choice when preparation completes. Original environment files and texture maps are never modified; a preview snapshot cannot replace a loaded environment source.

## Geometric displacement

The **Displacement** map slot automatically follows the selected set/UDIM. **Geometry displacement**, **Scale** and **Neutral** are directly above the viewport. The initial scale is 0.25 and neutral is 0.5; type exact values in the entries. The scale slider covers −2 to 2, and typed scales may extend beyond it within the supported deformation bounds. Signed scales invert the relief.

The displacement formula is `(height − neutral) × scale`, in primitive object units. The sphere's original radius is 1; the plane spans −1 to +1 on X/Y. Float height values retain their signed/HDR range: they are not gamma corrected, passed through OCIO, normalized to the tile range or clamped to 0–1. Unsigned integer input is normalized by its storage range. R (or grayscale Y) supplies height. Mudbox's `g...` filename gain is not automatically multiplied in this viewport; enter the required scale explicitly.

Sphere vertices move radially; plane vertices move along their local +Z axis. Surface normals are recomputed from displaced geometry before normal-map shading and Studio/HDRI lighting. Perspective-correct triangle rasterization and a depth buffer resolve visibility, including changed silhouettes. Disable the checkbox or set Scale to 0 to restore the analytic primitive exactly. Missing height maps leave geometry unchanged. Neutral affects height, independently of exposure and lighting.

This is a bounded preview: height uses the existing at-most-512px proxy and a 192×96 quad sphere or 128×128 quad plane (triangulated). Fine 4K/16K details can be missed; it is not a full-resolution production mesh, adaptive subdivision, combined UDIM model or shadow renderer. Pole samples are averaged to weld the sphere poles; longitude repeats and one selected tile wraps around the primitive. Preview scale is illustrative unless matched to the source model's units and displacement convention.

Geometry changes run on the preview worker and share cancellation/coalescing. A bounded 48 MiB cache reuses projected geometry for lighting/exposure changes. Closing the window cancels work; cached geometry remains bounded in memory. Nonfinite sampled heights, sphere collapse and offsets exceeding 4 object units are explicit errors; reduce scale or correct the source. Original EXRs, comparison results and exported textures are unchanged.
