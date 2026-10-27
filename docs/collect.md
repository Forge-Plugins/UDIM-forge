# Collect project dependencies

Use **Project → Collect project dependencies…** or search **Collect** with **Ctrl+K**.

1. Pause running operations and finish valid project settings. The collector takes a snapshot of the current workspace, including unsaved assignments and recipes.
2. Choose an existing parent and enter a **new folder name**. Existing destination folders are never overwritten.
3. If the project uses external OCIO, review its source and enable **Allow reading external OCIO configs…** only for trusted configurations.
4. Click **Review dependencies**. Inspect the file list, total space and any limitations shown.
5. Click **Collect project**. Copying and SHA-256 verification run in the background with progress and Cancel. A failed or cancelled collection removes its temporary folder and leaves originals unchanged.
6. Move the **entire resulting folder** to the other Windows computer, then open **Project.udimforge** in UDIM Forge 0.18.0 or later.

The collection includes loaded textures (including excluded files), texture inputs retained by open tools, comparison version textures (honoring the saved subfolder setting), source scan metadata sidecars, the selected PBR model and HDRI, referenced external OCIO configs and LUTs, and existing files referenced by the production queue. Files with the same basename from different roots keep separate locations. The source workspace and original project remain open and unchanged.

The project stores paths relative to its own folder and resolves them when opened. Save project preserves this form. **Save as does not copy assets**; use Collect again when moving to a different directory or after adding dependencies outside the collected folder. The Files directory mirrors the original directory hierarchy beneath a drive identifier. Keep its structure intact.

## Comparisons and queue

Comparison metrics survive relocation only when their source content hashes still match. Changed or unverifiable sources become stale and need analysis again. Opening a large collected comparison may take time while hashes are verified. Pending queue input revision guards are renewed only after matching collected content hashes; completed output and source hashes are checked on Resume. The queue stays paused, with new local storage isolated from the original project. Active conversion staging is not copied: resume through the saved queue or review and start the main conversion again. Historical report file contents are copied unchanged and may still display original paths.

## OCIO

Built-in configurations need no external files. For external configs the collector resolves the **current context**, copies every external FileTransform in color spaces (including inactive spaces), looks, view transforms and named transforms, and writes a local .ocio with relative LUT paths. Environment-dependent LUT selections are frozen to the context used during collection. Missing LUTs stop collection. On the receiving computer choose **Project → Review external OCIO…** before activating external configuration; collection never bypasses that trust step. Keep the original config separately if you need to edit context variants later.

## Models and boundaries

OBJ files with relative mtllib references collect their MTL files and relative texture references. MTL map options and absolute material/media references currently stop collection with an explicit message; make these references relative without options first. FBX is copied intact, including any embedded data. UDIM Forge uses the project's explicit texture assignments; unassigned external FBX material media is not discovered, and the review displays this limitation.

The collector does not copy the application, license secrets, machine caches, unrelated source-folder files, or a complete external renderer installation. Install UDIM Forge and activate your own license on the receiving computer if licensing is enabled. Linked/junction dependencies are rejected. Missing required inputs, insufficient free space, permission errors, changed files and copy mismatches stop publication. A collect-manifest.json records the copied payload hashes; it is an integrity record, not a cryptographic publisher signature. User-generated output files created after collection require another collection for a complete new handoff.
