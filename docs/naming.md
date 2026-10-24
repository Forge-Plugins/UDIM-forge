# Naming reference

## UDIM coordinates

Using zero-based UV coordinates, tile = 1001 + u + 10 × v. U runs from 0 to 9. The first tile is 1001, its right neighbor 1002 and the tile above it 1011.

- One-based u1_v1 corresponds to 1001.
- Zero-based u0_v0 corresponds to 1001.
- One-based u2_v1 corresponds to 1002.
- Zero-based u0_v1 corresponds to 1011.

Auto detects a series as zero-based when it contains u0 or v0. For an incomplete zero-based series without these names, explicitly select UV 0-based. Review Scan files before copying.

## Templates

Common UDIM output: {name}.{udim}{ext}. For example, body_Albedo.1001.exr.

Available fields: {name}, {ext}, {udim}, {u1}, {v1}, {u0}, {v0}, {map}, {set}, {gain}. The extension includes its dot. Templates must retain the base name, extension and tile coordinates. Invalid templates and duplicate target names block copying.

## Map and set naming

Map detection uses filename/folder rules, not image recognition. Unknown or conflicting names remain Other until assigned. Apply type updates the selected type and can rename output channel tokens across the whole series. Rename channel supports custom labels. Assign set groups channels whose basenames differ.

Folder layout controls whether outputs remain together or are separated by set/type. Naming changes affect exported copies. Mudbox gain is recorded in reports by default and is never multiplied into displacement pixels by the filename converter.

## Import into other tools

Use the target application's UDIM/UV tiling import mode. The file pattern alone does not configure the material or its input color spaces. Consult the matching version's documentation.

- [Foundry Mari UDIM coordinates](https://learn.foundry.com/mari/Content/user_guide/working_with_patches/using_udim_values.html)
- [Marmoset Toolbag UDIM documentation](https://docs.marmoset.co/docs/udim-mode-texture-module/)
- [Back to Quick start](quick-start.md)
