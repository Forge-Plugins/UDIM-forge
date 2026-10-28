# System compatibility and beta testing

Open **Help & Support → System check**, or search **System check** in Ctrl+K. The checks are optional and never run automatically at startup. Click **Run checks**; enable the optional GPU PBR test when investigating a graphics issue. Select a result for its full explanation and suggested action. Save a local JSON report or copy the summary for support. Nothing is uploaded automatically and reports omit user texture paths, account names and license keys.

## What is actually tested

- Windows x64 architecture, current installed/available physical RAM and the current screen/DPI information.
- A temporary write test in the settings directory or its nearest existing parent. This is not an export-destination preflight.
- Real PNG 16-bit, EXR float32 and TIFF float32 export/reopen of tiny synthetic images, including pixel values, channel count and bit depth.
- Native Unicode paths containing Polish and Japanese characters.
- An optional small GPU PBR frame in a dedicated process. It does not invalidate the open preview's GPU caches. An unavailable GPU produces a warning; CPU preview remains a separate supported path.

Checks run in a disposable subprocess with a 1.5 GiB memory cap and 90-second overall timeout. Cancel/close stops the probe; previous completed results remain available after cancellation. The application's parent RAM policy still applies. Only temporary synthetic images and a short settings write test are created; originals are never opened. On failure, local application logs retain diagnostic details, while the shareable result uses a generic error rather than private paths.

**Pass** means this check passed here. **Warning** identifies a limitation or something to investigate. **Fail** needs action. **Skipped** means not tested; it is not a pass. Eight GiB RAM is a starting guideline, not a certified minimum for every asset. Large models, 8K/16K sets and simultaneous tools can need substantially more RAM/VRAM. Configure global RAM and per-preview GPU budgets for the actual workload.

## Release verification matrix

| Scenario | Evidence / status |
| --- | --- |
| Current development computer, Windows x64, NVIDIA RTX 3090 | Automated native codec/GPU, queue, project recovery, comparison and report acceptance. |
| Portable executable in a Unicode folder, empty application profile, PATH without Python/Conda | Isolated runtime smoke: system probe, GUI checks, real CLI exports/resume and unchanged original hashes. This uses the same host OS. |
| MSI fresh install / upgrade / uninstall | Real per-user lifecycle using a separate test identity, desktop and registry association; preserves user files. |
| Completely clean Windows VM without developer software | Not tested. No configured VM was available. |
| AMD, Intel and integrated graphics | Not certified; external beta testing required. A local System check is useful but insufficient. |
| Other Windows builds, lower-memory computers | Not certified; external beta testing required. |
| Physical 125%, 150%, 200% display scaling | Pending manual hardware/OS verification. The Help sidebar scrolls at the minimum window size; this is not a DPI certification. |
| Network/cloud folders, disk disconnect, power loss | Existing cancellation/recovery tests do not certify every storage provider or a real hardware power-loss scenario. Pending external tests. |

## Short beta scenario

1. Install the MSI or extract the portable ZIP into a writable local folder; launch without Python setup.
2. Run System check and retain the JSON locally. Record OS/build, GPU, RAM, display scaling and app version.
3. Import a small UDIM set, inspect map assignments and source/output columns, choose formats/resolutions and review the export plan.
4. Export copies, validate the output, save a project, close/reopen and confirm assignments and queue state.
5. Compare A/B using a matching UV0 model/HDRI, navigate change regions and export an offline HTML report.
6. Repeat cancellation, an invalid input and a moved project. Original file hashes must remain unchanged. Attach reviewed diagnostics to support only when you choose to share them.

The app is a beta candidate until the pending rows are independently verified. The Gumroad product and licensing policy still need publisher configuration. A successful local system check does not change that release status.
