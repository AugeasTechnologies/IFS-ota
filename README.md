# IFS-ota — IonField Systems Control Center OTA channel

Public update channel for **IFS.exe** (the IonField Systems Control Center desktop app;
source is private in `AugeasTechnologies/PlasmaKnife`).

The app checks `latest.json` on launch and self-updates when a newer version is published.

## Releasing a new version
1. Bump `APP_VERSION` in the app's `backend.py` and rebuild `IFS.exe` (`build.ps1`).
2. Copy the new `IFS.exe` here, and bump `version` (+ `notes`) in `latest.json`.
3. Commit + push. Installed apps pick it up automatically on next launch.

`latest.json` schema: `{ "version": "x.y.z", "url": "<direct exe download>", "notes": "..." }`
