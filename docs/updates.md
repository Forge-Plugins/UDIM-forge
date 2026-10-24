# Downloads and updates

## Download the portable app

Use [GitHub Releases](https://github.com/Forge-Plugins/UDIM-forge/releases/latest) and download the Windows x64 portable ZIP, for example UDIM_Forge_v0.4.0_Windows_x64_Portable.zip. Extract the whole archive to a writable folder and run UDIM Forge.exe. Python is not required.

The Git repository contains documentation, changelog and issue templates. The portable program is a Release asset, not a file committed to Git. GitHub's automatically generated source archives contain the documentation repository, not the application. Choose the named Portable.zip asset.

## Check for a newer version

Open Help & Support → Check for updates…. The check runs in a background thread and uses GitHub's public latest-stable-release metadata. A GitHub account is not required. It runs only when requested, not automatically at startup.

The dialog shows the installed/latest version and release notes. Download portable opens the exact project's ZIP asset in your browser. Open Releases is available if a check fails or a matching Windows x64 ZIP is missing. No automatic installation or restart occurs.

A connection error, GitHub rate limit or missing release is reported separately; these states are never presented as an up-to-date result. Use Check again to retry. Draft and prerelease builds are not part of the stable update channel.

## Replace your portable copy

1. Finish or cancel active exports and close the old application.
2. Extract the new ZIP to a new folder or replace the old portable files while the app is closed.
3. Run the new UDIM Forge.exe. Update any shortcut if you changed the installation folder.

User settings, profiles and recovery storage remain in %APPDATA%\UDIM Studio for compatibility. OCIO config/LUT paths and texture/cache paths remain your own external resources. Updating the application does not replace original textures.

Each release includes SHA256SUMS.txt. On Windows, use Get-FileHash on the downloaded ZIP with Algorithm SHA256 and compare the result with that file. The checksum verifies the downloaded bytes against the published package; it is not a publisher signature.

- [Quick start](quick-start.md)
- [Troubleshooting](troubleshooting.md)
- [Contact / support](support.md)
