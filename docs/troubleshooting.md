# Troubleshooting

## Tiles are shifted or map types are wrong

Check Source format, especially incomplete zero-based UV sets. Map detection reads names rather than pixels. Correct a type with Apply type, adjust detection aliases, or use Assign set. Scan and review targets again. Resolve duplicate names before exporting.

## A file has no preview

Filename copying can work without a decoder preview. The application does not support every image variant. Deep EXR, unusual PSD color modes, large interlaced PNG/progressive JPEG and oversized compressed blocks may require resaving. Read the file-specific error instead of assuming a valid preview.

PSD/PSB reads the saved RGB/grayscale composite, not Photoshop layers/effects. Save with Maximize Compatibility enabled. Embedded thumbnails are not substituted for native data. ICC profiles and layer effects are not rendered.

## Large PSD/PSB opens slowly

ZIP/prediction composites require a persistent disk cache. Select its folder, drive and limit in Cache…. Use Queue… to prepare a folder or selected files in the background. Check available disk space and the queue status. Cancellation keeps completed cache entries; Retry rechecks sources. Cache ownership/leases protect active entries during cleanup.

## Colors look wrong

Check the actual source encoding, OCIO Configuration, Input space, Display and View. Inspect Input assignments for a more specific folder/set/type rule. A type-only Albedo rule beats an extension-only EXR rule. Use Reload config / LUTs then Apply after moving external LUT resources. Raw or individual channels remain available when a display config is unavailable.

Displacement, Normal and masks intentionally bypass OCIO. Pixel probes and statistics report native values before display exposure/gamma/OCIO. Reduced-detail previews below 100% do not replace exact native analysis.

## A/B results are missing or changed

Missing and ambiguous tiles are explicit statuses. Check the channel/UDIM pairing and chosen folders. Set analysis works on native samples, including supported channels/parts/pages; viewer RGB differences follow the chosen viewer channels. Set the threshold in native units. Changing original files invalidates affected saved results; reconnect an unavailable drive before restoring a session.

## Autosave or profile writes fail

The app retains its preceding valid checkpoint. Check folder permissions and free space. Settings, profiles and recovery storage intentionally remain under %APPDATA%\UDIM Studio for compatibility with earlier versions. Do not remove SQLite -wal/-shm files while running. Session exports keep folder/config paths; correct those paths after moving projects.

## Report a reproducible problem

Open Help & Support → Copy diagnostics, then Report a problem. Include steps, expected/actual behavior, file format/resolution/bit depth and the error text. A small synthetic reproducer is usually enough. Review any attachments for confidential textures or paths before posting publicly.

- [Contact / support](support.md)
- [Quick start](quick-start.md)

## Decoder resource limits

EXR headers and pixels use at most two native worker processes, each with a **768 MiB Windows Job Object limit**. Each request has a **60 second deadline**, including waiting for a worker; cancellation terminates the affected process. A crash or exceeded limit reports an error and the next file can start a new worker. IPC metadata is limited to 1 MiB and each pixel response to 64 MiB. Native EXR limits are 32 parts, 64 channels, 65,536 pixels per dimension, 4,096 per tile dimension, and a 64 MiB compression/block working estimate. Display proxies and block caches have separate bounded budgets. Small native pixel snapshots are limited to 32 MiB; 8K/16K sources remain streamed.

PSD/PSB uses the saved composite, never a document-layer renderer. Reads are limited to 64 MiB, RLE indices to 32 MiB and declared composites to 64 GiB. Metadata inspection has a 10-second deadline and at most 65,536 resource/global-tag records. ZIP preparation checks cancellation within compressed-input loops, stops after 300 seconds and removes its partial cache. Persistent disk-cache quota and free-space checks also apply. These are bounded buffers and cooperative deadlines, rather than a separate OS memory job for the pure Python PSD reader.

OBJ/FBX imports use a **2 GiB Windows job limit**, **120-second total deadline**, 256 MiB input ceiling, 6 million vertices and 4 million source triangles. The child waits for its memory budget before parsing. Transfers are checked for shape, type, indices and byte counts before loading into the UI; decoded array data is limited to 384 MiB. Logs are limited to 4 MiB. OBJ records above 1 MiB are rejected; material-library directives are removed only from the temporary import copy because this viewport uses the assigned UDIM set. Originals are unchanged.

Resource containment limits memory and hangs and keeps a native crash out of the UI process. It is **not a filesystem/network permission sandbox** or a guarantee against vulnerabilities in third-party libraries. Native buffers can coexist with application caches and a model import; per-worker limits are not a total application RAM limit.
