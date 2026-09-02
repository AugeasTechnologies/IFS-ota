# IFS-ota — IonField Systems Control Center OTA channel

Public update channel for **IFS.exe** (the IonField Systems Control Center desktop app;
source is private in `AugeasTechnologies/PlasmaKnife`).

The app checks `latest.json` on launch and self-updates when a newer version is published.

## What's in this repo — one file per purpose

| File | Purpose |
|------|---------|
| `IFS.exe` | The app itself. Portable use: just download and run. Also what the self-updater fetches. |
| `IFS-Control-Suite.zip` | **The download to give a customer whose Windows flags `IFS.exe`.** Same app, but the program Windows launches is python.org's PSF-signed `python.exe`, which SmartScreen already trusts. Unzip, run the shortcut — no install, no admin. |
| `IFS-Setup.exe` | The installer (Start menu, Add/Remove Programs, auto-launch watcher). |
| `opta/plasma_opta.bin` | Opta station firmware, flashed by the app over USB. |
| `display/plasma_display.4XE` + `.cfg` | Station touch-panel firmware, flashed via a 4D programmer. |
| `latest.json`, `opta/opta-latest.json`, `display/display-latest.json` | The manifests: version, download URL, sha256, source stamp. |

There are deliberately **no zip *copies* of the exe**: the 2026-08-18 audit found
`IFS-portable.zip` / `IFS-Setup.zip` still carrying the 1.0.79 build two weeks after 1.0.80
shipped — duplicate artifacts drift. `IFS-Control-Suite.zip` is not that: it is a distinct
artifact (embedded-runtime build, not a repack of `IFS.exe`), rebuilt each release by
`scripts/build-bundle.ps1` and stamped in `latest.json` with its own `bundle_version` +
`bundle_sha256`, so drift is visible the moment it happens.

## If Windows calls `IFS.exe` a trojan

It is a false positive with structural causes — a self-signed certificate that carries no
reputation, and a PyInstaller onefile binary that is brand-new on every release. The full
measured diagnosis lives in the app repo at `docs/ANTIVIRUS.md`.

What to do for a blocked customer, in order:

1. Point them at **`IFS-Control-Suite.zip`** (table above). It exists precisely for this.
2. Report the false positive to Microsoft: <https://www.microsoft.com/en-us/wdsi/filesubmission>,
   as *Software developer*, disputing the detection. Clears in ~24–72 h — but only for that
   binary, so it must be repeated per release while the certificate stays self-signed.
3. Do **not** tell them to add an antivirus exclusion. It trains customers to disable
   protection for us, and it is the exclusion that gets expensive if we are ever actually
   compromised.

## Releasing a new version
1. Bump `APP_VERSION` in the app's `backend.py` and rebuild `IFS.exe` (`build.ps1`).
2. Rebuild the bundle too (`scripts\build-bundle.ps1`) — it must never lag the exe.
3. Copy the new `IFS.exe` + `IFS-Control-Suite.zip` here; bump `version`, `bundle_version`,
   both sha256 fields (+ `notes`) in `latest.json`.
4. Commit + push. Installed apps pick it up automatically on next launch.
5. Submit the new `IFS.exe` to Microsoft as a known false positive (link in the section
   above). Every release is a new binary with zero reputation until the certificate is real.

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

Current stamp status: `opta-latest.json` is stamped (1.3.0 = `opta-plasma-firmware@4c9477d`) and
`latest.json` is stamped (app 1.0.82 = `PlasmaKnife@0efacfc`; 1.0.81 was the first app release built from
committed source). Display 0.2.0 is still **not committed** — stamp it as part of catching that
repo up.

`latest.json` also carries **`installer_version`**. A machine whose app folder is read-only (a
Program Files install run by a normal user) updates *through* the installer, so an installer older
than the version being offered would install a downgrade and then be offered the same update again
on every launch — a silent loop. The app refuses that and names the installer to ask for.

As of 1.0.82 `build.ps1` builds and signs `IFS-Setup.exe` itself from the same `$ver` (the `.iss`
takes `/DAppVersion=` and no longer hardcodes a number), so the two cannot drift apart. If ISCC is
missing on the build machine the script says so and warns you **not** to raise `installer_version`.
