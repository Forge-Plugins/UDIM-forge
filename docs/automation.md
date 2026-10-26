# CLI and local benchmarks

The same guarded codecs and durable production queue are available without opening the GUI. The Windows executable is a windowed application, so PowerShell should wait explicitly. All commands require a JSON report path separate from their inputs.

```powershell
$app = 'C:\path\UDIM Forge.exe'
Start-Process -FilePath $app -ArgumentList '--cli plan --source C:\Textures --output C:\Exports --report C:\Plan.json' -Wait
Start-Process -FilePath $app -ArgumentList '--cli run --plan C:\Plan.json --queue C:\Queue.sqlite3 --report C:\Run.json --memory-mib 4096 --workers 2' -Wait
Start-Process -FilePath $app -ArgumentList '--cli resume --queue C:\Queue.sqlite3 --report C:\Resume.json --memory-mib 4096' -Wait
```

Quote paths containing spaces inside ArgumentList. Repeated `--source` adds sets. `--recursive` includes subfolders. `--recipe` accepts a saved workflow recipe. Plan writes a full JSON job plan, without exporting images. Run executes that plan; Resume uses the durable queue. Create the path given to `--cancel-file` to pause a running command. Exit codes are 0 success, 1 unfinished/failed jobs, 2 invalid request, 130 cancellation. GUI-reviewed plans can include explicit OCIO baking; bare recipes requesting baking are rejected rather than silently ignoring color management.

Use **Help & Support > Run local benchmark** to save an opt-in report of synthetic 2K PNG16/EXR32 conversion and cached GPU preview timings. No source textures or paths are included and nothing is uploaded. GPU timing includes worker IPC/readback, not the complete GUI frame rate. An unavailable GPU is reported while conversion results remain useful. The command-line equivalent is `--benchmark C:\Benchmark.json`.

## Beta acceptance checklist

Before commercial rollout, run real workflows on a clean Windows account and several hardware configurations: NVIDIA/AMD/Intel, integrated/discrete GPUs, 100/150/200% DPI, low RAM and low free disk space. Test 8K/16K EXR/PSD/PSB, malformed files, cancellation, interruption during export, recovery, project relink, changed-source cache invalidation and private update failure. Check display clipping at minimum window size. Use diagnostic export preview to redact paths before sharing a report through the support issue form.

Record app version, OS, GPU/driver, memory budget, dataset dimensions, elapsed time, peak memory and pass/fail per workflow. The bundled local benchmark helps compare runs; it is not a substitute for human beta testing. The current GPU implementation has been exercised on NVIDIA RTX 3090; AMD/Intel and other machines require separate acceptance.

## Distribution and rollback

Portable packages preserve previous releases. The optional per-user x64 MSI provides desktop shortcut, uninstall support and `.udimforge` association under the current user. AppData project recovery/settings are preserved on uninstall. Make a backup of projects before upgrading or rolling back; older applications may not understand newer project schemas.

Offline license envelopes and private update inventories use publisher RSA signatures. This internal build has license enforcement disabled and does not start a trial or contact a store. Gumroad is selected, with purchase verification prepared in 0.14.1; the real product and any private update endpoint still require configuration. Private updates require explicit HTTPS or a signed local manifest and verify signed size/SHA-256 before making a package available; they do not auto-run installers. Public GitHub downloads remain disabled. Authenticode signing is not configured for this local release.

Run/resume now performs [export preflight](preflight.md). Add `--accept-preflight-warnings` only after reviewing the plan; blocking errors remain blocking. MSI lifecycle QA uses a separate product identity and association, checks real install/upgrade/uninstall on the development host, and preserves unmanaged files. It is not a clean-Windows VM or a guarantee for every target system.
