# OCIO and input spaces

OCIO in UDIM Forge manages color previews in Image Viewer and A/B/Wipe. It does not convert or rewrite your original texture data.

## Display setup

1. Open OCIO… and enable color previews.
2. Choose a bundled offline ACES configuration or browse to config.ocio. Keep external LUTs and search-path resources accessible.
3. Choose the actual Input space, followed by Display and View for your monitor.
4. Click Apply. Reload config / LUTs then Apply when external resources change.

The canvas is currently 8-bit SDR. Choose an SDR view; HDR monitor output and automatic monitor ICC profiling are not implemented.

Auto input assumes integer storage is sRGB texture data and floating storage uses the config's scene_linear role. This is a heuristic, not encoding detection. An EXR extension does not by itself prove that a file is ACEScg. Choose ACEScg only when that is its actual encoding; linear Rec.709/sRGB is another common possibility.

## Per-set and per-type assignments

Open Input assignments. Each rule has optional Set folder, Set name, Map type and Extension scopes, plus its Input space. Blank scopes match all. All filled scopes must match.

1. Click New rule.
2. Fill its scope and choose Input space from the loaded config.
3. Click Save rule to stage it, then Apply to activate the settings.

Examples:

- Albedo → the config's sRGB texture space.
- Extension exr → ACEScg, when those files were actually written as ACEScg.
- A specific version folder + Albedo + exr → ACEScg, overriding a general Albedo/sRGB rule.

Priority is deepest matching folder, then exact set name, map type, extension. The last row wins exact ties. Type-only rules beat extension-only rules. Folder rules include subfolders and do not match similarly named sibling folders. Color / Albedo / BaseColor / Diffuse are equivalent selectors. Set names use detected or assigned series names without channel/UDIM tokens; the editor suggests known names.

## Profiles and comparison sessions

Profile → Save as stores global OCIO settings and assignments together with naming/layout/map rules. Legacy profiles without OCIO leave current color settings unchanged. Folder scopes are absolute paths, so update them after moving a project.

In set comparison, Sessions… → Session OCIO settings… opens a local policy. Its A/B viewer's OCIO… button opens the same settings. Applying it creates an independent copy. Session files, autosave and recovery history keep this policy. Loading a session does not replace global settings. A new comparison inherits globals until locally configured; legacy sessions without OCIO also inherit globals.

## What remains native

Color/Albedo/BaseColor/Diffuse/Emissive RGB previews can use OCIO. Unclassified EXR can optionally be treated as color. Known Normal, Displacement, masks and ORM/RMA remain protected even when a matching assignment exists.

Exposure acts before the managed display transform. Legacy gamma/contrast are bypassed on managed sides. Raw mode, R/G/B/Alpha views, native pixel values, histogram/ROI statistics, difference thresholds, change regions, set analysis and CSV metrics stay native. Alpha is not color-transformed. Wipe reuses prepared display layers.

Filename copying, grading/export math, packing and resizing do not bake this preview transform. Tool thumbnails keep their raw-preview behavior. Changing OCIO refreshes display frames without invalidating native analysis results or decoding entire sources again.

## References

- [OpenColorIO processor and display documentation](https://opencolorio.readthedocs.io/en/latest/guides/developing/developing.html)
- [Back to Quick start](quick-start.md)

## External configurations in comparison sessions

Built-in configurations may be restored automatically. An external session config must be an absolute `.ocio` path. Loading a session asks before activating it, because OCIO can also read LUTs from its search paths. Declining keeps color transforms disabled for that session and preserves the original config and assignments. Open **Sessions → Session OCIO settings…** to review and allow it later. Automatic session recovery never activates an external config until you approve it during that run. Raw samples and analysis remain available.
