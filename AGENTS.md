# RelateSpace Updates — AI Context

> Public artifact repository only. **No application source code.**

## Purpose

Hosts the Tauri auto-update feed and signed Windows installers for **RelateSpace**.  
Source code lives in `../relatespace/` (private repo: `Schlomse/relatespace`).

## Structure

```
relatespace-updates/
├── latest.json              # Live update endpoint (always newest version)
└── releases/
    └── vX.Y.Z/
        ├── latest.json      # Immutable per-version manifest
        ├── RelateSpace_X.Y.Z_x64-setup.exe
        ├── RelateSpace_X.Y.Z_x64-setup.exe.sig
        ├── RelateSpace_X.Y.Z_x64_en-US.msi
        └── RelateSpace_X.Y.Z_x64_en-US.msi.sig
```

## Consumers

| Consumer | How it uses this repo |
|----------|----------------------|
| RelateSpace app | `tauri-plugin-updater` reads root `latest.json` |
| Relate Webapp | `/api/space/download` redirects to raw installer URLs |
| End users | Website download buttons via webapp API |

**Updater endpoint:**  
`https://raw.githubusercontent.com/Schlomse/relatespace-updates/main/latest.json`

## Publishing

Artifacts are published from the app repo:

```powershell
cd ../relatespace
npm run publish:update
# or: powershell -File scripts/publish-update.ps1
```

Default target: `../relatespace-updates` (resolved relative to script).

After publish, commit and push **this** repo:

```powershell
cd ../relatespace-updates
git add .
git commit -m "Publish vX.Y.Z update feed"
git push
```

## Rules for AI agents

1. **Do not add** React, Rust, or application source here.
2. **Only edit** `latest.json`, `releases/vX.Y.Z/`, and README/AGENTS docs.
3. **Do not change** installer filenames without updating `relatespace/src-tauri/tauri.conf.json` and webapp `spaceRelease.ts`.
4. **Signatures** must come from `relatespace/scripts/publish-update.ps1` (Tauri signer) — never fabricate.
5. Read workspace root [`AGENTS.md`](../AGENTS.md) and [`docs/RELEASE.md`](../docs/RELEASE.md) for full release coordination.

## Current state

- Latest published: **v0.1.0**
- Platforms: `windows-x86_64`, `windows-x86_64-nsis`, `windows-x86_64-msi`