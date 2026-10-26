# Command palette

Press **Ctrl+K** in the main window or a nonmodal tool window. The same palette is available at **Tools > Command palette**. Type a command name or a keyword such as `resize`, `ORM`, `EXR`, `compare`, `project`, `memory` or `diagnostics`. Common Polish keywords, including accented forms such as `rozdzielczość`, are also recognized; the interface remains English.

Search matches names, groups, descriptions and aliases. Multiple words narrow results. Whole-word matches rank before embedded substrings (for example ORM packing before the word format); simple subsequence matching also helps find abbreviated names. Search is local and queries are never interpreted as scripts, shell commands, paths or URLs.

Use **Up/Down** to select a result, **Page Up/Page Down** for larger jumps, **Enter** or **Run command** to execute, and **Escape** to close. Double-click also runs the selected command. Closing without running restores the previous text-field focus. Ctrl+K preserves Entry/Text content instead of triggering native text-deletion behavior.

Unavailable commands remain visible with a reason, such as a required scan, missing selection, active operation or empty Undo history. Availability is refreshed while the palette is open and checked again immediately before execution. Disabled commands cannot run through Enter or double-click. Commands use the main-table selection; opening from another tool does not silently substitute that tool's selection.

The catalog includes workspace scanning, source/output folders, export preflight, main and selected-file conversion, selected TX export, batch grading/resize/packing, task queue, image inspection, A/B and A/B/C/D comparisons, validators, PBR preview, OCIO, cache and RAM settings, projects, profiles, Undo/Redo, diagnostics, licensing, private updates and offline help. Export commands still pass through the existing preflight and export safeguards. Opening a tool does not itself start its export.

Click the star column or **Add favorite** to pin a command. Empty search lists favorites first, followed by your eight most recently run commands. **Favorites only** narrows the list to starred actions; keyword search still ranks relevance first.

Favorites, recent commands and custom shortcuts persist between application runs in `%APPDATA%\UDIM Studio\command-preferences.json`. Settings are saved atomically, separately from projects and profiles. Queries are not saved or sent to a remote service.

Open **Shortcuts / preferences…**, select a command, type a shortcut and choose **Assign**. **Clear** removes its binding. Supported combinations are Ctrl+Shift+letter, Ctrl+F1–F12, Ctrl+Shift+F1–F12, and standalone F7/F8/F9/F12. For example, assign Ctrl+Shift+R to Resize UDIM sets. Duplicate assignments are rejected with the name of the existing command; clear that assignment first. Reserved editing and application shortcuts, Ctrl+K, Alt/Win and AltGr combinations cannot be overridden.

Custom shortcuts work inside UDIM Forge, including nonmodal tools, and check the same availability and export safeguards as the palette. They do not run while a modal dialog is active. **Reset command preferences** resets favorites, recents and shortcuts and keeps the previous file as a uniquely named backup. A damaged or unsupported preferences file is preserved until you explicitly reset it.

 The palette does not open over modal preflight, trust or confirmation dialogs: finish or cancel that dialog first. No system-wide keyboard hook is installed.
