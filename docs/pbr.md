# PBR material preview

After scanning textures, click **PBR preview…** in the main header. Choose a texture set and a UDIM number. **Previous / Next** cycles through observed tiles of that set. **Sphere / Plane** chooses the preview primitive. One selected UDIM is wrapped onto the primitive; this is not a combined UDIM atlas or a mesh UV preview. **Reload main scan** refreshes the catalog from the current included main-list textures.

## Automatic map assignment

The preview groups maps using the existing texture-set identity and tile number. It recognizes Color/Albedo/BaseColor/Diffuse, Normal, Roughness, Metallic, AO and packed ORM/RMA. Separate asset folders remain separate sets unless explicitly assigned in the main application. Untiled, excluded and invalidly named files are omitted. RMA filenames/type folders are recognized locally even with older map-detection rules.

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

Normal, Roughness, Metallic, AO and packed data bypass color conversion. Normal maps are RGB tangent vectors encoded in 0–1, decoded to −1…1 and normalized. Choose **OpenGL (+Y)** or **DirectX (−Y)** to interpret the green component. **Normal strength** scales tangent X/Y before normalization. Signed floating-point normal encodings and object-space normals require prior conversion; their encoding is not inferred from storage depth.

Albedo and PBR scalar masks are clamped to 0–1 for shading. All adjustments affect preview buffers only; original pixels, displacement values, comparison results and exports remain unchanged.

## View controls

Drag to rotate, use the mouse wheel to zoom, and **Reset view** to restore the primitive view. **UV repeat** repeats the selected tile. **Light rotation**, **Exposure (EV)** and normal strength update the cached material without decoding source files again. The sidebar scrolls on smaller windows.

The renderer uses GGX specular, Smith visibility, Schlick Fresnel and diffuse lighting from procedural studio lights, with an approximate ambient term. Output uses a neutral Reinhard tone map followed by sRGB encoding. It is a material-inspection preview, not a reference render, HDRI environment, shadow renderer or a renderer-specific shader match. It does not evaluate displacement, mesh deformation, opacity, emission, clearcoat or anisotropy.

## Large textures and snapshots

Each selected source is prepared as a proxy with a longest edge of at most 512 pixels through the existing bounded streaming decoder. Only one material is active. PSD/PSB use their saved composite and existing preparation cache; unsupported multipart/page/photometric variants require a dedicated conversion. Sampled NaN/Inf, decoding failures, unavailable channels and source changes are reported. This preview does not replace full-resolution validation.

Rendering runs outside the UI thread on bounded display buffers, at 256 pixels during dragging and 512 pixels when settled. Pending work is replaced by the latest request; closing cancels work before releasing buffers. Map changes reload proxies; lighting/orbit changes reuse them. Use **Reload main scan** after changing originals externally; there is no continuous source-file watcher in this window.

**Save preview PNG…** saves the current display buffer, including the shaded material/background. It is a display image, not a texture export or linear render pass. A snapshot cannot overwrite any scanned original map.
