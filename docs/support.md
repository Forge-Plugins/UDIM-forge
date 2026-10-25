# Contact / support

UDIM Forge 0.10.0 is developed under Forge-Plugins. The GitHub repository is the public documentation, changelog and support hub. This is a commercial application. Application source, binaries and license delivery are not published in this Git repository.

- [Report a problem](https://github.com/Forge-Plugins/UDIM-forge/issues/new?template=bug_report.yml)
- [Request a feature](https://github.com/Forge-Plugins/UDIM-forge/issues/new?template=feature_request.yml)
- [Browse existing issues](https://github.com/Forge-Plugins/UDIM-forge/issues)
- [Project and online documentation](https://github.com/Forge-Plugins/UDIM-forge)

## Before posting

Use Help & Support → Copy diagnostics. Paste the result into the issue, add reproduction steps and explain expected versus actual behavior. Include the texture format, dimensions, bit depth and relevant settings. For OCIO problems, include the config name and selected input/display/view or assignment scope.

Diagnostics contain the application/dependency versions and operating-system basics. They do not include your source paths, settings, texture pixels or session contents. Copying diagnostics only puts text on your clipboard; the app does not send a report automatically. Browser links open only when selected.

GitHub issues are public. Prefer a small synthetic sample when reproducing a texture problem. Review screenshots and session exports before attaching them; session files contain folder/config paths. No separate email address or private upload channel has been configured.

## Local files and compatibility

### Persistent error logs and reviewed diagnostics

Errors are saved across launches in `%APPDATA%\UDIM Studio\logs`. Logs rotate at 2 MiB with two backups per process; older inactive logs are pruned. Local logs can contain project paths. Production workers also keep job-specific logs under `queue-work`.

Use **Help & Support → Export diagnostics…** to review a snapshot of recent application error logs and version information. Select each listed file to see its exact text. Local paths are masked by default; including them requires the explicit checkbox. Recognized credentials are redacted either way. **Refresh preview** takes a new snapshot. **Export reviewed ZIP…** writes exactly the reviewed text, including no entries logged after that snapshot. Nothing is uploaded automatically; attach the ZIP to an issue yourself after review. Source textures, settings and comparison-session contents are excluded. Worker details are recorded in the application log when a queue job fails; large per-job raw logs are not automatically bundled.

### Relink moved projects

Open **Compare sets… → Session → Relink moved project…**. Load a saved session if necessary. Choose replacement folders for A/B/C/D. An optional old/new project-root mapping also rebases shared OCIO configuration paths and folder-scoped input assignments. The most specific matching root wins. Preserve the relative config/LUT resource layout when moving a project.

**Preview remapping** lists the proposed folders. **Verify contents** runs SHA-256 checks in the background. **Apply verified project** updates the session only after verification; cancellation leaves the current project untouched. Identical file contents retain analysis even if the drive, timestamps or filesystem identity changed. Different, missing or unverifiable files require reanalysis. The session folders, reference, threshold and OCIO assignments remain associated with the project.

New analyses store content hashes. A legacy session without hashes can be upgraded while its original files remain accessible. If those originals have already disappeared, historical metrics cannot safely prove that a replacement is identical, so those comparisons are reanalyzed. Keep session exports backed up before moving projects. Relinking does not move texture files, migrate the production queue or rewrite external OCIO config/LUT contents.

Application branding is now UDIM Forge. Existing settings, profiles, cache ownership and session/report identifiers remain compatible with UDIM Studio. The settings location stays %APPDATA%\UDIM Studio. Rename/export operations still protect original source files; pixel-processing tools export modified copies.

- [Quick start](quick-start.md)
- [Troubleshooting](troubleshooting.md)
- [What’s new](changelog.md)

## Earlier WinError 17 conversion failures

Versions before 0.3.0 could prepare files in queue-work on C: and fail when renaming them to a different output drive. The fixed queue writes final staging on the destination drive. After updating, **Retry failed** requeues failures and resumes unfinished work. Existing prepared outputs are reused only after source/output hashes and settings match; other files are recalculated. Queue history and verified completed outputs are preserved. The updated Cancel current work button pauses processing, including native TX, rather than cancelling only plan preparation.
