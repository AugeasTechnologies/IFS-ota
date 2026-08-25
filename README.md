# IFS-ota — IonField Systems Control Center OTA channel

Public update channel for **IFS.exe** (the IonField Systems Control Center desktop app;
source is private in `AugeasTechnologies/PlasmaKnife`).

The app checks `latest.json` on launch and self-updates when a newer version is published.

## What's in this repo — one file per purpose

| File | Purpose |
|------|---------|
| `IFS.exe` | The app itself. Portable use: just download and run. Also what the self-updater fetches. |
| `IFS-Setup.exe` | The installer (Start menu, Add/Remove Programs, auto-launch watcher). |
| `opta/plasma_opta.bin` | Opta station firmware, flashed by the app over USB. |
| `display/plasma_display.4XE` + `.cfg` | Station touch-panel firmware, flashed via a 4D programmer. |
| `latest.json`, `opta/opta-latest.json`, `display/display-latest.json` | The manifests: version, download URL, sha256, source stamp. |

There are deliberately **no zip copies**: the 2026-08-18 audit found `IFS-portable.zip` /
`IFS-Setup.zip` still carrying the 1.0.79 build two weeks after 1.0.80 shipped — duplicate
artifacts drift. If someone needs a zip, they can zip the exe themselves; the files above are
the only release artifacts.

## Releasing a new version
1. Bump `APP_VERSION` in the app's `backend.py` and rebuild `IFS.exe` (`build.ps1`).
2. Copy the new `IFS.exe` here, and bump `version` (+ `notes`) in `latest.json`.
3. Commit + push. Installed apps pick it up automatically on next launch.

`latest.json` schema: `{ "version": "x.y.z", "url": "<direct exe download>", "notes": "..." }`

## Release rule — no binary without its source committed first

**Nothing lands in this repo unless the exact source that built it is pushed first**, and the
manifest names it via `source_repo` + `source_commit`. Every artifact here flashes or controls
plasma hardware in the field; a binary whose source only exists on one laptop is unauditable and
unrebuildable (this happened: app 1.0.0→1.0.80 and panel 0.2.0 shipped from uncommitted source).

| Artifact | Source of truth |
|----------|-----------------|
| `IFS.exe` / `IFS-Setup.exe` | `PlasmaKnife` |
| `opta/plasma_opta.bin` | `opta-plasma-firmware` |
| `display/plasma_display.4XE` | `Plasma-StandAlone-V0.1` |

Current stamp status: `opta-latest.json` is stamped (1.3.0 = `opta-plasma-firmware@4c9477d`);
the app 1.0.80 and display 0.2.0 sources are **not yet committed** — stamp them as part of
catching those repos up.
